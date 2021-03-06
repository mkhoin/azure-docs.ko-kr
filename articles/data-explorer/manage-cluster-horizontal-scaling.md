---
title: 변경 수요에 맞게 Azure 데이터 탐색기의 클러스터 수평적 크기 조정 (규모 확장) 관리
description: 이 문서에서는 수요 변화에 따라 Azure 데이터 탐색기 클러스터에서 규모를 확장 하 고 확장 하는 단계를 설명 합니다.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/09/2019
ms.openlocfilehash: 52a9c0a13723361bbc93362cdd9e2c73ef0372f2
ms.sourcegitcommit: b5ff5abd7a82eaf3a1df883c4247e11cdfe38c19
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74942242"
---
# <a name="manage-cluster-horizontal-scaling-scale-out-in-azure-data-explorer-to-accommodate-changing-demand"></a>변경 수요에 맞게 Azure 데이터 탐색기의 클러스터 수평적 크기 조정 (규모 확장) 관리

클러스터 크기를 적절하게 조정하는 것은 Azure 데이터 탐색기의 성능에 중요합니다. 정적 클러스터 크기는 사용량 미달 또는 사용량 초과로 이어질 수 있으며, 둘 다 이상적인 경우는 아닙니다. 클러스터의 수요는 절대 정확도로 예측할 수 없기 때문에 클러스터 *크기를 조정* 하 고 수요를 변경 하 여 용량 및 CPU 리소스를 추가 및 제거 하는 것이 좋습니다. 

Azure 데이터 탐색기 클러스터의 크기를 조정 하는 데는 두 가지 워크플로가 있습니다. 
* 수평 확장 (확장 및 축소 라고도 함)
* [수직 크기](manage-cluster-vertical-scaling.md)조정 (확장 및 축소 라고도 함)
이 문서에서는 가로 크기 조정 워크플로에 대해 설명 합니다.

## <a name="configure-horizontal-scaling"></a>가로 크기 조정 구성

수평 확장을 사용 하면 미리 정의 된 규칙 및 일정에 따라 인스턴스 수를 자동으로 조정할 수 있습니다. 클러스터에 대 한 자동 크기 조정 설정을 지정 하려면:

1. Azure Portal에서 Azure 데이터 탐색기 클러스터 리소스로 이동 합니다. **설정**아래에서 **Scale out**을 선택 합니다. 

2. **규모 확장** 창에서 **수동 크기**조정, **최적화 된 자동 크기 조정**또는 **사용자 지정 자동 크기**조정 등 원하는 자동 크기 조정 메서드를 선택 합니다.

### <a name="manual-scale"></a>수동 크기 조정

수동 크기 조정은 클러스터를 만드는 동안 기본 설정입니다. 클러스터에는 자동으로 변경 되지 않는 고정 용량이 있습니다. **인스턴스 개수** 막대를 사용 하 여 정적 용량을 선택 합니다. 클러스터의 크기 조정은 다른 변경을 수행할 때까지 해당 설정으로 유지 됩니다.

   ![수동 크기 조정 방법](media/manage-cluster-horizontal-scaling/manual-scale-method.png)

### <a name="optimized-autoscale-preview"></a>최적화 된 자동 크기 조정 (미리 보기)

최적화 된 자동 크기 조정 방법은 권장 되는 자동 크기 조정 방법입니다. 이 방법은 클러스터 성능과 비용을 최적화 합니다. 클러스터가 미달 사용 상태에 가까워지면에서 크기가 조정 됩니다. 이 작업은 비용을 절감 하지만 성능 수준을 유지 합니다. 클러스터가 과도 한 사용률에 도달 하는 경우 최적의 성능을 유지 하기 위해 확장 됩니다. 최적화 된 자동 크기 조정을 구성 하려면:

1. **최적화 된 자동 크기 조정**을 선택 합니다. 

1. 최소 인스턴스 수 및 최대 인스턴스 수를 선택 합니다. 클러스터 자동 크기 조정 범위는 부하에 따라 두 숫자 사이에 있습니다.

1. **저장**을 선택합니다.

   ![최적화 된 자동 크기 조정 방법](media/manage-cluster-horizontal-scaling/optimized-autoscale-method.png)

최적화 된 자동 크기 조정이 작동 하기 시작 합니다. 이제 해당 작업이 클러스터의 Azure 활동 로그에 표시 됩니다.

#### <a name="logic-of-optimized-autoscale"></a>최적화 된 자동 크기 조정 논리 

**규모 확장**

클러스터가 과도 한 사용률 상태에 가까워지면 최적의 성능을 유지 하기 위해 규모를 확장 합니다. 규모 확장은 다음과 같은 경우에 발생 합니다.
* 클러스터 인스턴스 수가 사용자가 정의한 최대 인스턴스 수를 벗어났습니다.
* 캐시 사용률은 한 시간 이상에 대해 높습니다.

