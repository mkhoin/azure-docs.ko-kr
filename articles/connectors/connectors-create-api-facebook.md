---
title: Facebook에 연결
description: Facebook REST API 및 Azure Logic Apps를 사용하여 타임라인 및 페이지 관리
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: 1aa936f902dc17c9a401959c19824f6c581547b1
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789829"
---
# <a name="manage-your-facebook-timeline-and-page-by-using-azure-logic-apps"></a>Azure Logic Apps를 사용 하 여 Facebook 타임 라인 및 페이지 관리

Facebook에 연결하여 타임라인에 게시, 페이지 피드 가져오기 등의 작업을 수행합니다. Facebook을 사용하면 다음과 같은 작업을 수행할 수 있습니다.

* Facebook에서 가져온 데이터를 기반으로 비즈니스 흐름을 빌드합니다. 
* 새 게시를 수신할 때 트리거를 사용합니다.
* 타임라인에 게시, 페이지 피드 가져오기 등의 작업을 사용합니다. 이러한 작업을 사용하여 응답을 가져오고 출력을 다른 작업에 사용할 수 있도록 설정합니다. 예를 들어 타임라인에 새 게시물이 있는 경우 해당 게시물을 가져와서 Twitter 피드에 푸시할 수 있습니다. 

이제 논리 앱을 만들어 시작할 수 있습니다. [논리 앱 만들기](../logic-apps/quickstart-create-first-logic-app-workflow.md)를 참조하세요.

## <a name="create-a-connection-to-facebook"></a>Facebook에 대한 연결 만들기

논리 앱에 이 커넥터를 추가할 때 Facebook에 연결할 권한을 논리 앱에 부여해야 합니다.

1. Facebook 계정에 로그인 합니다.

2. **권한 부여**를 선택하고 논리 앱에서 Facebook에 연결하고 사용할 수 있도록 허용합니다. 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 

## <a name="connector-reference"></a>커넥터 참조

커넥터의 OpenAPI (이전의 Swagger) 파일에 설명 된 대로 트리거, 작업 및 제한과 같은 기술 세부 정보는 [커넥터의 참조 페이지](/connectors/facebook/)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

* 다른 [Logic Apps 커넥터](../connectors/apis-list.md)에 대해 알아봅니다.