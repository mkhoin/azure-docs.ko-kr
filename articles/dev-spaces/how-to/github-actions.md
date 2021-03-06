---
title: Azure Kubernetes Service & GitHub 작업
services: azure-dev-spaces
ms.date: 11/04/2019
ms.topic: conceptual
description: GitHub 작업 및 Azure Dev Spaces를 사용 하 여 Azure Kubernetes Service에서 직접 끌어오기 요청에서 변경 내용을 검토 하 고 테스트 합니다.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, 컨테이너, GitHub 작업, 투구, 서비스 메시, 서비스 메시 라우팅, kubectl, k8s
manager: gwallace
ms.openlocfilehash: e20efc6b109eeef234dcd621374d25b812cdc0ce
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483924"
---
# <a name="github-actions--azure-kubernetes-service-preview"></a>Azure Kubernetes Service & GitHub 작업 (미리 보기)

Azure Dev Spaces는 리포지토리의 주 분기에 끌어오기 요청을 병합 하기 전에 AKS에서 직접 끌어오기 요청의 변경을 테스트할 수 있는 GitHub 작업을 사용 하 여 워크플로를 제공 합니다. 끌어오기 요청에 대 한 변경 내용을 검토 하는 실행 중인 응용 프로그램이 있는 경우 개발자와 팀 멤버 모두의 확신도를 높일 수 있습니다. 이 실행 중인 응용 프로그램은 제품 관리자 및 디자이너와 같은 팀 멤버가 개발 초기 단계를 진행 하는 동안 검토 프로세스의 일부가 될 수도 있습니다.

이 가이드에서는 다음을 수행하는 방법을 배우게 됩니다.

* Azure Dev Spaces를 Azure의 관리형 Kubernetes 클러스터에 설정합니다.
* 여러 마이크로서비스가 포함된 대규모 애플리케이션을 개발 공간에 배포합니다.
* GitHub 작업을 사용 하 여 CI/CD를 설정 합니다.
* 전체 애플리케이션의 컨텍스트 내에서 격리된 개발 공간에서 단일 마이크로서비스를 테스트합니다.

> [!IMPORTANT]
> 이 기능은 현재 미리 보기로 제공됩니다. [부속 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)에 동의하면 미리 보기를 사용할 수 있습니다. 이 기능의 몇 가지 측면은 일반 공급(GA) 전에 변경될 수 있습니다.

## <a name="prerequisites"></a>선행 조건

