---
title: '빠른 시작: 몰입형 판독기를 시작하는 iOS 앱 만들기(Swift)'
titleSuffix: Azure Cognitive Services
description: 이 빠른 시작에서는 iOS 앱을 처음부터 빌드하고 몰입형 판독기 기능을 추가합니다.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 08/01/2019
ms.author: metan
ms.openlocfilehash: 64fd7508244f2123cc10ee96ec4f805050aedfaa
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69899523"
---
# <a name="quickstart-create-an-ios-app-that-launches-the-immersive-reader-swift"></a>빠른 시작: 몰입형 판독기를 시작하는 iOS 앱 만들기(Swift)

[몰입형 판독기](https://www.onenote.com/learningtools)는 읽기 이해도를 향상시키기 위해 검증된 기술을 구현하는 포괄적으로 설계된 도구입니다.

이 빠른 시작에서는 iOS 앱을 처음부터 빌드하고 몰입형 판독기 SDK를 사용하여 몰입형 판독기를 통합합니다. 이 빠른 시작의 전체 작동 샘플은 [여기](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/samples/quickstart-swift)에서 확인할 수 있습니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

## <a name="prerequisites"></a>필수 조건

* [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)
* Azure AD(Azure Active Directory) 인증에 대해 구성된 몰입형 리더 리소스입니다. [다음 지침](./azure-active-directory-authentication.md)에 따라 설정하세요. 샘플 프로젝트 속성을 구성할 때 여기서 만든 일부 값이 필요합니다. 나중에 참조할 수 있도록 세션 출력을 텍스트 파일로 저장합니다.

## <a name="create-an-xcode-project"></a>Xcode 프로젝트 만들기

Xcode에서 새 프로젝트를 만듭니다.

![새 프로젝트](./media/ios/xcode-create-project.png)

**단일 보기 앱**을 선택합니다.

![새 단일 보기 앱](./media/ios/xcode-single-view-app.png)

## <a name="get-the-sdk-cocoapod"></a>SDK CocoaPod 가져오기
몰입형 리더 SDK를 사용하는 가장 쉬운 방법은 CocoaPods를 사용하는 것입니다. Cocoapods를 통해 설치하려면 다음을 수행합니다.
1. [Cocoapods 설치](http://guides.cocoapods.org/using/getting-started.html) - 시작 가이드에 따라 Cocoapods를 설치합니다.
2. Xcode 프로젝트의 루트 디렉터리에서 `pod init`를 실행하여 Podfile을 만듭니다.
3.  `pod 'immersive-reader-sdk', :path => 'https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/immersive-reader-sdk'`를 추가하여 CocoaPod를 Podfile에 추가합니다. Pofile은 다음과 같이 대상의 이름이 quickstart-swift로 바뀝니다.
 ```ruby
  platform :ios, '9.0'

  target 'quickstart-swift' do
  use_frameworks!
  # Pods for quickstart-swift
  pod 'immersive-reader-sdk', :path => 'https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/immersive-reader-sdk'
  end
```
4. 터미널의 Xcode 프로젝트 디렉터리에서 `pod install` 명령을 실행하여 몰입형 판독기 SDK Pod를 설치합니다.
5. SDK를 참조해야 하는 모든 파일에 `import immersive_reader_sdk`를 추가합니다.
6. `.xcodeproj` 파일이 아니라 `.xcworkspace` 파일을 열어 프로젝트를 열어야 합니다.

## <a name="acquire-an-azure-ad-authentication-token"></a>Azure AD 인증 토큰 획득

이 부분에는 위의 Azure AD 인증 구성 필수 요소 단계의 값이 필요합니다. 해당 세션에서 저장한 텍스트 파일을 다시 참조합니다.

````text
TenantId     => Azure subscription TenantId
ClientId     => Azure AD ApplicationId
ClientSecret => Azure AD Application Service Principal password
Subdomain    => Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')
````

ViewController.swift 파일이 포함된 기본 프로젝트 폴더에서 Constants.swift라는 Swift 클래스 파일을 만듭니다. 클래스를 다음 코드로 바꾸고, 해당하는 경우 값을 추가합니다. 이 파일은 머신에만 있는 로컬 파일로 유지하고, public이 되지 않아야 하는 비밀이 포함되어 있으므로 원본 제어에 커밋하지 않아야 합니다. 앱에서 비밀을 유지하지 않는 것이 좋습니다. 대신 백 엔드 서비스를 사용하여 토큰을 가져오는 것이 좋습니다. 이 경우 비밀을 앱 외부에 보관하고 디바이스에서 해제할 수 있습니다. 권한 없는 사용자가 토큰을 획득하여 몰입형 리더 서비스 및 청구에 사용하지 못하도록 백 엔드 API 엔드포인트를 인증 형식(예: [OAuth](https://oauth.net/2/))으로 보호해야 하며, 이 작업은 이 빠른 시작의 범위를 벗어납니다.

[!code-swift[Constants](~/ImmersiveReaderSdk/iOS/samples/quickstart-swift/quickstart-swift/Constants.swift)]

## <a name="set-up-the-app-to-run-without-a-storyboard"></a>스토리보드 없이 실행되도록 앱 설정

AppDelegate.swift를 열고 파일을 다음 코드로 바꿉니다.

[!code-swift[AppDelegate](~/ImmersiveReaderSdk/iOS/samples/quickstart-swift/quickstart-swift/AppDelegate.swift)]

## <a name="create-the-launch-view-controller-and-add-sample-content"></a>시작 뷰 컨트롤러 만들기 및 샘플 콘텐츠 추가

ViewController.swift의 이름을 LaunchViewController.swift로 바꾸고 파일을 다음 코드로 바꿉니다.

[!code-swift[LaunchViewController](~/ImmersiveReaderSdk/iOS/samples/quickstart-swift/quickstart-swift/LaunchViewController.swift)]

## <a name="build-and-run-the-app"></a>앱 빌드 및 실행

시뮬레이터 또는 디바이스 대상을 선택하여 Xcode에서 보관 체계를 설정합니다.
![보관 체계](./media/ios/xcode-archive-scheme.png)<br/>
![대상 선택](./media/ios/xcode-select-target.png)

Xcode에서 Ctrl+R을 누르거나 재생 단추를 클릭하여 프로젝트를 실행합니다. 그러면 지정한 시뮬레이터 또는 디바이스에서 앱이 시작됩니다.

앱에서 다음과 같이 표시됩니다.

![샘플 앱](./media/ios/sample-app-ipad.png)

“몰입형 판독기” 단추를 클릭하면 앱의 콘텐츠와 함께 몰입형 판독기가 시작됩니다.

![몰입형 판독기](./media/ios/immersive-reader-ipad.png)

## <a name="next-steps"></a>다음 단계

* [몰입형 리더 iOS SDK](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS) 및 [몰입형 리더 iOS SDK 참조](./ios-reference.md)를 살펴봅니다.
