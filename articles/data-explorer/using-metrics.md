---
title: 메트릭을 사용 하 여 Azure 데이터 탐색기 성능, 상태 및 사용 현황 모니터링
description: Azure 데이터 탐색기 메트릭을 사용 하 여 클러스터의 성능, 상태 및 사용량을 모니터링 하는 방법에 대해 알아봅니다.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/01/2019
ms.openlocfilehash: f5b47a5ae9d13711233d0e4852ec487af7344622
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74173784"
---
# <a name="monitor-azure-data-explorer-performance-health-and-usage-with-metrics"></a>메트릭을 사용 하 여 Azure 데이터 탐색기 성능, 상태 및 사용 현황 모니터링

Azure Data Explorer는 애플리케이션, 웹 사이트, IoT 디바이스 등으로부터 대량의 데이터 스트리밍에 대한 실시간 분석을 제공하는 속도가 빠른 완전 관리형 데이터 분석 서비스입니다. Azure Data Explorer를 사용하려면 먼저 클러스터를 만들고 이 클러스터에 데이터베이스를 하나 이상 만듭니다. 그런 다음, 데이터베이스에 대해 쿼리를 실행할 수 있도록 데이터베이스에 데이터를 수집(로드)합니다. Azure 데이터 탐색기 메트릭은 클러스터 리소스의 상태 및 성능에 대 한 주요 표시기를 제공 합니다. 이 문서에서 자세히 설명 하는 메트릭을 사용 하 여 특정 시나리오에서 Azure 데이터 탐색기 클러스터 상태 및 성능을 독립형 메트릭으로 모니터링할 수 있습니다. 또한 작업 [Azure 대시보드](/azure/azure-portal/azure-portal-dashboards) 및 [azure 경고](/azure/azure-monitor/platform/alerts-metric-overview)에 대 한 기준으로 메트릭을 사용할 수 있습니다.

## <a name="prerequisites"></a>선행 조건

