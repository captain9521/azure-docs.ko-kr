---
title: Azure Event Grid - 구독 유효성 검사 문제 해결
description: 이 문서에서는 구독 유효성 검사 문제를 해결하는 방법을 보여줍니다.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 48844859013507ab684ef8879b7b85dd6b6fe8cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86118990"
---
# <a name="troubleshoot-azure-event-grid-subscription-validations"></a>Azure Event Grid 구독 유효성 검사 문제 해결
이 문서에서는 이벤트 구독 유효성 검사 문제 해결에 대한 정보를 제공합니다. 

> [!IMPORTANT]
> 웹후크의 엔드포인트 유효성 검사에 대한 자세한 내용은 [웹후크 이벤트 전달](webhook-event-delivery.md)을 참조하세요.

## <a name="validate-event-grid-event-subscription-using-postman"></a>Postman을 사용하여 Event Grid 이벤트 구독 유효성 검사
다음은 Event Grid 이벤트의 웹후크 구독 유효성을 검사하기 위해 Postman을 사용하는 예입니다. 

![Postman을 사용하여 이벤트 그리드 이벤트 구독 유효성 검사](./media/troubleshoot-subscription-validation/event-subscription-validation-postman.png)

다음은 샘플 **SubscriptionValidationEvent** JSON입니다.

```json
[
  {
    "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
    "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "subject": "",
    "data": {
      "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    },
    "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
    "eventTime": "2018-01-25T22:12:19.4556811Z",
    "metadataVersion": "1",
    "dataVersion": "1"
  }
]
```

성공적인 응답 예제는 다음과 같습니다.

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

웹후크의 Event Grid 이벤트 유효성 검사에 대한 자세한 내용은 [Event Grid 이벤트를 사용하여 엔드포인트 유효성 검사](webhook-event-delivery.md#endpoint-validation-with-event-grid-events)를 참조하세요.


## <a name="validate-event-grid-event-subscription-using-curl"></a>Curl을 사용하여 Event Grid 이벤트 구독 유효성 검사 
다음은 Event Grid 이벤트의 웹후크 구독 유효성을 검사하기 위한 샘플 Curl 명령입니다. 

```bash
curl -X POST -d '[{"id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66","topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx","subject": "","data": {"validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"},"eventType": "Microsoft.EventGrid.SubscriptionValidationEvent","eventTime": "2018-01-25T22:12:19.4556811Z", "metadataVersion": "1","dataVersion": "1"}]' -H 'Content-Type: application/json' https://{your-webhook-url.com}
```

## <a name="validate-cloud-event-subscription-using-postman"></a>Postman을 사용하여 클라우드 이벤트 구독 유효성 검사
다음은 클라우드 이벤트의 웹후크 구독 유효성을 검사하기 위해 Postman을 사용하는 예입니다. 

![Postman을 사용하여 클라우드 이벤트 구독 유효성 검사](./media/troubleshoot-subscription-validation/cloud-event-subscription-validation-postman.png)

클라우드 이벤트를 통한 유효성 검사에 **HTTP OPTIONS** 메서드를 사용합니다. 웹후크의 클라우드 이벤트 유효성 검사에 대한 자세한 내용은 [클라우드 이벤트를 사용하여 엔드포인트 유효성 검사](webhook-event-delivery.md#endpoint-validation-with-event-grid-events)를 참조하세요.

## <a name="error-code-403"></a>오류 코드: 403
웹후크가 응답에서 403(사용할 수 없음)을 반환하는 경우 웹후크가 Azure Application Gateway 또는 Web Application Firewall 뒤에 있는지 확인합니다. 이 경우 다음 방화벽 규칙을 사용하지 않도록 설정하고 HTTP POST를 다시 수행해야 합니다.

  - 920300(Accept 헤더가 누락된 요청, 이 문제를 해결할 수 있음)
  - 942430(제한된 SQL 문자 이상 감지(인수): 특수 문자 # 초과(12))
  - 920230(여러 URL 인코딩이 감지됨)
  - 942130(SQL 삽입 공격: SQL Tautology가 감지되었습니다.)
  - 931130(가능한 원격 파일 포함(RFI) 공격 = 오프-도메인 참조/링크)

## <a name="next-steps"></a>다음 단계
도움이 더 필요한 경우 [Stack Overflow 포럼](https://stackoverflow.com/questions/tagged/azure-eventgrid)에 문제를 게시하거나 [지원 티켓](https://azure.microsoft.com/support/options/)을 여세요. 
