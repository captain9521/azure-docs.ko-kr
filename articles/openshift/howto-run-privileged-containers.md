---
title: Azure Red Hat OpenShift 클러스터에서 권한 있는 컨테이너 실행 | Microsoft Docs
description: 권한 있는 컨테이너를 실행 하 여 보안 및 규정 준수를 모니터링 합니다.
author: makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 12/05/2019
keywords: aro, openshift, aquasec, twistlock, red hat
ms.openlocfilehash: 914b29410a0f30e5c3d3a893c2e278ecbb83b648
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92218870"
---
# <a name="run-privileged-containers-in-an-azure-red-hat-openshift-cluster"></a>Azure Red Hat OpenShift 클러스터에서 권한 있는 컨테이너 실행

> [!IMPORTANT]
> Azure Red Hat OpenShift 3.11는 30 월 2022에 사용 중지 됩니다. 새 Azure Red Hat OpenShift 3.11 클러스터 만들기에 대 한 지원은 30 년 11 2020 월 30 일까 지 계속 됩니다. 사용 중지 후에는 나머지 Azure Red Hat OpenShift 3.11 클러스터가 종료 되어 보안 취약점을 방지 합니다.
> 
> 이 가이드에 따라 [Azure Red Hat OpenShift 4 클러스터를 만듭니다](tutorial-create-cluster.md).
> 특정 질문이 있는 경우 문의해 주시기 [바랍니다](mailto:arofeedback@microsoft.com).

Azure Red Hat OpenShift 클러스터에서 임의의 권한 있는 컨테이너를 실행할 수 없습니다.
두 보안 모니터링 및 규정 준수 솔루션은 ARO 클러스터에서 실행할 수 있습니다.
이 문서에서는 보안 제품 공급 업체의 일반 OpenShift 배포 설명서와의 차이점에 대해 설명 합니다.


공급 업체의 지침을 수행 하기 전에 다음 지침을 참조 하세요.
아래 제품 관련 단계의 섹션 제목은 공급 업체의 설명서에 있는 섹션 제목에 직접 참조 합니다.

## <a name="before-you-begin"></a>시작하기 전에

대부분의 보안 제품에 대 한 설명서에서는 클러스터 관리자 권한이 있다고 가정 합니다.
고객 관리자에 게 Azure Red Hat OpenShift에 대 한 모든 권한이 없습니다. 클러스터 전체 리소스를 수정 하는 데 필요한 권한은 제한 됩니다.

먼저를 실행 하 여 사용자가 고객 관리자로 클러스터에 로그인 했는지 확인 `oc get scc` 합니다. Customer admin 그룹의 멤버인 모든 사용자에 게는 클러스터에서 SCCs (보안 컨텍스트 제약 조건)를 볼 수 있는 권한이 있습니다.

그런 다음 `oc` 이진 버전이 인지 확인 `3.11.154` 합니다.
```
oc version
oc v3.11.154
kubernetes v1.11.0+d4cacc0
features: Basic-Auth GSSAPI Kerberos SPNEGO

Server https://openshift.aqua-test.osadev.cloud:443
openshift v3.11.154
kubernetes v1.11.0+d4cacc0
```

