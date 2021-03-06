---
title: Xamarin, iOS & Android에서 조정 된 인증 | Microsoft
titleSuffix: Microsoft identity platform
description: .NET 용 Azure AD 인증 라이브러리 (ADAL.NET)의 Microsoft Authenticator 사용할 수 있는 Xamarin iOS 응용 프로그램을 .NET 용 Microsoft Authentication Library (MSAL.NET)로 마이그레이션하는 방법에 대해 알아봅니다.
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/08/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a26f73354b99160275649855f7a2a616249ce05c
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74921839"
---
# <a name="use-microsoft-authenticator-or-microsoft-intune-company-portal-on-xamarin-applications"></a>Xamarin 응용 프로그램에서 Microsoft Authenticator 또는 Microsoft Intune 회사 포털 사용

Android 및 iOS에서 Microsoft Authenticator 또는 Microsoft Intune와 같은 broker 회사 포털 사용 (Android에만 해당):

- SSO (Single sign-on). 사용자는 각 응용 프로그램에 로그인 할 필요가 없습니다.
- 장치 id입니다. Broker는 장치에서 작업 공간에 연결 되었을 때 만들어진 장치 인증서에 액세스 합니다.
- 응용 프로그램 id 확인. 응용 프로그램은 broker를 호출할 때 리디렉션 URL을 전달 하 고 broker는이를 확인 합니다.

