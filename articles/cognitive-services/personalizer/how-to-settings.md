---
title: Personalizer 구성
titleSuffix: Azure Cognitive Services
description: 서비스 구성에는 서비스의 보상 처리 방법, 서비스의 탐색 빈도, 모델을 다시 학습하는 빈도 및 저장할 데이터의 양이 포함됩니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 4ab1dcf4f3554c941107ec653f717b3680543da2
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490733"
---
# <a name="configure-personalizer"></a>Personalizer 구성

서비스 구성에는 서비스의 보상 처리 방법, 서비스의 탐색 빈도, 모델을 다시 학습하는 빈도 및 저장할 데이터의 양이 포함됩니다.

## <a name="create-personalizer-resource"></a>Personalizer 리소스 만들기

각 피드백 루프에 대한 Personalizer 리소스를 만듭니다. 

1. [Azure 포털](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)에 로그인합니다. 이전 링크를 클릭하면 Personalizer 서비스의 **만들기** 페이지로 이동합니다. 
1. 서비스 이름을 입력하고 구독, 위치, 가격 책정 계층 및 리소스 그룹을 선택합니다.
1. 확인을 선택하고 **만들기**를 선택합니다.

<a name="configure-service-settings-in-the-azure-portal"></a>

## <a name="configure-service-in-the-azure-portal"></a>Azure Portal에서 서비스 구성

1. [Azure 포털](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)에 로그인합니다.
1. Personalizer 리소스를 찾습니다. 
1. **리소스 관리** 섹션에서 **구성**을 선택 합니다.

    Azure Portal을 벗어나기 전에 **키** 페이지에서 리소스 키 중 하나를 복사합니다. 이 정보는 [Personalizer SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer)를 사용할 때 필요합니다.

<a name="configure-reward-settings-for-the-feedback-loop-based-on-use-case"></a>

### <a name="configure-reward-for-the-feedback-loop-based-on-use-case"></a>사용 사례에 따라 피드백 루프의 보상 구성

피드백 루프의 보상 사용에 대 한 서비스를 구성 합니다. 다음 값을 변경 하면 현재 Personalizer 모델을 다시 설정 하 고 지난 2 일간의 데이터를 다시 학습 합니다.

![피드백 루프의 보상 값 구성](media/settings/configure-model-reward-settings.png)

|값|목적|
|--|--|
|보상 대기 시간|Personalizer가 순위 호출에 대한 보상 값을 수집하는 시간 길이를 설정하며, 순위 호출이 발생하는 순간부터 시작합니다. 이 값은 "Personalizer가 보상 호출을 대기 하는 시간"을 요청 하 여 설정 됩니다. 이 기간 후에 도착하는 보상은 기록되지만 학습에 사용되지는 않습니다.|
|기본 보상|순위 호출과 연결된 보상 대기 시간 동안 Personalizer가 보상 호출을 하나도 받지 않은 경우 Personalizer는 기본 보상을 할당합니다. 기본적으로 대부분의 시나리오에서 기본 보상은 0입니다.|
|보상 집계|동일한 순위 API 호출에 대해 여러 보상이 수신되는 경우 집계 메서드 **sum** 또는 **earliest**가 사용됩니다. Earliest는 가장 먼저 받은 점수를 선택하고 나머지 점수를 무시합니다. 중복 가능성이 있는 여러 호출 중에 고유한 보상을 원하는 경우에 유용합니다. |

이러한 값을 변경한 후에는 **저장**을 선택 해야 합니다.

### <a name="configure-exploration"></a>탐색 구성 

개인 설정은 대안을 탐색하여 새 패턴을 검색하고 사용자 동작 변경에 맞게 조정할 수 있습니다. 탐색 **값은** 탐색으로 응답 되는 순위 호출의 비율을 결정 합니다. 

이 값을 변경 하면 현재 Personalizer 모델을 다시 설정 하 고 지난 2 일간의 데이터를 다시 학습 합니다.

![탐색 값은 탐색으로 응답 되는 순위 호출의 비율을 결정 합니다.](media/settings/configure-exploration-setting.png)

이 값을 변경한 후에는 **저장**을 선택 해야 합니다.

### <a name="model-update-frequency"></a>모델 업데이트 빈도

