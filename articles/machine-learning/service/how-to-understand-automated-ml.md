---
title: 자동화 된 ML 결과 이해
titleSuffix: Azure Machine Learning
description: 자동화 된 각 기계 학습 실행에 대 한 차트 및 메트릭을 보고 이해 하는 방법에 대해 알아봅니다.
services: machine-learning
author: RachelKellam
ms.author: rakellam
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: 81f17de7627658b756edd19438a80fb32add859d
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74974090"
---
# <a name="understand-automated-machine-learning-results"></a>자동화 된 machine learning 결과 이해
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

이 문서에서는 자동화 된 각 기계 학습 실행에 대 한 차트 및 메트릭을 보고 이해 하는 방법에 대해 알아봅니다. 

További információk:
+ [분류 모델에 대 한 메트릭, 차트 및 곡선](#classification)
+ [회귀 모델에 대 한 메트릭, 차트 및 그래프](#regression)
+ [모델 interpretability 및 기능 중요도](#explain-model)

## <a name="prerequisites"></a>Előfeltételek

* Azure-előfizetés. Azure 구독이 없는 경우 시작 하기 전에 무료 계정을 만듭니다. 현재 [Azure Machine Learning의 무료 또는 유료 버전](https://aka.ms/AMLFree) 을 사용해 보세요.

* SDK 또는 Azure Machine Learning studio에서 자동화 된 machine learning 실행에 대 한 실험을 만듭니다.

    * SDK를 사용 하 여 [분류 모델](how-to-auto-train-remote.md) 또는 [회귀 모델](tutorial-auto-train-models.md) 작성
    * [Azure Machine Learning studio](how-to-create-portal-experiments.md) 를 사용 하 여 적절 한 데이터를 업로드 하 여 분류 또는 회귀 모델을 만듭니다.

## <a name="view-the-run"></a>실행 보기

자동화 된 기계 학습 실험을 실행 한 후에는 machine learning 작업 영역에서 실행 기록을 찾을 수 있습니다. 

1. Lépjen a munkaterülethez.

1. 작업 영역의 왼쪽 패널에서 **실험**을 선택 합니다.

   ![실험 메뉴 스크린샷](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-menu.png)

1. 실험 목록에서 탐색 하려는 실험을 선택 합니다.

   [![실험 목록](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list-expanded.png)

1. 아래쪽 테이블에서 **실행**을 선택 합니다.

   [![실험 실행](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run-expanded.png))

1. 모델에서 더 탐색 하려는 모델의 **알고리즘 이름을** 선택 합니다.

   [![실험 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model-expanded.png)

`RunDetails`[Jupyter 위젯을](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)사용할 때 실행 중에도 동일한 결과가 표시 됩니다.

## <a name="classification"></a>분류 결과

다음 메트릭과 차트는의 자동화 된 기계 학습 기능을 사용 하 여 작성 하는 모든 분류 모델에 사용할 수 있습니다 Azure Machine Learning

+ [Metrikák](#classification-metrics)
+ [혼동 행렬](#confusion-matrix)
+ [전체 자릿수-회수 차트](#precision-recall-chart)
+ [받는 사람 운영 특성 (또는 ROC)](#roc)
+ [리프트 곡선](#lift-curve)
+ [곡선 향상](#gains-curve)
+ [보정 그림](#calibration-plot)

### <a name="classification-metrics"></a>분류 메트릭

다음 메트릭은 분류 태스크에 대 한 각 실행 반복에 저장 됩니다.

|Metrika|Leírás|Számítás|추가 매개 변수
--|--|--|--|
AUC_Macro| CC는 받는 사람 운영 특성 곡선의 면적입니다. 매크로는 각 클래스에 대 한 자체 c의 산술 평균입니다.  | [변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average = "매크로"|
AUC_Micro| CC는 받는 사람 운영 특성 곡선의 면적입니다. 마이크로는 각 클래스에서 참 긍정 및 거짓 긍정을 결합 하 여 전역적으로 계산 됩니다.| [변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average = "마이크로"|
AUC_Weighted  | CC는 받는 사람 운영 특성 곡선의 면적입니다. 가중치는 각 클래스에 대 한 점수의 산술 평균 이며 각 클래스의 실제 인스턴스 수에 의해 가중치가 적용 됩니다.| [변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|average = "가중치가 적용"
accuracy|정확도는 실제 레이블과 정확히 일치 하는 예측 레이블의 백분율입니다. |[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |None|
average_precision_score_macro|평균 전체 자릿수는 각 임계값에서 달성 된 전체 자릿수의 가중치가 적용 된 평균으로 전체 자릿수 리콜 곡선을 요약 하 고, 가중치로 사용 된 이전 임계값의 회수를 증가 시킵니다. 매크로는 각 클래스의 평균 전체 자릿수 점수에 대 한 산술 평균입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average = "매크로"|
average_precision_score_micro|평균 전체 자릿수는 각 임계값에서 달성 된 전체 자릿수의 가중치가 적용 된 평균으로 전체 자릿수 리콜 곡선을 요약 하 고, 가중치로 사용 된 이전 임계값의 회수를 증가 시킵니다. 마이크로는 각 구분에서 참 긍정 및 거짓 긍정을 결합 하 여 전역적으로 계산 됩니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average = "마이크로"|
average_precision_score_weighted|평균 전체 자릿수는 각 임계값에서 달성 된 전체 자릿수의 가중치가 적용 된 평균으로 전체 자릿수 리콜 곡선을 요약 하 고, 가중치로 사용 된 이전 임계값의 회수를 증가 시킵니다. 가중치는 각 클래스에 대 한 평균 전체 자릿수 점수의 산술 평균 이며 각 클래스의 실제 인스턴스 수에 의해 가중치가 적용 됩니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average = "가중치가 적용"|
balanced_accuracy|균형 있는 정확도는 각 클래스에 대 한 정확한 회수의 산술 평균입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average = "매크로"|
f1_score_macro|F1 점수는 전체 자릿수 및 회수의 조화 평균입니다. 매크로는 각 클래스에 대 한 F1 점수의 산술 평균입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average = "매크로"|
f1_score_micro|F1 점수는 전체 자릿수 및 회수의 조화 평균입니다. 마이크로는 전체 참 긍정, 거짓 부정 및 거짓 긍정을 계산 하 여 전역적으로 계산 됩니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average = "마이크로"|
f1_score_weighted|F1 점수는 전체 자릿수 및 회수의 조화 평균입니다. 각 클래스에 대 한 F1 점수의 클래스 빈도로 가중치 평균|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average = "가중치가 적용"|
log_loss|이 함수는 확률 분류자의 예측이 지정 된 경우 진정한 레이블의 부정적 로그 가능성으로 정의 된 신경망 등의 다항 () 로지스틱 회귀 및 확장에서 사용 되는 손실 함수입니다. {0,1}에서 true 레이블 yt를 사용 하는 단일 샘플 및 yt = 1 인 예상 확률 yt의 경우 로그 손실은-log P (yt&#124;yt) =-(yt 로그 (yt) + (1-yt) 로그 (1-yt)입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|None|
norm_macro_recall|정규화 된 매크로 회수는 임의의 성능 점수가 0이 고 완벽 한 성능의 점수가 1이 되도록 정규화 된 매크로 회수입니다. 이는 norm_macro_recall: = (recall_score_macro-R)/(1-R)로 구현 됩니다. 여기서 R은 임의 예측에 대해 예상 되는 recall_score_macro 값 (예: 이진 분류의 경우 R = 0.5, C 클래스 분류 문제의 경우 R = (1/C))입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average = "매크로" |
precision_score_macro|전체 자릿수는 레이블이 올바르게 지정 된 예측 요소의 백분율입니다. 매크로는 각 클래스에 대 한 전체 자릿수의 산술 평균입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average = "매크로"|
precision_score_micro|전체 자릿수는 레이블이 올바르게 지정 된 예측 요소의 백분율입니다. 마이크로는 총 참 긍정 및 거짓 긍정을 계산 하 여 전역적으로 계산 됩니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average = "마이크로"|
precision_score_weighted|전체 자릿수는 레이블이 올바르게 지정 된 예측 요소의 백분율입니다. 가중치가 적용 되는 각 클래스에 대 한 전체 자릿수의 산술 평균은 각 클래스의 실제 인스턴스 수로 가중치를 지정 합니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average = "가중치가 적용"|
recall_score_macro|회수는 특정 클래스의 요소에 레이블이 올바르게 지정 된 비율입니다. 매크로는 각 클래스에 대 한 회수의 산술 평균입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average = "매크로"|
recall_score_micro|회수는 특정 클래스의 요소에 레이블이 올바르게 지정 된 비율입니다. 마이크로는 총 참 긍정, 거짓 부정 및 거짓 긍정을 계산 하 여 전역적으로 계산 됩니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average = "마이크로"|
recall_score_weighted|회수는 특정 클래스의 요소에 레이블이 올바르게 지정 된 비율입니다. 가중치는 각 클래스에 대 한 최대 회수의 산술 평균 이며 각 클래스의 실제 인스턴스 수로 가중치가 지정 됩니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average = "가중치가 적용"|
weighted_accuracy|가중치가 적용 되는 정확도는 각 예제에 지정 된 가중치가 해당 예제의 true 클래스에서 true 인스턴스의 비율과 같음을 나타내는 정확도입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight는 대상의 각 요소에 대 한 해당 클래스의 비율과 같은 벡터입니다.|
<a name="confusion-matrix"></a>
### <a name="confusion-matrix"></a>혼동 행렬
#### <a name="what-is-a-confusion-matrix"></a>혼동 행렬 이란?
혼동 행렬은 분류 모델의 성능을 설명 하는 데 사용 됩니다. 각 행은 데이터 집합의 실제 클래스 또는 실제 클래스의 인스턴스를 표시 하 고, 각 열은 모델에 의해 예측 된 클래스의 인스턴스를 나타냅니다. 

#### <a name="what-does-automated-ml-do-with-the-confusion-matrix"></a>자동 ML에서 혼동 행렬을 사용 하 여 수행 하는 작업은 무엇 인가요?
분류 문제의 경우 Azure Machine Learning는 작성 된 각 모델에 대 한 혼동 행렬을 자동으로 제공 합니다. 각 혼동 행렬에 대해 자동화 된 ML은 true 레이블 (행)과 비교 하 여 예측 된 각 레이블 (열)의 빈도를 표시 합니다. 이 어두운 색은 행렬의 특정 부분에서 더 높은 수입니다. 

#### <a name="what-does-a-good-model-look-like"></a>좋은 모델의 모양은 무엇입니까?
모델에서 제공 하는 예측 값과 데이터 집합의 실제 값을 비교 합니다. 이로 인해 모델의 값 대부분이 대각선을 따라 증가 하는 경우 기계 학습 모델의 정확도는 높아집니다. 즉, 모델이 올바른 값을 예측 합니다. 모델에 불균형 클래스가 있는 경우 혼동 행렬은 편향 모델을 검색 하는 데 도움이 됩니다.

##### <a name="example-1-a-classification-model-with-poor-accuracy"></a>예 1: 정확도가 낮은 분류 모델
![정확도가 낮은 분류 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix1.png)

##### <a name="example-2-a-classification-model-with-high-accuracy"></a>예 2: 정확도가 높은 분류 모델 
![정확도가 높은 분류 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix2.png)

##### <a name="example-3-a-classification-model-with-high-accuracy-and-high-bias-in-model-predictions"></a>예 3: 모델 예측에서 정확도 및 높은 바이어스가 높은 분류 모델
![모델 예측의 정확도 및 높은 바이어스가 높은 분류 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-biased-model.png)

<a name="precision-recall-chart"></a>
### <a name="precision-recall-chart"></a>전체 자릿수-회수 차트
#### <a name="what-is-a-precision-recall-chart"></a>전체 자릿수 리콜 차트 란?
전체 자릿수 리콜 곡선은 전체 자릿수와 모델 회수 간의 관계를 보여 줍니다. 용어 전체 자릿수는 모델에서 모든 인스턴스의 레이블을 올바르게 표시 하는 기능을 나타냅니다. 회수는 분류자가 특정 레이블의 모든 인스턴스를 찾을 수 있는 기능을 나타냅니다.

#### <a name="what-does-automated-ml-do-with-the-precision-recall-chart"></a>자동 ML은 전체 자릿수 리콜 차트를 어떻게 사용 하나요?

이 차트에서는 각 모델에 대 한 전체 자릿수 리콜 곡선을 비교 하 여 특정 비즈니스 문제에 대 한 전체 자릿수와 회수 간에 허용 되는 관계를 가진 모델을 확인할 수 있습니다. 이 차트는 모델의 모든 클래스와 연결 된 매크로 평균 정밀도 재호출, 마이크로 평균 전체 자릿수 리콜 및 전체 자릿수 재호출을 보여 줍니다. 

매크로-평균은 각 클래스와 별개로 메트릭을 계산 하 고 평균을 취하여 모든 클래스를 동일 하 게 처리 합니다. 그러나 마이크로 평균은 평균 계산을 위해 모든 클래스의 기여를 집계 합니다. 데이터 집합에 클래스 불균형이 있는 경우에는 마이크로 평균이 좋습니다.

#### <a name="what-does-a-good-model-look-like"></a>좋은 모델의 모양은 무엇입니까?
비즈니스 문제의 목표에 따라 이상적인 전체 자릿수 리콜 곡선이 다를 수 있습니다. 몇 가지 예가 아래에 나와 있습니다.

##### <a name="example-1-a-classification-model-with-low-precision-and-low-recall"></a>예 1: 낮은 정밀도 및 낮은 회수를 포함 하는 분류 모델
![낮은 정밀도 및 낮은 회수를 포함 하는 분류 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall1.png)

##### <a name="example-2-a-classification-model-with-100-precision-and-100-recall"></a>예 2: ~ 100% precision 및 ~ 100% 회수를 사용 하는 분류 모델 
분류 모델의 높은 전체 자릿수 및 회수를 ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall2.png)
<a name="roc"></a>
### <a name="roc-chart"></a>ROC 차트

#### <a name="what-is-a-roc-chart"></a>ROC 차트 란?
받는 사람 운영 특성 (또는 ROC)은 정확 하 게 분류 된 레이블과 특정 모델에 대해 잘못 분류 된 레이블의 그림입니다. ROC 곡선은 매우 높은 바이어스를 가진 데이터 집합에서 모델을 학습 하는 경우에는 거짓 긍정 레이블을 표시 하지 않기 때문에 더 저렴 한 정보를 제공 합니다.

#### <a name="what-does-automated-ml-do-with-the-roc-chart"></a>자동화 된 ML은 ROC 차트를 사용 하 여 수행 하는 작업은 무엇 인가요?
자동화 된 ML은 모델의 모든 클래스와 연결 된 매크로 평균 정밀도 재호출, 마이크로 평균 정밀도 재호출 및 전체 자릿수 재호출을 생성 합니다. 

매크로-평균은 각 클래스와 별개로 메트릭을 계산 하 고 평균을 취하여 모든 클래스를 동일 하 게 처리 합니다. 그러나 마이크로 평균은 평균 계산을 위해 모든 클래스의 기여를 집계 합니다. 데이터 집합에 클래스 불균형이 있는 경우에는 마이크로 평균이 좋습니다.

#### <a name="what-does-a-good-model-look-like"></a>좋은 모델의 모양은 무엇입니까?
이상적으로 모델은 100% 진정한 긍정 요금에 가까울수록 0% false 긍정 률에 가까울수록 좋습니다. 

##### <a name="example-1-a-classification-model-with-low-true-labels-and-high-false-labels"></a>예 1: 작은 레이블 및 상위 거짓 레이블이 있는 분류 모델
![작은 레이블 및 상위 거짓 레이블이 있는 분류 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-1.png)

##### <a name="example-2-a-classification-model-with-high-true-labels-and-low-false-labels"></a>예 2: 중요 한 레이블 및 하위 false 레이블이 있는 분류 모델
매우 중요 한 레이블 및 낮은 false 레이블이 있는 분류 모델을 ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-2.png)
<a name="lift-curve"></a>
### <a name="lift-chart"></a>리프트 차트
#### <a name="what-is-a-lift-chart"></a>리프트 차트 란?
리프트 차트는 분류 모델의 성능을 평가 하는 데 사용 됩니다. 모델을 정확 하 게 사용 하지 않고도 생성 된 모델을 사용 하 여 수행할 수 있는 더 나은 방법을 보여 줍니다.
#### <a name="what-does-automated-ml-do-with-the-lift-chart"></a>자동 ML은 리프트 차트를 사용 하 여 수행 하는 작업은 무엇 인가요?
특정 모델의 가치를 얻기 위해 자동으로 작성 된 모델의 리프트를 기준선에 Azure Machine Learning 비교할 수 있습니다.
#### <a name="what-does-a-good-model-look-like"></a>좋은 모델의 모양은 무엇입니까?

##### <a name="example-1-a-classification-model-that-does-worse-than-a-random-selection-model"></a>예 1: 임의 선택 모델 보다 더 나쁜 분류 모델
![임의 선택 모델 보다 더 나쁜 분류 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve1.png)
##### <a name="example-2-a-classification-model-that-performs-better-than-a-random-selection-model"></a>예 2: 임의 선택 모델 보다 더 나은 성능을 발휘 하는 분류 모델
더 나은](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve2.png)
를 수행 하는 분류 모델을 ![합니다.<a name="gains-curve"></a>
### <a name="gains-chart"></a>향상 차트
#### <a name="what-is-a-gains-chart"></a>향상 된 차트는 무엇 인가요?

향상 된 차트는 데이터의 각 부분에 따라 분류 모델의 성능을 평가 합니다. 이는 데이터 집합의 각 백분위 수에 대해 보여 주며, 임의 선택 모델에 비해 더 나은 성능을 보여 줍니다.

#### <a name="what-does-automated-ml-do-with-the-gains-chart"></a>자동 ML의 이점 차트는 어떻게 되나요?
누적 이득 차트를 사용 하 여 모델에서 원하는 이득에 해당 하는 비율을 사용 하 여 분류 구분을 선택할 수 있습니다. 이 정보는 함께 제공 되는 리프트 차트의 결과를 확인 하는 또 다른 방법을 제공 합니다.

#### <a name="what-does-a-good-model-look-like"></a>좋은 모델의 모양은 무엇입니까?
##### <a name="example-1-a-classification-model-with-minimal-gain"></a>예 1: 최소한의 획득으로 분류 모델 사용
![최소한의 이득을 갖춘 분류 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve1.png)

##### <a name="example-2-a-classification-model-with-significant-gain"></a>예 2: 상당한 이득을 가진 분류 모델
상당한 이득을 가진 분류 모델 ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve2.png)
<a name="calibration-plot"></a>
### <a name="calibration-chart"></a>보정 차트

#### <a name="what-is-a-calibration-chart"></a>보정 차트 란?
보정 플롯은 예측 모델의 신뢰도를 표시 하는 데 사용 됩니다. 예측 확률과 실제 확률 간의 관계를 표시 하 여이를 수행 합니다. 여기서 "probability"는 특정 인스턴스가 일부 레이블 아래에 속할 가능성을 나타냅니다.
#### <a name="what-does-automated-ml-do-with-the-calibration-chart"></a>자동 ML에서 보정 차트를 사용 하 여 수행 하는 작업은 무엇 인가요?
모든 분류 문제에 대해 지정 된 예측 모델의 마이크로 평균, 매크로 평균 및 각 클래스에 대 한 보정 줄을 검토할 수 있습니다.

매크로-평균은 각 클래스와 별개로 메트릭을 계산 하 고 평균을 취하여 모든 클래스를 동일 하 게 처리 합니다. 그러나 마이크로 평균은 평균 계산을 위해 모든 클래스의 기여를 집계 합니다. 
#### <a name="what-does-a-good-model-look-like"></a>좋은 모델의 모양은 무엇입니까?
 잘 보정 된 모델은 y = x 줄에 맞게 정렬 됩니다. 보다 확실 한 모델은 예측 확률이 있지만 실제 확률은 없는 y = 0 줄에 맞춰집니다. 


##### <a name="example-1-a-well-calibrated-model"></a>예 1: 잘 보정 된 모델
![ 잘 보정 되는 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve1.png)

##### <a name="example-2-an-over-confident-model"></a>예 2: 보다 확실 한 모델
![보다 확실 한 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve2.png)

## <a name="regression"></a>회귀 결과

다음 메트릭과 차트는의 자동화 된 기계 학습 기능을 사용 하 여 작성 하는 모든 회귀 모델에 사용할 수 있습니다 Azure Machine Learning

+ [Metrikák](#reg-metrics)
+ [예측 및 True](#pvt)
+ [잔차의 히스토그램](#histo)


### <a name="reg-metrics"></a>회귀 메트릭

회귀 또는 예측 작업에 대해 각 실행 반복에 다음 메트릭이 저장 됩니다.

|Metrika|Leírás|Számítás|추가 매개 변수
--|--|--|--|
explained_variance|설명 된 가변성은 수학적 모델이 지정 된 데이터 집합의 변형에 대해 계정을 지정 하는 비율입니다. 원본 데이터의 분산이 오류 분산에 대 한 비율입니다. 오류의 평균은 0 인 경우 설명 된 가변성과 같습니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|None|
r2_score|R2는 평균을 출력 하는 기준선 모델과 비교 했을 때의 결정 계수 또는 백분율 감소입니다. |[변동이](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|None|
spearman_correlation|스페어만 상관 관계는 두 데이터 집합 간의 관계에 대 한 단 조성에의 nonparametric 측정값입니다. 피어슨 상관 관계와 달리 스페어만 상관 관계는 두 데이터 집합이 일반적으로 분산 되어 있다고 가정 하지 않습니다. 다른 상관 관계 계수와 마찬가지로이는-1과 + 1 사이의 차이가 없으며 0은 상관 관계가 없음을 의미 합니다. -1 또는 + 1의 상관 관계는 정확한 단조 관계를 의미 합니다. 긍정 상관 관계는 x가 증가할 것을 의미 하므로 y가 됩니다. 부정 상관 관계는 x가 늘어나고 y가 감소 함을 의미 합니다.|[변동이](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|None|
mean_absolute_error|절대 평균 오차는 대상과 예측의 절대값에 대 한 절대값의 예상 값입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|None|
normalized_mean_absolute_error|정규화 된 평균 절대 오차는 절대 오차를 데이터 범위로 나눈 값을 의미 합니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|데이터의 범위로 나누기|
median_absolute_error|중간 절대 오차는 대상과 예측 간의 모든 절대 차이의 중앙값입니다. 이 손실은 이상 값의 강력 합니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|None|
normalized_median_absolute_error|정규화 된 중앙값 절대 오차는 데이터 범위에 따라 나눈 절대 오차입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|데이터의 범위로 나누기|
root_mean_squared_error|제곱 평균 제곱 오차는 대상과 예측의 예상 제곱 차이의 제곱근입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|None|
normalized_root_mean_squared_error|정규화 된 근본 평균 제곱 오차는 평균 제곱 오차를 데이터 범위로 나눈 값입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|데이터의 범위로 나누기|
root_mean_squared_log_error|제곱근 평균 제곱 로그 오류는 예상 되는 제곱 로그 오류의 제곱근입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|None|
normalized_root_mean_squared_log_error|정규화 된 루트 평균 제곱 로그 오류는 근본 평균 제곱 로그 오류를 데이터 범위로 나눈 값입니다.|[변동이](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|데이터의 범위로 나누기|

### <a name="pvt"></a>예측 및 실제 차트
#### <a name="what-is-a-predicted-vs-true-chart"></a>예측 된 차트 및 진정한 차트
예측 된 값과 True는 회귀 문제에 대 한 예측 값과 해당 하는 실제 값 간의 관계를 보여 줍니다. 이 그래프를 사용 하 여 y = x 줄에 더 가깝게 모델의 성능을 측정할 수 있습니다. 예측 값은 예측 모델의 정확도를 향상 시킬 수 있습니다.

#### <a name="what-does-automated-ml-do-with-the-predicted-vs-true-chart"></a>자동화 된 ML은 예측 된 차트와 실제 차트에서 수행 하는 작업은 무엇 인가요?
각 실행 후에 각 회귀 모델에 대 한 예측 및 실제 그래프를 볼 수 있습니다. 데이터 개인 정보 보호를 위해 값이 함께 표시 되 고 각 bin의 크기가 차트 영역의 아래쪽 부분에 막대 그래프로 표시 됩니다. 모델을 사용할 이상적인 값에 대해 오류 여백을 보여 주는 더 밝은 음영 영역을 사용 하 여 예측 모델을 비교할 수 있습니다.

#### <a name="what-does-a-good-model-look-like"></a>좋은 모델의 모양은 무엇입니까?
##### <a name="example-1-a-classification-model-with-low-accuracy"></a>예 1: 정확도가 낮은 분류 모델
![예측 정확도가 낮은 회귀 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression1.png)

##### <a name="example-2-a-regression-model-with-high-accuracy"></a>예 2: 정확도가 높은 회귀 모델 
[예측에서 정확도가 높은 회귀 모델 ![](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2-expanded.png)



### <a name="histo"></a>잔차 차트의 히스토그램
#### <a name="what-is-a-residuals-chart"></a>잔차 차트 란?
잔여는 관찰 된 y – 예측 된 y를 나타냅니다. 낮은 바이어스를 사용 하 여 오류 여백을 표시 하려면 잔차의 히스토그램을 0 중심의 종 모양의 종 모양으로 지정 해야 합니다. 
#### <a name="what-does-automated-ml-do-with-the-residuals-chart"></a>자동 ML은 잔차 차트를 사용 하 여 수행 하는 작업은 무엇 인가요?
자동 ML은 예측의 오류 분포를 보여 주는 잔차 차트를 자동으로 제공 합니다.
#### <a name="what-does-a-good-model-look-like"></a>좋은 모델의 모양은 무엇입니까?
좋은 모델에는 일반적으로 0 주위에 종 모양의 또는 오류가 있습니다.

##### <a name="example-1-a-regression-model-with-bias-in-its-errors"></a>예 1: 오류가 발생 한 편차를 포함 하는 회귀 모델
![오류에서 바이어스를 사용 하 여 SA 회귀 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression3.png)

##### <a name="example-2-a-regression-model-with-more-even-distribution-of-errors"></a>예제 2: 오류를 보다 균등 하 게 분산 하는 회귀 모델
![오류를 균등 하 게 배포 하는 회귀 모델](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression4.png)

## <a name="explain-model"></a>모델 interpretability 및 기능 중요도
자동화 된 ML은 실행에 대 한 machine learning interpretability 대시보드를 제공 합니다.
Interpretability 기능을 사용 하도록 설정 하는 방법에 대 한 자세한 내용은 자동화 된 ML 실험에서 interpretability를 사용 하도록 설정 [하는 방법](how-to-machine-learning-interpretability-automl.md) 을 참조 하세요.

## <a name="next-steps"></a>Következő lépések

+ Azure Machine Learning에서 [자동화 된 ml](concept-automated-ml.md) 에 대해 자세히 알아보세요.
+ 자동화 된 [Machine Learning 모델 설명](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model) 샘플 노트북을 사용해 보세요.