* Azure 구독이 없는 경우 [무료 azure 계정](https://azure.microsoft.com/free/)을 만듭니다.

* [클러스터 및 데이터베이스](create-cluster-database-portal.md)를 만듭니다.

## <a name="sign-in-to-the-azure-portal"></a>Azure 포털에 로그인합니다.

[Azure Portal](https://portal.azure.com/)에 로그인합니다.

## <a name="using-metrics"></a>메트릭 사용

Azure 데이터 탐색기 클러스터에서 **메트릭** 을 선택 하 여 메트릭 창을 열고 클러스터에 대 한 분석을 시작 합니다.

![메트릭 선택](media/using-metrics/select-metrics.png)

메트릭 창에서 다음을 수행 합니다.

![메트릭 창](media/using-metrics/metrics-pane.png)

1. 메트릭 차트를 만들려면 아래에 설명 된 대로 **메트릭 이름 및 메트릭 별** 관련 **집계** 를 선택 합니다. **리소스** 및 **메트릭 네임 스페이스** 선택기는 Azure 데이터 탐색기 클러스터에 미리 선택 됩니다.

    **메트릭** | **Unit** | **집계** | **메트릭 설명**
    |---|---|---|---|
    | 캐시 사용률 | Percent | 평균, 최대값, 최소값 | 현재 클러스터에서 사용 중인 할당 된 캐시 리소스의 비율입니다. 캐시는 정의 된 캐시 정책에 따라 사용자 활동에 할당 된 SSD 크기를 나타냅니다. 80% 이하의 평균 캐시 사용률은 클러스터에서 지속 가능한 상태입니다. 평균 캐시 사용률이 80%를 초과 하는 경우 클러스터는 저장소에 최적화 된 가격 책정 계층으로 [확장](manage-cluster-vertical-scaling.md) 되거나 더 많은 인스턴스로 [확장](manage-cluster-horizontal-scaling.md) 되어야 합니다. 또는 캐시 정책을 조정 합니다 (캐시의 일 수 미만). 캐시 사용률이 100%를 초과 하는 경우 캐싱 정책에 따라 캐시 되는 데이터의 크기가 클러스터의 총 캐시 크기 보다 큽니다. |
    | CPU | Percent | 평균, 최대값, 최소값 | 클러스터의 컴퓨터에서 현재 사용 중인 할당 된 계산 리소스의 비율입니다. 클러스터에 대해 80% 이하의 평균 CPU가 유지 될 수 있습니다. CPU의 최대값은 100% 이며,이는 데이터를 처리 하는 추가 계산 리소스가 없음을 의미 합니다. 클러스터가 제대로 수행 되지 않으면 CPU의 최대값을 확인 하 여 차단 된 특정 Cpu가 있는지 확인 합니다. |
    | 처리 된 이벤트 (Event Hubs) | 개수 | 최대, 최소, 합계 | Event hubs에서 읽어서 클러스터에서 처리 한 총 이벤트 수입니다. 이벤트는 거부 된 이벤트 및 클러스터 엔진에서 받은 이벤트로 분할 됩니다. |
    | 수집 대기 시간 | 초 | 평균, 최대값, 최소값 | 데이터가 클러스터에서 수신 된 시간부터 쿼리가 준비 될 때까지 데이터 수집의 대기 시간입니다. 수집 대기 시간 기간은 수집 시나리오에 따라 달라 집니다. |
    | 수집 결과 | 개수 | 개수 | 실패 하 고 성공한 수집 작업의 총 수입니다. **분할 적용** 을 사용 하 여 성공 및 실패 결과 버킷을 만들고 크기 (**값** > **상태**)를 분석 합니다.|
    | 수집 사용률 | Percent | 평균, 최대값, 최소값 | 수집을 수행 하기 위해 용량 정책에서 할당 된 총 리소스의 데이터를 수집 하는 데 사용 되는 실제 리소스의 비율입니다. 기본 용량 정책은 수집에 투자 된 클러스터 리소스의 최대 512 동시 수집 작업 또는 75%를 초과 하지 않습니다. 80% 이하의 평균 수집 사용률은 클러스터에 대해 지속 가능한 상태입니다. 수집 사용률의 최대값은 100% 이며,이는 모든 클러스터 수집 기능이 사용 되 고 수집 큐가 발생할 수 있음을 의미 합니다. |
    | 수집 볼륨 (MB) | 개수 | 최대, 최소, 합계 | 압축 전 클러스터에 수집 된 데이터의 총 크기 (MB)입니다. |
    | 연결 유지 | 개수 | Avg | 클러스터의 응답성을 추적 합니다. 완전히 응답성이 높은 클러스터는 값 1을 반환 하 고 차단 되거나 연결이 끊어진 클러스터는 0을 반환 합니다. |
    | 쿼리 기간 | 초 | 개수, 평균, 최소값, 최대값, 합계 | 쿼리 결과가 수신 될 때까지 걸리는 총 시간입니다 (네트워크 대기 시간을 포함 하지 않음). |
    | | | |

    [지원 되는 Azure 데이터 탐색기 클러스터 메트릭에](/azure/azure-monitor/platform/metrics-supported#microsoftkustoclusters) 대 한 추가 정보

2. **메트릭 추가** 단추를 선택 하 여 동일한 차트에 표시 되는 여러 메트릭을 표시 합니다.
3. **+ 새 차트** 단추를 선택 하 여 한 뷰에서 여러 차트를 볼 수 있습니다.
4. 시간 선택을 사용 하 여 시간 범위를 변경 합니다 (기본값: 지난 24 시간).
5. [ **추가 필터** 를 사용 하 고](/azure/azure-monitor/platform/metrics-getting-started#apply-dimension-filters-and-splitting) 차원이 있는 메트릭에 분할을 적용 합니다.
6. **대시보드에 고정** 을 선택 하 여 차트 구성을 다시 볼 수 있도록 대시보드에 추가 합니다.
7. 설정 된 조건을 사용 하 여 메트릭을 시각화 하도록 **새 경고 규칙** 을 설정 합니다. 새 경고 규칙에는 사용자 차트의 대상 리소스, 메트릭, 분할 및 필터 차원이 포함됩니다. [경고 규칙 만들기 창](/azure/azure-monitor/platform/metrics-charts#create-alert-rules)에서 이러한 설정을 수정 합니다.

[메트릭 탐색기](/azure/azure-monitor/platform/metrics-getting-started)사용에 대 한 추가 정보입니다.


## <a name="next-steps"></a>다음 단계

* [자습서: Azure 데이터 탐색기에서 모니터링 데이터 수집 및 쿼리](/azure/data-explorer/ingest-data-no-code)
* [진단 로그를 사용 하 여 Azure 데이터 탐색기 수집 작업 모니터링](/azure/data-explorer/using-diagnostic-logs)
* [빠른 시작: Azure 데이터 탐색기에서 데이터 쿼리](web-query-data.md)
