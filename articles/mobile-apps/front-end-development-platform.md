---
title: Visual Studio 및 Azure 서비스를 사용 하 여 클라이언트 응용 프로그램을 빌드하기 위한 프런트 엔드 개발 플랫폼 선택
description: 클라이언트 응용 프로그램을 빌드하기 위한 지원 되는 네이티브 플랫폼 및 플랫폼 간 언어에 대해 알아봅니다.
author: elamalani
manager: elamalani
ms.service: vs-appcenter
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: 891967e74e52c1311d73cbe13fff8c95e479478c
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795498"
---
# <a name="choose-mobile-development-framework"></a>모바일 개발 프레임 워크 선택
개발자는 클라이언트 쪽 기술을 사용 하 여 플랫폼 간 접근 방식으로 특정 프레임 워크 및 패턴을 사용 하 여 모바일 응용 프로그램을 빌드할 수 있습니다. 개발자는 각자의 결정에 따라 네이티브 (개념-C 및 Java와 같은 언어를 사용 하는 네이티브 단일 플랫폼) 응용 프로그램, Xamarin, .NET C#및를 사용 하는 플랫폼 간 응용 프로그램, 하이브리드 (Cordova 사용) 및 해당 변형을 빌드할 수 있습니다. 몇.

## <a name="native-platforms"></a>네이티브 플랫폼
네이티브 응용 프로그램을 빌드하려면 플랫폼 특정 프로그래밍 언어, Sdk, 개발 환경 및 OS 공급 업체에서 제공 하는 기타 도구가 필요 합니다.

### <a name="ios"></a>iOS
Apple에서 만들어지고 개발한 앱은 Apple 장치 (iPhone 및 iPad)에서 실행 됩니다.

- **프로그래밍 언어** -목표-C, Swift
- **IDE** -Xcode
- **Sdk** -iOS sdk

### <a name="android"></a>Android
Google 및 전 세계에서 가장 인기 있는 OS를 통해 설계 된 응용 프로그램은 다양 한 스마트폰 및 태블릿에서 실행할 수 있습니다.

- **프로그래밍 언어** -Java, Kotlin 
- **IDE** Android Studio 및 Android 개발자 도구 
- **SDK** -Android SDK

### <a name="windows"></a>Windows
- **프로그래밍 언어** -C#
- **IDE** -Visual Studio, Visual Studio Code
- **SDK** -Windows SDK

#### <a name="pros"></a>장점
- 좋은 사용자 환경
- 네이티브 라이브러리와의 인터페이스를 제공 하는 고성능 및 성능을 갖춘 응답성이 뛰어난 응용 프로그램
- 매우 안전한 응용 프로그램

#### <a name="cons"></a>단점
- 응용 프로그램은 한 플랫폼 에서만 실행 됩니다.
- 더 많은 개발자 리소스와 응용 프로그램을 빌드하는 데 비용이 많이 듭니다.
- 낮은 코드 재사용

## <a name="cross-platforms-and-hybrid-applications"></a>플랫폼 간 및 하이브리드 응용 프로그램
플랫폼 간 응용 프로그램은 네이티브 모바일 응용 프로그램을 한 번 작성 하 고, 코드를 공유 하 고, iOS, Android 및 Windows에서 실행할 수 있는 기능을 제공 합니다.

### <a name="xamarin"></a>Xamarin
Microsoft에서 담당 하는 [Xamarin](https://visualstudio.microsoft.com/xamarin/) 을 사용 하면 IOS, Android 및 Windows를 비롯 C#한 여러 플랫폼에서 작동 하는 클래스 라이브러리와 런타임을 사용 하 여 강력한 플랫폼 간 모바일 응용 프로그램을 빌드할 수 있으며, 네이티브 (해석 되지 않은 )를 사용할 수 있습니다. Xamarin은 네이티브 플랫폼의 모든 기능을 결합 하 고 다양 한 강력한 기능을 추가 합니다.

- **프로그래밍 언어** -C#
- **IDE** -Windows 또는 Mac의 Visual Studio

### <a name="react-native"></a>React Native
2015에서 Facebook에 의해 출시 된 [네이티브](https://facebook.github.io/react-native/) 는 IOS 및 Android 용 모바일 응용 프로그램의 실제를 작성 하기 위한 [오픈 소스](https://github.com/facebook/react-native) JavaScript 프레임 워크입니다. 이는 사용자 인터페이스를 빌드하기 위한 Facebook의 Facebook JavaScript 라이브러리에 대 한 응답을 기반으로 하며, 브라우저를 대상으로 지정 하는 대신 모바일 플랫폼을 대상으로 합니다. 네이티브에 반응 하면 웹 구성 요소 대신 네이티브 구성 요소가 빌딩 블록으로 사용 됩니다.
 
- **프로그래밍 언어** -JavaScript
- **IDE** -Visual Studio Code

### <a name="unity"></a>Unity
 Unity는 개발자가 Windows, iOS, Android 및 Xbox를 비롯 한 다양 한 플랫폼에 대해를 사용 C# 하 여 고품질 2D/3d 응용 프로그램을 만들 수 있도록 하는 게임을 만드는 데 최적화 된 엔진입니다.

### <a name="cordova"></a>Cordova
Cordova를 사용 하면 Apache Codova에 대 한 Visual Studio Tools를 사용 Visual Studio Code 하거나 Cordova 용 확장을 사용 하 여 하이브리드 응용 프로그램을 빌드할 수 있습니다. 하이브리드 접근 방식을 사용 하면 웹 사이트와 구성 요소를 공유 하 고 Cordova를 기반으로 하는 호스트 된 웹 응용 프로그램 방식으로 웹 서버 기반 응용 프로그램을 다시 사용할 수 있습니다.

#### <a name="pros"></a>장점
- 여러 플랫폼에 대 한 코드 베이스를 만들어 코드 유용성 향상
- 여러 플랫폼에서 더 광범위 한 대상 그룹을 만듭니다.
- 개발 시간 대폭 감소
- 쉽게 시작 하 고 업데이트할 수 있습니다.

#### <a name="cons"></a>단점
- 성능 저하
- 유연성이 부족 합니다.
- 각 플랫폼에는 네이티브 응용 프로그램의 창의적인 기능을 지 원하는 고유한 기능 집합이 있습니다.
- 향상 된 UI 디자인 시간
- 도구 제한
