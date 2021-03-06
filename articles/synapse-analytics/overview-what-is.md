---
title: Azure Synapse Analytics란?
description: Azure Synapse Analytics 개요
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 10/28/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: 16e13a18f93da9063a7eb08e3a2df27db9e3090f
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321694"
---
# <a name="what-is-azure-synapse-analytics-workspaces-preview"></a>Azure Synapse Analytics(작업 영역 미리 보기)란?

[!INCLUDE [preview](includes/note-preview.md)]

엔터프라이즈 분석은 원시 데이터, 정제된 데이터 또는 고도로 선별된 데이터 등 모든 종류의 데이터에 대해 대규모로 작동해야 합니다. 이를 위해서는 일반적으로 기업은 빅 데이터 및 데이터 웨어하우징 기술을 관계형 저장소 및 데이터 레이크의 데이터 전반에서 작동하는 복잡한 데이터 파이프라인에 결합해야 합니다. 이와 같은 종류의 솔루션은 빌드, 유지 관리 및 보안이 어렵습니다. 복잡성으로 인해 기업에 필요한 인사이트 제공이 지연됩니다.

**Azure Synapse** 는 데이터 웨어하우스와 빅 데이터 시스템 전체에서 인사이트를 얻는 시간을 앞당길 수 있는 통합 분석 서비스입니다. Azure Synapse는 엔터프라이즈 데이터 웨어하우징에 사용되는 최고의 **SQL** 기술, 빅 데이터에 사용되는 **Spark** 기술, 데이터 통합 및 ETL/ELT를 위한 **파이프라인** 을 결합합니다. **Synapse Studio** 는 관리, 모니터링, 코딩 및 보안을 위한 통합 환경을 제공합니다. Synapse는 **PowerBI** , **CosmosDB** , **AzureML** 과 같은 다른 Azure 서비스와 긴밀하게 통합됩니다.

## <a name="key-features--benefits"></a>주요 기능 및 이점

### <a name="industry-leading-sql"></a>업계 최고의 SQL

* **Synapse SQL** 은 기업에서 친숙한 표준 T-SQL 환경을 사용하여 데이터 웨어하우징 및 데이터 가상화 시나리오를 구현할 수 있도록 하는 분산 쿼리 시스템입니다. 또한 SQL의 기능을 확장하여 스트리밍 및 기계 학습 시나리오를 처리합니다.

* Synapse SQL은 **서버리스** 및 **전용** 리소스 모델을 모두 제공하여 필요에 맞는 소비 및 청구 옵션을 제공합니다. 예측 가능한 성능 및 비용을 위해 전용 SQL 풀을 생성하여 SQL 테이블에 저장된 데이터를 위한 처리 성능을 예약합니다. 계획하지 않은 워크로드나 버스티 워크로드에는 항상 사용 가능한 서버리스 SQL 엔드포인트를 사용합니다.
* 기본 제공 **스트리밍** 기능을 사용하여 클라우드 데이터 원본의 데이터를 SQL 테이블로 이동합니다.
* [T-SQL PREDICT 함수](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest)를 통해 데이터의 점수를 매기는 **기계 학습** 모델을 사용하여 AI와 SQL을 통합합니다.

### <a name="industry-standard-apache-spark"></a>업계 표준 Apache Spark

**Azure Synapse용 Apache Spark** 는 데이터 준비, 데이터 엔지니어링, ETL 및 기계 학습에 사용되는 가장 인기있는 오픈 소스 빅 데이터 엔진인 Apache Spark를 긴밀하고 원활하게 통합합니다.

* Linux Foundation Delta Lake가 기본적으로 지원되는 Apache Spark 2.4용 AzureML 통합 및 SparkML 알고리즘이 포함된 ML 모델
* 클러스터 관리에 대해 걱정할 필요가 없는 간소화된 리소스 모델
* 신속한 Spark 시작 및 적극적인 자동 크기 조정
* Spark 애플리케이션 내에서 기존 .NET 코드와 C# 전문 지식을 활용할 수 있도록 .NET for Spark 기본 지원

### <a name="interop-of-sql-and-apache-spark-on-your-data-lake"></a>Data Lake에서 SQL과 Apache Spark 상호 운용

Azure Synapse는 SQL과 Spark를 함께 사용하는 기존의 기술 장벽을 제거합니다. 사용자의 요구 사항과 전문 지식에 맞게 원활하게 조합하여 사용할 수 있습니다.

* 공유 Hive 호환 메타데이터 시스템을 통해 Data Lake의 파일에 정의된 테이블을 Spark 또는 Hive에서 원활하게 사용할 수 있습니다.
* SQL과 Spark는 데이터 레이크에 저장된 Parquet, CSV, TSV 및 JSON 파일을 직접 검색하고 분석할 수 있습니다.
* SQL과 Spark 데이터베이스 간 데이터 이동을 위한 빠른고 확장성 있는 로드 및 언로드

### <a name="built-in-data-integration-via-pipelines"></a>파이프라인을 통한 기본 제공 데이터 통합

Azure Synapse에는 Azure Data Factory와 동일한 데이터 통합 엔진과 환경이 기본으로 제공되기 때문에 Synapse Analytics를 종료하지 않고도 다양한 규모의 ETL 파이프라인을 만들 수 있습니다.

* 90개 이상의 데이터 원본에서 데이터 수집
* 데이터 흐름 활동을 사용하는 코드 없는 ETL
* Notebooks, Spark 작업, 저장 프로시저, SQL 스크립트 등 오케스트레이션

### <a name="unified-management-monitoring-and-security"></a>통합 관리, 모니터링 및 보안

Azure Synapse는 엔터프라이즈에서 분석 리소스를 관리하고, 사용 및 활동을 모니터링하며, 보안을 강화할 수 있는 단일 방법을 제공합니다.

* 사용자를 역할에 할당하여 분석 리소스에 대한 액세스 간소화
* 데이터와 코드에 대한 세분화된 액세스 제어
* SQL과 Spark 전체에서 리소스, 사용 및 사용자를 모니터링할 수 있는 단일 대시보드

### <a name="synapse-studio"></a>Synapse Studio

**Synapse Studio** 는 데이터 엔지니어를 위해 모든 기능을 하나로 묶은 웹 네이티브 환경이기 때문에 완벽한 솔루션을 구축하는 데 필요한 모든 작업을 한 곳에서 수행할 수 있습니다.

* 엔드투엔드 분석 솔루션을 한 곳에 구축: 수집, 검색, 준비, 오케스트레이션, 시각화
* SQL이나 Spark 코드를 작성하는 데이터 엔지니어를 위한 업계 최고의 생산성: 작성, 디버깅 및 성능 최적화
* 엔터프라이즈 CI/CD 프로세스와 통합

## <a name="next-steps"></a>다음 단계

* [Azure Synapse Analytics 시작](get-started.md)
* [작업 영역 만들기](quickstart-create-workspace.md)
* [서버리스 SQL 풀 사용](quickstart-sql-on-demand.md)
