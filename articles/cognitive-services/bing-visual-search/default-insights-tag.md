---
title: 기본 인사이트 태그 - Bing Visual Search
titleSuffix: Azure Cognitive Services
description: Bing Visual Search가 이미지에 대해 반환하는 기본 인사이트에 대한 세부 정보를 제공합니다.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: scottwhi
ms.openlocfilehash: f5df94f95ea486f069ece264b8d09de1d9fc8607
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93095019"
---
# <a name="default-insights-tag"></a>기본 인사이트 태그

> [!WARNING]
> Bing Search API Cognitive Services에서 Bing Search 서비스로 이동 합니다. **2020 년 10 월 30 일부 터** [여기](https://aka.ms/cogsvcs/bingmove)에 설명 된 프로세스에 따라 Bing Search의 새 인스턴스를 프로 비전 해야 합니다.
> Cognitive Services를 사용 하 여 프로 비전 된 Bing Search API는 향후 3 년 동안 또는 기업계약 종료 될 때까지 먼저 발생 합니다.
> 마이그레이션 지침은 [Bing Search Services](https://aka.ms/cogsvcs/bingmigration)를 참조 하십시오.

기본 인사이트 태그는 `displayName` 필드가 빈 문자열로 설정된 태그입니다. 다음 예제는 가능한 기본 인사이트(작업) 목록을 보여 줍니다. 응답에 포함되는 작업 목록은 이미지에 따라 달라집니다. 또한 각 작업에 대한 속성 목록도 이미지에 따라 달라지므로 사용하기 전에 해당 속성이 있는지 확인합니다.

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {
          "_type" : "ImageModuleAction",
          "actionType" : "PagesIncluding",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageShoppingSourcesAction",
          "actionType" : "ShoppingSources",
          "data" : {
            "offers" : [...]
          }
        },
        {
          "image" : {...},
          "actionType" : "MoreSizes"
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "VisualSearch",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRecipesAction",
          "actionType" : "Recipes",
          "data" : {
            "value" : [...]
          }
        },
        {
          "image" : {...},
          "actionType" : "ImageById"
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "ProductVisualSearch",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRelatedSearchesAction",
          "actionType" : "RelatedSearches",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRelatedSearchesAction",
          "actionType" : "DocumentLevelSuggestions",
          "data" : {
            "value" : [...]
          }
        }
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

## <a name="pagesincluding-insight"></a>PagesIncluding 인사이트

PagesIncluding 인사이트는 이 이미지를 포함하는 웹 페이지 목록을 제공합니다. 실제로 `Image` 개체 목록이 며 필드에는 `hostPageUrl` 이미지를 포함 하는 웹 페이지에 대 한 URL이 포함 되어 있습니다. 예제 사용에 대 한 자세한 [내용은 참고 항목을 참조 하세요.](./bing-insights-usage.md#pagesincluding-insight-example)

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "PagesIncluding",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https://www.bing.com\/images\/search?",
              "name" : "Today's smoking hot country",
              "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=OIP...",
              "datePublished" : "2017-09-20T12:00:00.0000000Z",
              "contentUrl" : "http:\/\/contoso.com\/wordstuff",
              "hostPageUrl" : "http:\/\/contoso.com\/2017\/09\/20\/car",
              "contentSize" : "122540 B",
              "encodingFormat" : "jpeg",
              "hostPageDisplayUrl" : "contoso.com\/2017\/09\/20\/car",
              "width" : 894,
              "height" : 1200,
              "thumbnail" : {
                "width" : 474,
                "height" : 636
              },
              "imageInsightsToken" : "ccid_CO5GEthj*mid_5323B1",
              "insightsMetadata" : {
                "pagesIncludingCount" : 12,
                "availableSizesCount" : 7
              },
              "imageId" : "5323B1900FB9087B6B45D176D234E1F2F23CD3A5",
              "accentColor" : "55585B"
            }
          ]
        }
      }
```

## <a name="shoppingsources-insight"></a>ShoppingSources 인사이트

ShoppingSources 인사이트는 사용자가 이미지에 표시된 항목을 구입할 수 있는 웹 사이트 목록을 제공합니다. 제품 목록에는 사용자가 항목을 구매할 수 있는 웹 페이지의 URL, 항목의 가격, 등급 또는 검토 정보가 포함 됩니다. 사용 예를 보려면 [ShoppingSources 예제](./bing-insights-usage.md#shoppingsources-insight-example)를 참조하세요.

```json
      {
        "_type" : "ImageShoppingSourcesAction",
        "actionType" : "ShoppingSources",
        "data" : {
          "offers" : [
            {
              "name" : "Apple Pie",
              "url" : "https:\/\/contoso.com\/product_p\/l10.htm",
              "description" : "A taste of the crust, apple, and pie filling...",
              "seller" : {
                "name" : "Contoso"
              },
              "price" : 3.99,
              "priceCurrency" : "USD",
              "aggregateRating" : {
                "ratingValue" : 5
              },
              "lastUpdated" : "2018-04-16T00:00:00.0000000"
            }
          ]
        }
      }