* Azure 구독. Azure 구독이 없는 경우 [체험 계정](https://azure.microsoft.com/free)을 만들 수 있습니다.
* [Azure CLI 설치][azure-cli-installed]
* [투구 2.13-2.16가 설치][helm-installed]되었습니다.
* [Github 작업을 사용][github-actions-beta-signup]하는 github 계정.
* AKS 클러스터에서 실행 되는 [Azure Dev Spaces 자전거 공유 샘플 응용 프로그램](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp/README.md) 입니다.

## <a name="create-an-azure-container-registry"></a>Azure Container Registry 만들기

ACR (Azure Container Registry) 만들기:

```cmd
az acr create --resource-group MyResourceGroup --name <acrName> --sku Basic
```

> [!IMPORTANT]
> ACR 이름은 Azure 내에서 고유 해야 하 고 5-50 영숫자 문자를 포함 해야 합니다. 사용 하는 모든 문자는 소문자 여야 합니다.

이후 단계에서 사용 되므로 출력에서 *loginServer* 값을 저장 합니다.

## <a name="create-a-service-principal-for-authentication"></a>인증을 위한 서비스 주체 만들기

[Az ad sp create-rbac][az-ad-sp-create-for-rbac] 를 사용 하 여 서비스 주체를 만듭니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```cmd
az ad sp create-for-rbac --sdk-auth --skip-assignment
```

이후 단계에서 사용 되므로 JSON 출력을 저장 합니다.


[Az aks show][az-aks-show] 를 사용 하 여 aks 클러스터의 *id* 를 표시 합니다.

```cmd
az aks show -g MyResourceGroup -n MyAKS  --query id
```

[Az acr show][az-acr-show] 를 사용 하 여 acr의 *id* 를 표시 합니다.

```cmd
az acr show --name <acrName> --query id
```

[Az role 할당 create][az-role-assignment-create] 를 사용 하 여 AKS 클러스터에 대 한 *참가자* 액세스 및 ACR에 *acrpush* 액세스를 제공 합니다.

```cmd
az role assignment create --assignee <ClientId> --scope <AKSId> --role Contributor
az role assignment create --assignee <ClientId>  --scope <ACRId> --role AcrPush
```

> [!IMPORTANT]
> 서비스 사용자에 게 해당 리소스에 대 한 액세스 권한을 부여 하려면 AKS 클러스터와 ACR의 소유자 여야 합니다.

## <a name="configure-your-github-action"></a>GitHub 작업 구성

> [!IMPORTANT]
> 리포지토리에 대해 GitHub 동작을 사용 하도록 설정 해야 합니다. 리포지토리에 대 한 GitHub 동작을 사용 하도록 설정 하려면 GitHub에서 리포지토리로 이동 하 고 작업 탭을 클릭 한 다음이 리포지토리에 대 한 작업을 사용 하도록 설정 합니다.

분기 리포지토리로 이동 하 고 *설정*을 클릭 합니다. 왼쪽 세로 막대에서 *비밀* 을 클릭 합니다. *새 암호 추가* 를 클릭 하 여 아래에 새 암호를 각각 추가 합니다.

1. *AZURE_CREDENTIALS*: 서비스 사용자 만들기의 전체 출력입니다.
1. *RESOURCE_GROUP*: AKS 클러스터에 대 한 리소스 그룹입니다 (이 예에서는 *myresourcegroup*).
1. *CLUSTER_NAME*: AKS 클러스터의 이름 (이 예에서는 *MyAKS*)입니다.
1. *CONTAINER_REGISTRY*: ACR에 대 한 *loginServer* 입니다.
1. *HOST*: < MASTER_SPACE >를 사용 하는 개발 공간에 대 한 호스트 *입니다. <* APP_NAME > < HOST_SUFFIX > 합니다 .이 예제에서는이 예에서는 *dev.bikesharingweb.fedcab0987.eus.azds.io*입니다.
1. *HOST_SUFFIX*: Dev 공간에 대 한 호스트 접미사 (이 예제에서는 *fedcab0987.eus.azds.io*)입니다.
1. *IMAGE_PULL_SECRET*: 사용 하려는 비밀의 이름 (예: *데모 암호)* 입니다.
1. *MASTER_SPACE*: 부모 개발 공간의 이름으로,이 예제에서는 *dev*입니다.
1. *REGISTRY_USERNAME*: 서비스 사용자가 만든 JSON 출력의 *clientId* 입니다.
1. *REGISTRY_PASSWORD*: 서비스 사용자가 만든 JSON 출력의 *clientSecret* 입니다.

> [!NOTE]
> 이러한 암호는 모두 GitHub 작업에서 사용 되며 [github/워크플로/자전거 .yml][github-action-yaml]에서 구성 됩니다.

## <a name="create-a-new-branch-for-code-changes"></a>코드 변경에 대 한 새 분기 만들기

`BikeSharingApp/`로 이동 하 고 *자전거 이미지*라는 새 분기를 만듭니다.

```cmd
cd dev-spaces/samples/BikeSharingApp/
git checkout -b bike-images
```

[Bikes/node.js][bikes-server-js] 를 편집 하 여 줄 232 및 233을 제거 합니다.

```javascript
    // Hard code image url *FIX ME*
    theBike.imageUrl = "/static/logo.svg";
```

이제 섹션은 다음과 같습니다.

```javascript
    var theBike = result;
    theBike.id = theBike._id;
    delete theBike._id;
```

파일을 저장 한 다음 `git add` 및 `git commit`를 사용 하 여 변경 내용을 준비 합니다.

```cmd
git add Bikes/server.js 
git commit -m "Removing hard coded imageUrl from /bikes/:id route"
```

## <a name="push-your-changes"></a>변경 내용 푸시

`git push`를 사용 하 여 새 분기를 분기 리포지토리로 푸시합니다.

```cmd
git push origin bike-images
```

푸시가 완료 되 면 GitHub의 분기 리포지토리로 이동 하 여 분기 리포지토리에서 *마스터* 분기를 *자전거 이미지* 분기와 비교 하 여 기본 분기로 사용 하 여 끌어오기 요청을 만듭니다.

끌어오기 요청이 열리면 *작업* 탭으로 이동 합니다. 새 작업이 시작 되었으며 *자전거* 서비스를 빌드하고 있는지 확인 합니다.

## <a name="view-the-child-space-with-your-changes"></a>변경 내용으로 자식 공간 보기

작업이 완료 되 면 끌어오기 요청의 변경 사항을 기반으로 새 하위 공간에 대 한 URL이 포함 된 주석이 표시 됩니다.

> [!div class="mx-imgBorder"]
> ![GitHub 동작 Url](../media/github-actions/github-action-url.png)

주석에서 URL을 열어 *bikesharingweb* 서비스로 이동 합니다. 사용자로 *Aurelia Briggs (고객)* 를 선택 하 고 임대에 대 한 자전거를 선택 합니다. 자전거에 대 한 자리 표시자 이미지가 더 이상 표시 되지 않는지 확인 합니다.

분기의 *마스터* 분기에 변경 내용을 병합 하는 경우 다른 작업을 실행 하 여 부모 개발 공간에서 전체 응용 프로그램을 다시 빌드하고 실행 합니다. 이 예제에서 부모 공간은 *dev*입니다. 이 작업은 [github/워크플로/bikesharing.clients.core][github-action-bikesharing-yaml]에서 구성 됩니다.

## <a name="clean-up-your-azure-resources"></a>Azure 리소스 정리

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>다음 단계

Azure Dev Spaces를 통해 여러 컨테이너에서 더 복잡한 애플리케이션을 개발할 수 있는 방법 및 사용자가 다양한 환경에서 다양한 코드 버전이나 분기로 작업하여 공동 개발을 간소화하는 방법을 알아봅니다.

> [!div class="nextstepaction"]
> [Azure Dev Spaces에서 팀 개발][team-quickstart]

[azure-cli-installed]: /cli/azure/install-azure-cli?view=azure-cli-latest
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-aks-show]: /cli/azure/aks?view=azure-cli-latest#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[bikes-server-js]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/server.js#L232-L233
[bike-sharing-gh]: https://github.com/Azure/dev-spaces/
[bike-sharing-values-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml
[github-actions-beta-signup]: https://github.com/features/actions
[github-action-yaml]: https://github.com/Azure/dev-spaces/blob/master/.github/workflows/bikes.yml
[github-action-bikesharing-yaml]: https://github.com/Azure/dev-spaces/blob/master/.github/workflows/bikesharing.yml
[helm-installed]: https://v2.helm.sh/docs/using_helm/#installing-helm
[tiller-rbac]: https://helm.sh/docs/using_helm/#role-based-access-control
[supported-regions]: ../about.md#supported-regions-and-configurations
[sp-acr]: ../../container-registry/container-registry-auth-service-principal.md
[sp-aks]: ../../aks/kubernetes-service-principal.md
[team-quickstart]: ../quickstart-team-development.md
