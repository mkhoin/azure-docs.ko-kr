---
title: ML 실험 & 메트릭 기록
titleSuffix: Azure Machine Learning
description: Azure ML 실험을 모니터링 하 고 실행 메트릭을 모니터링 하 여 모델 생성 프로세스를 향상 시킵니다. 학습 스크립트에 로깅을 추가 하 고 실행의 기록 된 결과를 확인 합니다.  실행 .log, start_logging 또는 ScriptRunConfig를 사용 합니다.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/05/2019
ms.custom: seodec18
ms.openlocfilehash: 3de34c1da20df17fb5fb65cef28669fb73ff33a5
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74978563"
---
# <a name="monitor-azure-ml-experiment-runs-and-metrics"></a>Azure ML 실험 실행 및 메트릭 모니터링
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

실험 및 모니터링 실행 메트릭을 추적 하 여 모델 생성 프로세스를 개선 합니다. 이 문서에서는 학습 스크립트에 로깅 코드를 추가 하 고, 실험 실행을 제출 하 고, 실행을 모니터링 하 고, Azure Machine Learning 결과를 검사 하는 방법에 대해 알아봅니다.

> [!NOTE]
> 자동화 된 Machine Learning 실행 또는 교육 작업을 실행 하는 Docker 컨테이너와 같은 학습 중 다른 원본의 정보를 기록할 수도 Azure Machine Learning. 이러한 로그는 문서화 되어 있지 않습니다. 문제가 발생 하 고 Microsoft 지원에 문의 하는 경우 문제 해결 중에 이러한 로그를 사용할 수 있습니다.

> [!TIP]
> 이 문서의 정보는 주로 모델 학습 프로세스를 모니터링 하려는 데이터 과학자 및 개발자를 위한 것입니다. 할당량, 완료 된 학습 실행 또는 완료 된 모델 배포와 같이 Azure Machine learning의 리소스 사용 및 이벤트를 모니터링 하는 데 관심이 있는 관리자는 [모니터링 Azure Machine Learning](monitor-azure-machine-learning.md)을 참조 하세요.

## <a name="available-metrics-to-track"></a>추적할 수 있는 메트릭

실험을 학습 하는 동안 실행에 다음 메트릭을 추가할 수 있습니다. 실행에서 추적할 수 있는 항목의 자세한 목록을 보려면 [실행 클래스 참조 설명서](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)를 참조 하세요.

|Type (Típus)| Python 함수 | Megjegyzések|
|----|:----|:----|
|스칼라 값 |Függvény:<br>`run.log(name, value, description='')`<br><br>Példa:<br>실행 .log ("정확도", 0.95) |지정 된 이름을 사용 하 여 실행에 숫자 또는 문자열 값을 기록 합니다. 실행에 메트릭을 기록 하면 메트릭이 실험의 실행 레코드에 저장 됩니다.  실행 내에서 동일한 메트릭을 여러 번 기록할 수 있으며 그 결과는 해당 메트릭의 벡터로 간주 됩니다.|
|목록|Függvény:<br>`run.log_list(name, value, description='')`<br><br>Példa:<br>log_list ("정확도", [0.6, 0.7, 0.87])를 실행 합니다. | 지정 된 이름을 사용 하 여 실행에 값 목록을 기록 합니다.|
|행|Függvény:<br>`run.log_row(name, description=None, **kwargs)`<br>Példa:<br>실행 log_row ("Y X X", x = 1, Y = 0.4) | *Log_row* 를 사용 하 여 k워 gs에 설명 된 대로 여러 열이 있는 메트릭을 만듭니다. 각 명명 된 매개 변수는 지정 된 값을 사용 하 여 열을 생성 합니다.  *log_row* 를 한 번 호출 하 여 임의의 튜플을 기록 하거나 루프에서 여러 번 호출 하 여 전체 테이블을 생성할 수 있습니다.|
|Table|Függvény:<br>`run.log_table(name, value, description='')`<br><br>Példa:<br>log_table ("Y over X", {"x": [1, 2, 3], "Y": [0.6, 0.7, 0.89]}) | 지정 된 이름을 사용 하 여 실행에 사전 개체를 기록 합니다. |
|Lemezképek|Függvény:<br>`run.log_image(name, path=None, plot=None)`<br><br>Példa:<br>`run.log_image("ROC", plt)` | 이미지를 실행 레코드에 기록 합니다. Log_image를 사용 하 여 이미지 파일 또는 matplotlib 플롯을 실행에 기록 합니다.  이러한 이미지는 실행 레코드에서 표시 되 고 비교할 수 있습니다.|
|실행 태그|Függvény:<br>`run.tag(key, value=None)`<br><br>Példa:<br>run. 태그 ("selected", "yes") | 문자열 키와 선택적 문자열 값을 사용 하 여 실행에 태그를 지정 합니다.|
|파일 또는 디렉터리 업로드|Függvény:<br>`run.upload_file(name, path_or_stream)`<br> <br> Példa:<br>upload_file (best_model "model.pkl", "/model.pkl")를 실행 합니다. | 실행 레코드에 파일을 업로드 합니다. 는 지정 된 출력 디렉터리에서 자동으로 캡처 파일을 실행 하며, 대부분의 실행 형식에 대해 기본적으로 "./출력"으로 지정 됩니다.  추가 파일을 업로드 해야 하거나 출력 디렉터리가 지정 되지 않은 경우에만 upload_file을 사용 합니다. 출력 디렉터리에 업로드 되도록 이름에 `outputs`를 추가 하는 것이 좋습니다. 을 호출 하 여이 실행 레코드와 연결 된 모든 파일을 나열할 수 있습니다 `run.get_file_names()`|

