---
title: 데이터 흐름 성능 및 튜닝 가이드 매핑
description: Azure Data Factory에서 데이터 흐름의 매핑 성능에 영향을 주는 주요 요소에 대해 알아봅니다.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.custom: seo-lt-2019
ms.date: 10/07/2019
ms.openlocfilehash: fb2a11850370766ab174c67dd122f33879fb432a
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74928526"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>데이터 흐름 매핑 성능 및 튜닝 가이드

Azure Data Factory에서 데이터 흐름을 매핑하면 대규모로 데이터 변환을 디자인, 배포 및 오케스트레이션 할 수 있는 코드 없는 인터페이스를 제공 합니다. 데이터 흐름 매핑을 잘 모르는 경우 [데이터 흐름 매핑 개요](concepts-data-flow-overview.md)를 참조 하세요.

ADF UX에서 데이터 흐름을 디자인 하 고 테스트할 때 클러스터가 준비 될 때까지 기다리지 않고 실시간으로 데이터 흐름을 실행 하려면 디버그 모드로 전환 해야 합니다. 자세한 내용은 [디버그 모드](concepts-data-flow-debug-mode.md)를 참조 하세요.

## <a name="monitoring-data-flow-performance"></a>데이터 흐름 성능 모니터링

매핑 데이터 흐름을 디자인 하는 동안 구성 패널의 데이터 미리 보기 탭을 클릭 하 여 각 변환을 단위 테스트할 수 있습니다. 논리를 확인 한 후 파이프라인에서 작업으로 종단 간 데이터 흐름을 테스트 합니다. 데이터 흐름 실행 작업을 추가 하 고 디버그 단추를 사용 하 여 데이터 흐름의 성능을 테스트 합니다. 데이터 흐름의 실행 계획 및 성능 프로필을 열려면 파이프라인의 출력 탭에 있는 ' 작업 ' 아래에서 안경 아이콘을 클릭 합니다.

![데이터 흐름 모니터](media/data-flow/mon002.png "데이터 흐름 모니터 2")

 이 정보를 사용 하 여 다양 한 크기의 데이터 원본에 대 한 데이터 흐름의 성능을 예측할 수 있습니다. 자세한 내용은 [데이터 흐름 매핑 모니터링](concepts-data-flow-monitoring.md)을 참조 하세요.

![데이터 흐름 모니터링](media/data-flow/mon003.png "데이터 흐름 모니터 3")

 파이프라인 디버그 실행의 경우에는 웜 클러스터에 전체 성능 계산의 클러스터 설정 시간 1 분 정도가 필요 합니다. 기본 Azure Integration Runtime를 초기화 하는 경우에는 회전 시간이 약 5 분 정도 걸릴 수 있습니다.

## <a name="increasing-compute-size-in-azure-integration-runtime"></a>Azure Integration Runtime에서 계산 크기를 늘립니다.

더 많은 코어가 있는 Integration Runtime Spark 계산 환경에서 노드 수를 늘리고 데이터를 읽고, 쓰고, 변환할 수 있는 더 많은 처리 능력을 제공 합니다.
* 처리 속도가 입력 요금 보다 높은 경우 **계산에 최적화** 된 클러스터를 시도 합니다.
* 메모리에 더 많은 데이터를 캐시 하려면 메모리 액세스에 **최적화** 된 클러스터를 시도 합니다.

![새 IR](media/data-flow/ir-new.png "새 IR")

Integration Runtime를 만드는 방법에 대 한 자세한 내용은 [Azure Data Factory의 Integration Runtime](concepts-integration-runtime.md)를 참조 하세요.

### <a name="increase-the-size-of-your-debug-cluster"></a>디버그 클러스터의 크기를 늘립니다.

