---
title: 테스트 드라이브 기술 구성, Microsoft 상업적 marketplace
description: 테스트 드라이브에 대해 알아봅니다. 시험 사용을 통해 새 고객이 제품을 시험 하 여 구매할 수 있습니다.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 08/13/2019
author: keferna
ms.author: keferna
ms.openlocfilehash: bf253c4e3b273bc1500e5abbd4dfc045a7909ffd
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93125685"
---
# <a name="test-drive-technical-configuration"></a>시험 사용 기술 구성

Microsoft 상업적 marketplace의 시험 사용 옵션을 사용 하면 제품의 주요 기능에 대 한 실습 자체 기반 둘러보기를 구성할 수 있습니다. 시험 사용을 통해 새 고객이 제품을 구입 하기 전에 제품을 사용해 볼 수 있습니다. 자세한 내용은 [시험 사용이란?](what-is-test-drive.md)을 참조하세요.

제품에 대 한 시험 사용을 더 이상 제공 하지 않으려는 경우 **제품 설치** 페이지로 돌아와서 **테스트 드라이브 사용** 확인란의 선택을 취소 합니다. 일부 제품 유형에는 테스트 드라이브를 사용할 수 없습니다.

## <a name="azure-resource-manager-test-drive"></a>시험 드라이브 Azure Resource Manager

