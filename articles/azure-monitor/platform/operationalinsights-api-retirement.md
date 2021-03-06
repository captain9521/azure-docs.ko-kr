---
title: Azure Monitor API 사용 중지
description: 이전 버전의 OperationalInsights 리소스 공급자 API를 사용 중지 하는 방법을 설명 합니다.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/29/2020
ms.openlocfilehash: 7ddfb3221c73c740a339dd8d9d4f60cca46c6425
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2020
ms.locfileid: "93026771"
---
# <a name="operationalinsights-api-version-retirement"></a>OperationalInsights API 버전 사용 중지
Microsoft는 최신/지원 되는 버전으로 원활 하 게 전환할 수 있도록 API를 사용 중지 하기 전에 최소 12 개월 전에 알림을 제공 합니다. **OperationalInsights** 리소스 공급자 api에 대 한 새로운 버전 (2020-08-01)을 출시 했으며 2024 년 2 월 29 일에 이전 API 버전을 사용 중지 합니다.

이제 버전 2020-08-01을 사용 하 여 [전용 클러스터](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters), [고객 관리 키](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys), [개인 링크](https://docs.microsoft.com/azure/azure-monitor/platform/private-link-security) 및 [데이터 내보내기와](https://docs.microsoft.com/azure/azure-monitor/platform/logs-data-export)같은 새로운 기능의 이점을 얻을 수 있습니다. 또한 새로운 기능 및 최적화는 현재 API에만 추가 됩니다.

2 월 29 일 이후 2024 Azure Monitor는 더 이상 2020-08-01 보다 이전 Api 버전을 지원 하지 않습니다. 업그레이드 하지 않으려는 경우 이전 버전에서 보낸 요청은 2024 년 2 월 29 일까 지 Azure Monitor 서비스에서 계속 제공 됩니다.

## <a name="migration-steps"></a>마이그레이션 단계
사용 하는 구성 방법에 따라 **REST** 요청 및 **템플릿 리소스 관리자** 새 버전을 업데이트 해야 합니다. API 버전을 업데이트 하려면 아래 예제를 따르세요.

1. REST API 요청은 요청 URL의 API 버전을 사용 합니다. 다음 예제와 같이 해당 버전을 최신 버전 (2020-08-01)으로 바꿉니다.

    ```rest
    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}?api-version=2020-08-01
    ```

2. Azure Resource Manager 템플릿에서는 리소스의 **apiVersion** 속성에 있는 API 버전을 사용 합니다. 다음 예제와 같이 해당 버전을 최신 버전 (2020-08-01)으로 바꿉니다.


    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2019-08-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string",
                "metadata": {
                "description": "Name of the workspace."
                }
            },
            "resources": [
            {
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('workspaceName')]",
                "apiVersion": "2020-08-01",
                "location": "westus",
                "properties": {
                    "sku": {
                        "name": "pergb2018"
                    },
                    "retentionInDays": 30,
                    "features": {
                        "searchVersion": 1,
                        "legacy": 0,
                        "enableLogAccessUsingOnlyResourcePermissions": true
                    }
                }
            }
        ]
    }
    }
    ```


## <a name="next-steps"></a>다음 단계

- [OperationalInsights 작업 영역 API에 대 한 참조](/rest/api/loganalytics/workspaces)를 참조 하세요.
