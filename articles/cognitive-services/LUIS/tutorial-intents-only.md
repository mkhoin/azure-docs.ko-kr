---
title: '자습서: 의도 예측 - LUIS'
titleSuffix: Azure Cognitive Services
description: 이 자습서에서는 사용자의 의도를 예측하는 사용자 지정 앱을 만듭니다. 이 앱은 이메일 주소 또는 날짜 등의 발화 텍스트에서 다양한 데이터 요소를 추출하지 않으므로 가장 간단한 형식의 LUIS 앱입니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 11/20/2019
ms.author: diberry
ms.openlocfilehash: 4d096ee829a425af3763c212daf5049acccf9f19
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325908"
---
# <a name="tutorial-build-a-luis-app-to-determine-user-intentions"></a>자습서: 사용자 의도를 확인하는 LUIS 앱 빌드

이 자습서에서는 발화(텍스트)에 따라 사용자의 의도를 예측하는 사용자 지정 앱을 만듭니다. 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

**이 자습서에서 학습할 내용은 다음과 같습니다.**

> [!div class="checklist"]
> * 새 앱 만들기 
> * 의도 만들기
> * 예제 발화 추가
> * 앱 교육
> * 앱 게시
> * 엔드포인트에서 의도 예측 가져오기


[!INCLUDE [LUIS Free account](includes/quickstart-tutorial-use-free-starter-key.md)]

## <a name="user-intentions-as-intents"></a>사용자 의도

이 앱의 목적은 대화, 자연어 텍스트의 의도를 확인하는 것입니다. 

`I'd like to order a veggie pizza with a salad on the side.`

이러한 의도는 **의도**로 분류됩니다. 

|Intent|목적|
|--|--|
|`ModifyOrder`|사용자의 피자 주문을 확인합니다.|
|`Greeting`|Bot 대화를 시작합니다.|
|`ConfirmOrder`|피자 주문을 확인합니다.|
|`None`|사용자가 앱에서 대답할 수 없는 내용을 질문하고 있는지 확인합니다. 이 의도가 앱 만들기의 일부로 제공된 경우에는 삭제할 수 없습니다. |

## <a name="create-a-new-app"></a>새 앱 만들기

[!INCLUDE [Follow these steps to create a new LUIS app](includes/create-pizza-app.md)]

## <a name="create-a-new-intent"></a>새 의도 만들기 

1. 포털의 앱 **빌드** 섹션 내에서 **+ 만들기**를 선택합니다. 새 의도 이름 `OrderPizza`를 입력한 다음, **완료**를 선택합니다.

    사용자가 피자를 주문하려는 경우 `OrderPizza` 의도가 예측됩니다. 

1. 사용자가 요청할 것으로 예상되는 여러 예제 발언을 이 의도에 추가합니다.

    |`OrderPizza` 예제 발화|
    |--|
    |`can i get a pepperoni pizza and a can of coke please`|
    |`can i get a small pizza with onions peppers and olives`|
    |`delivery for a small pepperoni pizza`|
    |`pickup a cheddar cheese pizza large with extra anchovies`|
    |`i need 2 large cheese pizzas 6 large pepperoni pizzas and 1 large supreme pizza`|

    ![예제 발화 추가](media/tutorial-intents-only/add-example-utterances-for-pizza-order.png)

    _예제 발언_을 입력하면 LUIS 앱이 이 의도에 대해 예측해야 하는 발언의 종류를 학습하게 됩니다. 

    [!INCLUDE [Do not use too few utterances](includes/do-not-use-too-few-utterances.md)]    

## <a name="create-remaining-intents"></a>나머지 의도 만들기

1. `Greeting` 의도를 만들고 다음 예제 발화를 추가합니다. 사용자가 새 피자 주문 대화를 시작하고 있는지 확인하기 위한 의도입니다.

    |`Greeting` 예제 발화|
    |--|
    |`Hi`|
    |`Hello`|
    |`Hey`|
    |`Start`|
    |`Begin`|

