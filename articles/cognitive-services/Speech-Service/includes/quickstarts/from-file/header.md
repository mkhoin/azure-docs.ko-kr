---
title: '빠른 시작: 오디오 파일에서 음성 인식 - Speech Service'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 2f7ec24667514fb131af29321a53f97210e58fc9
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74819422"
---
이 빠른 시작에서는 [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md)를 사용하여 오디오 파일에서 음성을 인식합니다. 몇 가지 필수 구성 요소를 충족한 후 파일에서 음성을 인식하는 작업은 다음 다섯 가지 단계를 수행하면 됩니다.
> [!div class="checklist"]
> * 구독 키 및 지역에서 ````SpeechConfig```` 개체를 만듭니다.
> * .WAV 파일 이름을 지정하는 ````AudioConfig```` 개체를 만듭니다.
> * 위의 ````SpeechConfig```` 및 ````AudioConfig```` 개체를 사용하여 ````SpeechRecognizer```` 개체를 만듭니다.
> * ````SpeechRecognizer```` 개체를 사용하여 단일 발화에 대한 인식 프로세스를 시작합니다.
> * 반환된 ````SpeechRecognitionResult````를 검사합니다.
