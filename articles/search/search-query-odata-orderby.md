---
title: OData order by 참조
titleSuffix: Azure Cognitive Search
description: Azure Cognitive Search 쿼리에서 order by를 사용 하는 방법에 대 한 구문 및 언어 참조 설명서입니다.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 99ec639b88f3334530243242aadfa0ab52a40df0
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113155"
---
# <a name="odata-orderby-syntax-in-azure-cognitive-search"></a>Azure Cognitive Search의 OData $orderby 구문

 [OData **$orderby** 매개 변수](query-odata-filter-orderby-syntax.md) 를 사용 하 여 Azure Cognitive Search에서 검색 결과에 대 한 사용자 지정 정렬 순서를 적용할 수 있습니다. 이 문서에서는 **$orderby** 구문에 대해 자세히 설명 합니다. 검색 결과를 표시할 때 **$orderby** 를 사용 하는 방법에 대 한 일반적인 정보는 [Azure Cognitive Search에서 검색 결과](search-pagination-page-layout.md)를 사용 하는 방법을 참조 하세요.

## <a name="syntax"></a>구문

**$Orderby** 매개 변수는 최대 32 개의 **order by 절**에 대 한 쉼표로 구분 된 목록을 허용 합니다. Order by 절의 구문은 다음 EBNF ([Extended Backus-Backus-naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form))에서 설명 합니다.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'
```

대화형 구문 다이어그램도 사용할 수 있습니다.

> [!div class="nextstepaction"]
> [Azure Cognitive Search에 대 한 OData 구문 다이어그램](https://azuresearch.github.io/odata-syntax-diagram/#order_by_clause)

> [!NOTE]
> 전체 EBNF [Azure Cognitive Search에 대 한 OData 식 구문 참조](search-query-odata-syntax-reference.md) 를 참조 하세요.

각 절에는 정렬 기준이 있으며, 필요에 따라 정렬 방향 (오름차순 또는 내림차순 `desc``asc`)이 있습니다. 방향을 지정 하지 않는 경우 기본값은 오름차순입니다. 정렬 기준은 `sortable` 필드의 경로 이거나 [`geo.distance`](search-query-odata-geo-spatial-functions.md) 또는 [`search.score`](search-query-odata-search-score-function.md) 함수를 호출할 수 있습니다.

여러 문서에 동일한 정렬 기준이 있고 `search.score` 함수를 사용 하지 않는 경우 (예: 숫자 `Rating` 필드를 기준으로 정렬 하는 경우와 4 개의 문서 등급을 모두 4로 정렬 하는 경우) 타이는 문서 점수를 기준으로 내림차순으로 나뉩니다. 문서 점수가 동일한 경우 (예: 요청에 전체 텍스트 검색 쿼리가 지정 되지 않은 경우) 연결 된 문서의 상대 순서는 결정 되지 않습니다.

여러 정렬 기준을 지정할 수 있습니다. 식의 순서는 최종 정렬 순서를 결정합니다. 예를 들어 점수를 기준으로 내림차순으로 정렬 하 고 그 다음에 등급을 정렬 하려면 구문을 `$orderby=search.score() desc,Rating desc`합니다.

`geo.distance`$orderby**에서** 에 대한 구문은 **$filter**에 있을 때와 같습니다. `geo.distance`$orderby**에서** 를 사용하는 경우 적용되는 필드는 `Edm.GeographyPoint` 형식이어야 하고 `sortable`이어야 합니다.

`search.score`$orderby**에서** 에 대한 구문은 `search.score()`입니다. `search.score` 함수는 매개 변수를 사용 하지 않습니다.

## <a name="examples"></a>예

호텔을 기본 요금의 오름차순으로 정렬합니다.

    $orderby=BaseRate asc

호텔을 등급의 내림차순으로 정렬한 후 기본 요금의 오름차순으로 정렬합니다(오름차순이 기본값임).

    $orderby=Rating desc,BaseRate

지정 된 좌표의 거리를 기준으로 내림차순으로 호텔을 정렬 합니다.

    $orderby=Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

검색을 기준으로 호텔을 내림차순으로 정렬 한 다음, 지정 된 좌표의 거리를 기준으로 오름차순으로 정렬 합니다. 관련성 점수와 등급이 동일한 두 호텔 사이에 가장 가까운 것이 먼저 나열 됩니다.

    $orderby=search.score() desc,Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>다음 단계  

- [Azure Cognitive Search에서 검색 결과를 사용 하는 방법](search-pagination-page-layout.md)
- [Azure Cognitive Search에 대 한 OData 식 언어 개요](query-odata-filter-orderby-syntax.md)
- [Azure Cognitive Search에 대 한 OData 식 구문 참조](search-query-odata-syntax-reference.md)
- [문서 &#40;검색 Azure Cognitive Search REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
