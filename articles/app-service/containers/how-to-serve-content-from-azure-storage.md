---
title: Linux에서 사용자 지정 저장소 컨테이너 연결
description: Azure App Service에서 Linux 컨테이너에 사용자 지정 네트워크 공유를 연결 하는 방법에 대해 알아봅니다. 앱 간에 파일을 공유 하 고, 원격으로 정적 콘텐츠를 관리 하 고, 로컬에서 액세스 합니다.
author: msangapu-msft
ms.topic: article
ms.date: 2/04/2019
ms.author: msangapu
ms.openlocfilehash: 00c60edeefa5fd8d1304aa5fc301a3b0304f5ca3
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671783"
---
# <a name="attach-azure-storage-containers-to-linux-containers"></a>Linux 컨테이너에 Azure Storage 컨테이너 연결

이 가이드에서는 [Azure Storage](/azure/storage/common/storage-introduction)를 사용 하 여 Linux의 App Service에 네트워크 공유를 연결 하는 방법을 보여 줍니다. 보안 콘텐츠, 콘텐츠 이식성, 영구 저장소, 여러 앱에 대 한 액세스 및 여러 가지 전송 방법 등의 이점이 있습니다.

## <a name="prerequisites"></a>전제 조건

- 기존 웹앱(Linux의 App Service 또는 Web App for Containers)
- [Azure CLI](/cli/azure/install-azure-cli)(2.0.46 이상)

## <a name="create-azure-storage"></a>Azure Storage 만들기

> [!NOTE]
> Azure Storage는 기본이 아닌 스토리지이며 별도로 청구되고, 웹앱에 포함되지 않습니다.
>
> 사용자 고유의 저장소를 사용 하면 인프라 제한으로 인 한 저장소 방화벽 구성 사용을 지원 하지 않습니다.
>

Azure [Azure 스토리지 계정](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli)을 만듭니다.

```azurecli
#Create Storage Account
az storage account create --name <storage_account_name> --resource-group myResourceGroup

#Create Storage Container
az storage container create --name <storage_container_name> --account-name <storage_account_name>
```

## <a name="upload-files-to-azure-storage"></a>Azure Storage에 파일 업로드

스토리지 계정에 로컬 디렉터리를 업로드하려면 다음 예제와 같이 [`az storage blob upload-batch`](https://docs.microsoft.com/cli/azure/storage/blob?view=azure-cli-latest#az-storage-blob-upload-batch) 명령을 사용합니다.

```azurecli
az storage blob upload-batch -d <full_path_to_local_directory> --account-name <storage_account_name> --account-key "<access_key>" -s <source_location_name>
```

## <a name="link-storage-to-your-web-app-preview"></a>웹앱에 스토리지 연결(미리 보기)

> [!CAUTION]
> 웹앱의 기존 디렉터리를 스토리지 계정에 연결하면 디렉터리 콘텐츠가 삭제됩니다. 기존 앱에 대한 파일을 마이그레이션하는 경우 시작하기 전에 앱 및 해당 콘텐츠의 백업을 만듭니다.
>

스토리지 계정을 App Service 앱의 디렉터리에 탑재하려면 [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) 명령을 사용합니다. 스토리지 형식은 AzureBlob 또는 AzureFiles일 수 있습니다. 이 컨테이너에 AzureBlob을 사용합니다.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureBlob --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

스토리지 계정에 연결하려는 다른 디렉터리에 대해 이 작업을 수행해야 합니다.

## <a name="verify"></a>Verify

스토리지 컨테이너가 웹앱에 연결되면 다음 명령을 실행하여 이를 확인할 수 있습니다.

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="use-custom-storage-in-docker-compose"></a>Docker Compose에서 사용자 지정 저장소 사용

Azure Storage는 사용자 지정 id를 사용 하 여 다중 컨테이너 앱으로 탑재할 수 있습니다. 사용자 지정 id 이름을 보려면 [`az webapp config storage-account list --name <app_name> --resource-group <resource_group>`](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-list)를 실행 합니다.

*Docker-compose.ci.build.yml* 파일에서 `volumes` 옵션을 `custom-id`에 매핑합니다. 다음은 그 예입니다.

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - <custom-id>:<path_in_container>
```

## <a name="next-steps"></a>다음 단계

- [Azure App Service에서 웹앱 구성](../configure-common.md)