기본적으로 디버그를 설정 하면 각 데이터 팩터리에 대해 자동으로 생성 되는 기본 Azure Integration runtime을 사용 합니다. 이 기본 Azure IR은 8 개 코어에 대해 설정 되 고, 4 개 코어에 대해 4 개, 일반 계산 속성을 사용 하는 4 개 코어, 작업자 노드에 대해 4 개 큰 데이터를 사용 하 여 테스트 하는 경우 더 큰 구성으로 Azure IR를 만들고 디버그를 전환할 때이 새 Azure IR를 선택 하 여 디버그 클러스터의 크기를 늘릴 수 있습니다. 그러면 ADF가 데이터 흐름을 사용 하 여 데이터 미리 보기 및 파이프라인 디버그에이 Azure IR를 사용 하도록 합니다.

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Azure SQL Database 및 Azure SQL Data Warehouse 최적화

### <a name="partitioning-on-source"></a>원본에서 분할

1. **최적화** 탭으로 이동 하 여 **분할 설정** 을 선택 합니다.
1. **원본**을 선택 합니다.
1. **파티션 수**아래에서 AZURE SQL DB에 대 한 최대 연결 수를 설정 합니다. 더 높은 설정을 시도 하 여 데이터베이스에 대 한 병렬 연결을 확보할 수 있습니다. 그러나 일부 경우에는 제한 된 수의 연결을 사용 하 여 더 빠른 성능을 얻을 수 있습니다.
1. 특정 테이블 열 또는 쿼리로 분할할 것인지 여부를 선택 합니다.
1. **열**을 선택한 경우 파티션 열을 선택 합니다.
1. **쿼리**를 선택한 경우 데이터베이스 테이블의 파티션 구성표와 일치 하는 쿼리를 입력 합니다. 이 쿼리를 통해 원본 데이터베이스 엔진은 파티션 제거를 활용할 수 있습니다. 원본 데이터베이스 테이블을 분할할 필요가 없습니다. 원본이 아직 분할 되지 않은 경우 ADF는 원본 변환에서 선택한 키에 따라 Spark 변환 환경에서 데이터 분할을 계속 사용 합니다.

![원본 파트](media/data-flow/sourcepart3.png "원본 파트")

### <a name="source-batch-size-input-and-isolation-level"></a>원본 일괄 처리 크기, 입력 및 격리 수준

원본 변환의 **원본 옵션** 아래에서 다음 설정이 성능에 영향을 줄 수 있습니다.

* 일괄 처리 크기는 ADF가 행 단위 대신 메모리의 집합에 데이터를 저장 하도록 지시 합니다. 일괄 처리 크기는 선택적 설정 이며 적절 하 게 크기가 조정 되지 않은 경우 계산 노드에서 리소스가 부족 할 수 있습니다.
* 쿼리를 설정 하면 처리를 위해 데이터 흐름에 도착 하기 전에 원본에서 행을 필터링 할 수 있습니다. 이를 통해 초기 데이터를 더 빠르게 가져올 수 있습니다. 쿼리를 사용 하는 경우 Azure SQL DB에 대 한 선택적 쿼리 힌트 (예: 커밋되지 않은 읽기)를 추가할 수 있습니다.
* 커밋되지 않은 읽기는 원본 변환에서 더 빠른 쿼리 결과를 제공 합니다.

![원본](media/data-flow/source4.png "원본")

### <a name="sink-batch-size"></a>싱크 일괄 처리 크기

데이터 흐름의 행 단위 처리를 방지 하려면 Azure SQL DB 및 Azure SQL DW 싱크에 대 한 설정 탭에서 **일괄 처리 크기** 를 설정 합니다. 일괄 처리 크기를 설정한 경우 ADF는 제공 된 크기에 따라 일괄 처리로 데이터베이스 쓰기를 처리 합니다.

![싱크](media/data-flow/sink4.png "sink")

### <a name="partitioning-on-sink"></a>싱크에 분할