> [!NOTE]
> 스칼라, 목록, 행 및 테이블에 대 한 메트릭은 float, integer 또는 string 형식일 수 있습니다.

## <a name="choose-a-logging-option"></a>로깅 옵션 선택

실험을 추적 하거나 모니터링 하려면 실행을 제출할 때 로깅을 시작 하는 코드를 추가 해야 합니다. 다음은 실행 제출을 트리거하는 방법입니다.
* __Start_logging__ -학습 스크립트에 로깅 기능을 추가 하 고 지정 된 실험에서 대화형 로깅 세션을 시작 합니다. **start_logging** 노트북 등의 시나리오에서 사용할 대화형 실행을 만듭니다. 세션 중에 기록 된 모든 메트릭은 실험에서 실행 레코드에 추가 됩니다.
* __ScriptRunConfig__ -학습 스크립트에 로깅 함수를 추가 하 고 실행을 사용 하 여 전체 스크립트 폴더를 로드 합니다.  **ScriptRunConfig** 는 스크립트 실행에 대 한 구성을 설정 하기 위한 클래스입니다. 이 옵션을 사용 하 여 모니터링 코드를 추가 하 여 완료를 알리거나 모니터링할 시각적 위젯을 가져올 수 있습니다.

## <a name="set-up-the-workspace"></a>작업 영역 설정
로깅을 추가 하 고 실험을 제출 하기 전에 작업 영역을 설정 해야 합니다.

