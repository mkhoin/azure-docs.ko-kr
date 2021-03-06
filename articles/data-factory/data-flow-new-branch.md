---
title: 데이터 흐름 새 분기 변환 매핑
description: 데이터 흐름 새 분기 변환 Azure Data Factory 매핑
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 02/12/2019
ms.openlocfilehash: b4617689fe1ab14856bde9a4e8134b12aa6d815b
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930309"
---
# <a name="azure-data-factory-mapping-data-flow-new-branch-transformation"></a>데이터 흐름 새 분기 변환 Azure Data Factory 매핑

![분기 옵션](media/data-flow/menu.png "메뉴")

분기는 데이터 흐름의 현재 데이터 스트림을 사용하고 다른 스트림에 복제합니다. 새 분기를 사용하여 동일한 데이터 스트림에 대해 여러 개의 작업 및 변환 세트를 수행할 수 있습니다.

예: 데이터 흐름에는 선택한 열 집합과 데이터 형식 변환의 원본 변환이 있습니다. 해당 원본 바로 뒤에 파생 열을 배치합니다. 파생 열에서 이름과 성을 결합하여 새로운 “전체 이름” 필드를 만드는 새 필드를 만들었습니다.

한 행에서 변환 세트와 싱크를 사용하여 새 스트림을 처리하고, 새 분기를 사용하여 해당 스트림의 복사본을 만든 다음, 다른 변환 세트로 동일한 데이터를 변환할 수 있습니다. 복사된 데이터를 별도의 분기로 변환하면 이후에 해당 데이터를 별도의 위치로 싱크할 수 있습니다.

> [!NOTE]
> “새 분기”는 분기하려는 현재 위치 뒤에 후속 변환이 있는 경우에만 + 변환 메뉴에 동작으로 표시됩니다. 즉, 선택 후 다른 변환을 추가할 때까지는 “새 분기” 옵션이 끝에 표시되지 않습니다.

![분기](media/data-flow/branch2.png "분기 2")
