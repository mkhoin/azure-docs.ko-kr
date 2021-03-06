---
title: Azure PowerShell 스크립트 샘플 - 관리되는 애플리케이션 배포 | Microsoft Docs
description: 구독에 관리되는 애플리케이션 정의를 배포하는 Azure PowerShell 샘플 스크립트를 제공합니다.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: d450c2292f5d2982b7c8b266cc6780a72d426321
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72332566"
---
# <a name="deploy-a-managed-application-for-a-service-catalog-with-powershell"></a>PowerShell을 사용하여 서비스 카탈로그용 관리되는 애플리케이션 배포

이 스크립트는 서비스 카탈로그에서 관리되는 애플리케이션 정의를 배포합니다.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/managed-applications/create-application/create-application.ps1 "Create application")]


## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 관리되는 애플리케이션을 배포합니다. 표에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [New-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/new-azmanagedapplication) | 관리되는 애플리케이션을 만듭니다. 템플릿에 대한 정의 ID 및 매개 변수를 제공합니다. |


## <a name="next-steps"></a>다음 단계

* 관리되는 애플리케이션에 대한 소개는 [Azure Managed Application 개요](../overview.md)를 참조하세요.
* PowerShell에 대한 자세한 내용은 [Azure PowerShell 설명서](https://docs.microsoft.com/powershell/azure/get-started-azureps)를 참조하세요.