1. 작업 영역을 로드 합니다. 작업 영역 구성을 설정 하는 방법에 대해 자세히 알아보려면 [작업 영역 구성 파일](how-to-configure-environment.md#workspace)을 참조 하세요.

   ```python
   from azureml.core import Experiment, Run, Workspace
   import azureml.core
  
   ws = Workspace.from_config()
   ```
  
## <a name="option-1-use-start_logging"></a>옵션 1: start_logging 사용

**start_logging** 노트북 등의 시나리오에서 사용할 대화형 실행을 만듭니다. 세션 중에 기록 된 모든 메트릭은 실험에서 실행 레코드에 추가 됩니다.

다음 예에서는 로컬 Jupyter 노트북에서 로컬에 있는 간단한 기본 모델 볼록 볼록 모델을 학습 합니다. 다른 환경에 실험을 제출 하는 방법에 대 한 자세한 내용은 [Azure Machine Learning를 사용 하 여 모델 학습을 위한 계산 대상 설정](https://docs.microsoft.com/azure/machine-learning/service/how-to-set-up-training-targets)을 참조 하세요.

1. 로컬 Jupyter 노트북에서 교육 스크립트를 만듭니다. 

   ```python
   # load diabetes dataset, a well-known small dataset that comes with scikit-learn
   from sklearn.datasets import load_diabetes
   from sklearn.linear_model import Ridge
   from sklearn.metrics import mean_squared_error
   from sklearn.model_selection import train_test_split
   from sklearn.externals import joblib

   X, y = load_diabetes(return_X_y = True)
   columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
   data = {
      "train":{"X": X_train, "y": y_train},        
      "test":{"X": X_test, "y": y_test}
   }
   reg = Ridge(alpha = 0.03)
   reg.fit(data['train']['X'], data['train']['y'])
   preds = reg.predict(data['test']['X'])
   print('Mean Squared Error is', mean_squared_error(preds, data['test']['y']))
   joblib.dump(value = reg, filename = 'model.pkl');
   ```

2. Azure Machine Learning SDK를 사용 하 여 실험 추적을 추가 하 고 지속 된 모델을 실험 실행 레코드에 업로드 합니다. 다음 코드는 실험 실행에 대 한 태그, 로그 및 모델 파일 업로드를 추가 합니다.

   ```python
    # Get an experiment object from Azure Machine Learning
    experiment = Experiment(workspace=ws, name="train-within-notebook")
    
    # Create a run object in the experiment
    run =  experiment.start_logging()
    # Log the algorithm parameter alpha to the run
    run.log('alpha', 0.03)
    
    # Create, fit, and test the scikit-learn Ridge regression model
    regression_model = Ridge(alpha=0.03)
    regression_model.fit(data['train']['X'], data['train']['y'])
    preds = regression_model.predict(data['test']['X'])
    
    # Output the Mean Squared Error to the notebook and to the run
    print('Mean Squared Error is', mean_squared_error(data['test']['y'], preds))
    run.log('mse', mean_squared_error(data['test']['y'], preds))
    
    # Save the model to the outputs directory for capture
    model_file_name = 'outputs/model.pkl'
    
    joblib.dump(value = regression_model, filename = model_file_name)
    
    # upload the model file explicitly into artifacts 
    run.upload_file(name = model_file_name, path_or_stream = model_file_name)
    
    # Complete the run
    run.complete()
   ```

    이 스크립트는 실행을 완료로 표시 하는 ```run.complete()```로 종료 됩니다.  이 함수는 일반적으로 대화형 노트북 시나리오에서 사용 됩니다.

## <a name="option-2-use-scriptrunconfig"></a>옵션 2: ScriptRunConfig 사용

[**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) 는 스크립트 실행에 대 한 구성을 설정 하기 위한 클래스입니다. 이 옵션을 사용 하 여 모니터링 코드를 추가 하 여 완료를 알리거나 모니터링할 시각적 위젯을 가져올 수 있습니다.

이 예제에서는 위의 기본 이상 정보 볼록 모델을 확장 합니다. 실험에서 실행 중인 메트릭 및 학습 된 모델을 캡처하기 위해 모델의 알파 값에 대 한 간단한 매개 변수 스윕을 수행 합니다. 이 예제는 사용자 관리 환경에 대해 로컬로 실행 됩니다. 

1. `train.py`학습 스크립트를 만듭니다.

   ```python
   # train.py

   import os
   from sklearn.datasets import load_diabetes
   from sklearn.linear_model import Ridge
   from sklearn.metrics import mean_squared_error
   from sklearn.model_selection import train_test_split
   from azureml.core.run import Run
   from sklearn.externals import joblib

   import numpy as np

   #os.makedirs('./outputs', exist_ok = True)

   X, y = load_diabetes(return_X_y = True)

   run = Run.get_context()

   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
   data = {"train": {"X": X_train, "y": y_train},
          "test": {"X": X_test, "y": y_test}}

   # list of numbers from 0.0 to 1.0 with a 0.05 interval
   alphas = mylib.get_alphas()

   for alpha in alphas:
      # Use Ridge algorithm to create a regression model
      reg = Ridge(alpha = alpha)
      reg.fit(data["train"]["X"], data["train"]["y"])

      preds = reg.predict(data["test"]["X"])
      mse = mean_squared_error(preds, data["test"]["y"])
      # log the alpha and mse values
      run.log('alpha', alpha)
      run.log('mse', mse)

      model_file_name = 'ridge_{0:.2f}.pkl'.format(alpha)
      # save model in the outputs folder so it automatically get uploaded
      with open(model_file_name, "wb") as file:
          joblib.dump(value = reg, filename = model_file_name)

      # upload the model file explicitly into artifacts 
      run.upload_file(name = model_file_name, path_or_stream = model_file_name)

      # register the model
      #run.register_model(file_name = model_file_name)

      print('alpha is {0:.2f}, and mse is {1:0.2f}'.format(alpha, mse))
  
   ```

2. `train.py` 스크립트는 볼록 모델에서 사용할 알파 값 목록을 가져올 수 있는 `mylib.py`을 참조 합니다.

   ```python
   # mylib.py
  
   import numpy as np

   def get_alphas():
      # list of numbers from 0.0 to 1.0 with a 0.05 interval
      return np.arange(0.0, 1.0, 0.05)
   ```

3. 사용자 관리 로컬 환경을 구성 합니다.

   ```python
   from azureml.core import Environment
    
   # Editing a run configuration property on-fly.
   user_managed_env = Environment("user-managed-env")
    
   user_managed_env.python.user_managed_dependencies = True
    
   # You can choose a specific Python environment by pointing to a Python path 
   #user_managed_env.python.interpreter_path = '/home/johndoe/miniconda3/envs/myenv/bin/python'
   ```

4. 사용자 관리 환경에서 실행 되도록 ```train.py``` 스크립트를 제출 합니다. 이 전체 스크립트 폴더는 학습을 위해 ```mylib.py``` 파일을 포함 하 여 제출 됩니다.

   ```python
   from azureml.core import ScriptRunConfig
    
   exp = Experiment(workspace=ws, name="train-on-local")
   src = ScriptRunConfig(source_directory='./', script='train.py')
   src.run_config.environment = user_managed_env
   run = exp.submit(src)
   ```

## <a name="manage-a-run"></a>실행 관리

[학습 실행 시작, 모니터링 및 취소](how-to-manage-runs.md) 문서는 실험을 관리 하는 방법에 대 한 특정 Azure Machine Learning 워크플로를 강조 표시 합니다.

## <a name="view-run-details"></a>실행 세부 정보 보기

### <a name="view-activequeued-runs-from-the-browser"></a>브라우저에서 활성/대기 중인 실행 보기

모델 학습에 사용 되는 계산 대상은 공유 리소스입니다. 따라서 지정 된 시간에 큐에 대기 중이거나 활성 상태인 실행이 여러 개 있을 수 있습니다. 브라우저에서 특정 계산 대상에 대 한 실행을 확인 하려면 다음 단계를 사용 합니다.

1. [Azure Machine Learning studio](https://ml.azure.com/)에서 작업 영역을 선택한 다음 페이지의 왼쪽에서 __계산__ 을 선택 합니다.

1. 학습 __클러스터__ 를 선택 하 여 학습에 사용 되는 계산 대상 목록을 표시 합니다. 그런 다음 클러스터를 선택 합니다.

    ![학습 클러스터 선택](./media/how-to-track-experiments/select-training-compute.png)

1. __실행__을 선택 합니다. 이 클러스터를 사용 하는 실행 목록이 표시 됩니다. 특정 실행에 대 한 세부 정보를 보려면 __실행__ 열의 링크를 사용 합니다. 실험에 대 한 세부 정보를 보려면 __실험__ 열의 링크를 사용 합니다.

    ![학습 클러스터에 대 한 실행 선택](./media/how-to-track-experiments/show-runs-for-compute.png)
    
    > [!TIP]
    > 실행에는 자식 실행이 포함 될 수 있으므로 하나의 학습 작업으로 여러 항목이 생성 될 수 있습니다.

실행이 완료 되 면이 페이지에 더 이상 표시 되지 않습니다. 완료 된 실행에 대 한 정보를 보려면 스튜디오의 __실험__ 섹션을 방문 하 여 실험 및 실행을 선택 합니다. 자세한 내용은 [쿼리 실행 메트릭](#queryrunmetrics) 섹션을 참조 하세요.

### <a name="monitor-run-with-jupyter-notebook-widget"></a>Jupyter 노트북 위젯을 사용 하 여 모니터 실행
**ScriptRunConfig** 메서드를 사용 하 여 실행을 제출 하는 경우 [Jupyter 위젯을](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py)사용 하 여 실행 진행률을 볼 수 있습니다. A futtatás elküldéséhez hasonlóan a vezérlő aszinkron módon működik, és 10-15 másodpercenként élő állapotfrissítést biztosít a feladat befejeződéséig.

1. 실행이 완료 될 때까지 대기 하는 동안 Jupyter 위젯을 봅니다.

   ```python
   from azureml.widgets import RunDetails
   RunDetails(run).show()
   ```

   ![Jupyter 노트북 위젯의 스크린샷](./media/how-to-track-experiments/run-details-widget.png)

   작업 영역에서 동일한 표시에 대 한 링크를 가져올 수도 있습니다.

   ```python
   print(run.get_portal_url())
   ```

2. **[자동화 된 기계 학습 실행의 경우]** 이전 실행에서 차트에 액세스 하려면입니다. `<<experiment_name>>`를 적절 한 실험 이름으로 바꿉니다.

   ``` 
   from azureml.widgets import RunDetails
   from azureml.core.run import Run

   experiment = Experiment (workspace, <<experiment_name>>)
   run_id = 'autoML_my_runID' #replace with run_ID
   run = Run(experiment, run_id)
   RunDetails(run).show()
   ```

   ![자동화 된 Machine Learning에 대 한 jupyter 노트북 위젯](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)


파이프라인에 대 한 추가 세부 정보를 보려면 표를 탐색 하려는 파이프라인을 클릭 합니다. 그러면 Azure Machine Learning studio에서 팝업로 차트가 렌더링 됩니다.

### <a name="get-log-results-upon-completion"></a>Naplóeredmények lekérése a befejezéskor

대기 하는 동안 다른 작업을 실행할 수 있도록 백그라운드에서 모델 학습 및 모니터링이 수행 됩니다. 더 많은 코드를 실행 하기 전에 모델이 학습을 완료할 때까지 기다릴 수도 있습니다. **ScriptRunConfig**를 사용 하는 경우 ```run.wait_for_completion(show_output = True)```를 사용 하 여 모델 교육이 완료 된 시간을 표시할 수 있습니다. ```show_output``` 플래그는 자세한 출력을 제공 합니다. 

<a id="queryrunmetrics"></a>

### <a name="query-run-metrics"></a>쿼리 실행 메트릭

```run.get_metrics()```를 사용 하 여 학습 된 모델의 메트릭을 볼 수 있습니다. 이제 위의 예제에서 기록 된 모든 메트릭을 가져와 최상의 모델을 결정할 수 있습니다.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-the-experiment-in-your-workspace-in-azure-machine-learning-studiohttpsmlazurecom"></a>[Azure Machine Learning studio](https://ml.azure.com) 에서 작업 영역의 실험 보기

실험 실행이 완료 되 면 기록 된 실험 실행 레코드를 찾아볼 수 있습니다. [Azure Machine Learning studio](https://ml.azure.com)에서 기록에 액세스할 수 있습니다.

실험 탭으로 이동 하 여 실험을 선택 합니다. 실험 실행 대시보드로 이동 하 여 각 실행에 대해 기록 되는 추적 된 메트릭과 차트를 볼 수 있습니다. 이 경우에는 MSE 및 알파 값이 기록 됩니다.

  ![Azure Machine Learning studio에서 실행 세부 정보](./media/how-to-track-experiments/experiment-dashboard.png)

특정 실행으로 드릴 다운 하 여 해당 출력 또는 로그를 보거나 제출한 실험의 스냅숏을 다운로드 하 여 다른 사람과 실험 폴더를 공유할 수 있습니다.

### <a name="viewing-charts-in-run-details"></a>실행 세부 정보에서 차트 보기

여러 가지 방법으로 로깅 Api를 사용 하 여 실행 중에 여러 유형의 메트릭을 기록 하 고 Azure Machine Learning studio에서 차트로 볼 수 있습니다.

|기록 값|예제 코드| 포털에서 보기|
|----|----|----|
|숫자 값의 배열 기록| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|단일 가변 꺾은선형 차트|
|반복 해 서 사용 되는 것과 같은 메트릭 이름을 사용 하 여 단일 숫자 값을 기록 합니다 (예: for loop 내에서).| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| 단일 가변 꺾은선형 차트|
|두 개의 숫자 열이 있는 행을 반복 해 서 기록|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|두 가변 꺾은선형 차트|
|두 개의 숫자 열이 있는 로그 테이블|`run.log_table(name='Sine Wave', value=sines)`|두 가변 꺾은선형 차트|


## <a name="example-notebooks"></a>노트북 예제
다음 노트북은이 문서의 개념을 보여 줍니다.
* [사용 방법-azureml/교육/학습-노트북 내](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook)
* [사용 방법-azureml/교육/교육-로컬](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [how-to-use-azureml/트레이닝/logging-api/logging-api](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Következő lépések

다음 단계를 수행 하 여 Python 용 Azure Machine Learning SDK를 사용 하는 방법을 알아보세요.

* 최상의 모델을 등록 하 고 자습서에서 배포 하는 방법에 대 한 예제를 참조 하 [여 Azure Machine Learning를 사용 하 여 이미지 분류 모델을 학습](tutorial-train-models-with-aml.md)합니다.

* [Azure Machine Learning를 사용 하 여 PyTorch 모델을 학습](how-to-train-pytorch.md)하는 방법을 알아봅니다.