```

## <a name="moresizes-insight"></a>MoreSizes 인사이트

MoreSizes 정보는 Bing에서 인터넷에 있는 이미지의 크기 (크거나 작음) 수를 식별 합니다 ( `availableSizesCount` 필드 참조).

```json
      {
        "image" : {
          "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=detai...",
          "name" : "Making Apple Pie",
          "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP....",
          "datePublished" : "2013-06-21T12:00:00.0000000Z",
          "contentUrl" : "http:\/\/contoso.com\/content\/uploads\/2013\/06\/apple-pie.jpg",
          "hostPageUrl" : "http:\/\/contoso.com\/2013\/06\/21\/making-apple-pie\/",
          "contentSize" : "134847 B",
          "encodingFormat" : "jpeg",
          "hostPageDisplayUrl" : "contoso.com\/2013\/06\/21\/making-apple-pie",
          "width" : 1050,
          "height" : 765,
          "thumbnail" : {
            "width" : 474,
            "height" : 345
          },
          "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CF...",
          "insightsMetadata" : {
            "recipeSourcesCount" : 6,
            "pagesIncludingCount" : 103,
            "availableSizesCount" : 28
          },
          "imageId" : "D12339146CFEDF3D409CC7A66D2C98D0D71904D4",
          "accentColor" : "3A0B01"
        },
        "actionType" : "MoreSizes"
      },
```

## <a name="visualsearch-insight"></a>VisualSearch 인사이트

VisualSearch 인사이트는 시각적으로 원본 이미지와 유사한(원본 이미지에 표시된 콘텐츠와 유사한 콘텐츠 포함) 이미지 목록을 제공합니다. 예제 사용에 대 한 자세한 내용은 [Visualsearch 통찰력 예](./bing-insights-usage.md#visualsearch-insight-example)를 참조 하세요.

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "VisualSearch",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=...",
              "name" : "An apple pie...",
              "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.z...",
              "datePublished" : "2017-03-18T00:17:00.0000000Z",
              "contentUrl" : "http:\/\/contoso.net\/images\/8\/8a\/an_apple_pie.png",
              "hostPageUrl" : "http:\/\/contoso.com\/wiki\/an_apple_pie.png",
              "contentSize" : "87930 B",
              "encodingFormat" : "png",
              "hostPageDisplayUrl" : "contoso.com\/wiki\/an_apple_pie.png",
              "width" : 263,
              "height" : 192,
              "thumbnail" : {
                "width" : 474,
                "height" : 346
              },
              "imageInsightsToken" : "ccid_zhRxfGkI*mid_1DCBA7AA6D231...",
              "insightsMetadata" : {
                "recipeSourcesCount" : 6,
                "pagesIncludingCount" : 103,
                "availableSizesCount" : 28
              },
              "imageId" : "1DCBA7AA6D23147F9DD06D47DB3A38EB25389",
              "accentColor" : "3E0D01"
            }
          ]
        }
      }
```

## <a name="recipes-insight"></a>Recipes 인사이트

