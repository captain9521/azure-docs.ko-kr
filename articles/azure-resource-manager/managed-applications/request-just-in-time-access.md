---
title: Just-In-Time 액세스 요청
description: 게시자 Azure Managed Applications 관리 되는 응용 프로그램에 just-in-time 액세스를 요청 하는 방법을 설명 합니다.
author: MSEvanhi
ms.topic: conceptual
ms.date: 09/25/2020
ms.author: evanhi
ms.openlocfilehash: 5e1a929924e2c291e0044da99f3ae5d7d1c3b894
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91371571"
---
# <a name="enable-and-request-just-in-time-access-for-azure-managed-applications"></a>Azure Managed Applications에 대 한 just-in-time 액세스를 사용 하도록 설정 및 요청

관리 되는 응용 프로그램의 소비자는 관리 되는 리소스 그룹에 영구적으로 액세스할 수 있는 권한을 꺼려할 수 있습니다. 관리자 응용 프로그램의 게시자는 관리 되는 리소스에 액세스 해야 하는 경우 소비자에 게 정확 하 게 알리는 것이 좋습니다. 소비자에 게 관리 되는 리소스에 대 한 액세스 권한을 부여 하는 것을 더 잘 제어할 수 있도록 Azure Managed Applications JIT (just-in-time) 액세스 라는 기능을 제공 합니다. 이 기능은 현재 미리 보기로 제공됩니다.

JIT 액세스를 사용하면 문제 해결 또는 유지 관리를 위해 관리형 애플리케이션의 리소스에 대한 높은 액세스 권한을 요청할 수 있습니다. 항상 리소스에 대한 읽기 전용 액세스 권한을 가지지만 특정 기간 동안 더 큰 액세스 권한을 가질 수 있습니다.

액세스 권한을 부여 하는 작업 흐름은 다음과 같습니다.

1. Marketplace에 관리 되는 응용 프로그램을 추가 하 고 JIT 액세스를 사용할 수 있도록 지정 합니다.

1. 배포 하는 동안 소비자는 관리 되는 응용 프로그램의 해당 인스턴스에 대 한 JIT 액세스를 사용 하도록 설정 합니다.

1. 배포 후 소비자는 JIT 액세스에 대 한 설정을 변경할 수 있습니다.

1. 관리 되는 리소스를 업데이트 하거나 문제를 해결 해야 하는 경우 액세스 요청을 보냅니다.

1. 소비자가 요청을 승인 합니다.

이 문서에서는 게시자가 JIT 액세스를 설정 하 고 요청을 제출 하는 데 사용 하는 작업에 중점을 둔 JIT 액세스 요청 승인에 대해 알아보려면 [Azure Managed Applications의 just-in-time 액세스 승인](approve-just-in-time-access.md)을 참조 하세요.

## <a name="add-jit-access-step-to-ui"></a>UI에 JIT 액세스 단계 추가

파일의 CreateUiDefinition.js에는 소비자가 JIT 액세스를 사용할 수 있도록 하는 단계를 포함 합니다. 제품에 대 한 JIT 기능을 지원 하려면 파일의 CreateUiDefinition.js에 다음 콘텐츠를 추가 합니다.

"단계":

```json
{
    "name": "jitConfiguration",
    "label": "JIT Configuration",
    "subLabel": {
        "preValidation": "Configure JIT settings for your application",
        "postValidation": "Done"
    },
    "bladeTitle": "JIT Configuration",
    "elements": [
        {
          "name": "jitConfigurationControl",
          "type": "Microsoft.Solutions.JitConfigurator",
          "label": "JIT Configuration"
        }
    ]
}
```

"출력":

```json
"jitAccessPolicy": "[steps('jitConfiguration').jitConfigurationControl]"
```

> [!NOTE]
> JIT 액세스는 미리 보기 상태입니다. JIT 구성의 스키마는 이후 반복에서 변경 될 수 있습니다.

## <a name="enable-jit-access"></a>JIT 액세스 사용

파트너 센터에서 제품을 만들 때 JIT 액세스를 사용 하도록 설정 했는지 확인 합니다.

1. [파트너 센터](https://partner.microsoft.com/dashboard/commercial-marketplace/overview)에서 상용 Marketplace 포털에 로그인 합니다.

1. 새 관리 되는 응용 프로그램을 만드는 방법에 대 한 지침은 [Azure 응용 프로그램 제품 만들기](../../marketplace/partner-center-portal/create-new-azure-apps-offer.md)의 단계를 따르세요.

1. **기술 구성** 페이지에서 **JIT (Just-in-time) 액세스 사용** 확인란을 선택 합니다.

   :::image type="content" source="./media/request-just-in-time-access/enable-just-in-time-access.png" alt-text="Just-In-Time VM 액세스 사용":::

UI에 JIT 구성 단계를 추가 하 고 상용 marketplace 제품에서 JIT 액세스를 사용 하도록 설정 했습니다. 소비자는 관리 되는 응용 프로그램을 배포할 때 [해당 인스턴스에 대 한 JIT 액세스를 켤](approve-just-in-time-access.md#enable-during-deployment)수 있습니다.

## <a name="request-access"></a>액세스 요청

소비자의 관리 되는 리소스에 액세스 해야 하는 경우 특정 역할, 시간 및 기간에 대 한 요청을 보냅니다. 그런 다음 소비자가 요청을 승인 해야 합니다.

JIT 액세스 요청을 보내려면 다음을 수행 합니다.

1. 액세스 해야 하는 관리 되는 응용 프로그램에 대 한 **JIT 액세스** 를 선택 합니다.

1. **적격 역할**을 선택 하 고 원하는 역할에 대 한 작업 열에서 **활성화** 를 선택 합니다.

   ![액세스 요청 활성화](./media/request-just-in-time-access/send-request.png)

1. **역할 활성화** 양식에서 역할이 활성화 될 시작 시간 및 기간을 선택 합니다. **활성화** 를 선택 하 여 요청을 보냅니다.

   ![액세스 활성화](./media/request-just-in-time-access/activate-access.png) 

1. 알림을 보고 새 JIT 요청이 소비자에 게 성공적으로 전송 되었는지 확인 합니다.

   ![알림](./media/request-just-in-time-access/in-progress.png)

   이제 소비자가 [요청을 승인할](approve-just-in-time-access.md#approve-requests)때까지 기다려야 합니다.

1. 관리 되는 응용 프로그램에 대 한 모든 JIT 요청의 상태를 보려면 **Jit 액세스** 및 **요청 기록**을 선택 합니다.

   ![상태 보기](./media/request-just-in-time-access/view-status.png)

## <a name="known-issues"></a>알려진 문제

JIT 액세스를 요청 하는 계정의 보안 주체 ID를 관리 되는 응용 프로그램 정의에 명시적으로 포함 해야 합니다. 계정은 패키지에 지정 된 그룹에만 포함 될 수 있습니다. 이 제한은 향후 릴리스에서 수정 될 예정입니다.

## <a name="next-steps"></a>다음 단계

JIT 액세스 요청을 승인 하는 방법에 대 한 자세한 내용은 [Azure Managed Applications에서 just-in-time 액세스 승인](approve-just-in-time-access.md)을 참조 하세요.