데이터를 대상 테이블에 분할 하지 않은 경우에도 싱크 변환에서 데이터를 분할 하는 것이 좋습니다. 분할 된 데이터를 사용 하면 모든 연결을 강제 적용 하 여 단일 노드/파티션을 사용 하는 것 보다 훨씬 빠르게 로드 됩니다. 싱크의 최적화 탭으로 이동 하 여 *라운드 로빈* 분할을 선택 하 여 싱크에 쓸 이상적인 파티션 수를 선택 합니다.

### <a name="disable-indexes-on-write"></a>쓰기 시 인덱스 사용 안 함

파이프라인에서 [저장 프로시저 작업](transform-data-using-stored-procedure.md) 을 추가 하 여 싱크에 의해 작성 된 대상 테이블의 인덱스를 비활성화 하는 데이터 흐름 작업 앞에 추가 합니다. 데이터 흐름 작업 후 해당 인덱스를 사용 하도록 설정 하는 다른 저장 프로시저 작업을 추가 합니다.

### <a name="increase-the-size-of-your-azure-sql-db-and-dw"></a>Azure SQL DB 및 DW의 크기를 늘립니다.

처리량을 늘리고 DTU 한도에 도달 하면 Azure 제한을 최소화 하기 위해 파이프라인 실행 전에 원본 크기 조정 및 Azure SQL DB 및 DW를 싱크 합니다. 파이프라인 실행이 완료 되 면 데이터베이스를 다시 일반 실행 속도로 조정 합니다.

### <a name="azure-sql-dw-only-use-staging-to-load-data-in-bulk-via-polybase"></a>[Azure SQL DW 전용] 준비를 사용 하 여 Polybase를 통해 데이터를 대량으로 로드

DW에 행 단위 삽입을 방지 하려면 ADF가 [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)를 사용할 수 있도록 싱크 설정에서 **준비 사용** 을 선택 합니다. PolyBase를 사용 하면 ADF가 데이터를 대량으로 로드할 수 있습니다.
* 파이프라인에서 데이터 흐름 활동을 실행할 경우 대량 로드 중 데이터를 준비 하기 위해 Blob 또는 ADLS Gen2 저장소 위치를 선택 해야 합니다.

## <a name="optimizing-for-files"></a>파일 최적화

각 변환에서 데이터 팩터리가 사용할 파티션 구성표를 최적화 탭에서 설정할 수 있습니다.
* 작은 파일의 경우 Spark에 작은 파일을 분할 하 라고 요청 하는 것 보다 *단일 파티션* 선택이 더 빠르고 빠르게 작동할 수 있습니다.
* 원본 데이터에 대 한 충분 한 정보가 없는 경우 *라운드 로빈* 분할을 선택 하 고 파티션 수를 설정 합니다.
* 데이터에 적절 한 해시 키가 될 수 있는 열이 있는 경우 *해시 분할*을 선택 합니다.

데이터 미리 보기 및 파이프라인 디버그에서 디버그할 때 파일 기반 원본 데이터 집합의 제한 및 샘플링 크기는 읽은 행 수가 아니라 반환 된 행 수에만 적용 됩니다. 이로 인해 디버그 실행의 성능에 영향을 줄 수 있으며 흐름이 실패할 수 있습니다.
* 디버그 클러스터는 기본적으로 작은 단일 노드 클러스터 이며 디버깅을 위해 샘플 작은 파일을 사용 하는 것이 좋습니다. 디버그 설정으로 이동 하 여 임시 파일을 사용 하 여 데이터의 작은 하위 집합을 가리킵니다.

    ![디버그 설정](media/data-flow/debugsettings3.png "디버그 설정")

### <a name="file-naming-options"></a>파일 명명 옵션

Blob 또는 ADLS 파일 저장소를 기록 하는 데이터 흐름 매핑에서 변환 된 데이터를 작성 하는 가장 일반적인 방법입니다. 싱크에서 명명 된 파일이 아니라 컨테이너 또는 폴더를 가리키는 데이터 집합을 선택 해야 합니다. 매핑 데이터 흐름은 실행을 위해 Spark를 사용 하므로 출력은 파티션 구성표에 따라 여러 파일로 분할 됩니다.

