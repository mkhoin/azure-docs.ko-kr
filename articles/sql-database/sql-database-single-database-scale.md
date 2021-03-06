---
title: 단일 데이터베이스 리소스 크기 조정
description: 이 문서에서는 Azure SQL Database에서 단일 데이터베이스에 사용할 수 있는 컴퓨팅 및 스토리지 리소스의 크기를 조정하는 방법을 설명합니다.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 04/26/2019
ms.openlocfilehash: 8d4917bb8956185e0cb557368fbb0c64343c0ac6
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422550"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Azure SQL Database에서 단일 데이터베이스 리소스 크기 조정

이 문서에서는 프로 비전 된 계산 계층에서 Azure SQL Database 사용할 수 있는 계산 및 저장소 리소스의 크기를 조정 하는 방법을 설명 합니다. 또는 서버 리스 [계산 계층이](sql-database-serverless.md) 사용 된 계산에 대 한 초당 계산 자동 크기 조정 및 요금 청구를 제공 합니다.

## <a name="change-compute-size-vcores-or-dtus"></a>계산 크기 변경 (vCores 또는 Dtu)

VCores 또는 Dtu 수를 처음 선택 하 고 나면 [Azure Portal](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [transact-sql](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update)또는 [REST API](https://docs.microsoft.com/rest/api/sql/databases/update)를 사용 하 여 실제 환경에 따라 단일 데이터베이스를 동적으로 확장 또는 축소할 수 있습니다.

다음 비디오에서는 서비스 계층 및 컴퓨팅 크기를 동적으로 변경하여 단일 데이터베이스에 대해 사용 가능한 DTU를 늘리는 방법을 보여줍니다.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]

> [!IMPORTANT]
> 경우에 따라 사용하지 않는 공간을 회수하기 위해 데이터베이스를 축소해야 할 수도 있습니다. 자세한 내용은 [Azure SQL Database의 파일 공간 관리](sql-database-file-space-management.md)를 참조하세요.

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>서비스 계층 또는 크기 조정 계산 크기 변경의 영향

서비스 계층 또는 계산 크기를 변경 하는 작업은 주로 다음 단계를 수행 하는 서비스와 관련이 있습니다.

1. 데이터베이스에 대 한 새 계산 인스턴스 만들기  

    요청 된 서비스 계층 및 계산 크기로 새 계산 인스턴스가 만들어집니다. 서비스 계층 및 계산 크기 변경 사항의 일부 조합에서는 데이터 복사를 포함 하는 새 계산 인스턴스에서 데이터베이스 복제본을 만들어야 하며 전체 대기 시간에 크게 영향을 줄 수 있습니다. 에 관계 없이이 단계에서 데이터베이스는 온라인 상태로 유지 되 고 연결은 원래 계산 인스턴스의 데이터베이스로 계속 전달 됩니다.

2. 새 계산 인스턴스로 연결 라우팅 전환

    원래 계산 인스턴스의 데이터베이스에 대 한 기존 연결이 삭제 됩니다. 새 계산 인스턴스의 데이터베이스에 대 한 모든 새 연결이 설정 됩니다. 서비스 계층 및 계산 크기 변경의 일부 조합에서 데이터베이스 파일은 스위치 중에 분리 되어 다시 첨부 됩니다.  이 스위치를 사용할 경우 데이터베이스를 일반적으로 30 초 미만으로 사용할 수 없고 몇 초 동안만 자주 서비스 중단이 발생할 수 있습니다. 연결이 끊어질 때 실행 되는 장기 실행 트랜잭션이 있는 경우이 단계의 지속 시간은 중단 된 트랜잭션을 복구 하는 데 시간이 오래 걸릴 수 있습니다. [가속화 된 데이터베이스 복구](sql-database-accelerated-database-recovery.md) 는 장기 실행 트랜잭션을 중단 하는 것으로 인 한 영향을 줄일 수 있습니다.

> [!IMPORTANT]
> 워크플로의 단계 중에는 데이터가 손실 되지 않습니다. 서비스 계층을 변경 하는 동안 Azure SQL Database를 사용 하는 응용 프로그램 및 구성 요소에서 [재시도 논리](sql-database-connectivity-issues.md) 를 몇 가지 구현 했는지 확인 합니다.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>서비스 계층 또는 크기 조정 계산 크기 변경의 대기 시간

단일 데이터베이스 또는 탄력적 풀의 서비스 계층을 변경 하거나 계산 크기를 조정 하는 데 예상 되는 대기 시간은 다음과 같이 매개 변수화 됩니다.

|서비스 계층|기본 단일 데이터베이스,</br>Standard (S0-S1)|기본 탄력적 풀</br>Standard (S2-S 12), </br>대규모 </br>범용 단일 데이터베이스 또는 탄력적 풀|프리미엄 또는 중요 비즈니스용 단일 데이터베이스 또는 탄력적 풀|
|:---|:---|:---|:---|
|**기본 단일 데이터베이스,</br> Standard (S0-S1)**|사용 된 공간에 독립적인 일정 한 시간 대기 시간 &nbsp;&bull;</br>&bull; &nbsp;일반적으로 5 분 미만|데이터 복사로 인해 사용 되는 데이터베이스 공간에 비례 하는 &bull; &nbsp;대기 시간</br>일반적으로 사용 되는 공간의 GB 당 1 분 미만 &bull; &nbsp;|데이터 복사로 인해 사용 되는 데이터베이스 공간에 비례 하는 &bull; &nbsp;대기 시간</br>일반적으로 사용 되는 공간의 GB 당 1 분 미만 &bull; &nbsp;|
|**기본 탄력적 풀, </br>Standard (S2-S 12), </br>Hyperscale, </br>범용 단일 데이터베이스 또는 탄력적 풀**|데이터 복사로 인해 사용 되는 데이터베이스 공간에 비례 하는 &bull; &nbsp;대기 시간</br>일반적으로 사용 되는 공간의 GB 당 1 분 미만 &bull; &nbsp;|사용 된 공간에 독립적인 일정 한 시간 대기 시간 &nbsp;&bull;</br>&bull; &nbsp;일반적으로 5 분 미만|데이터 복사로 인해 사용 되는 데이터베이스 공간에 비례 하는 &bull; &nbsp;대기 시간</br>일반적으로 사용 되는 공간의 GB 당 1 분 미만 &bull; &nbsp;|
|**프리미엄 또는 중요 비즈니스용 단일 데이터베이스 또는 탄력적 풀**|데이터 복사로 인해 사용 되는 데이터베이스 공간에 비례 하는 &bull; &nbsp;대기 시간</br>일반적으로 사용 되는 공간의 GB 당 1 분 미만 &bull; &nbsp;|데이터 복사로 인해 사용 되는 데이터베이스 공간에 비례 하는 &bull; &nbsp;대기 시간</br>일반적으로 사용 되는 공간의 GB 당 1 분 미만 &bull; &nbsp;|데이터 복사로 인해 사용 되는 데이터베이스 공간에 비례 하는 &bull; &nbsp;대기 시간</br>일반적으로 사용 되는 공간의 GB 당 1 분 미만 &bull; &nbsp;|

> [!TIP]
> 진행 중인 작업을 모니터링 하려면 [sql REST API를 사용 하 여 작업 관리](https://docs.microsoft.com/rest/api/sql/operations/list), [CLI를 사용](/cli/azure/sql/db/op)하 여 작업 관리, [t-sql을 사용 하 여 작업 모니터링](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) 및 다음 두 가지 PowerShell 명령 ( [AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) 및 [AzSqlDatabaseActivity)](/powershell/module/az.sql/stop-azsqldatabaseactivity)을 참조 하세요.

### <a name="cancelling-service-tier-changes-or-compute-rescaling-operations"></a>서비스 계층 변경 취소 또는 계산 크기 조정 작업

서비스 계층 변경 또는 계산 크기 조정 작업을 취소할 수 있습니다.

#### <a name="azure-portal"></a>Azure 포털

데이터베이스 개요 블레이드에서 **알림으로** 이동 하 고 진행 중인 작업이 있음을 나타내는 타일을 클릭 합니다.

![진행 중인 작업](media/sql-database-single-database-scale/ongoing-operations.png)

그런 다음 **이 작업 취소**단추를 클릭 합니다.

![진행 중인 작업 취소](media/sql-database-single-database-scale/cancel-ongoing-operation.png)

#### <a name="powershell"></a>PowerShell

PowerShell 명령 프롬프트에서 `$resourceGroupName`, `$serverName`및 `$databaseName`를 설정 하 고 다음 명령을 실행 합니다.

```powershell
$operationName = (az sql db op list --resource-group $resourceGroupName --server $serverName --database $databaseName --query "[?state=='InProgress'].name" --out tsv)
if (-not [string]::IsNullOrEmpty($operationName)) {
    (az sql db op cancel --resource-group $resourceGroupName --server $serverName --database $databaseName --name $operationName)
        "Operation " + $operationName + " has been canceled"
}
else {
    "No service tier change or compute rescaling operation found"
}
```

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>서비스 계층 또는 크기 조정 계산 크기 변경 시 추가 고려 사항

- 상위 서비스 계층이나 컴퓨팅 크기로 업그레이드하는 경우 더 큰 크기(최대 크기)를 명시적으로 지정하지 않는 한 최대 데이터베이스 크기는 증가하지 않습니다.
- 데이터베이스를 다운그레이드하려면 데이터베이스 사용 공간이 대상 서비스 계층 및 컴퓨팅 크기의 최대 허용 크기보다 작아야 합니다.
- **프리미엄**에서 **표준** 계층으로 다운그레이드하는 경우 (1) 데이터베이스의 최대 크기가 대상 컴퓨팅 크기에서 지원되고 (2) 최대 크기가 대상 컴퓨팅 크기의 포함된 스토리지 용량을 초과하는 경우 추가 스토리지 비용이 적용됩니다. 예를 들어 최대 크기가 500 GB 인 P1 데이터베이스가 s 3으로 다운 되는 경우 S3에서 최대 크기인 1tb를 지원 하 고 포함 된 저장소 크기는 250 GB 이므로 추가 저장소 비용이 적용 됩니다. 따라서 추가 스토리지 용량은 500GB – 250GB = 250GB입니다. 추가 스토리지 가격 책정에 대한 자세한 내용은 [SQL Database 가격 책정](https://azure.microsoft.com/pricing/details/sql-database/)을 참조하세요. 실제 사용된 공간의 크기가 포함된 스토리지 용량보다 작은 경우 데이터베이스 최대 크기를 포함된 크기로 줄여 이러한 추가 비용을 방지할 수 있습니다.
- [지역 복제](sql-database-geo-replication-portal.md)를 사용하도록 설정된 데이터베이스를 업그레이드하는 경우, 주 데이터베이스를 업그레이드하기 전에 먼저 해당 보조 데이터베이스를 원하는 서비스 계층 및 컴퓨팅 크기로 업그레이드합니다(최상의 성능을 위한 일반 지침). 다른 버전으로 업그레이드할 때 보조 데이터베이스를 먼저 업그레이드해야 합니다.
- [지역 복제](sql-database-geo-replication-portal.md)를 사용하도록 설정된 데이터베이스를 업그레이드하는 경우, 주 데이터베이스를 업그레이드하기 전에 먼저 해당 보조 데이터베이스를 원하는 서비스 계층 및 컴퓨팅 크기로 업그레이드합니다(최상의 성능을 위한 일반 지침). 다른 버전으로 다운그레이드할 때 주 데이터베이스를 먼저 다운그레이드해야 합니다.
- 복원 서비스는 여러 서비스 계층에서 서로 다르게 제공됩니다. **기본** 계층으로 다운그레이드하는 경우 백업 보존 기간이 더 짧아집니다. [Azure SQL Database 백업](sql-database-automated-backups.md)을 참조하세요.
- 데이터베이스의 새로운 속성은 변경이 완료될 때까지 적용되지 않습니다.

### <a name="billing-during-compute-rescaling"></a>계산 크기 조정 동안의 요금 청구

사용량 또는 데이터베이스가 한 시간 미만 동안 활성 상태였는지 여부와 관계없이, 해당 시간에 적용된 최고 서비스 계층 + 컴퓨팅 크기를 사용하여 데이터베이스가 있었던 각 시간에 대해 요금이 청구됩니다. 예를 들어 단일 데이터베이스를 만들고 5분 후 삭제하더라도 청구서에는 데이터베이스 1시간 사용에 대한 요금이 반영됩니다.

## <a name="change-storage-size"></a>스토리지 크기 변경

### <a name="vcore-based-purchasing-model"></a>vCore 기반 구매 모델

- 저장소는 1GB 증분 단위로 최대 크기 제한까지 프로비전할 수 있습니다. 구성 가능한 최소 데이터 저장소는 5GB입니다.
- 단일 데이터베이스에 대한 스토리지는 [Azure Portal](https://portal.azure.com), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update), 또는 [REST API](https://docs.microsoft.com/rest/api/sql/databases/update)를 사용하여 해당 최대 크기를 늘리거나 줄여서 프로비전할 수 있습니다.
- SQL Database는 로그 파일에 대해 추가 스토리지의 30% 및 TempDB에 대해 vCore당 32GB를 자동으로 할당하지만 384GB를 초과하지 않도록 합니다. TempDB는 모든 서비스 계층의 연결형 SSD에 있습니다.
- 단일 데이터베이스에 대한 스토리지의 가격은 데이터 스토리지 및 로그 스토리지 용량 합계에 해당 서비스 계층의 스토리지 단가를 곱한 값입니다. TempDB의 비용은 vCore 가격에 포함됩니다. 추가 스토리지 가격에 대한 자세한 내용은 [SQL Database 가격 책정](https://azure.microsoft.com/pricing/details/sql-database/)을 참조하세요.

> [!IMPORTANT]
> 경우에 따라 사용하지 않는 공간을 회수하기 위해 데이터베이스를 축소해야 할 수도 있습니다. 자세한 내용은 [Azure SQL Database의 파일 공간 관리](sql-database-file-space-management.md)를 참조하세요.

### <a name="dtu-based-purchasing-model"></a>DTU 기반 구매 모델

- 단일 데이터베이스에 대한 DTU 가격에는 특정 크기의 저장소가 추가 비용 없이 포함됩니다. 포함된 용량 외 추가 스토리지는 최대 250GB씩 총 1TB이 최대 크기 제한까지 추가 비용을 내고 프로비전할 수 있고 1TB 이상일 경우 256GB씩 프로비전할 수 있습니다. 포함된 스토리지 크기 및 최대 크기 제한에 대한 자세한 내용은 [단일 데이터베이스: 스토리지 크기 및 컴퓨팅 크기](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)를 참조하세요.
- 단일 데이터베이스에 대한 추가 스토리지는 Azure Portal, [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update), 또는 [REST API](https://docs.microsoft.com/rest/api/sql/databases/update)를 통해 해당하는 최대 크기를 늘려서 프로비전할 수 있습니다.
- 단일 데이터베이스에 대한 추가 스토리지 가격은 추가 스토리지 용량에 해당 서비스 계층의 추가 스토리지 단가를 곱한 것입니다. 추가 스토리지 가격에 대한 자세한 내용은 [SQL Database 가격 책정](https://azure.microsoft.com/pricing/details/sql-database/)을 참조하세요.

> [!IMPORTANT]
> 경우에 따라 사용하지 않는 공간을 회수하기 위해 데이터베이스를 축소해야 할 수도 있습니다. 자세한 내용은 [Azure SQL Database의 파일 공간 관리](sql-database-file-space-management.md)를 참조하세요.

## <a name="p11-and-p15-constraints-when-max-size-greater-than-1-tb"></a>최대 크기가 1tb 보다 큰 경우 P11 및 P15 제약 조건

프리미엄 계층에 1TB 이상 저장소는 중국 동부, 중국 북부, 독일 중부, 독일 북동쪽, 미국 서 부, US DoD 지역 및 미국 정부 중부를 제외한 모든 지역에서 사용할 수 있습니다. 이러한 지역에서 프리미엄 계층 저장소 최대 크기는 1TB로 제한됩니다. 다음 고려 사항 및 제한 사항은 최대 크기가 1TB보다 큰 P11 및 P15 데이터베이스에 적용됩니다.

- P11 또는 P15 데이터베이스의 최대 크기가 1tb 보다 큰 값으로 설정 된 경우에는 해당 값을 P11 또는 P15 데이터베이스에만 복원 하거나 복사할 수 있습니다.  이후 크기 조정 작업 시 할당 된 공간 크기가 새 계산 크기의 최대 크기 제한을 초과 하지 않는 경우 데이터베이스를 다른 계산 크기로 재조정 수 있습니다.
- 활성 지역 복제 시나리오의 경우:
  - 지역에서 복제 관계 설정: 주 데이터베이스가 P11 또는 P15인 경우 보조 데이터베이스는 P11 또는 P15이어야 합니다. 낮은 컴퓨팅 크기는 1TB 초과를 지원하지 않으므로 보조 데이터베이스로 거부됩니다.
  - 지역에서 복제 관계에서 주 데이터베이스 업그레이드: 주 데이터베이스에서 최대 크기를 1TB보다 크게 변경하면 보조 데이터베이스에서도 동일한 변경 사항이 트리거됩니다. 변경 내용을 적용하려면 주 데이터베이스에서 두 가지 업그레이드에 성공해야 합니다. 1TB보다 큰 옵션에 대한 지역 제한 사항이 적용됩니다. 1TB보다 큰 크기를 지원하지 않는 지역에 보조 데이터베이스가 있는 경우 주 데이터베이스는 업그레이드되지 않습니다.
- 1TB보다 큰 크기의 P11/P15 데이터베이스 로드에 대한 Import/Export 서비스 사용은 지원되지 않습니다. SqlPackage.exe를 사용하여 데이터를 [가져오기](sql-database-import.md) 및 [내보내기](sql-database-export.md)합니다.

## <a name="next-steps"></a>다음 단계

전반적인 리소스 제한은 [SQL Database vCore 기반 리소스 제한 - 단일 데이터베이스](sql-database-vcore-resource-limits-single-databases.md) 및 [SQL Database DTU 기반 리소스 제한 - 탄력적 풀](sql-database-dtu-resource-limits-single-databases.md)을 참조하세요.