## <a name="product-specific-steps-for-aqua-security"></a>바다색 보안을 위한 제품별 단계
수정 하려는 기본 지침은 [바다색 보안 배포 설명서](https://docs.aquasec.com/docs/openshift-red-hat)에서 찾을 수 있습니다. 여기에 나와 있는 단계는 바다색 배포 설명서와 함께 실행 됩니다.

첫 번째 단계는 업데이트 되는 필수 SCCs에 주석을 추가 하는 것입니다. 이러한 주석은 클러스터의 동기화 Pod가 이러한 SSCs로 변경 내용을 되돌리지 못하도록 합니다.

```
oc annotate scc hostaccess openshift.io/reconcile-protect=true
oc annotate scc privileged openshift.io/reconcile-protect=true
```

### <a name="step-1-prepare-prerequisites"></a>1 단계: 필수 구성 요소 준비
클러스터 관리자 역할 대신 ARO Customer Admin으로 클러스터에 로그인 해야 합니다.

프로젝트 및 서비스 계정을 만듭니다.
```
oc new-project aqua-security
oc create serviceaccount aqua-account -n aqua-security
```

클러스터-판독기 역할을 할당 하는 대신 다음 명령을 사용 하 여 고객 관리 클러스터 역할을 청록색 계정에 할당 합니다.
```
oc adm policy add-cluster-role-to-user customer-admin-cluster system:serviceaccount:aqua-security:aqua-account
oc adm policy add-scc-to-user privileged system:serviceaccount:aqua-security:aqua-account
oc adm policy add-scc-to-user hostaccess system:serviceaccount:aqua-security:aqua-account
```

1 단계의 나머지 지침에 따라 계속 합니다.  이러한 지침은 바다색 레지스트리의 비밀을 설정 하는 방법을 설명 합니다.

### <a name="step-2-deploy-the-aqua-server-database-and-gateway"></a>2 단계: 바다색 서버, 데이터베이스 및 게이트웨이 배포
바다색 문서에 설명 된 단계에 따라 바다색-콘솔. yaml을 설치 합니다.

제공 된를 수정 `aqua-console.yaml` 합니다.  및 라는 레이블이 지정 된 상위 두 개체를 제거 합니다 `kind: ClusterRole` `kind: ClusterRoleBinding` .  사용자 관리자에 게 및 개체를 수정할 수 있는 권한이 없기 때문에 이러한 리소스는 생성 되지 않습니다 `ClusterRole` `ClusterRoleBinding` .

두 번째 수정은의 부분에 대 한 것입니다 `kind: Route` `aqua-console.yaml` . 파일의 개체에 대해 다음 yaml를 바꿉니다 `kind: Route` `aqua-console.yaml` .
```
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  labels:
    app: aqua-web
  name: aqua-web
  namespace: aqua-security
spec:
  port:
    targetPort: aqua-web
  tls:
    insecureEdgeTerminationPolicy: Redirect
    termination: edge
  to:
    kind: Service
    name: aqua-web
    weight: 100
  wildcardPolicy: None
```

나머지 지침을 따릅니다.

### <a name="step-3-login-to-the-aqua-server"></a>3 단계: 바다색 서버에 로그인
이 섹션은 어떤 방식으로든 수정 되지 않습니다.  바다색 문서를 따르세요.

다음 명령을 사용 하 여 바다색 콘솔 주소를 가져옵니다.
```
oc get route aqua-web -n aqua-security
```

### <a name="step-4-deploy-aqua-enforcers"></a>4 단계: 바다색 Enforcers 배포
Enforcers를 배포할 때 다음 필드를 설정 합니다.

| 필드          | 값         |
| -------------- | ------------- |
| 오케스트레이터   | OpenShift     |
| ServiceAccount | 바다색-계정  |
| Project        | 바다색-보안 |

## <a name="product-specific-steps-for-prisma-cloud--twistlock"></a>Twistlock Sma Cloud/에 대 한 제품별 단계

수정 하려는 기본 지침은 기본 [Sma 클라우드 배포 설명서](https://docs.paloaltonetworks.com/prisma/prisma-cloud/19-11/prisma-cloud-compute-edition-admin/install/install_openshift.html) 에서 확인할 수 있습니다.

"설치 하는 방법" `twistcli` 에 설명 된 대로 도구를 설치 하 여 시작 합니다.

새 OpenShift 프로젝트 만들기
```
oc new-project twistlock
```

선택적 섹션인 "개인 레지스트리에 대 한 대상 레지스트리를 푸시합니다." 섹션을 건너뜁니다. Azure Red Hat OpenShift에서는 작동 하지 않습니다. 대신 온라인 레지스트리를 사용 하십시오.

아래에 설명 된 수정 내용을 적용 하는 동안 공식 설명서를 따를 수 있습니다.
"콘솔 설치" 섹션으로 시작 합니다.

### <a name="install-console"></a>콘솔 설치

`oc create -f twistlock_console.yaml`2 단계에서 네임 스페이스를 만들 때 오류가 발생 합니다.
명령을 사용 하 여 이전에 만든 네임 스페이스를 안전 하 게 무시할 수 있습니다 `oc new-project` .

`azure-disk`저장소 형식에 사용 합니다.

### <a name="create-an-external-route-to-console"></a>콘솔에 대 한 외부 경로 만들기

Oc 명령을 선호 하는 경우 설명서 또는 아래 지침을 따를 수 있습니다.
다음 경로 정의를 컴퓨터의 twistlock_route 라는 파일에 복사 합니다.
```
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  labels:
    name: console
  name: twistlock-console
  namespace: twistlock
spec:
  port:
    targetPort: mgmt-http
  tls:
    insecureEdgeTerminationPolicy: Redirect
    termination: edge
  to:
    kind: Service
    name: twistlock-console
    weight: 100
  wildcardPolicy: None
```
그런 후 다음을 실행 합니다.
```
oc create -f twistlock_route.yaml
```

다음 명령을 사용 하 여 Twistlock 콘솔에 할당 된 URL을 가져올 수 있습니다. `oc get route twistlock-console -n twistlock`

### <a name="configure-console"></a>콘솔 구성

Twistlock 설명서를 따르세요.

### <a name="install-defender"></a>Defender 설치

`oc create -f defender.yaml`2 단계에서는 클러스터 역할 및 클러스터 역할 바인딩을 만들 때 오류가 발생 합니다.
이런 예외는 무시할 수 있습니다.

방어자는 계산 노드에만 배포 됩니다. 노드 선택기를 사용 하 여 제한할 필요가 없습니다.
