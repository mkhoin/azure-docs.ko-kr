---
title: Azure Portal 및 다양 한 Sdk를 사용 하 여 파티션 키가 많은 Azure Cosmos 컨테이너를 만듭니다.
description: Azure Portal 및 다른 Sdk를 사용 하 여 파티션 키가 Azure Cosmos DB 컨테이너를 만드는 방법에 대해 알아봅니다.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: e2ea934140322a13f07a90f4246bacd3f9dbe6c9
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721100"
---
# <a name="create-containers-with-large-partition-key"></a>파티션 키가 많은 컨테이너 만들기

Azure Cosmos DB는 해시 기반 파티션 구성표를 사용 하 여 데이터의 수평 확장을 구현 합니다. 3 2019 년 5 월 이전에 생성 된 모든 Azure Cosmos 컨테이너는 파티션 키의 첫 100 바이트를 기준으로 해시를 계산 하는 해시 함수를 사용 합니다. 처음 100 바이트와 동일한 파티션 키가 여러 개 있는 경우 해당 논리 파티션은 서비스에서 동일한 논리 파티션으로 간주 됩니다. 이로 인해 파티션 크기 할당량이 잘못 된 것과 같은 문제가 발생할 수 있으며 파티션 키 전체에서 고유 인덱스가 적용 됩니다. 이 문제를 해결 하기 위해 많은 파티션 키가 도입 되었습니다. 이제 Azure Cosmos DB는 최대 2kb의 값이 있는 대량 파티션 키를 지원 합니다.

확장 된 해시 함수 버전의 기능을 사용 하 여 대량 파티션 키를 지원 합니다 .이 기능을 사용 하면 최대 2kb의 긴 파티션 키에서 고유한 해시를 생성할 수 있습니다. 이 해시 버전은 파티션 키 크기와 관계 없이 파티션 키 카디널리티가 높은 시나리오에도 권장 됩니다. 파티션 키 카디널리티는 컨테이너에 있는 논리 파티션 ~ 3만의 순서와 같이 고유한 논리적 파티션 수로 정의 됩니다. 이 문서에서는 Azure Portal 및 다른 Sdk를 사용 하 여 파티션 키가 많은 컨테이너를 만드는 방법을 설명 합니다.

## <a name="create-a-large-partition-key-azure-portal"></a>대량 파티션 키 만들기 (Azure Portal)

큰 파티션 키를 만들려면 Azure Portal를 사용 하 여 새 컨테이너를 만들 때 **내 파티션 키가 100-바이트 옵션 보다 큰지** 확인 합니다. 대량 파티션 키가 필요 하지 않거나 1.18 이전 버전의 Sdk에서 실행 중인 응용 프로그램이 있는 경우 확인란의 선택을 취소 합니다.

![Azure Portal를 사용 하 여 대량 파티션 키 만들기](./media/large-partition-keys/large-partition-key-with-portal.png)

## <a name="create-a-large-partition-key-powershell"></a>대량 파티션 키 만들기 (PowerShell)

대량 파티션 키를 지 원하는 컨테이너를 만들려면를 참조 하세요.

* [큰 파티션 키 크기를 사용 하 여 Azure Cosmos 컨테이너 만들기](manage-with-powershell.md##create-container-big-pk)

## <a name="create-a-large-partition-key-net-sdk"></a>대량 파티션 키 만들기 (.Net SDK)

.NET SDK를 사용 하 여 파티션 키가 많은 컨테이너를 만들려면 `PartitionKeyDefinitionVersion.V2` 속성을 지정 합니다. 다음 예제에서는 파티션 속성을 파티션 내에서 지정 하는 방법을 보여 줍니다.

### <a name="v3-net-sdk"></a>v3 .NET SDK

```csharp
await database.CreateContainerAsync(
    new ContainerProperties(collectionName, $"/longpartitionkey")
    {
        PartitionKeyDefinitionVersion = PartitionKeyDefinitionVersion.V2,
    })
```

### <a name="v2-net-sdk"></a>v2 .NET SDK

```csharp
DocumentCollection collection = await newClient.CreateDocumentCollectionAsync(
database,
     new DocumentCollection
        {
           Id = Guid.NewGuid().ToString(),
           PartitionKey = new PartitionKeyDefinition
           {
             Paths = new Collection<string> {"/longpartitionkey" },
             Version = PartitionKeyDefinitionVersion.V2
           }
         },
      new RequestOptions { OfferThroughput = 400 });
```

## <a name="supported-sdk-versions"></a>지원되는 SDK 버전

다음 최소 버전의 Sdk에서는 많은 파티션 키가 지원 됩니다.

|SDK 형식  | 최소 버전   |
|---------|---------|
|.Net     |    1.18     |
|Java 동기화     |   2.4.0      |
|Java Async   |  2.5.0        |
| REST API | `x-ms-version` 요청 헤더를 사용 하 여 `2017-05-03` 보다 높은 버전입니다.|
| Resource Manager 템플릿 | 버전 2는 `partitionKey` 개체 내의 `"version":2` 속성을 사용 합니다. |

현재 Power BI 및 Azure Logic Apps 내에서 파티션 키가 많은 컨테이너를 사용할 수 없습니다. 이러한 응용 프로그램에서 많은 파티션 키가 없는 컨테이너를 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

* [Azure Cosmos DB에서 분할](partitioning-overview.md)
* [Azure Cosmos DB의 요청 단위](request-units.md)
* [컨테이너 및 데이터베이스의 처리량 프로비전](set-throughput.md)
* [Azure Cosmos 계정 작업](account-overview.md)
