---
title: Azure API Management 정책 샘플 - 상관 관계 ID를 포함하는 헤더 추가 | Microsoft Docs
description: Azure API Management 정책 샘플 - 상관 관계 ID를 포함하는 헤더를 인바운드 요청에 추가하는 방법을 보여줍니다.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 9f091345a4eaf174c47959cad3cb4525fd926689
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70074222"
---
# <a name="add-a-header-containing-a-correlation-id"></a>상관 관계 ID를 포함하는 헤더 추가

이 문서에서는 상관 관계 ID를 포함하는 헤더를 인바운드 요청에 추가하는 방법을 보여 주는 Azure API Management 정책 샘플을 제공합니다. 정책 코드를 설정하거나 편집하려면 [정책 설정 또는 편집](../set-edit-policies.md)에 설명된 단계를 따릅니다. 다른 예제를 보려면 [정책 샘플](../policy-samples.md)을 참조하세요.

## <a name="policy"></a>정책

코드를 **인바운드** 블록에 붙여넣습니다.

[!code-xml[Main](../../../api-management-policy-samples/examples/Add correlation id to inbound request.policy.xml)]

## <a name="next-steps"></a>다음 단계

APIM 정책에 대해 자세히 알아보기:

+ [변환 정책](../api-management-transformation-policies.md)
+ [정책 샘플](../policy-samples.md)

