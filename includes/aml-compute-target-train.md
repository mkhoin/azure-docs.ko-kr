---
title: 포함 파일
description: 포함 파일
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 12/04/2019
ms.openlocfilehash: 25a865296d2ebaabcf9043341878928763d61a6f
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74875567"
---
**계산 대상은 한 학습 작업에서 다음 학습 작업까지 다시 사용할 수 있습니다**. 예를 들어 원격 VM을 사용자의 작업 영역에 연결한 후에는 여러 작업에 다시 사용할 수 있습니다.  기계 학습 파이프라인의 경우 각 계산 대상에 대해 적절 한 [파이프라인 단계](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py) 를 사용 합니다.

|학습 &nbsp;대상|[자동화 된 ML](../articles/machine-learning/service/concept-automated-ml.md) | [ML 파이프라인](../articles/machine-learning/service/concept-ml-pipelines.md) | [Azure Machine Learning 디자이너](../articles/machine-learning/service/concept-designer.md)
|----|:----:|:----:|:----:|
|[로컬 컴퓨터](../articles/machine-learning/service/how-to-set-up-training-targets.md#local)| 예 | &nbsp; | &nbsp; |
|[Azure Machine Learning 계산 클러스터](../articles/machine-learning/service/how-to-set-up-training-targets.md#amlcompute)| 예 & <br/>하이퍼 매개 변수&nbsp;튜닝 | 예 | 예 |
|[원격 VM](../articles/machine-learning/service/how-to-set-up-training-targets.md#vm) | 예 & <br/>하이퍼 매개 변수 튜닝 | 예 | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#databricks)| 예 | 예 | &nbsp; |
|[Azure 데이터 레이크 분석](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#adla) | &nbsp; | 예 | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/service/how-to-set-up-training-targets.md#hdinsight) | &nbsp; | 예 | &nbsp; |
|[Azure Batch](../articles/machine-learning/service/how-to-set-up-training-targets.md#azbatch) | &nbsp; | 예 | &nbsp; |
