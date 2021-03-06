---
title: Azure Functions에 대한 연속 배포
description: Azure App Service의 연속 배포 기능을 사용 하 여 함수를 게시 합니다.
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.topic: conceptual
ms.date: 09/25/2019
ms.openlocfilehash: e49c235e11eea17fdd1a7ff7751cc0493934d725
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "83123685"
---
# <a name="continuous-deployment-for-azure-functions"></a>Azure Functions에 대한 연속 배포

Azure Functions를 사용 하 여 [소스 제어 통합](functions-deployment-technologies.md#source-control)을 통해 코드를 지속적으로 배포할 수 있습니다. 원본 제어 통합을 사용 하면 코드 업데이트에서 Azure로 배포를 트리거하는 워크플로를 사용할 수 있습니다. Azure Functions를 처음 접하는 경우 [Azure Functions 개요](functions-overview.md)를 검토 하 여 시작 하세요.

연속 배포는 여러 개의 빈번한 기여를 통합 하는 프로젝트에 적합 한 옵션입니다. 연속 배포를 사용 하는 경우 팀이 쉽게 공동 작업을 수행할 수 있도록 하는 코드의 단일 소스를 유지 관리 합니다. 다음 원본 코드 위치에서 Azure Functions의 연속 배포를 구성할 수 있습니다.

* [Azure Repos](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

Azure에서 함수에 대 한 배포 단위는 함수 앱입니다. 함수 앱의 모든 함수는 동시에 배포 됩니다. 연속 배포를 사용 하도록 설정한 후에는 Azure Portal의 함수 코드에 대 한 액세스가 다른 곳으로 설정 되어 있기 때문에 *읽기 전용* 으로 구성 됩니다.

## <a name="requirements-for-continuous-deployment"></a>연속 배포에 대 한 요구 사항

연속 배포를 성공적으로 수행 하려면 디렉터리 구조가 필요한 Azure Functions 기본 폴더 구조와 호환 되어야 합니다.

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

>[!NOTE]  
> 소비 계획에서 실행 되는 Linux 앱에 대해서는 연속 배포가 아직 지원 되지 않습니다. 

## <a name="set-up-continuous-deployment"></a><a name="credentials"></a>연속 배포 설정

기존 함수 앱에 대 한 연속 배포를 구성 하려면 다음 단계를 완료 합니다. 이 단계에서는 GitHub 리포지토리와의 통합을 보여 주지만, Azure Repos 또는 다른 소스 코드 리포지토리에 대해 비슷한 단계가 적용 됩니다.

1. [Azure Portal](https://portal.azure.com)의 함수 앱에서 **Deployment Center**를 선택 하 고 **GitHub**를 선택한 다음 **권한 부여**를 선택 합니다. 이미 GitHub를 승인한 경우 **계속** 을 선택 하 고 다음 단계를 건너뜁니다. 

    :::image type="content" source="./media/functions-continuous-deployment/github.png" alt-text="Azure App Service Deployment Center":::

3. GitHub에서 **AzureAppService 권한 부여**를 선택 합니다.

    :::image type="content" source="./media/functions-continuous-deployment/authorize.png" alt-text="Azure App Service Deployment Center":::

    GitHub 암호를 입력 한 다음 **계속**을 선택 합니다.

4. 다음 빌드 공급자 중 하나를 선택 합니다.

    * **App Service 빌드 서비스**: 빌드가 필요 하지 않거나 제네릭 빌드가 필요한 경우에 가장 적합 합니다.
    * **Azure Pipelines (미리 보기)**: 빌드를 보다 세부적으로 제어 해야 하는 경우에 가장 적합 합니다. 이 공급자는 현재 미리 보기 상태입니다.

    **계속**을 선택합니다.

5. 지정한 원본 제어 옵션과 관련 된 정보를 구성 합니다. GitHub의 경우 **조직**, **리포지토리**및 **분기**에 대 한 값을 입력 하거나 선택 해야 합니다. 값은 코드의 위치를 기반으로 합니다. 그런 다음 **계속**을 선택 합니다.

    :::image type="content" source="./media/functions-continuous-deployment/github-specifics.png" alt-text="Azure App Service Deployment Center":::

6. 모든 세부 정보를 검토 한 다음 **마침** 을 선택 하 여 배포 구성을 완료 합니다.

프로세스가 완료 되 면 지정 된 원본의 모든 코드가 앱에 배포 됩니다. 이 시점에서 배포 원본의 변경 내용은 Azure의 함수 앱에 대 한 변경 내용 배포를 트리거합니다.

> [!NOTE]
> 연속 통합을 구성한 후에는 함수 포털에서 더 이상 원본 파일을 편집할 수 없습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure Functions에 대한 모범 사례](functions-best-practices.md)