1. `Confirm` 의도를 만들고 다음 예제 발화를 추가합니다. 사용자가 주문을 완료하고 주문 정보를 수락하는지 확인하기 위한 의도입니다. 

    |`Confirm` 예제 발화|
    |--|
    |`Go ahead`|
    |`ok`|
    |`Yes`|
    |`Sure`|


## <a name="none-intent-example-utterances"></a>None 의도 예제 발화

[!INCLUDE [Follow these steps to add the None intent to the app](includes/add-example-utterances-none-intent.md)]

## <a name="train-the-app"></a>앱 학습 

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app"></a>앱 게시 

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)] 

## <a name="get-intent-prediction"></a>의도 예측 가져오기

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. 주소 표시줄의 URL 끝으로 이동하여 다음을 입력합니다.

    `get a medium vegetarian pizza for delivery` 

    예제 발화와 완전히 똑같지는 않으므로, LUIS가 이 의도에서 예측해야 하는 내용을 배울 수 있는지 여부를 확인하는 좋은 테스트입니다.

    마지막 쿼리 문자열 매개 변수는 `query`로 발화 **쿼리**입니다. 이 발화는 예제 발화 중 하나와 같지 않습니다. 좋은 테스트이며, 상위 채점 의도로 `OrderPizza`을 반환해야 합니다. 

    ```JSON
    {
        "query": "get a medium vegetarian pizza for delivery",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.6488959
                },
                "None": {
                    "score": 0.139966831
                },
                "Confirm": {
                    "score": 0.00736504374
                },
                "Greeting": {
                    "score": 0.003970454
                }
            },
            "entities": {}
        }
    }
    ```

    이 앱에 현재 엔터티가 없기 때문에 엔터티 배열이 비어 있습니다(추출할 발화 내의 데이터 단위). 

    JSON 결과는 최고 채점 의도를 **`prediction.topIntent`** 속성으로 식별합니다. 모든 점수는 1에서 0 사이이며, 1에 가까울수록 더 좋은 점수입니다. 

1. **인사말** 의도를 대상으로 삼도록 URL **쿼리** 매개 변수를 변경합니다.

    `Howdy`

    예제 발화와 완전히 똑같지는 않으므로, LUIS가 이 의도에서 예측해야 하는 내용을 배울 수 있는지 여부를 확인하는 좋은 테스트입니다. 

    ```json
    {
        "query": "howdy",
        "prediction": {
            "topIntent": "Greeting",
            "intents": {
                "Greeting": {
                    "score": 0.446016937
                },
                "Confirm": {
                    "score": 0.2390079
                },
                "None": {
                    "score": 0.09119555
                },
                "OrderPizza": {
                    "score": 0.00109590159
                }
            },
            "entities": {}
        }
    }    
    ```
 
    이 예측의 신뢰도 점수는 44%입니다. 신뢰도 점수를 높이려면 예제 발화 15~30개를 추가하세요.  

## <a name="client-application-next-steps"></a>클라이언트-애플리케이션 다음 단계

LUIS가 JSON 응답은 반환한 후에 이 요청에서 LUIS가 완료됩니다. LUIS는 사용자 발화에 대답을 제공하지 않고, 자연어에 요청되는 정보의 형식만 식별합니다. 대화형 후속 작업은 Azure Bot 같은 클라이언트 애플리케이션을 통해 제공됩니다. 


[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>관련 정보

* [엔터티의 형식](luis-concept-entity-types.md)
* [학습 방법](luis-how-to-train.md)
* [게시 방법](luis-how-to-publish-app.md)
* [LUIS 포털에서 테스트하는 방법](luis-interactive-test.md)
* [Azure Bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)


## <a name="next-steps"></a>다음 단계

이 자습서에서는 LUIS 앱을 만들고, 의도를 만들고, 각 의도에 예제 발화를 추가하고, 없음 의도에 예제 발화를 추가하고, 엔드포인트에서 학습/게시/테스트했습니다. LUIS 모델을 빌드하는 기본 단계입니다. 

> [!div class="nextstepaction"]
> [이 앱에 분해 가능한 엔터티 추가](tutorial-machine-learned-entity.md)
