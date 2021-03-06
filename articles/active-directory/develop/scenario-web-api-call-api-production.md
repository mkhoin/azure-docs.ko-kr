---
title: 웹 api를 호출 하는 웹 api를 프로덕션으로 이동-Microsoft id 플랫폼 | Microsoft
description: 웹 Api를 호출 하는 web API를 프로덕션으로 이동 하는 방법에 대해 알아봅니다.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d59a5b2a74c10e36103713725113cbe8c9cc412
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965172"
---
# <a name="web-api-that-calls-web-apis---move-to-production"></a>웹 Api를 호출 하는 web API-프로덕션으로 이동

웹 Api를 호출 하는 토큰을 획득 한 후에는 앱을 프로덕션으로 이동할 수 있습니다.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>További információ

사용자 고유의 web API에서 web Api를 호출 하는 방법의 기본 사항을 배웠으므로 웹 Api를 호출 하는 보호 된 웹 API를 빌드하는 데 사용 되는 코드를 설명 하는이 자습서를 참조할 수 있습니다.

| Minta | Platform | Leírás |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | ASP.NET Core 2.2 웹 API, 데스크톱 (WPF) | ASP.NET Core 2.2 웹 API 호출 Microsoft Graph (Microsoft identity platform (v2.0)를 사용 하 여 WPF 응용 프로그램에서 호출 됩니다. |
