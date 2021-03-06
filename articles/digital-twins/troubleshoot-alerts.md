---
title: 경고 설정
titleSuffix: Azure Digital Twins
description: Azure Digital Twins 메트릭에 대해 경고를 사용 하도록 설정 하는 방법을 참조 하세요.
author: baanders
ms.author: baanders
ms.date: 7/28/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: ae7e85624f5da06603ddc2675787b84203bc987b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90087214"
---
# <a name="troubleshooting-azure-digital-twins-alerts"></a>Azure Digital Twins 문제 해결: 경고

Azure Digital Twins는 리소스의 상태에 대 한 정보를 제공 하는 서비스 인스턴스에 대 한 [메트릭을](troubleshoot-metrics.md) 수집 합니다. 이러한 메트릭을 사용 하 여 Azure Digital Twins 서비스의 전반적인 상태와 연결 된 리소스를 평가할 수 있습니다.

**경고** 는 메트릭 데이터에서 중요 한 조건이 발견 될 때 사용자에 게 사전에 알려 줍니다. 시스템 사용자가 문제를 알아채기 전에 경고를 통해 문제를 식별하여 해결할 수 있습니다. 경고에 대 한 자세한 내용은 [*Microsoft Azure의 경고 개요*](../azure-monitor/platform/alerts-overview.md)를 참조 하세요.

## <a name="turn-on-alerts"></a>경고 설정

Azure Digital Twins 인스턴스에 대해 경고를 사용 하도록 설정 하는 방법은 다음과 같습니다.

1. [Azure Portal](https://portal.azure.com) 에 로그인 하 고 Azure Digital twins 인스턴스로 이동 합니다. 포털 검색 표시줄에 이름을 입력 하 여 찾을 수 있습니다. 

2. 메뉴에서 **경고** 를 선택 하 고 **+ 새 경고 규칙**을 선택 합니다.

3. 다음에 나오는 *경고 규칙 만들기* 페이지에서 프롬프트에 따라 조건, 트리거할 작업 및 경고 세부 정보를 정의할 수 있습니다.     
    * **범위** 세부 정보는 인스턴스에 대 한 세부 정보로 자동으로 채워집니다.
    * 경고 트리거 및 응답을 사용자 지정 하는 **조건** 및 **작업 그룹** 세부 정보를 정의 합니다.
    * **경고 규칙 세부 정보** 섹션에서 규칙의 이름과 설명 (선택 사항)을 입력 합니다. 경고가 생성 되는 즉시 활성화 되도록 하려면 _만들 때 경고 규칙 사용_ 확인란을 선택할 수 있습니다.
        - _리소스 그룹_ 및 _심각도_ 수준을 선택 하는 경우도 있습니다.

4. 경고 규칙 _만들기_ 단추를 선택 하 여 경고 규칙을 만듭니다.

:::image type="content" source="media/troubleshoot-alerts/create-alert-rule.png" alt-text="범위, 조건, 작업 그룹 및 경고 규칙 세부 정보에 대 한 섹션을 포함 하는 경고 규칙 만들기 페이지를 보여 주는 스크린샷" lightbox="media/troubleshoot-alerts/create-alert-rule.png":::

이러한 필드를 작성 하는 방법에 대 한 단계별 연습은 [*Microsoft Azure의 경고 개요*](../azure-monitor/platform/alerts-overview.md)를 참조 하세요. 다음은 Azure Digital Twins에 대해 단계가 표시 되는 몇 가지 예입니다.

### <a name="select-conditions"></a>조건 선택

Azure Digital Twins에 사용할 수 있는 경고 신호의 유형을 보여 주는 *Select 조건* 프로세스의 발췌 정보는 다음과 같습니다. 이 페이지에서 신호의 유형을 필터링 하 고 목록에서 원하는 신호를 선택할 수 있습니다.

:::image type="content" source="media/troubleshoot-alerts/configure-signal-logic.png" alt-text="범위, 조건, 작업 그룹 및 경고 규칙 세부 정보에 대 한 섹션을 포함 하는 경고 규칙 만들기 페이지를 보여 주는 스크린샷":::

신호를 선택한 후에는 경고의 논리를 구성 하 라는 메시지가 표시 됩니다. 차원을 필터링 하 고, 경고에 대 한 임계값을 설정 하 고, 조건에 대 한 확인 빈도를 설정할 수 있습니다. 평균 라우팅 실패율 메트릭이 5%를 초과 하는 경우에 대 한 경고를 설정 하는 예는 다음과 같습니다.

:::image type="content" source="media/troubleshoot-alerts/configure-signal-logic-2.png" alt-text="범위, 조건, 작업 그룹 및 경고 규칙 세부 정보에 대 한 섹션을 포함 하는 경고 규칙 만들기 페이지를 보여 주는 스크린샷":::

### <a name="verify-success"></a>성공 확인

경고를 설정 하면 인스턴스에 대 한 *경고* 페이지에 다시 표시 됩니다.
 
:::image type="content" source="media/troubleshoot-alerts/alerts-post.png" alt-text="범위, 조건, 작업 그룹 및 경고 규칙 세부 정보에 대 한 섹션을 포함 하는 경고 규칙 만들기 페이지를 보여 주는 스크린샷" lightbox="media/troubleshoot-alerts/alerts-post.png":::

## <a name="next-steps"></a>다음 단계

* Azure Monitor 경고에 대 한 자세한 내용은 [*Microsoft Azure의 경고 개요*](../azure-monitor/platform/alerts-overview.md)를 참조 하세요.
* Azure Digital Twins 메트릭에 대 한 자세한 내용은 [*문제 해결: Azure Monitor 사용 하 여 메트릭 보기*](troubleshoot-metrics.md)를 참조 하세요.
* 메트릭에 대해 진단 로깅을 사용 하도록 설정 하는 방법에 대 한 자세한 내용은 [*문제 해결: 진단 설정*](troubleshoot-diagnostics.md)을 참조 하세요.
