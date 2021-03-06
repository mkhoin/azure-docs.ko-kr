---
title: '다중 클래스 승격 된 의사 결정 트리: 모듈 참조'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning에서 다중 클래스 승격 된 의사 결정 트리 모듈을 사용 하 여 레이블이 지정 된 데이터를 사용 하는 분류자를 만드는 방법에 대해 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 11/19/2019
ms.openlocfilehash: 7f39d393b96b1515e4815abdc28ac4079f271c1b
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74232597"
---
# <a name="multiclass-boosted-decision-tree"></a>다중 클래스 향상된 의사 결정 트리

이 문서에서는 Azure Machine Learning designer (미리 보기)의 모듈을 설명 합니다.

이 모듈을 사용 하 여 승격 된 의사 결정 트리 알고리즘을 기반으로 기계 학습 모델을 만들 수 있습니다.

승격 된 의사 결정 트리는 두 번째 트리가 첫 번째 트리의 오류를 수정 하 고, 세 번째 트리가 첫 번째 및 두 번째 트리의 오류를 수정 하는 등의 앙상블 학습 방법입니다. 예측은 트리의 앙상블을 기반으로 합니다.

## <a name="how-to-configure"></a>구성 방법 

이 모듈은 학습 되지 않은 분류 모델을 만듭니다. 분류는 감독 된 학습 방법 이므로 모든 행에 대 한 값이 있는 레이블 열을 포함 하는 *레이블이 지정 된 데이터 집합이* 필요 합니다.

[학습 모델](././train-model.md)을 사용 하 여 이러한 유형의 모델을 학습할 수 있습니다. 

1.  파이프라인에 **다중 클래스 승격 된 의사 결정 트리** 모듈을 추가 합니다.

1.  **강사 모드 만들기** 옵션을 설정 하 여 모델을 학습 하는 방법을 지정 합니다.

    + **단일 매개 변수**: 모델을 구성 하는 방법을 아는 경우 특정 값 집합을 인수로 제공할 수 있습니다.


    *  **트리 당 리프의 최대 수** 는 모든 트리에서 만들 수 있는 최대 터미널 노드 (리프) 수를 제한 합니다.
    
        이 값을 늘리면 잠재적으로 과잉 맞춤 되 고 학습 시간이 길어질 때 트리의 크기가 증가 하 고 더 높은 정밀도를 달성할 수 있습니다.
  
    * **리프 노드당 최소 샘플 수** 는 트리에서 터미널 노드 (리프)를 만드는 데 필요한 사례 수를 나타냅니다.  

         이 값을 늘리면 새 규칙을 만들기 위한 임계값이 증가 합니다. 예를 들어 기본값 1을 사용 하는 경우에도 단일 case를 사용 하면 새 규칙을 만들 수 있습니다. 값을 5로 늘리면 학습 데이터에 동일한 조건을 충족 하는 5 개 이상의 사례가 포함 되어야 합니다.

    * 학습 **률** 은 학습 하는 동안 단계 크기를 정의 합니다. 0에서 1 사이의 숫자를 입력 하십시오.

         학습 속도는 최적의 솔루션에 대 한 학습자 수렴의 속도 또는 속도를 결정 합니다. 단계 크기가 너무 크면 최적의 솔루션을 과도 하 게 사용할 수 있습니다. 단계 크기가 너무 작은 경우 학습은 최상의 솔루션에서 수렴 하는 데 더 오래 걸립니다.

    * **생성 된 트리 수** 앙상블에서 만들 의사 결정 트리의 총 수를 나타냅니다. 더 많은 의사 결정 트리를 만들어 잠재적으로 더 나은 검사를 얻을 수 있지만 학습 시간이 늘어납니다.

    *  **난수 초기값** 은 임의의 초기값으로 사용할 음수가 아닌 정수를 선택적으로 설정 합니다. 초기값을 지정 하면 동일한 데이터 및 매개 변수가 있는 실행에 대해 재현 가능성 됩니다.  

         임의 초기값은 기본적으로 42로 설정 됩니다. 다른 임의 초기값을 사용 하 여 연속 실행을 실행 하면 결과가 달라질 수 있습니다.

> [!Note]
> 담당자 **모드 만들기** 를 **단일 매개 변수로**설정한 경우 태그가 지정 된 데이터 집합 및 [모델 학습](./train-model.md) 모듈을 연결 합니다.

## <a name="next-steps"></a>다음 단계

Azure Machine Learning [사용할 수 있는 모듈 집합](module-reference.md) 을 참조 하세요. 
