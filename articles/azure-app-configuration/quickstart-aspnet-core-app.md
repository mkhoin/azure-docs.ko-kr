---
title: ASP.NET Core 지원 Azure App Configuration용 빠른 시작 | Microsoft Docs
description: ASP.NET Core 앱 지원 Azure App Configuration 사용 빠른 시작
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET Core
ms.workload: tbd
ms.date: 10/11/2019
ms.author: yegu
ms.openlocfilehash: 91712b3f730317e65cda7b48c8f5636b2fb9ab2c
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185096"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>빠른 시작: App Configuration을 사용하여 ASP.NET Core 앱 만들기

이 빠른 시작에서는 Azure App Configuration을 ASP.NET Core 앱에 통합하여 코드와 별도로 애플리케이션 설정의 스토리지 및 관리를 중앙 집중화합니다. ASP.NET Core는 애플리케이션에서 지정한 하나 이상의 데이터 원본의 설정을 사용하여 단일 키-값 기반 구성 개체를 작성합니다. 이러한 데이터 원본은 *구성 공급자*라고 합니다. App Configuration의 .NET Core 클라이언트가 이러한 공급자로 구현되므로 서비스가 다른 데이터 원본처럼 표시됩니다.

## <a name="prerequisites"></a>필수 조건

- Azure 구독 - [체험 구독 만들기](https://azure.microsoft.com/free/)
- [.NET Core SDK](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>App Configuration 저장소 만들기

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. **구성 탐색기** >  **+ 만들기**를 선택하여 다음 키-값 쌍을 추가합니다.

    | 키 | 값 |
    |---|---|
    | TestApp:Settings:BackgroundColor | 흰색 |
    | TestApp:Settings:FontSize | 24 |
    | TestApp:Settings:FontColor | 검정 |
    | TestApp:Settings:Message | Azure App Configuration의 정보 |

    지금은 **레이블**과 **콘텐츠 형식**을 비워 두세요.

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core 웹앱 만들기

[.NET Core CLI(명령줄 인터페이스)](https://docs.microsoft.com/dotnet/core/tools/)를 사용하여 새 ASP.NET Core MVC 웹앱 프로젝트를 만듭니다. Visual Studio 대신 .NET Core CLI를 사용하면 Windows, macOS 및 Linux 플랫폼에서 사용할 수 있다는 이점이 있습니다.

1. 프로젝트에 대한 새 폴더를 만듭니다. 이 빠른 시작에서는 *TestAppConfig*라는 이름으로 지정합니다.

2. 새 폴더에서 다음 명령을 실행하여 새 ASP.NET Core MVC 웹앱 프로젝트를 만듭니다.

    ```CLI
        dotnet new mvc --no-https
    ```

## <a name="add-secret-manager"></a>비밀 관리자 추가

비밀 관리자를 사용하려면 *.csproj* 파일에 `UserSecretsId` 요소를 추가합니다.

- *.csproj* 파일을 엽니다. 여기에 표시된 대로 `UserSecretsId` 요소를 추가합니다. 같은 GUID를 사용해도 되고, 이 값을 원하는 값으로 바꿔도 됩니다. 파일을 저장합니다.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

    <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.App" />
        <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
    </ItemGroup>

    </Project>
    ```

암호 관리자 도구는 개발 작업에 대한 중요한 데이터를 프로젝트 트리 외부에 저장합니다. 이 방법을 사용하면 소스 코드 내에서 앱 암호를 실수로 공유하는 경우를 방지할 수 있습니다. 비밀 관리자에 대한 자세한 내용은 [ASP.NET Core에서 개발 중인 앱 비밀 보안 스토리지](https://docs.microsoft.com/aspnet/core/security/app-secrets)를 참조하세요.

## <a name="connect-to-an-app-configuration-store"></a>App Configuration 저장소에 연결

1. 다음 명령을 실행하여 `Microsoft.Azure.AppConfiguration.AspNetCore` NuGet 패키지에 대한 참조를 추가합니다.

    ```CLI
        dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore --version 2.0.0-preview-010060003-1250
    ```
2. 다음 명령을 실행하여 프로젝트에 대한 패키지를 복원합니다.

    ```CLI
        dotnet restore
    ```
3. *ConnectionStrings:AppConfig*라는 비밀을 비밀 관리자에 추가합니다.

    이 비밀에는 App Configuration 저장소에 액세스하기 위한 연결 문자열이 포함되어 있습니다. 다음 명령의 값을 App Configuration 저장소의 연결 문자열로 바꿉니다.

    이 명령은 *.csproj* 파일이 있는 동일한 디렉터리에서 실행해야 합니다.

    ```CLI
        dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    > [!IMPORTANT]
    > 일부 셸에서는 연결 문자열을 따옴표로 묶지 않으면 연결 문자열이 잘립니다. `dotnet user-secrets` 명령의 출력에 전체 연결 문자열이 표시되는지 확인합니다. 표시되지 않으면 연결 문자열을 따옴표로 묶고 명령을 다시 실행합니다.

    비밀 관리자는 웹앱을 로컬로 테스트하는 데만 사용됩니다. 예를 들어 앱이 [Azure App Service](https://azure.microsoft.com/services/app-service/web)에 배포되면 비밀 관리자 대신 App Service의 애플리케이션 설정 **연결 문자열**을 사용하여 연결 문자열을 저장합니다.

    이 비밀은 구성 API를 사용하여 액세스됩니다. 콜론(:)은 지원되는 모든 플랫폼에서 구성 API를 통해 구성 이름으로 작동합니다. [환경별 구성](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0)을 참조하세요.

4. *Program.cs*를 열고, .NET Core App Configuration 공급자에 대한 참조를 추가합니다.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

5. `config.AddAzureAppConfiguration()` 메서드를 호출하여 App Configuration을 사용하도록 `CreateWebHostBuilder` 메서드를 업데이트합니다.
    
    > [!IMPORTANT]
    > .NET Core 3.0에서 `CreateHostBuilder`는 `CreateWebHostBuilder`를 대체합니다.  사용자 환경에 따라 올바른 구문을 선택합니다.

    ### <a name="update-createwebhostbuilder-for-net-core-2x"></a>.NET Core 2.x용 `CreateWebHostBuilder` 업데이트

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

    ### <a name="update-createhostbuilder-for-net-core-3x"></a>.NET Core 3.x용 `CreateHostBuilder` 업데이트

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
        })
        .UseStartup<Startup>());
    ```

6. 보기 > 홈 디렉터리에서 *Index.cshtml*을 열고, 해당 콘텐츠를 다음 코드로 바꿉니다.

    ```HTML
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"];
        }
    </style>

    <h1>@Configuration["TestApp:Settings:Message"]</h1>
    ```

7. 보기 > 공유 디렉터리에서 *_Layout.cshtml*을 열고, 해당 콘텐츠를 다음 코드로 바꿉니다.

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>@ViewData["Title"] - hello_world</title>

        <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
        <link rel="stylesheet" href="~/css/site.css" />
    </head>
    <body>
        <div class="container body-content">
            @RenderBody()
        </div>

        <script src="~/lib/jquery/dist/jquery.js"></script>
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>

        @RenderSection("Scripts", required: false)
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>로컬로 앱 빌드 및 실행

1. .NET Core CLI를 사용하여 앱을 빌드하려면 명령 셸에서 다음 명령을 실행합니다.

    ```CLI
       dotnet build
    ```

2. 빌드가 성공적으로 완료되면 다음 명령을 실행하여 웹앱을 로컬로 실행합니다.

    ```CLI
        dotnet run
    ```

3. 브라우저 창을 열고, 로컬로 호스팅되는 웹앱에 대한 기본 URL인 `http://localhost:5000`으로 이동합니다.

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 새 App Configuration 저장소를 만들고, [App Configuration 공급자](https://go.microsoft.com/fwlink/?linkid=2074664)를 통해 ASP.NET Core 웹앱에서 사용했습니다. 구성 설정을 동적으로 새로 고치도록 ASP.NET Core 앱을 구성하는 방법을 알아보려면 다음 자습서를 계속 진행하세요.

> [!div class="nextstepaction"]
> [동적 구성을 사용하도록 설정](./enable-dynamic-configuration-aspnet-core.md)
