---
title: Bing Autosuggest API를 사용한 검색어 제안
titleSuffix: Azure Cognitive Services
description: 이 문서에서는 Bing Autosuggest API를 사용 하 여 쿼리 용어를 제안 하는 개념과 쿼리 길이가 관련성이 높은 영향을 제안 하는 개념을 설명 합니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: cdbbd6afeedc1c8808e02aefa268be4fe0de5f9f
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2020
ms.locfileid: "94363377"
---
# <a name="suggesting-query-terms"></a>쿼리 용어 제안

> [!WARNING]
> Bing Search API Cognitive Services에서 Bing Search 서비스로 이동 합니다. **2020 년 10 월 30 일부 터** [여기](https://aka.ms/cogsvcs/bingmove)에 설명 된 프로세스에 따라 Bing Search의 새 인스턴스를 프로 비전 해야 합니다.
> Cognitive Services를 사용 하 여 프로 비전 된 Bing Search API는 향후 3 년 동안 또는 기업계약 종료 될 때까지 먼저 발생 합니다.
> 마이그레이션 지침은 [Bing Search Services](https://aka.ms/cogsvcs/bingmigration)를 참조 하십시오.

일반적으로 사용자는 애플리케이션의 검색 상자에 새 문자를 입력할 때마다 Bing Autosuggest API를 호출합니다. 쿼리 문자열의 완전성은 API가 반환하는 제안된 쿼리 용어의 관련성에 영향을 줍니다. 쿼리 문자열이 완전할수록 제안된 쿼리 용어 목록의 관련성이 커집니다. 예를 들어 API가 `s`에 대해 반환할 수 있는 제안은 `sailing dinghies`에 대해 반환하는 쿼리보다 관련성이 낮을 가능성이 큽니다.

## <a name="example-request"></a>요청 예

다음 예제에서는 *sail* 에 대해 제안된 쿼리 문자열을 반환하는 요청을 보여 줍니다. [q](/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference#query) 쿼리 매개 변수를 설정할 때 사용자의 부분 쿼리 용어를 URL 인코딩해야 합니다. 예를 들어 사용자가 *sailing les* 를 입력한 경우 `q`를 `sailing+les` 또는 `sailing%20les`로 설정합니다.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

다음 응답에는 제안된 쿼리 용어를 포함하는 [SearchAction](/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference#searchaction) 개체 목록이 포함됩니다.

```json
{
    "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
    "displayText" : "sailing lessons seattle",
    "query" : "sailing lessons seattle",
    "searchKind" : "WebSearch"
}, ...
```

## <a name="using-suggested-query-terms"></a>제안된 쿼리 용어 사용

각 제안에는 `displayText`, `query` 및 `url` 필드가 포함됩니다. `displayText` 필드에는 검색 상자의 드롭다운 목록을 채우는 데 사용하는 제안된 쿼리가 포함됩니다. 응답에 포함된 모든 제안을 지정된 순서대로 표시해야 합니다.

다음 예제에서는 Bing Autosuggest API에서 제안된 쿼리 용어와 함께 드롭다운 검색 상자를 보여 줍니다.

![드롭다운 검색 상자 목록 자동 제안](../media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

사용자가 드롭다운 목록에서 제안된 쿼리를 선택하면 개발자는 `query` 필드의 쿼리 용어를 사용하여 [Bing Web Search API](../../bing-web-search/overview.md)를 호출하고 결과를 직접 표시할 수 있습니다. 또는 `url` 필드에 URL을 사용하여 사용자를 Bing 검색 결과 페이지로 보낼 수도 있습니다.

## <a name="next-steps"></a>다음 단계

* [Bing Autosuggest API란?](../get-suggested-search-terms.md)