> [!NOTE]
> Scale out 논리는 현재 수집 사용률 및 CPU 메트릭을 고려 하지 않습니다. 이러한 메트릭이 사용 사례에 중요 한 경우 [사용자 지정 자동 크기 조정을](#custom-autoscale)사용 합니다.

**규모 확장**

클러스터가 미달 사용 상태에 도달 하는 경우 비용을 절감 하지만 성능을 유지 합니다. 여러 메트릭을 사용 하 여 클러스터에서 크기를 조정 하는 것이 안전한 지 확인 합니다. 다음 규칙은 규모 조정이 수행 되기 7 일 동안 매일 평가 됩니다.
* 인스턴스 수가 정의 된 최소 인스턴스 수를 2 이상 초과 합니다.
* 리소스가 오버 로드 되지 않도록 하려면 scale in이 수행 되기 전에 다음 메트릭을 확인 해야 합니다. 
    * 캐시 사용률은 높지 않습니다.
    * CPU 평균 미만 
    * 수집 사용률이 평균 미만입니다. 
    * 스트리밍 수집 사용률 (스트리밍 수집을 사용 하는 경우)이 크지 않음
    * 연결 유지 이벤트는 정의 된 최소, 적절 하 게 처리 된 시간 이상입니다.
    * 쿼리 제한 없음 
    * 실패 한 쿼리 수가 정의 된 최소값 보다 낮은 경우

> [!NOTE]
> 현재 확장 논리에는의 최적화 된 확장을 구현 하기 전에 7 일 평가판이 필요 합니다. 이 평가는 24 시간 마다 한 번씩 수행 됩니다. 빠른 변경이 필요한 경우 [수동 크기 조정을](#manual-scale)사용 합니다.

### <a name="custom-autoscale"></a>사용자 지정 자동 크기 조정

사용자 지정 자동 크기 조정을 사용 하 여 지정한 메트릭에 따라 동적으로 클러스터를 확장할 수 있습니다. 다음 그림에는 사용자 지정 자동 크기 조정을 구성 하는 흐름과 단계가 나와 있습니다. 자세한 내용은 그래픽을 참조 하세요.

1. **크기 자동 조정 설정 이름** 상자에 *확장: 캐시 사용률*등의 이름을 입력 합니다. 

   ![크기 조정 규칙](media/manage-cluster-horizontal-scaling/custom-autoscale-method.png)

2. **크기 조정 모드**의 경우 **메트릭에 따라 크기 조정**을 선택 합니다. 이 모드는 동적 크기 조정을 제공 합니다. **특정 인스턴스 수로 크기 조정을**선택할 수도 있습니다.

3. **+ 규칙 추가를**선택 합니다.

4. 오른쪽의 **크기 조정 규칙** 섹션에서 각 설정에 대 한 값을 입력 합니다.

    **조건**

    | 설정 | 설명 및 값 |
    | --- | --- |
    | **시간 집계** | **평균** 같은 집계 기준을 선택합니다. |
    | **메트릭 이름** | **캐시 사용률**과 같이 크기 조정 작업의 기준이 될 메트릭을 선택합니다. |
    | **시간 조직 통계** | **평균**, **최솟값**, **최댓값**, **합계** 중에서 선택합니다. |
    | **연산자** | **보다 크거나 같음**과 같은 적절한 옵션을 선택합니다. |
    | **임계값** | 적절한 값을 선택합니다. 예를 들어 캐시 사용률의 경우 80%가 좋은 시작점입니다. |
    | **기간(분)** | 시스템이 메트릭을 계산하는 경우에 다시 확인하는 적절한 기간을 선택합니다. 처음에는 기본값 10분으로 시작합니다. |
    |  |  |

    **작업**

    | 설정 | 설명 및 값 |
    | --- | --- |
    | **작업** | 규모를 감축하거나 규모를 확장하는 적절한 옵션을 선택합니다. |
    | **인스턴스 수** | 메트릭 조건을 충족할 경우 추가하거나 제거할 노드 또는 인스턴스 수를 선택합니다. |
    | **정지(분)** | 크기 조정 작업 간의 적절한 시간 간격을 선택합니다. 기본값 5분으로 시작합니다. |
    |  |  |

5. **추가**를 선택합니다.

6. 왼쪽의 **인스턴스 제한** 섹션에서 각 설정에 대 한 값을 입력 합니다.

    | 설정 | 설명 및 값 |
    | --- | --- |
    | **최소** | 사용률에 관계없이 클러스터가 인스턴스 수를 이보다 낮게 줄이지 않습니다. |
    | **최댓값** | 사용률에 관계없이 클러스터가 인스턴스 수를 이보다 높게 늘리지 않습니다. |
    | **기본값** | 기본 인스턴스 수입니다. 이 설정은 리소스 메트릭을 읽는 데 문제가 있는 경우에 사용 됩니다. |
    |  |  |

7. **저장**을 선택합니다.

이제 Azure 데이터 탐색기 클러스터에 대해 수평적 크기 조정을 구성 했습니다. 수직 크기 조정을 위한 다른 규칙을 추가 합니다. 클러스터 크기 조정 문제에 대 한 지원이 필요한 경우 Azure Portal에서 [지원 요청을 여세요](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) .

## <a name="next-steps"></a>다음 단계

* [메트릭을 사용 하 여 Azure 데이터 탐색기 성능, 상태 및 사용 현황 모니터링](using-metrics.md)
* 클러스터의 적절 한 크기 조정에 대 한 [클러스터 수직 크기 조정을 관리](manage-cluster-vertical-scaling.md) 합니다.
