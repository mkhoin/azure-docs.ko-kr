---
title: Azure Cosmos DB 쿼리 언어 낮음
description: 대문자 데이터를 소문자로 변환한 후 문자열 식을 반환 하는 Azure Cosmos DB의 낮은 SQL 시스템 함수에 대해 알아봅니다.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8f7cd864d90b8ab17a180a00a7a6d4bb683da9be
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873304"
---
# <a name="lower-azure-cosmos-db"></a>LOWER (Azure Cosmos DB)
 대문자 데이터를 소문자로 변환한 후에 문자열 식을 반환합니다.  
  
## <a name="syntax"></a>구문
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>인수
  
*str_expr*  
   는 문자열 식입니다.  
  
## <a name="return-types"></a>반환 유형
  
  문자열 식을 반환합니다.  
  
## <a name="examples"></a>예시
  
  다음 예에서는 쿼리에서 `LOWER`를 사용 하는 방법을 보여 줍니다.  
  
```sql
SELECT LOWER("Abc") AS lower
```  
  
 결과 집합은 다음과 같습니다.  
  
```json
[{"lower": "abc"}]  
  
```  

## <a name="next-steps"></a>다음 단계

- [문자열 함수 Azure Cosmos DB](sql-query-string-functions.md)
- [시스템 함수 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 소개](introduction.md)