이는 가상 머신 또는 Azure 앱 제품에 대 한 유일한 테스트 드라이브 옵션 이며 매우 자세한 설정이 필요 합니다. [배포 구독 세부 정보](#deployment-subscription-details) 및 [테스트 드라이브 목록](#test-drive-listings)에 대 한 아래 섹션을 읽은 다음 [Azure Resource Manager 테스트 드라이브 구성](azure-resource-manager-test-drive.md)에 대 한 별도의 항목을 계속 진행 합니다.

## <a name="hosted-test-drive"></a>호스트 된 테스트 드라이브

Microsoft는 서비스 프로 비전 및 프로 비전 해제를 호스트 하 고 유지 관리 하 여 테스트 드라이브를 설정 하는 복잡성을 제거할 수 있습니다. 이 유형의 테스트 드라이브에 대 한 구성은 테스트 드라이브가 Dynamics 365 Customer Engagement를 대상으로 하는지 또는 Dynamics 365 작업 대상을 대상으로 하는지 여부에 관계 없이 동일 합니다.

- **최대 동시 시험 사용 수** (필수) – 시험 사용을 동시에 사용할 수 있는 최대 고객 수를 설정합니다. 테스트 드라이브가 활성화 된 상태에서 각 동시 사용자는 Dynamics 365 라이선스를 사용 하므로 최대 제한 집합을 지원할 수 있는 충분 한 라이선스가 있는지 확인 합니다. 권장 값은 3-5입니다.

- **테스트 드라이브 기간** (필수) – 테스트 드라이브가 각 고객에 대해 활성 상태로 유지 되는 시간 (분)을 입력 합니다. 이 기간이 지나면 세션이 종료 되 고 더 이상 라이선스 중 하나를 사용 하지 않습니다. 권장 값은 제품의 복잡성에 따라 2~24시간입니다. 이 기간은 전체 시간에만 설정할 수 있습니다. 예를 들어 "2 시간"은 유효 합니다. "1.5 시간"은 그렇지 않습니다. 시간이 만료된 후 시험 사용에 다시 액세스하려는 경우 새 세션을 요청할 수 있습니다.

- **인스턴스 URL** (필수) – 고객이 시험 사용을 시작하는 URL입니다. 일반적으로 샘플 데이터가 설치된 앱을 실행하는 Dynamics 365 인스턴스의 URL입니다(예: `https://testdrive.crm.dynamics.com`).

- **인스턴스 웹 api url** (필수) – Microsoft 365 계정에 로그인 하 고 **설정**  >  **사용자 지정**  >  **개발자 리소스**  >  **인스턴스 웹 API (서비스 루트 URL)** 로 이동 하 여 Dynamics 365 인스턴스의 web api url을 검색 하 고 여기에 있는 URL (예:)을 복사 `https://testdrive.crm.dynamics.com/api/data/v9.0` 합니다.

- **역할 이름** (필수) – 사용자 지정 Dynamics 365 시험 사용에서 정의한 보안 역할 이름을 제공합니다(예: test-drive-role). 시험 사용 중 사용자에게 이 역할이 할당됩니다.

시험 사용을 위한 Dynamics 365 환경을 설정 하 고 테 넌 트에서 테스트 드라이브 사용자를 프로 비전 하 고 프로 비전 할 수 있는 AppSource 권한을 부여 하는 방법에 대 한 도움말을 보려면 [다음 지침](https://github.com/Microsoft/AppSource/blob/patch-1/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md)을 따르세요.

## <a name="logic-app-test-drive"></a>논리 앱 테스트 드라이브

이 유형의 테스트 드라이브는 Microsoft에서 호스팅하지 않습니다. 이를 통해 다양 한 복잡 한 솔루션 아키텍처를 포함 하는 Dynamics 365 제품 또는 기타 사용자 지정 리소스에 연결할 수 있습니다. 논리 앱 시험 사용을 설정하는 방법에 대한 자세한 내용은 GitHub의 [작업](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) 및 [고객 참여](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app)를 참조하세요.

- **지역** (필수, 단일 선택 드롭다운 목록) - 현재 시험 사용을 사용할 수 있는 26개의 Azure 지원 지역이 있습니다. 논리 앱에 대한 리소스는 선택한 지역에 배포됩니다. 논리 앱이 특정 지역에 저장된 사용자 지정 리소스를 포함하는 경우 여기서 해당 지역을 선택해야 합니다. 포털에서 논리 앱을 Azure 구독에 로컬로 완전히 배포하고 이를 선택하기 전에 제대로 작동하는지 확인하는 것이 가장 좋습니다.

- **최대 동시 시험 사용 수** (필수) – 시험 사용을 동시에 사용할 수 있는 최대 고객 수를 설정합니다. 이러한 시험 사용은 이미 배포되어 있어 고객은 배포를 기다리지 않고 즉시 액세스할 수 있습니다.

- **시험 사용 기간** (필수) – 시험 사용이 활성 상태로 유지되는 기간(시간 단위)을 입력합니다. 이 기간이 끝나면 시험 사용이 자동으로 종료됩니다.

- **Azure 리소스 그룹 이름** (필수)-논리 앱 테스트 드라이브가 저장 되는 [azure 리소스 그룹](../azure-resource-manager/management/overview.md#resource-groups)이름을 입력 합니다.

- **Azure 논리 앱 이름** (필수) – 시험 사용을 사용자에게 할당하는 논리 앱의 이름을 입력합니다. 이 논리 앱은 위의 Azure 리소스 그룹에 저장되어야 합니다.

- **논리 앱 이름 프로 비전** 해제 (필수) – 고객이 완료 되 면 테스트 드라이브를 프로 비전 해제 하는 논리 앱의 이름을 입력 합니다. 이 논리 앱은 위의 Azure 리소스 그룹에 저장되어야 합니다.

## <a name="power-bi-test-drive"></a>Power BI 시험 사용

대화형 Power BI 시각적 개체를 시연하려는 제품은 포함된 링크를 사용하여 사용자 지정 빌드 대시보드를 시험 사용으로 공유할 수 있으며 추가 기술 구성은 필요하지 않습니다. 여기서는 포함 된 Power BI URL을 업로드 하기만 하면 됩니다.

Power BI 앱을 설정 하는 방법에 대 한 자세한 내용은 [Power BI 앱 이란?](/power-bi/service-template-apps-overview) 을 참조 하세요.

## <a name="deployment-subscription-details"></a>배포 구독 세부 정보

Microsoft가 사용자를 대신해서 시험 사용을 배포하도록 하려면 별도의 고유한 Azure 구독을 만들고 제공합니다(Power BI 시험 사용에는 필요하지 않음).

- **Azure 구독 ID** (Azure Resource Manager 및 Logic Apps에 필요) - 리소스 사용량 보고 및 청구를 위해 Azure 계정 서비스에 대한 액세스 권한을 부여하는 구독 ID를 입력합니다. 아직 없는 경우 시험 사용에 사용할 [별도의 Azure 구독을 만드는 것](../cost-management-billing/manage/create-subscription.md)이 좋습니다. [Azure Portal](https://portal.azure.com/)에 로그인하여 왼쪽 메뉴의 **구독** 탭으로 이동하면 Azure 구독 ID를 확인할 수 있습니다. 이 탭을 선택하면 구독 ID(예: "a83645ac-1234-5ab6-6789-1h234g764ghty")가 표시됩니다.

- **AZURE ad 테 넌 트 id** (필수) – ad (Azure Active Directory) [테 넌 트 id](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in)를 입력 합니다. 이 ID를 찾으려면 [Azure Portal](https://portal.azure.com/)에 로그인하고 왼쪽 메뉴에서 Active Directory 탭을 선택하고 **속성** 을 선택한 다음 나열된 **디렉터리 ID** 번호(예: 50c464d3-4930-494c-963c-1e951d15360e)를 찾습니다. [https://www.whatismytenantid.com](https://www.whatismytenantid.com)에서 도메인 이름 주소를 사용하여 조직의 테넌트 ID를 조회할 수도 있습니다.

- **Azure AD 테넌트 이름** (Dynamic 365에 필요) – Azure AD(Active Directory) 이름을 입력합니다. 이 이름을 찾으려면 [Azure Portal](https://portal.azure.com/)에 로그인합니다. 그러면 오른쪽 위 계정 이름 아래에 테넌트 이름이 나열됩니다.

- **AZURE ad 앱 id** (필수) – AZURE ACTIVE DIRECTORY (AD) [응용 프로그램 id](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in)를 입력 합니다. 이 ID를 찾으려면 [Azure Portal](https://portal.azure.com/)에 로그인 하 고, 왼쪽 메뉴에서 Active Directory 탭을 선택 하 고, **앱 등록** 를 선택한 다음 나열 된 **응용 프로그램 ID** 번호 (예:)를 찾습니다 `50c464d3-4930-494c-963c-1e951d15360e` .

- **AZURE ad 앱 클라이언트 암호** (필수) – azure ad 응용 프로그램 [클라이언트 암호](../active-directory/develop/howto-create-service-principal-portal.md#option-2-create-a-new-application-secret)를 입력 합니다. 이 값을 찾으려면 [Azure Portal](https://portal.azure.com/)에 로그인합니다. 왼쪽 메뉴에서 **Azure Active Directory** 탭을 선택 하 **앱 등록** 를 선택 하 고 테스트 드라이브 앱을 선택 합니다. 그런 다음 **인증서 및 비밀** 을 선택 하 고, **새 클라이언트 암호** 를 선택 하 고, 설명을 입력 하 고, **만료** 됨 **을 선택 하** 고, **추가** 를 선택 합니다. 이 값을 적어 두어야 합니다. 값을 복사 하기 전에 페이지에서 다른 곳으로 이동 하지 마세요.

## <a name="test-drive-listings"></a>테스트 드라이브 목록

파트너 센터의 **테스트** 드라이브 탭에 있는 **테스트 드라이브 목록** 옵션에는 테스트 드라이브를 사용할 수 있는 언어 (및 시장)가 표시 되 고 현재 영어 (미국)만 사용할 수 있는 위치입니다. 또한 이 페이지에는 언어별 목록의 상태와 목록이 추가된 날짜/시간이 표시됩니다. 각 언어/시장에 대해 테스트 드라이브 세부 정보 (설명, 사용자 설명서, 비디오 등)를 정의 해야 합니다.

- **설명** (필수): 시험 사용에 대 한 설명, 표시 될 내용, 사용자가 시험해 볼 기능, 탐색할 기능 및 사용자가 제품을 확보할 지 여부를 결정 하는 데 도움이 되는 모든 관련 정보를 설명 합니다. 이 필드에는 최대 3,000자의 텍스트를 입력할 수 있습니다.

- **액세스 정보** (Azure Resource Manager 및 논리 시험 사용에 필요):이 테스트 드라이브에 액세스 하 고 사용 하기 위해 고객이 알아야 할 사항에 대해 설명 합니다. 제품 사용에 대한 시나리오와 고객이 시험 사용을 통해 기능에 액세스하기 위해 알아야 할 사항을 정확히 살펴봅니다. 이 필드에는 최대 10,000자의 텍스트를 입력할 수 있습니다.

- **사용자 설명서** (필수): 테스트 드라이브 환경의 심층 연습입니다. 사용자 설명서는 고객이 시험 사용을 통해 얻을 수 있는 사항을 정확하게 다루며, 고객에게 있을 수 있는 질문에 대한 참조로 사용됩니다. 업로드한 파일은 PDF 형식이어야 하며 최대 255자로 이름이 지정되어야 합니다.

- **비디오: 비디오 추가** (선택 사항): 다른 곳에서 호스트 되는 비디오는 링크 및 미리 보기 이미지 (533 x 324 픽셀)를 사용 하 여 여기에서 참조할 수 있으므로 고객은 제품의 기능을 성공적으로 사용 하 고 이점을 강조 하는 시나리오를 이해 하는 방법을 비롯 하 여 테스트 드라이브를 보다 잘 이해할 수 있도록 정보를 단계별로 볼 수 있습니다.
  - **이름** (필수)
  - **URL** (YouTube 또는 Vimeo only; 필수)
  - **축소판 그림** (533 x 324 픽셀) – 이미지는 PNG 형식 이어야 합니다.

현재 파트너 센터에서 테스트 드라이브를 만들고 있는 경우 계속 하기 전에 **초안 저장** 을 선택 합니다.

## <a name="next-steps"></a>다음 단계

- [시험 드라이브 모범 사례](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [개요](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)(PDF, 팝업 차단이 해제 되어 있는지 확인)
- [상업용 마켓플레이스에서 기존 제품 업데이트](partner-center-portal/update-existing-offer.md)

## <a name="additional-resources"></a>추가 리소스

## <a name="feedback"></a>피드백

[이 제품](https://feedback.azure.com/forums/216369-azure-marketplace) 에 대 한 사용자 의견 제출 및 보기
