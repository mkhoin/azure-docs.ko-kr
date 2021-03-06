---
title: 워크 로드 관리를 위한 리소스 클래스
description: 리소스 클래스를 사용 하 여 Azure SQL Data Warehouse 쿼리의 동시성 및 계산 리소스를 관리 하는 방법에 대 한 지침입니다.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload-management
ms.date: 12/04/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: seo-lt-2019
ms.openlocfilehash: 30a3be1365f152a88713604570169091f09f0536
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74975434"
---
# <a name="workload-management-with-resource-classes-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse 리소스 클래스를 사용 하 여 워크 로드 관리

리소스 클래스를 사용 하 여 Azure SQL Data Warehouse의 쿼리에 대 한 메모리 및 동시성을 관리 하는 방법에 대 한 지침입니다.  

## <a name="what-are-resource-classes"></a>리소스 클래스 정의

쿼리의 성능 용량은 사용자의 리소스 클래스에 의해 결정 됩니다.  리소스 클래스는 쿼리 실행을 위한 계산 리소스와 동시성을 관리하는, Azure SQL Data Warehouse에 사전 정의된 리소스 제한입니다. 리소스 클래스를 통해 동시에 실행 되는 쿼리 수와 각 쿼리에 할당 된 계산 리소스에 대 한 제한을 설정 하 여 쿼리에 대 한 리소스를 구성할 수 있습니다.  메모리와 동시성 사이에는 상충 관계가 있습니다.

- 리소스 클래스가 작을수록 쿼리 당 최대 메모리를 줄이고 동시성을 증가 시킵니다.
- 리소스 클래스가 클수록 쿼리당 최대 메모리가 증가 하지만 동시성이 줄어듭니다.

리소스 클래스에는 다음과 같은 두 가지 유형이 있습니다.

- 고정 된 데이터 집합 크기의 동시성이 증가 하는 데 적합 한 정적 리소스 클래스입니다.
- 동적 리소스 클래스- 크기가 증가하고 서비스 수준의 확장으로 향상된 성능이 필요한 데이터 집합에 적합합니다.