일반적인 파티션 구성표는 _단일 파일로 출력_을 선택 하 여 모든 출력 파트 파일을 싱크의 단일 파일에 병합 하는 것입니다. 이 작업을 수행 하려면 단일 클러스터 노드의 단일 파티션으로 출력이 줄어듭니다. 많은 양의 원본 파일을 단일 출력 파일에 결합 하는 경우 클러스터 노드 리소스가 부족 할 수 있습니다.

계산 노드 리소스가 고갈 되지 않도록 하려면 데이터 흐름의 기본 최적화 된 체계를 유지 하 고 출력 폴더의 모든 파트 파일을 새 단일 파일에 병합 하는 복사 작업을 파이프라인에 추가 합니다. 이 기법은 파일 병합에서 변환 작업을 분리 하 고 _출력을 단일 파일로 설정 하_는 것과 동일한 결과를 얻을 수 있습니다.

### <a name="looping-through-file-lists"></a>파일 목록 반복

원본 변환이 각 작업에 대해를 통해 반복 하는 대신 여러 파일을 반복 하는 경우 매핑 데이터 흐름이 더 효율적으로 실행 됩니다. 원본 변환에서 와일드 카드 또는 파일 목록을 사용 하는 것이 좋습니다. 데이터 흐름 프로세스는 Spark 클러스터 내에서 루핑 발생을 허용 하 여 더 빨리 실행 됩니다. 자세한 내용은 [원본 변환에서 와일드 카드](data-flow-source.md#file-based-source-options)를 참조 하세요.

예를 들어 7 월 2019의 데이터 파일 목록이 Blob Storage 폴더에서 처리 하려는 경우 다음은 원본 변환에서 사용할 수 있는 와일드 카드입니다.

```DateFiles/*_201907*.txt```

와일드 카드를 사용 하 여 파이프라인에는 데이터 흐름 활동이 하나만 포함 됩니다. 이는 내에서 데이터 흐름 실행 작업이 포함 된 ForEach를 사용 하 여 일치 하는 모든 파일에 대해 반복 하는 Blob 저장소에 대 한 조회 보다 더 잘 수행 됩니다.

### <a name="optimizing-for-cosmosdb"></a>CosmosDB 최적화

CosmosDB 싱크에 대 한 처리량 및 일괄 처리 속성 설정은 파이프라인 데이터 흐름 작업에서 해당 데이터 흐름을 실행 하는 동안에만 적용 됩니다. 원래 컬렉션 설정은 데이터 흐름을 실행 한 후 CosmosDB에서 적용 됩니다.

* 일괄 처리 크기: 데이터의 대략적인 행 크기를 계산 하 고 행 크기 * 일괄 처리 크기가 200만 보다 적은지 확인 합니다. 이 경우 일괄 처리 크기를 늘려 더 나은 처리량을 얻습니다.
* 처리량: 문서를 CosmosDB에 더 빨리 쓸 수 있도록 여기에서 더 높은 처리량 설정을 설정 합니다. 높은 처리량 설정에 따라 높은 수준의 비용을 염두에 두십시오.
*   쓰기 처리량 예산: 분당 총 RUs 보다 작은 값을 사용 합니다. Spark 파티션 수가 많은 데이터 흐름이 있는 경우 예산 처리량을 설정 하면 해당 파티션에 대 한 균형을 높일 수 있습니다.

## <a name="next-steps"></a>다음 단계

성능 관련 다른 데이터 흐름 문서를 참조 하세요.

- [데이터 흐름 최적화 탭](concepts-data-flow-overview.md#optimize)
- [데이터 흐름 작업](control-flow-execute-data-flow-activity.md)
- [데이터 흐름 성능 모니터링](concepts-data-flow-monitoring.md)