이러한 기능 중 하나를 사용 하도록 설정 하려면 응용 프로그램 개발자가 `PublicClientApplicationBuilder.CreateApplication` 메서드를 호출할 때 `WithBroker()` 매개 변수를 사용 해야 합니다. `.WithBroker()`는 기본적으로 true로 설정 됩니다. 또한 개발자는 [iOS](#brokered-authentication-for-ios) 또는 [Android](#brokered-authentication-for-android) 응용 프로그램에 대 한 단계를 수행 해야 합니다.

## <a name="brokered-authentication-for-ios"></a>IOS에 대 한 조정 된 인증

Xamarin.ios 앱이 [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458) 앱과 통신할 수 있도록 하려면 다음 단계를 수행 합니다.

### <a name="step-1-enable-broker-support"></a>1 단계: broker 지원 사용
Broker 지원은 PublicClientApplication 기준으로 설정 됩니다. 기본적으로 사용하지 않도록 설정되어 있습니다. PublicClientApplicationBuilder를 통해 PublicClientApplication을 만들 때 `WithBroker()` 매개 변수를 사용 합니다 (기본적으로 true로 설정).

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>2 단계: 콜백을 처리 하도록 AppDelegate 업데이트
MSAL.NET (Microsoft Authentication Library for .NET)에서 broker를 호출할 때 broker는 `AppDelegate` 클래스의 `OpenUrl` 메서드를 통해 응용 프로그램을 다시 호출 합니다. MSAL이 broker의 응답을 대기 하므로 응용 프로그램은 MSAL.NET를 호출 해야 합니다. 이 협력을 사용 하도록 설정 하려면 `AppDelegate.cs` 파일을 업데이트 하 여 다음 메서드를 재정의 합니다.

```CSharp
public override bool OpenUrl(UIApplication app, NSUrl url, 
                             string sourceApplication,
                             NSObject annotation)
{
    if (AuthenticationContinuationHelper.IsBrokerResponse(sourceApplication))
    {
      AuthenticationContinuationHelper.SetBrokerContinuationEventArgs(url);
      return true;
    }
    
    else if (!AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url))
    {               
         return false;                
    }
    
    return true;     
}           
```

이 메서드는 응용 프로그램이 시작 될 때마다 호출 됩니다. Broker에서 응답을 처리 하 고 MSAL.NET에서 시작한 인증 프로세스를 완료할 수 있는 기회가 사용 됩니다.

### <a name="step-3-set-a-uiviewcontroller"></a>3 단계: UIViewController () 설정
계속 `AppDelegate.cs`개체 창을 설정 해야 합니다. 일반적으로 Xamarin iOS를 사용 하면 개체 창을 설정할 필요가 없습니다. Broker에서 응답을 보내고 받으려면 개체 창이 필요 합니다. 

이렇게 하려면 두 가지 작업을 수행 합니다. 
1. `AppDelegate.cs`에서 `App.RootViewController`를 새 `UIViewController()`설정 합니다. 이 할당을 통해 broker에 대 한 호출을 사용 하 여 UIViewController가 있는지 확인할 수 있습니다. 올바르게 설정 되지 않은 경우 다음 오류가 나타날 수 있습니다. `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
1. AcquireTokenInteractive 호출에서 `.WithParentActivityOrWindow(App.RootViewController)` 사용 하 여 사용할 개체 창에 대 한 참조를 전달 합니다.

**예:**

`App.cs`:
```CSharp
   public static object RootViewController { get; set; }
```
`AppDelegate.cs`:
```CSharp
   LoadApplication(new App());
   App.RootViewController = new UIViewController();
```
토큰 획득 호출에서 다음을 수행 합니다.
```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```

### <a name="step-4-register-a-url-scheme"></a>4 단계: URL 구성표 등록
MSAL.NET는 Url을 사용 하 여 broker를 호출한 다음 broker 응답을 앱으로 다시 반환 합니다. 왕복을 완료 하려면 `Info.plist` 파일에 앱에 대 한 URL 체계를 등록 합니다.

`CFBundleURLSchemes` 이름은 접두사로 `msauth.`를 포함 하 고 그 뒤에 `CFBundleURLName`해야 합니다.

`$"msauth.(BundleId)"`

**예:**

`msauth.com.yourcompany.xforms`

> [!NOTE]
> 이 URL 구성표는 broker에서 응답을 받을 때 앱을 고유 하 게 식별 하는 데 사용 되는 리디렉션 URI의 일부가 됩니다.

```XML
 <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.yourcompany.xforms</string>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.com.yourcompany.xforms</string>
        </array>
      </dict>
    </array>
```

### <a name="step-5-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>5 단계: LSApplicationQueriesSchemes 섹션에 broker 식별자 추가
MSAL은 `–canOpenURL:`를 사용 하 여 broker가 장치에 설치 되어 있는지 확인 합니다. IOS 9에서 Apple은 응용 프로그램에서 쿼리할 수 있는 스키마를 잠 궜 습니다. 

`Info.plist` 파일의 `LSApplicationQueriesSchemes` 섹션에 `msauthv2`를 추가 합니다.

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="step-6-register-your-redirect-uri-in-the-application-portal"></a>6 단계: 응용 프로그램 포털에서 리디렉션 URI 등록
Broker를 사용 하면 리디렉션 URI에 추가 요구 사항이 추가 됩니다. 리디렉션 URI의 형식은 다음과 _같아야 합니다_ .
```CSharp
$"msauth.{BundleId}://auth"
```
**예:**
```CSharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```
리디렉션 URI가 `Info.plist` 파일에 포함 된 `CFBundleURLSchemes` 이름과 일치 하는지 확인 합니다.

### <a name="step-7-make-sure-the-redirect-uri-is-registered-with-your-app"></a>7 단계: 리디렉션 URI가 앱에 등록 되었는지 확인

응용 프로그램에 대 한 유효한 리디렉션 URI로 https://portal.azure.com) 이 리디렉션 URI를 앱 등록 포털에 등록 해야 합니다. 

포털에는 번들 ID에서 조정 된 회신 URI를 계산 하는 데 도움이 되는 새로운 경험 앱 등록 포털이 있습니다.

1. 앱 등록에서 **인증** 을 선택 하 고 **새 환경 체험**을 선택 합니다.

   ![새 앱 등록 환경 사용해 보기](media/msal-net-use-brokers-with-xamarin-apps/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

1. **플랫폼 추가를**선택 합니다.

   ![플랫폼 추가](media/msal-net-use-brokers-with-xamarin-apps/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

1. 플랫폼 목록이 지원 되는 경우 **iOS**를 선택 합니다.

   ![IOS 구성](media/msal-net-use-brokers-with-xamarin-apps/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

1. 요청에 따라 번들 ID를 입력 하 고 **구성**을 선택 합니다.

   ![번들 ID 입력](media/msal-net-use-brokers-with-xamarin-apps/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

1. 리디렉션 URI가 계산 됩니다.

   ![리디렉션 URI 복사](media/msal-net-use-brokers-with-xamarin-apps/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

## <a name="brokered-authentication-for-android"></a>Android에 대 한 조정 된 인증

MSAL.NET는 현재 Xamarin.ios 플랫폼만 지원 합니다. Xamarin Android 플랫폼용 broker를 아직 지원 하지 않습니다.

MSAL Android native library는이를 이미 지원 합니다. 자세한 내용은 [Android에서](brokered-auth.md) 조정 된 인증을 참조 하세요.

## <a name="next-steps"></a>다음 단계

[MSAL.NET를 사용 하 여 유니버설 Windows 플랫폼 관련 고려 사항](msal-net-uwp-considerations.md)에 대해 알아봅니다.