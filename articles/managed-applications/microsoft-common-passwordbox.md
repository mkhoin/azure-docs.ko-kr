---
title: Azure PasswordBox UI 요소 | Microsoft Docs
description: Azure Portal의 Microsoft.Common.PasswordBox UI 요소에 대해 설명합니다. 사용자가 관리 되는 응용 프로그램을 배포할 때 비밀 값을 제공할 수 있도록 합니다.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: ab51a4096745c2930199685ac678638a956f21e0
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2019
ms.locfileid: "74151538"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Microsoft.Common.PasswordBox UI 요소

암호를 제공하고 확인하는 데 사용할 수 있는 컨트롤입니다.

## <a name="ui-sample"></a>UI 샘플

![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>스키마

```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-zA-Z0-9]{8,}$",
    "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="sample-output"></a>샘플 출력

```json
"p4ssw0rd"
```

## <a name="remarks"></a>설명

- 이 요소는 `defaultValue` 속성을 지원하지 않습니다.
- `constraints`의 구현에 대한 자세한 내용은 [Microsoft.Common.TextBox](microsoft-common-textbox.md)를 참조하세요.
- `options.hideConfirmation`을 **true**로 설정하면 사용자의 암호를 확인하기 위한 두 번째 텍스트 상자가 숨겨집니다. 기본값은 **false**입니다.

## <a name="next-steps"></a>다음 단계

* UI 정의 만들기에 대한 소개는 [CreateUiDefinition 시작](create-uidefinition-overview.md)을 참조하세요.
* UI 요소의 공용 속성에 대한 설명은 [CreateUiDefinition 요소](create-uidefinition-elements.md)를 참조하세요.
