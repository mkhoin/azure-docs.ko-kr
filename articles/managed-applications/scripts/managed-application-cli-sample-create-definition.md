---
title: Managed Application 정의 만들기 - Azure CLI
description: 구독에서 관리되는 애플리케이션 정의를 만드는 Azure CLI 스크립트 샘플을 제공합니다.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: b5ac119aa2c4d79c046a6eeabe252801496caa1c
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72528918"
---
# <a name="create-a-managed-application-definition-with-azure-cli"></a>Azure CLI를 사용하여 관리되는 애플리케이션 정의 만들기

이 스크립트는 관리되는 애플리케이션 정의를 서비스 카탈로그에 게시합니다. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-definition/create-definition.sh "Create definition")]


## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 관리되는 애플리케이션 정의를 만듭니다. 표에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az managedapp definition create](https://docs.microsoft.com/cli/azure/managedapp/definition#az-managedapp-definition-create) | 관리되는 애플리케이션 정의를 만듭니다. 필요한 파일이 포함된 패키지를 제공합니다. |


## <a name="next-steps"></a>다음 단계

* 관리되는 애플리케이션에 대한 소개는 [Azure Managed Application 개요](../overview.md)를 참조하세요.
* Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](https://docs.microsoft.com/cli/azure)를 참조하세요.
