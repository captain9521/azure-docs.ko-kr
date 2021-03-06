---
title: Azure Database for MySQL-유연한 서버 (미리 보기)-예약 된 유지 관리-Azure Portal
description: Azure Portal에서 Azure Database for MySQL 유연한 서버에 대 한 예약 된 유지 관리 설정을 구성 하는 방법에 대해 알아봅니다.
author: niklarin
ms.author: nlarin
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: c8251eb2a89a7481ebc981f2b89668c363651b39
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91315017"
---
# <a name="manage-scheduled-maintenance-settings-for-azure-database-for-mysql--flexible-server"></a>Azure Database for MySQL – 유연한 서버에 대 한 예약 된 유지 관리 설정 관리
 
Azure 구독에서 각 유연한 서버에 대 한 유지 관리 옵션을 지정할 수 있습니다. 옵션에는 예정 된 유지 관리 이벤트에 대 한 유지 관리 일정 및 알림 설정이 포함 됩니다.

> [!IMPORTANT]
> Azure Database for MySQL-유연한 서버는 미리 보기 상태입니다.
 
## <a name="prerequisites"></a>필수 구성 요소
이 방법 가이드를 완료하려면 다음이 필요합니다.
- [Azure Database for MySQL 유연한 서버](quickstart-create-server-portal.md)
 
## <a name="specify-maintenance-schedule-options"></a>유지 관리 일정 옵션 지정
 
1. MySQL server 페이지의 **설정** 제목에서 **유지** 관리를 선택 하 여 예약 된 유지 관리 옵션을 엽니다.
2. 기본 (시스템 관리) 일정은 임의의 요일 이며 유지 관리를 위한 60 분 창은 오전 11 시에서 오전 7 시 사이에 로컬 서버 시간으로 시작 합니다. 이 일정을 사용자 **지정 하려면 사용자 지정 일정**을 선택 합니다. 그런 다음 기본 요일 및 유지 관리 시작 시간에 대 한 60 분 창을 선택할 수 있습니다.
 
## <a name="notifications-about-scheduled-maintenance-events"></a>예약 된 유지 관리 이벤트에 대 한 알림
 
Azure Service Health를 사용 하 여 예정 된 서버에서 예정 된 유지 관리 및 예정 된 유지 관리 작업에 대 한 [알림을 볼](../../service-health/service-notifications.md) 수 있습니다. Azure Service Health에서 유지 관리 이벤트에 대 한 알림을 받도록 경고를 [설정할](../../service-health/resource-health-alert-monitor-guide.md) 수도 있습니다.
 
## <a name="next-steps"></a>다음 단계  
 
* [Azure Database for MySQL-유연한 서버에서 예약 된 유지 관리](concepts-maintenance.md) 에 대해 알아봅니다.
* [Azure Service Health](../../service-health/overview.md) 에 대 한 간결한 정보