Recipes 인사이트는 이미지에 표시된 음식을 만드는 조리법이 포함된 웹 페이지 목록을 제공합니다. 사용 예를 보려면 [조리법 정보 예](./bing-insights-usage.md#recipes-insight-example)를 참조 하세요.

```json
      {
        "_type" : "ImageRecipesAction",
        "actionType" : "Recipes",
        "data" : {
          "value" : [
            {
              "name" : "Granny's Apple Pie",
              "url" : "http:\/\/contoso.com\/recipes\/appetizer\/apple-pie.html",
              "description" : "I love Granny's apple pie. Sooooo delicious...",
              "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=A63002cd9",
              "creator" : {
                "_type" : "Person",
                "name" : "Charlene Whitney"
              },
              "aggregateRating" : {
                "text" : "5",
                "ratingValue" : 5,
                "bestRating" : 5,
                "reviewCount" : 1
              },
              "cookTime" : "PT45M",
              "prepTime" : "PT1H",
              "totalTime" : "PT1H45M"
            }
          ]
        }
      }
```


## <a name="imagebyid-insight"></a>ImageById 인사이트

ImageById 인 사이트는 `Image` 다음에 대 한 정보를 요청 하는 이미지의 개체를 제공 합니다.

```json
      {
        "image" : {
          "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=deta...",
          "name" : "Making Apple Pie",
          "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP...",
          "datePublished" : "2013-06-21T12:00:00.0000000Z",
          "contentUrl" : "http:\/\/contoso.com\/content\/uploads\/2013\/06\/apple-pie.jpg",
          "hostPageUrl" : "http:\/\/contoso.com\/2013\/06\/21\/making-apple-pie\/",
          "contentSize" : "134847 B",
          "encodingFormat" : "jpeg",
          "hostPageDisplayUrl" : "contoso.com\/2013\/06\/21\/making-apple-pie",
          "width" : 1050,
          "height" : 765,
          "thumbnail" : {
            "width" : 474,
            "height" : 345
          },
          "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CFE...",
          "insightsMetadata" : {
            "recipeSourcesCount" : 6,
            "pagesIncludingCount" : 103,
            "availableSizesCount" : 28
          },
          "imageId" : "D12339146CFEDF3D409A66D2C98D0D71904D4",
          "accentColor" : "3A0B01"
        },
        "actionType" : "ImageById"
      },
```

## <a name="productvisualsearch-insight"></a>ProductVisualSearch 인사이트

ProductVisualSearch 인사이트는 시각적으로 원본 이미지에 표시된 제품과 유사한 제품의 이미지 목록을 제공합니다. `insightsMetadata` 필드에는 제품을 구입할 수 있는 제안 및 제품 가격에 대한 정보가 포함될 수 있습니다.

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "ProductVisualSearch",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=detail...",
              "name" : "Contoso 4-Piece Kitchen Package...",
              "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.l9hzaabu-RJd...",
              "datePublished" : "2017-07-16T04:28:00.0000000Z",
              "contentUrl" : "https:\/\/www.contoso.com\/assets\/media\/images\/prod...",
              "hostPageUrl" : "https:\/\/www.contoso.com\/4-piece-kitchen-package...",
              "contentSize" : "13594 B",
              "encodingFormat" : "jpeg",
              "hostPageDisplayUrl" : "https:\/\/www.contoso.com\/4-piece-kitchen-package...",
              "width" : 450,
              "height" : 332,
              "thumbnail" : {
                "width" : 474,
                "height" : 349
              },
              "imageInsightsToken" : "ccid_l9hzaabu*mid_70A8B616355D681DB9A5A...",
              "insightsMetadata" : {
                "shoppingSourcesCount" : 1,
                "recipeSourcesCount" : 0,
                "aggregateOffer" : {
                  "name":"4-Piece Kitchen Package with...",
                  "priceCurrency":"USD",
                  "lowPrice":2756,
                  "offers" : [
                    {
                      "name" : "4-Piece Kitchen Package with...",
                      "url" : "https:\/\/www.fabrikam.com\/1234.html?ref=bing",
                      "description" : "This 36 Frenchdoor refrigerator by...",
                      "seller" : {
                        "name" : "Fabrikam",
                        "image" : {
                          "url" : "https:\/\/tse1.mm.bing.net\/th?id=A818f811..."
                        }
                      },
                      "price" : 2756,
                      "priceCurrency" : "USD",
                      "availability" : "InStock",
                      "lastUpdated" : "2018-02-20T00:00:00.0000000"
                    }
                  ],
                  "offerCount":1
                },
                "pagesIncludingCount" : 4,
                "availableSizesCount" : 2
              },
              "imageId" : "70A8B616355D681DA5980A8D0514BCC995A3",
              "accentColor" : "60646B"
            }
          ]
        }
      }
```

## <a name="relatedsearches-insight"></a>RelatedSearches 인사이트

RelatedSearches 인사이트는 다른 사용자의 검색어를 기준으로 다른 사용자가 수행한 관련 검색 목록을 제공합니다. 사용 예는 [RelatedSearches 통찰력 예](./bing-insights-usage.md#relatedsearches-insight-example)를 참조 하세요.

```json
      {
        "_type" : "ImageRelatedSearchesAction",
        "actionType" : "RelatedSearches",
        "data" : {
          "value" : [
            {
              "text" : "Homemade Apple Pies Recipes",
              "displayText" : "Homemade Apple Pies Recipes",
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Homemade...",
              "thumbnail" : {
                "url" : "https:\/\/tse1.mm.bing.net\/th?q=Homemade+Apple+Pies"
              }
            }
          ]
        }
      }
```

## <a name="documentlevelsuggestions-insight"></a>DocumentLevelSuggestions 인사이트

DocumentLevelSuggestions 정보는 이미지의 내용에 따라 제안 된 검색 용어 목록을 제공 합니다.

```json
      {
        "_type" : "ImageRelatedSearchesAction",
        "actionType" : "DocumentLevelSuggestions",
        "data" : {
          "value" : [
            {
              "text" : "American Apple Pie",
              "displayText" : "American Apple Pie",
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=American",
              "thumbnail" : {
                "url" : "https:\/\/tse3.mm.bing.net\/th?q=American+Apple+Pie."
              }
            }
          ]
        }
      }
```

## <a name="next-steps"></a>다음 단계

Bing [insights 사용의 예](bing-insights-usage.md) 를 확인 하 여 bing에서 시각적 정보를 표시 하는 방법을 확인 합니다.

첫 번째 요청으로 신속하게 시작하려면 빠른 시작:

* [C#](quickstarts/csharp.md)

* [Java](quickstarts/java.md)

* [node.js](quickstarts/nodejs.md)

* [Python](quickstarts/python.md).
