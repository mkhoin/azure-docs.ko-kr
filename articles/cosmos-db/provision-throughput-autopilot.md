---
title: Autopilot 모드에서 Azure Cosmos 컨테이너 및 데이터베이스를 만듭니다.
description: Autopilot 모드에서 이점, 사용 사례 및 Azure Cosmos 데이터베이스 및 컨테이너를 프로 비전 하는 방법에 대해 알아봅니다.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 2259343d2c7bca1f60a5256efcd572e6cc21b565
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74706046"
---
# <a name="create-azure-cosmos-containers-and-databases-in-autopilot-mode-preview"></a>Autopilot 모드에서 Azure Cosmos 컨테이너 및 데이터베이스 만들기 (미리 보기)

Azure Cosmos DB를 사용 하면 수동 모드 또는 autopilot 모드에서 컨테이너에 대 한 처리량을 프로 비전 할 수 있습니다. 이 문서에서는 autopilot 모드의 이점 및 사용 사례에 대해 설명 합니다.

> [!NOTE]
> Autopilot 모드는 현재 공개 미리 보기에서 사용할 수 있습니다. Azure Cosmos 계정에 대 한 autopilot 기능을 사용 하도록 설정 하려면이 문서의 [autopilot 사용](#enable-autopilot) 섹션을 참조 하세요. 새 데이터베이스 및 컨테이너에 대해서만 autopilot을 사용 하도록 설정할 수 있으며, 기존 컨테이너 및 데이터베이스에 대해서는 사용할 수 없습니다.

이제 처리량 수동 프로 비전 외에도 autopilot 모드에서 Azure cosmos 컨테이너를 구성할 수 있습니다. Autopilot 모드로 구성 된 Azure Cosmos 컨테이너 및 데이터베이스는 **sla를 손상 시 키 지 않고 응용 프로그램 요구 사항에 따라 프로 비전 된 처리량을 자동으로 조정 합니다.**

더 이상 프로 비전 된 처리량을 수동으로 관리 하거나 속도 제한 문제를 처리할 필요가 없습니다. Autopilot 모드로 구성 된 Azure Cosmos 컨테이너는 워크 로드의 가용성, 대기 시간, 처리량 또는 성능에 영향을 주지 않고 워크 로드에 대 한 응답으로 즉시 크기를 조정할 수 있습니다. 높은 사용률에서 autopilot 모드로 구성 된 Azure Cosmos 컨테이너는 진행 중인 작업에 영향을 주지 않고 확장 하거나 축소할 수 있습니다.

Autopilot 모드에서 컨테이너 및 데이터베이스를 구성할 때 초과 하지 `Tmax` 최대 처리량을 지정 해야 합니다. 그러면 컨테이너는 `0.1*Tmax < T < Tmax` 범위 내에서 워크 로드 요구 사항에 따라 즉시 크기를 조정할 수 있습니다. 즉, 컨테이너와 데이터베이스는 워크 로드 요구 사항에 따라 즉시 크기를 조정 하며, 구성 된 최대 처리량 값의 10%에서 낮은 값까지, 그리고 구성 된 최대 처리량 값에 따라 조정 됩니다. 언제 든 지 autopilot 데이터베이스 또는 컨테이너의 최대 처리량 (Tmax) 설정을 변경할 수 있습니다. Autopilot 옵션을 사용 하면 컨테이너 또는 데이터베이스당 400 r u/s 최소 처리량이 더 이상 적용 되지 않습니다.

Autopilot 미리 보기 중에는 컨테이너 또는 데이터베이스에 대해 지정 된 최대 처리량에 대해 시스템이 계산 된 저장소 제한 내에서 작동할 수 있습니다. 저장소 제한을 초과 하는 경우 최대 처리량이 더 높은 값으로 자동 조정 됩니다. Autopilot 모드에서 데이터베이스 수준 처리량을 사용 하는 경우 데이터베이스 내에서 허용 되는 컨테이너 수는 (0.001 * 최대 처리량)로 계산 됩니다. 예를 들어 2만 autopilot r u/s를 프로 비전 하는 경우 데이터베이스에는 20 개의 컨테이너가 있을 수 있습니다.

## <a name="benefits-of-autopilot-mode"></a>Autopilot 모드의 이점

Autopilot 모드로 구성 된 Azure Cosmos 컨테이너는 다음과 같은 이점을 제공 합니다.

* **단순:** Autopilot 모드의 컨테이너는 다양 한 컨테이너에 대해 프로 비전 된 처리량 (RUs) 및 용량을 수동으로 관리 하는 복잡성을 제거 합니다.

* **확장 가능:** Autopilot 모드의 컨테이너는 필요에 따라 프로 비전 된 처리량 용량을 원활 하 게 확장 합니다. 클라이언트 연결 및 응용 프로그램에 대 한 중단이 없으며 기존 Sla에 영향을 주지 않습니다.

* **비용 효율적:** Autopilot 모드로 구성 된 Azure Cosmos 컨테이너를 사용 하는 경우 작업에 필요한 리소스에 대해서만 비용을 지불 합니다.

* **항상 사용 가능:** Autopilot 모드의 Azure Cosmos 컨테이너는 전 세계적으로 분산 된 내결함성 및 고가용성 백 엔드를 사용 하 여 데이터 내구성 및 고가용성을 보장 합니다.

## <a name="use-cases-of-autopilot-mode"></a>Autopilot 모드의 사용 사례

Autopilot 모드로 구성 된 Azure Cosmos 컨테이너에 대 한 사용 사례는 다음과 같습니다.

* **가변 작업:** 매일 사용 하는 사용량이 가장 많은 응용 프로그램을 실행 하는 경우 매일 또는 매년 여러 번 사용 합니다. 예제에는 인적 자원, 예산 및 운영 보고용 응용 프로그램이 포함 됩니다. 이러한 시나리오의 경우 autopilot 모드로 구성 된 컨테이너를 사용할 수 있으며, 더 이상 최대 또는 평균 용량을 위해 수동으로 프로 비전 할 필요가 없습니다.

* **예측할 수 없는 워크 로드:** 하루 종일 데이터베이스를 사용 하는 워크 로드를 실행 하는 경우 예측 하기 어려운 활동의 최대 수입니다. 예에는 날씨 예측이 변경 될 때 활동의 서 수가 표시 되는 트래픽 사이트가 포함 되어 있습니다. Autopilot 모드에서 구성 된 컨테이너는 응용 프로그램의 최대 부하 요구에 맞게 용량을 조정 하 고 작업의 서 수가 초과 되 면 크기를 다시 조정 합니다.

* **새 응용 프로그램:** 새 응용 프로그램을 배포 하는 경우 필요한 프로 비전 된 처리량 (즉, RUs 수)이 확실 하지 않은 경우 Autopilot 모드로 구성 된 컨테이너를 사용 하 여 응용 프로그램의 용량 요구 사항 및 요구 사항에 맞게 자동으로 확장할 수 있습니다.

* **자주 사용 하지 않는 응용 프로그램:** 몇 시간 동안만 응용 프로그램을 사용 하는 경우 (예: 소용량 응용 프로그램/웹/블로그 사이트) 일 또는 주 또는 월에 몇 번만 사용 됩니다.

* **개발 및 테스트 데이터베이스:** 개발자는 근무 시간 중에 Azure Cosmos 계정을 사용 하지만 야간 또는 주말에는 필요 하지 않습니다. Autopilot 모드로 구성 된 컨테이너는 사용 하지 않을 때 최소로 축소 됩니다.

* **예약 된 프로덕션 워크 로드/쿼리:** 단일 컨테이너에 일련의 예약 된 요청/작업/쿼리가 있고 절대 낮은 처리량으로 실행 하려는 유휴 기간이 있는 경우이 작업을 쉽게 수행할 수 있습니다. 예약 된 쿼리/요청이 autopilot 모드로 구성 된 컨테이너에 제출 되 면 필요에 따라 자동으로 확장 되 고 작업을 실행 합니다.

이전 문제에 대 한 해결 방법은 구현에서 상당한 시간이 필요 하지만 구성 또는 코드의 복잡성을 야기 하 고 이러한 문제를 해결 하기 위해 수동으로 작업 해야 하는 경우가 많습니다. Autopilot 모드에서는 위의 시나리오를 즉시 사용할 수 있으므로 이러한 문제에 대해 더 이상 걱정할 필요가 없습니다.

## <a name="comparison--containers-configured-in-manual-mode-vs-autopilot-mode"></a>비교-수동 모드 및 autopilot 모드에서 구성 된 컨테이너

|  | 수동 모드에서 구성 된 컨테이너  | Autopilot 모드로 구성 된 컨테이너 |
|---------|---------|---------|
| **프로 비전 된 처리량** | 수동으로 프로 비전 됨 | 워크 로드 사용 패턴을 기반으로 자동으로 즉시 확장 됩니다. |
| **요청/작업의 요율 제한 (429)**  | 소비가 프로 비전 된 용량을 초과 하는 경우 발생할 수 있습니다. | 소비 된 처리량이 autopilot 모드에서 선택 하는 최대 처리량 내에 있는 경우에는 발생 하지 않습니다.   |
| **용량 계획** |  초기 용량 계획을 수행 하 고 필요한 처리량을 프로 비전 해야 합니다. |    용량 계획에 대해 걱정할 필요가 없습니다. 시스템은 용량 계획 및 용량 관리를 자동으로 처리 합니다. |
| **가격 책정** | 시간당 수동 프로 비전 된 r u/초 | 단일 쓰기 지역 계정의 경우 시간당 autopilot p s/s를 사용 하 여 시간 단위로 사용 되는 처리량에 대 한 비용을 지불 합니다. <br/><br/>여러 쓰기 지역이 있는 계정의 경우 autopilot에 대 한 추가 요금이 부과 되지 않습니다. 동일한 다중 마스터 r u/시간 급여를 사용 하 여 시간별로 사용 된 처리량에 대 한 비용을 지불 합니다. |
| **작업 유형에 가장 적합 합니다.** |  예측 가능 하 고 안정적인 작업|   예측할 수 없는 작업 및 가변 작업  |

## <a id="enable-autopilot"></a>Azure Portal에서 autopilot 사용

Azure Portal에서를 사용 하 여 Azure Cosmos 계정에서 autopilot를 사용해 볼 수 있습니다. Autopilot 옵션을 사용 하도록 설정 하려면 다음 단계를 수행 합니다.

1. Azure Portal에 로그인 [합니다.](https://portal.azure.com)

2. Azure Cosmos 계정으로 이동 하 여 **새 기능** 탭을 엽니다. 다음 스크린샷에 표시 된 것 처럼 **자동 파일럿** 을 선택 하 고 **등록** 합니다.

![Autopilot 모드에서 컨테이너 만들기](./media/provision-throughput-autopilot/enable-autopilot-azure-portal.png)

## <a name="create-a-database-or-a-container-with-autopilot-mode"></a>Autopilot 모드를 사용 하 여 데이터베이스 또는 컨테이너 만들기

데이터베이스 또는 컨테이너를 만드는 동안 autopilot를 구성할 수 있습니다. 새 데이터베이스 또는 컨테이너에 대해 다음 단계를 사용 하 여 autopilot를 사용 하도록 설정 하 고 최대 처리량을 지정 합니다.

1. [Azure Portal](https://portal.azure.com) 또는 [Azure Cosmos 탐색기에 로그인 합니다.](https://cosmos.azure.com/)

1. Azure Cosmos 계정으로 이동 하 여 **데이터 탐색기** 탭을 엽니다.

1. **새 컨테이너**를 선택 하 고 컨테이너의 이름, 파티션 키를 입력 합니다. **Autopilot** 옵션을 선택 하 고 Autopilot 옵션을 사용 하는 경우 컨테이너가 초과할 수 없는 최대 처리량을 선택 합니다.

   ![Autopilot 모드에서 컨테이너 만들기](./media/provision-throughput-autopilot/create-container-autopilot-mode.png)

1. **확인**을 선택합니다.

비슷한 단계를 통해 autopilot 모드에서 프로 비전 된 처리량을 사용 하 여 데이터베이스를 만들 수도 있습니다.

## <a id="autopilot-limits"></a>Autopilot에 대 한 처리량 및 저장소 제한

다음 표에서는 autopilot 모드의 여러 옵션에 대 한 최대 전체 및 저장소 제한을 보여 줍니다.

|최대 처리량 제한  |최대 저장소 제한  |
|---------|---------|
|4000 r u/초  |   50GB    |
|2만 r u/초  |  200GB  |
|10만 r u/초    |  1TB   |
|50만 r u/초    |  5TB  |

## <a name="next-steps"></a>다음 단계

* [논리 파티션](partition-data.md)에 대해 자세히 알아봅니다.
* [Azure Cosmos 컨테이너의 처리량을 프로비전](how-to-provision-container-throughput.md)하는 방법을 알아봅니다.
* [Azure Cosmos 데이터베이스의 처리량을 프로비전](how-to-provision-database-throughput.md)하는 방법을 알아봅니다.