모든 활성 이벤트의 Reward API 호출에서 학습된 최신 모델은 Personalizer Rank 호출에서 자동으로 사용되지 않습니다. **모델 업데이트 빈도**는 Rank 호출에서 사용하는 모델의 업데이트 빈도를 설정합니다. 

높은 모델 업데이트 빈도는 사용자 동작에서 변경 사항을 밀접하게 추적하려는 상황에 유용합니다. 라이브 뉴스, 바이럴 콘텐츠 또는 라이브 상품 입찰에서 실행하는 사이트를 예로 들 수 있습니다. 이러한 시나리오에서 15분 빈도를 사용할 수 있습니다. 대부분 사용 사례의 경우 낮은 업데이트 빈도가 효과적입니다. 1분 업데이트 빈도는 Personalizer를 사용하여 애플리케이션의 코드 디버깅, 데모 실행 또는 Machine Learning 측면을 대화형으로 테스트할 때 유용합니다.

![모델 업데이트 빈도는 새 Personalizer 모델을 다시 학습시키는 빈도를 설정합니다.](media/settings/configure-model-update-frequency-settings-15-minutes.png)

이 값을 변경한 후에는 **저장**을 선택 해야 합니다.

### <a name="data-retention"></a>데이터 보존

**데이터 보존 기간**은 Personalizer가 데이터 로그를 보관하는 기간(일)을 설정합니다. 과거 데이터 로그는 Personalizer의 효율성을 측정하고 학습 정책을 최적화하는 데 사용되는 [오프라인 평가](concepts-offline-evaluation.md)를 수행하는 데 필요합니다.

이 값을 변경한 후에는 **저장**을 선택 해야 합니다.

## <a name="export-the-personalizer-model"></a>Personalizer 모델 내보내기

**모델 및 학습 설정**에 대 한 리소스 관리 섹션에서 모델 생성 및 마지막 업데이트 날짜를 검토 하 고 현재 모델을 내보냅니다. Azure Portal 또는 Personalizer API를 사용하여 보관을 위해 모델 파일을 내보낼 수 있습니다. 

![현재 Personalizer 모델 내보내기](media/settings/export-current-personalizer-model.png)

## <a name="import-and-export-learning-policy"></a>학습 정책 가져오기 및 내보내기

**모델 및 학습 설정**에 대 한 리소스 관리 섹션에서 새 학습 정책을 가져오거나 현재 학습 정책을 내보냅니다.
이전 내보내기에서 학습 정책 파일을 가져오거나 오프 라인 평가 중에 검색 된 최적화 된 정책을 다운로드할 수 있습니다. 이러한 파일을 수동으로 변경 하면 machine learning 성능 및 오프 라인 평가의 정확도에 영향을 주며, Microsoft는 기계 학습 및 평가의 정확도 나 수동으로 편집한 정책으로 인해 발생 하는 서비스 예외를 보장할 수 없습니다.

## <a name="clear-data-for-your-learning-loop"></a>학습 루프의 데이터 지우기

1. Azure Portal에서 Personalizer 리소스에 대해 **모델 및 학습 설정** 페이지에서 **데이터 지우기**를 선택 합니다.
1. 모든 데이터를 지우고 학습 루프를 원래 상태로 다시 설정 하려면 3 개의 확인란을 모두 선택 합니다.

    ![Azure Portal에서 Personalizer 리소스의 데이터를 지웁니다.](./media/settings/clear-data-from-personalizer-resource.png)

    |값|목적|
    |--|--|
    |개인 설정 및 보상 데이터를 기록 합니다.|이 로깅 데이터는 오프 라인 평가에 사용 됩니다. 리소스를 다시 설정 하는 경우 데이터를 지웁니다.|
    |Personalizer 모델을 다시 설정 합니다.|이 모델은 재 학습 마다 변경 됩니다. 이 학습 빈도는 **구성** 페이지의 **모델 업로드 빈도** 에서 지정 합니다. |
    |학습 정책을 기본값으로 설정 합니다.|오프 라인 평가의 일환으로 학습 정책을 변경한 경우 원래 학습 정책으로 다시 설정 됩니다.|

1. **선택한 데이터 지우기** 를 선택 하 여 지우기 프로세스를 시작 합니다. 상태는 Azure 알림에서 오른쪽 위 탐색에서 보고 됩니다. 

## <a name="next-steps"></a>다음 단계


[지역 가용성에 대해 알아보기](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
