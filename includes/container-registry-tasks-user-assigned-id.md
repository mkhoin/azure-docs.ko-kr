---
title: 포함 파일
description: 포함 파일
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/12/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0e2acb346fad87e0c1c7fd7de1389d8fc86206d0
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642112"
---
### <a name="create-a-user-assigned-identity"></a>사용자 할당 ID 만들기

[Az identity create][az-identity-create] 명령을 사용 하 여 구독에 *Myacrtasksid* 라는 id를 만듭니다. 이전에 사용한 것과 동일한 리소스 그룹을 사용 하 여 컨테이너 레지스트리를 만들 수 있습니다.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

다음 단계에서 사용자 할당 id를 구성 하려면 [az identity show][az-identity-show] 명령을 사용 하 여 id의 리소스 id, 서비스 주체 id 및 클라이언트 id를 변수에 저장 합니다.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)

# Get client ID of the user-assigned identity
clientID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query clientId --output tsv)
```

<!-- LINKS - Internal -->
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show