리소스 클래스는 동시성 슬롯을 사용 하 여 리소스 사용량을 측정 합니다.  [동시성 슬롯](#concurrency-slots) 은이 문서의 뒷부분에 설명 되어 있습니다.

- 리소스 클래스에 대 한 리소스 사용률을 보려면 [메모리 및 동시성 제한](memory-concurrency-limits.md)을 참조 하세요.
- 리소스 클래스를 조정 하기 위해 다른 사용자로 쿼리를 실행 하거나 [현재 사용자의 리소스 클래스](#change-a-users-resource-class) 멤버 자격을 변경할 수 있습니다.

### <a name="static-resource-classes"></a>정적 리소스 클래스

정적 리소스 클래스는 [데이터 웨어하우스 단위로](what-is-a-data-warehouse-unit-dwu-cdwu.md)측정 되는 현재 성능 수준에 관계 없이 동일한 양의 메모리를 할당 합니다. 쿼리는 성능 수준에 관계 없이 동일한 메모리 할당을 가져오기 때문에 [데이터 웨어하우스를 확장](quickstart-scale-compute-portal.md) 하면 리소스 클래스 내에서 더 많은 쿼리를 실행할 수 있습니다.  정적 리소스 클래스는 데이터 볼륨이 알려져 있고 상수 인 경우에 적합 합니다.

정적 리소스 클래스는 다음과 같은 미리 정의 된 데이터베이스 역할로 구현 됩니다.

- staticrc10
- staticrc20
- staticrc30
- staticrc40
- staticrc50
- staticrc60
- staticrc70
- staticrc80

### <a name="dynamic-resource-classes"></a>동적 리소스 클래스

동적 리소스 클래스는 현재 서비스 수준에 따라 가변 크기의 메모리를 할당 합니다. 정적 리소스 클래스는 더 높은 동시성 및 정적 데이터 볼륨에 유용 하지만 동적 리소스 클래스는 데이터의 증가 또는 가변 양에 더 적합 합니다.  더 큰 서비스 수준으로 확장 하면 쿼리가 자동으로 더 많은 메모리를 얻게 됩니다.  

동적 리소스 클래스는 다음과 같은 미리 정의 된 데이터베이스 역할로 구현 됩니다.

- smallrc
- mediumrc
- largerc
- xlargerc

각 리소스 클래스에 대 한 메모리 할당은 다음과 같습니다. 

| Szolgáltatásszint  | smallrc           | mediumrc               | largerc                | xlargerc               |
|:--------------:|:-----------------:|:----------------------:|:----------------------:|:----------------------:|
| DW100c         | 25%               | 25%                    | 25%                    | 70%                    |
| DW200c         | 12.5%             | 12.5%                  | 가로                    | 70%                    |
| DW300c         | 8%                | 10%                    | 가로                    | 70%                    |
| DW400c         | 6.25%             | 10%                    | 가로                    | 70%                    |
| DW500c lehetőséget         | 20%               | 10%                    | 가로                    | 70%                    |
| DW1000c<br> DW30000c | 3%       | 10%                    | 가로                    | 70%                    |



### <a name="default-resource-class"></a>기본 리소스 클래스

기본적으로 각 사용자는 동적 리소스 클래스 **smallrc**의 멤버입니다.

서비스 관리자의 리소스 클래스는 smallrc에서 고정 되며 변경할 수 없습니다.  서비스 관리자는 프로 비전 프로세스 중에 생성 된 사용자입니다.  이 컨텍스트의 서비스 관리자는 새 서버를 사용 하 여 새 SQL Data Warehouse 인스턴스를 만들 때 "서버 관리자 로그인"에 대해 지정 된 로그인입니다.

> [!NOTE]
> Active Directory 관리자로 정의 된 사용자 또는 그룹은 서비스 관리자 이기도 합니다.
>
>

## <a name="resource-class-operations"></a>리소스 클래스 작업

리소스 클래스는 데이터 관리 및 조작 작업에 대 한 성능을 향상 시 키도 록 설계 되었습니다. 또한 복잡 한 쿼리는 큰 리소스 클래스에서 실행 되는 이점을 누릴 수 있습니다. 예를 들어, 대량 조인 및 정렬에 대 한 쿼리 성능은 리소스 클래스가 메모리에서 실행 될 수 있을 만큼 충분히 클 때 향상 될 수 있습니다.

### <a name="operations-governed-by-resource-classes"></a>리소스 클래스에 의해 제어 되는 작업

이러한 작업은 리소스 클래스에 의해 제어 됩니다.

- 삽입-선택, 업데이트, 삭제
- SELECT (사용자 테이블을 쿼리 하는 경우)
- ALTER INDEX-다시 작성 또는 다시 구성
- ALTER TABLE REBUILD
- CREATE INDEX
- 클러스터형 COLUMNSTORE 인덱스 만들기
- CTAS (CREATE TABLE AS SELECT)
- Az adatok betöltése
- DMS (데이터 이동 서비스)에서 수행 하는 데이터 이동 작업

> [!NOTE]  
> Dmv (동적 관리 뷰) 또는 다른 시스템 뷰에 대 한 SELECT 문은 동시성 제한의 영향을 받지 않습니다. 실행 되는 쿼리 수에 관계 없이 시스템을 모니터링할 수 있습니다.
>
>

### <a name="operations-not-governed-by-resource-classes"></a>리소스 클래스에 의해 제어 되지 않는 작업

사용자가 더 큰 리소스 클래스의 멤버인 경우에도 일부 쿼리는 항상 smallrc 리소스 클래스에서 실행 됩니다. 이러한 예외 쿼리는 동시성 제한에 계산 되지 않습니다. 예를 들어 동시성 한도가 16 인 경우 사용 가능한 동시성 슬롯에 영향을 주지 않고 많은 사용자가 시스템 뷰에서 선택할 수 있습니다.

다음 문은 리소스 클래스에서 제외 되며 항상 smallrc에서 실행 됩니다.

- 테이블 만들기 또는 삭제
- ALTER TABLE ... 파티션 전환, 분할 또는 병합
- ALTER INDEX DISABLE
- DROP INDEX
- 통계 만들기, 업데이트 또는 삭제
- TRUNCATE TABLE
- ALTER AUTHORIZATION
- CREATE LOGIN
- 사용자 만들기, 변경 또는 삭제
- CREATE, ALTER 또는 DROP PROCEDURE
- 뷰 만들기 또는 삭제
- 값 삽입
- 시스템 뷰 및 Dmv에서 선택
- 하겠습니다
- DBCC

<!--
Removed as these two are not confirmed / supported under SQL DW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="concurrency-slots"></a>동시성 슬롯

동시성 슬롯은 쿼리 실행에 사용할 수 있는 리소스를 추적 하는 편리한 방법입니다. 이는 사용자가 공동으로 제한 되기 때문에 공동으로 사용자의 수를 예약 하기 위해 구입한 티켓과 같습니다. 데이터 웨어하우스 당 총 동시성 슬롯 수는 서비스 수준에 따라 결정 됩니다. 쿼리가 실행을 시작 하려면 충분 한 동시성 슬롯을 예약할 수 있어야 합니다. 쿼리가 완료 되 면 동시성 슬롯을 해제 합니다.  

- 10 개의 동시성 슬롯을 사용 하 여 실행 되는 쿼리는 두 개의 동시성 슬롯으로 실행 되는 쿼리 보다 5 배 더 많은 계산 리소스에 액세스할 수 있습니다.
- 각 쿼리에 10 개의 동시성 슬롯이 필요 하 고 40 개의 동시성 슬롯이 있는 경우 4 개의 쿼리만 동시에 실행 될 수 있습니다.

리소스 제어 쿼리만 동시성 슬롯을 사용 합니다. 시스템 쿼리와 일부 trivial 쿼리는 슬롯을 사용 하지 않습니다. 사용 되는 동시성 슬롯의 정확한 수는 쿼리의 리소스 클래스에 의해 결정 됩니다.

## <a name="view-the-resource-classes"></a>리소스 클래스 보기

리소스 클래스는 미리 정의 된 데이터베이스 역할로 구현 됩니다. 리소스 클래스에는 동적 및 정적 이라는 두 가지 유형이 있습니다. 리소스 클래스 목록을 보려면 다음 쿼리를 사용 합니다.

```sql
SELECT name
FROM   sys.database_principals
WHERE  name LIKE '%rc%' AND type_desc = 'DATABASE_ROLE';
```

## <a name="change-a-users-resource-class"></a>사용자의 리소스 클래스 변경

리소스 클래스는 데이터베이스 역할에 사용자를 할당 하 여 구현 됩니다. 사용자가 쿼리를 실행 하면 쿼리는 사용자의 리소스 클래스를 사용 하 여 실행 됩니다. 예를 들어 사용자가 staticrc10 데이터베이스 역할의 멤버인 경우 해당 쿼리는 적은 양의 메모리를 사용하여 실행됩니다. 데이터베이스 사용자가 xlargerc 또는 staticrc80 데이터베이스 역할의 멤버인 경우 해당 쿼리는 많은 양의 메모리를 사용하여 실행됩니다.

사용자의 리소스 클래스를 높이려면 [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql)를 사용하여 큰 리소스 클래스의 데이터베이스 역할에 사용자를 추가합니다.  아래 코드는 largerc 데이터베이스 역할에 사용자를 추가합니다.  각 요청은 시스템 메모리의 22%를 가져옵니다.

```sql
EXEC sp_addrolemember 'largerc', 'loaduser';
```

리소스 클래스를 줄이려면 [sp_droprolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql)를 사용 합니다.  'Loaduser'가 다른 리소스 클래스 또는 구성원이 아닌 경우 3% 메모리가 부여되는 기본 smallrc 리소스 클래스를 이동합니다.  

```sql
EXEC sp_droprolemember 'largerc', 'loaduser';
```

## <a name="resource-class-precedence"></a>리소스 클래스 우선 순위

사용자는 여러 리소스 클래스의 멤버일 수 있습니다. 사용자가 둘 이상의 리소스 클래스에 속하는 경우:

- 동적 리소스 클래스는 정적 리소스 클래스 보다 우선적으로 적용 됩니다. 예를 들어 사용자가 mediumrc (동적) 및 staticrc80 (정적) 둘 다의 멤버인 경우 쿼리는 mediumrc를 사용 하 여 실행 됩니다.
- 더 큰 리소스 클래스가 더 작은 리소스 클래스 보다 우선적으로 적용 됩니다. 예를 들어 사용자가 mediumrc 및 largerc의 멤버인 경우 쿼리는 largerc를 사용 하 여 실행 됩니다. 마찬가지로 사용자가 staticrc20 및 statirc80 둘 다의 멤버인 경우 쿼리는 staticrc80 리소스 할당을 사용 하 여 실행 됩니다.

## <a name="recommendations"></a>Ajánlatok

>[!NOTE]
>워크 로드 및 예측 가능한 성능을 보다 효과적으로 제어 하기 위해 워크 로드 관리 기능 ([워크 로드 격리](sql-data-warehouse-workload-isolation.md), [분류](sql-data-warehouse-workload-classification.md) 및 [중요도](sql-data-warehouse-workload-importance.md))을 활용 하는 것이 좋습니다.  
>
>

로드 작업 또는 특정 유형의 쿼리를 실행하는 전용 사용자를 만드는 것이 좋습니다. 리소스 클래스를 자주 변경하는 대신 해당 사용자에게 영구 리소스 클래스를 제공합니다. 정적 리소스 클래스를 사용하면 워크로드를 전반적으로 더 잘 제어할 수 있으므로 동적 리소스 클래스를 고려하기 전에 정적 리소스 클래스를 사용하는 것이 좋습니다.

### <a name="resource-classes-for-load-users"></a>부하 사용자에 대 한 리소스 클래스

`CREATE TABLE`는 기본적으로 클러스터형 columnstore 인덱스를 사용 합니다. Columnstore 인덱스로 데이터를 압축 하는 작업은 메모리를 많이 사용 하는 작업이 며 메모리 압력으로 인덱스 품질을 낮출 수 있습니다. 메모리 압력은 데이터 로드 시 더 높은 리소스 클래스가 필요하도록 할 수 있습니다. 로드에 충분 한 메모리가 있는지 확인 하려면 로드를 실행 하기 위해 지정 된 사용자를 만들고 해당 사용자를 더 높은 리소스 클래스에 할당 하면 됩니다.

로드를 효율적으로 처리 하는 데 필요한 메모리는 로드 된 테이블의 특성과 데이터 크기에 따라 달라 집니다. 메모리 요구 사항에 대 한 자세한 내용은 [행 그룹 품질 최대화](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md)를 참조 하세요.

메모리 요구 사항을 확인 한 후에는 정적 또는 동적 리소스 클래스에 로드 사용자를 할당할지 여부를 선택 합니다.

- 테이블 메모리 요구 사항이 특정 범위에 속하는 경우 정적 리소스 클래스를 사용 합니다. 적절 한 메모리를 사용 하 여 로드를 실행 합니다. 데이터 웨어하우스의 크기를 조정할 때 로드에 메모리가 더 필요 하지 않습니다. 정적 리소스 클래스를 사용 하 여 메모리 할당은 일정 하 게 유지 됩니다. 이 일관성은 메모리를 절약 하 고 더 많은 쿼리를 동시에 실행할 수 있습니다. 새 솔루션은 더 많은 제어를 제공 하므로 먼저 정적 리소스 클래스를 사용 하는 것이 좋습니다.
- 테이블 메모리 요구 사항이 매우 다양 한 경우 동적 리소스 클래스를 사용 합니다. 로드 하려면 현재 DWU 또는 cDWU 수준에서 제공 하는 것 보다 많은 메모리가 필요할 수 있습니다. 데이터 웨어하우스를 확장하면 로드 작업에 메모리를 더 추가해 로드를 더 빠르게 수행할 수 있습니다.

### <a name="resource-classes-for-queries"></a>쿼리에 대 한 리소스 클래스

일부 쿼리는 계산 집약적이고 일부는 그렇지 않습니다.  

- 쿼리는 복잡하지만 높은 동시성을 요구하지 않는 경우 동적 리소스 클래스를 선택합니다.  예를 들어 매일 또는 매주 보고서를 생성하는 경우 리소스는 가끔 필요합니다. 보고서가 많은 양의 데이터를 처리하는 경우 데이터 웨어하우스를 확장하면 사용자의 기존 리소스 클래스에 더 많은 메모리를 제공할 수 있습니다.
- 하루 종일 리소스 기대치가 다를 경우 정적 리소스 클래스를 선택 합니다. 예를 들어 정적 리소스 클래스는 여러 사용자가 데이터 웨어하우스를 쿼리할 때 잘 작동 합니다. 데이터 웨어하우스의 크기를 조정 하는 경우 사용자에 게 할당 된 메모리 양이 변경 되지 않습니다. 따라서 시스템에서 더 많은 쿼리를 병렬로 실행할 수 있습니다.

적절한 메모리 부여는 쿼리되는 데이터의 양, 테이블 스키마의 특성 및 다양한 조인, 선택 및 그룹 조건자 등의 많은 요인에 따라 달라집니다. 일반적으로 더 많은 메모리를 할당 하면 쿼리를 더 빠르게 완료할 수 있지만 전반적인 동시성이 줄어듭니다. 동시성이 문제가 되지 않는 경우 메모리를 과도하게 할당해도 처리량에는 문제가 없습니다.

성능을 튜닝 하려면 다른 리소스 클래스를 사용 합니다. 다음 섹션에서는 최상의 리소스 클래스를 파악 하는 데 도움이 되는 저장 프로시저를 제공 합니다.

## <a name="example-code-for-finding-the-best-resource-class"></a>최상의 리소스 클래스를 찾기 위한 예제 코드

다음 지정 된 저장 프로시저를 사용 하 여 지정 된 SLO에서 리소스 클래스 당 동시성 및 메모리 부여를 파악 하 고 지정 된 리소스 클래스의 분할 되지 않은 CCI 테이블에 대해 메모리 집약적 CCI 작업을 위한 최상의 리소스 클래스를 확인할 수 있습니다.

이 저장 프로시저의 용도는 다음과 같습니다.

1. 지정 된 SLO에서 리소스 클래스 별로 동시성 및 메모리 부여를 확인 합니다. 사용자는이 예제에 표시 된 것 처럼 스키마와 tablename 모두에 대해 NULL을 제공 해야 합니다.  
2. 지정된 리소스 클래스에서 분할되지 않은 CCI 테이블의 메모리 집약적 CCI 작업(로드, 테이블 복사, 인덱스 다시 작성 등)에 대한 최상의 리소스 클래스를 확인합니다. 저장 프로시저는 테이블 스키마를 사용 하 여 필요한 메모리 부여를 확인 합니다.

### <a name="dependencies--restrictions"></a>종속성 & 제한 사항

- 이 저장 프로시저는 분할된 cci 테이블의 메모리 요구 사항을 계산하도록 디자인되지 않았습니다.
- 이 저장 프로시저는 CTAS/INSERT-SELECT의 SELECT 부분에 대한 메모리 요구사항을 고려하지 않으며 SELECT로 가정합니다.
- 이 저장 프로시저는이 저장 프로시저가 만들어진 세션에서 사용할 수 있는 임시 테이블을 사용 합니다.
- 이 저장 프로시저는 현재 제공(예를 들어, 하드웨어 구성, DMS 구성)에 따라 다르며 변경될 경우 이 저장된 프로시저는 올바르게 작동하지 않습니다.  
- 이 저장 프로시저는 기존 동시성 한도 기능에 따라 다르며 변경되는 경우 이 저장 프로시저는 올바르게 작동하지 않습니다.  
- 이 저장 프로시저는 기존 리소스 클래스 제공에 따라 다르며 변경되는 경우 이 저장 프로시저는 올바르게 작동하지 않습니다.  

>[!NOTE]  
>제공 된 매개 변수를 사용 하 여 저장 프로시저를 실행 한 후 출력을 가져오지 않는 경우 두 가지 사례가 있을 수 있습니다.
>
>1. DW 매개 변수 중 하나에 잘못 된 SLO 값이 포함 되어 있습니다.
>2. 또는 테이블에 대 한 CCI 작업에 일치 하는 리소스 클래스가 없습니다.
>
>예를 들어 DW100c에서 사용할 수 있는 가장 높은 메모리 부여는 1gb이 고, 테이블 스키마의 너비는 1gb의 요구 사항을 충족할 만큼 충분 한 경우입니다.

### <a name="usage-example"></a>사용 예

Szintaxis:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`
  
1. @DWU: NULL 매개 변수를 제공 하 여 DW DB에서 현재 DWU를 추출 하거나 ' DW100c ' 형식으로 지원 되는 DWU를 제공 합니다.
2. 테이블의 스키마 이름을 제공 @SCHEMA_NAME:
3. 관심 있는 테이블 이름을 제공 @TABLE_NAME:

이 저장 프로시저를 실행 하는 예제:

```sql
EXEC dbo.prc_workload_management_by_DWU 'DW2000c', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000c', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

다음 문은 앞의 예제에서 사용 된 Table1을 만듭니다.
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

### <a name="stored-procedure-definition"></a>저장 프로시저 정의

```sql
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(@DWU VARCHAR(7),
 @SCHEMA_NAME VARCHAR(128),
 @TABLE_NAME VARCHAR(128)
)
AS

IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.

SELECT @DWU = 'DW'+ CAST(CASE WHEN Mem> 4 THEN Nodes*500 
  ELSE Mem*100 
  END AS VARCHAR(10)) +'c'
    FROM (
      SELECT Nodes=count(distinct n.pdw_node_id), Mem=max(i.committed_target_kb/1000/1000/60)
        FROM sys.dm_pdw_nodes n
        CROSS APPLY sys.dm_pdw_nodes_os_sys_info i
        WHERE type = 'COMPUTE')A
END

-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END;

-- Creating ref. temp table (CTAS) to hold mapping info.
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
SELECT 'DW100c' AS DWU,4 AS max_queries,4 AS max_slots,1 AS slots_used_smallrc,1 AS slots_used_mediumrc,2 AS slots_used_largerc,4 AS slots_used_xlargerc,1 AS slots_used_staticrc10,2 AS slots_used_staticrc20,4 AS slots_used_staticrc30,4 AS slots_used_staticrc40,4 AS slots_used_staticrc50,4 AS slots_used_staticrc60,4 AS slots_used_staticrc70,4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200c', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300c', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400c', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW500c', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000c', 32, 40, 1, 4, 8, 28, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500c', 32, 60, 1, 6, 13, 42, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000c', 48, 80, 2, 8, 17, 56, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW2500c', 48, 100, 3, 10, 22, 70, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW3000c', 64, 120, 3, 12, 26, 84, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW5000c', 64, 200, 6, 20, 44, 140, 1, 2, 4, 8, 16, 32, 64, 128
  UNION ALL
    SELECT 'DW6000c', 128, 240, 7, 24, 52, 168, 1, 2, 4, 8, 16, 32, 64, 128
  UNION ALL
    SELECT 'DW7500c', 128, 300, 9, 30, 66, 210, 1, 2, 4, 8, 16, 32, 64, 128
  UNION ALL
    SELECT 'DW10000c', 128, 400, 12, 40, 88, 280, 1, 2, 4, 8, 16, 32, 64, 128
  UNION ALL
    SELECT 'DW15000c', 128, 600, 18, 60, 132, 420, 1, 2, 4, 8, 16, 32, 64, 128
  UNION ALL
    SELECT 'DW30000c', 128, 1200, 36, 120, 264, 840, 1, 2, 4, 8, 16, 32, 64, 128 
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map  
AS
(
  SELECT CONVERT(varchar(20), 'SloDWGroupSmall') AS wg_name, slots_used_smallrc AS slots_used FROM alloc WHERE DWU = @DWU
UNION ALL
  SELECT CONVERT(varchar(20), 'SloDWGroupMedium') AS wg_name, slots_used_mediumrc AS slots_used FROM alloc WHERE DWU = @DWU
UNION ALL
  SELECT CONVERT(varchar(20), 'SloDWGroupLarge') AS wg_name, slots_used_largerc AS slots_used FROM alloc WHERE DWU = @DWU
UNION ALL
  SELECT CONVERT(varchar(20), 'SloDWGroupXLarge') AS wg_name, slots_used_xlargerc AS slots_used FROM alloc WHERE DWU = @DWU
  UNION ALL
  SELECT 'SloDWGroupC00',1
  UNION ALL
    SELECT 'SloDWGroupC01',2
  UNION ALL
    SELECT 'SloDWGroupC02',4
  UNION ALL
    SELECT 'SloDWGroupC03',8
  UNION ALL
    SELECT 'SloDWGroupC04',16
  UNION ALL
    SELECT 'SloDWGroupC05',32
  UNION ALL
    SELECT 'SloDWGroupC06',64
  UNION ALL
    SELECT 'SloDWGroupC07',128
)

-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.slots_used * 250 AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.slots_used * 250 AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.slots_used * 250 AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.slots_used * 250 AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.slots_used * 250 AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.slots_used * 250 AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.slots_used * 250 AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.slots_used * 250 AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.slots_used * 250 AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.slots_used * 250 AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.slots_used * 250 AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.slots_used * 250 AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used and m1.wg_name = 'SloDWGroupSmall'
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used and m2.wg_name = 'SloDWGroupMedium'
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used and m3.wg_name = 'SloDWGroupLarge'
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used and m4.wg_name = 'SloDWGroupXLarge'
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used and m5.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used and m6.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used and m7.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used and m8.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used and m9.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used and m10.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used and m11.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used and m12.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  WHERE   a1.DWU = @DWU
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;

-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239)
                                AND  co.max_length <= 32
                                THEN COUNT(co.column_id)
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239)
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id)
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as
(
SELECT  CASE
          WHEN FLOOR(8 * (CAST (CAST(REPLACE(REPLACE(@DWU,'DW',''),'c','') AS INT) AS FLOAT)/6000)) > 0
            AND CHARINDEX(@DWU,'c')=0
          THEN FLOOR(8 * (CAST (CAST(REPLACE(REPLACE(@DWU,'DW',''),'c','') AS INT) AS FLOAT)/6000))
          ELSE 1
        END AS multiplication_factor
)
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB
       FROM    size
       , load_multiplier
       , #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="next-steps"></a>Következő lépések

데이터베이스 사용자 및 보안을 관리 하는 방법에 대 한 자세한 내용은 [SQL Data Warehouse에서 데이터베이스 보안][Secure a database in SQL Data Warehouse]을 참조 하세요. 더 큰 리소스 클래스가 클러스터형 columnstore 인덱스 품질을 향상 시킬 수 있는 방법에 대 한 자세한 내용은 [columnstore 압축을 위한 메모리 최적화](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md)를 참조 하세요.

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Rebuilding indexes to improve segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
