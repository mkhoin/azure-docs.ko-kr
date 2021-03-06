---
title: 서비스 공급자 작업 보기
description: 고객은 Azure 위임 된 리소스 관리를 통해 서비스 공급자가 수행한 작업을 확인 하기 위해 기록 된 활동을 볼 수 있습니다.
ms.date: 12/6/2019
ms.topic: conceptual
ms.openlocfilehash: 69517e942aa9f82be16fa3d0e7d6f9252de44d4c
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932685"
---
# <a name="view-service-provider-activity"></a>서비스 공급자 작업 보기

Azure 위임 된 리소스 관리에 대 한 구독을 위임 받은 고객은 [Azure 활동 로그](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview) 데이터를 확인 하 여 수행 된 모든 작업을 볼 수 있습니다. 이렇게 하면 고객의 azure AD (Azure Active Directory) 테 넌 트 내에서 사용자가 수행한 작업을 비롯 하 여 서비스 공급자가 Azure 위임 된 리소스 관리를 통해 수행 하는 작업을 완벽 하 게 파악할 수 있습니다.

## <a name="view-activity-log-data"></a>활동 로그 데이터 보기

Azure Portal의 **모니터** 메뉴에서 [활동 로그를 볼](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview#view-the-activity-log) 수 있습니다. 특정 구독에 대 한 결과를 제한 하려면 필터를 사용 하 여 특정 구독을 선택할 수 있습니다. [활동 로그 이벤트](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view) 를 프로그래밍 방식으로 보고 검색할 수도 있습니다.

> [!NOTE]
> 서비스 공급자의 테 넌 트에 있는 사용자는 Azure 위임 된 리소스 관리에 대 한 해당 구독이 등록 된 경우 고객 테 넌 트에서 위임 된 구독에 대 한 활동 로그 결과를 볼 수 있습니다 (또는 [읽기 권한자 액세스](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) 권한을 포함 하는 다른 기본 제공 역할).

활동 로그에 작업 이름과 해당 상태가 수행 된 날짜 및 시간과 함께 표시 됩니다. **시작 된 이벤트** 열은 작업을 수행한 사용자 (Azure 위임 된 리소스 관리를 통해 작동 하는 서비스 공급자의 테 넌 트에 있는 사용자 또는 고객의 테 넌 트에 있는 사용자)를 표시 합니다. 사용자가 해당 구독에 할당 된 테 넌 트 또는 역할이 아닌 사용자 이름이 표시 됩니다.

로깅된 활동은 지난 90 일간 Azure Portal에서 사용할 수 있습니다. 90 일 넘게이 데이터를 저장 하는 방법을 알아보려면 [의 Log Analytics 작업 영역에서 Azure 활동 로그 수집 및 분석](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-collect) 을 참조 하세요 Azure Monitor

## <a name="set-alerts-for-critical-operations"></a>중요 한 작업에 대 한 경고 설정

서비스 공급자나 자체 테 넌 트의 사용자가 수행 하는 중요 한 작업을 계속 인식 하려면 [활동 로그 경고](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-alerts)를 만드는 것이 좋습니다. 예를 들어, 구독에 대 한 모든 관리 작업을 추적 하거나 특정 리소스 그룹의 가상 머신을 삭제 하는 경우 알림이 표시 되도록 할 수 있습니다. 경고를 만들 때 사용자는 고객의 테 넌 트에 있는 사용자가 수행 하는 작업과 관리 테 넌 트를 포함 합니다.

자세한 내용은 [활동 로그 경고 만들기 및 관리](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)를 참조 하세요.

## <a name="create-log-queries"></a>로그 쿼리 만들기

쿼리를 만들어 로깅 작업을 분석 하거나 특정 항목에 초점을 맞출 수 있습니다. 예를 들어 감사를 사용 하려면 구독에 대해 수행 된 모든 관리 수준 작업에 대해 보고 해야 합니다. 이러한 작업만 필터링 하 고 사용자, 날짜 또는 다른 값을 기준으로 결과를 정렬 하는 쿼리를 만들 수 있습니다.

자세한 내용은 [Azure Monitor의 로그 쿼리 개요](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

- [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/)에 대해 자세히 알아봅니다.
- Azure Portal에서 [서비스 공급자 제품을 보고 관리](view-manage-service-providers.md) 하는 방법에 대해 알아봅니다.