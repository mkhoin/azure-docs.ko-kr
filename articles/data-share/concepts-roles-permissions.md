---
title: Azure Data Share의 역할 및 요구 사항
description: 데이터 공급자 및 데이터 소비자가 Azure 데이터 공유에서 데이터를 공유 하는 데 필요한 액세스 제어 역할 및 요구 사항에 대해 알아봅니다.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 34c73a6bd400da076c68f308a2100a0f4569bd04
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490582"
---
# <a name="roles-and-requirements-for-azure-data-share"></a>Azure Data Share의 역할 및 요구 사항 

이 문서에서는 azure 데이터 공유를 사용 하 여 데이터를 공유 하 고 Azure 데이터 공유를 사용 하 여 데이터를 수락 및 수신 하는 데 필요한 역할을 설명 합니다. 

## <a name="roles-and-requirements"></a>역할 및 요구 사항

Azure 데이터 공유는 Azure 서비스 (이전에는 Msi)의 관리 되는 Id를 사용 하 여 데이터 공급자가 공유할 데이터를 읽을 수 있을 뿐만 아니라 데이터 소비자로 공유 되는 데이터를 받을 수 있도록 기본 저장소 계정에 인증 합니다. 따라서 데이터 공급자와 데이터 소비자 간에는 자격 증명 교환이 없습니다. 

관리 서비스 ID에는 기본 저장소 계정 또는 SQL database에 대 한 액세스 권한을 부여 해야 합니다. Azure 데이터 공유 서비스는 Azure 데이터 공유 리소스의 관리 서비스 ID을 사용 하 여 데이터를 읽고 씁니다. Azure 데이터 공유의 사용자에 게는 데이터를 공유 하는 저장소 계정 또는 SQL database에 대 한 관리 서비스 ID에 대 한 역할 할당을 만들 수 있는 기능이 필요 합니다. 

저장소의 경우 역할 할당을 만들 수 있는 권한이 **소유자** 역할, 사용자 액세스 관리자 역할 또는 Microsoft의 사용자 지정 역할 (권한 부여/역할 할당/쓰기 권한이 할당 됨)에 있습니다. 

해당 하는 저장소 계정의 소유자가 아닌 경우 Azure 데이터 공유 리소스의 관리 되는 Id에 대 한 역할 할당을 직접 만들 수 없는 경우 Azure 관리자에 게 사용자를 대신 하 여 역할 할당을 만들도록 요청할 수 있습니다. 

다음은 데이터 공유 리소스 관리 Id에 할당 된 역할에 대 한 요약입니다.

| |  |  |
|---|---|---|
|**저장소 유형**|**Data Provider 저장소**|**데이터 소비자 대상 저장소**|
|Azure Blob Storage| 저장소 Blob 데이터 판독기 | 저장소 Blob 데이터 기여자
|Azure Data Lake Gen1 | 소유자 | 지원되지 않음
|Azure Data Lake Gen2 | 저장소 Blob 데이터 판독기 | 저장소 Blob 데이터 기여자
|Azure SQL | dbo | dbo 
|

### <a name="data-providers"></a>데이터 공급자 
Azure 데이터 공유에 데이터 집합을 추가 하려면 데이터 공급자 데이터 공유 리소스 관리 id를 Storage Blob 데이터 판독기 역할에 추가 해야 합니다. 사용자가 Azure를 통해 데이터 집합을 추가 하 고 저장소 계정의 소유자 이거나 Microsoft. 권한 부여/역할 할당/쓰기 권한이 할당 된 사용자 지정 역할의 멤버인 경우 Azure 데이터 공유 서비스에서 자동으로이 작업을 수행 합니다. 

또는 사용자가 Azure 관리자에 게 저장소 Blob 데이터 판독기 역할에 데이터 공유 리소스 관리 id를 수동으로 추가할 수 있습니다. 관리자가 수동으로이 역할 할당을 만들면 저장소 계정의 소유자 이거나 사용자 지정 역할을 할당 하는 것이 무효가 됩니다. 이는 Azure Storage 또는 Azure Data Lake Gen2에서 공유 되는 데이터에 적용 됩니다. 

Azure Data Lake Gen1에서 데이터를 공유 하는 경우 역할 할당은 소유자 역할에 대해 수행 되어야 합니다. 

데이터 공유 리소스의 관리 되는 Id에 대 한 역할 할당을 만들려면 다음 단계를 수행 합니다.

1. 저장소 계정으로 이동 합니다.
1. **Access Control(IAM)** 을 선택합니다.
1. **역할 할당 추가를**선택 합니다.
1. *역할*아래에서 *저장소 Blob 데이터 판독기*를 선택 합니다.
1. *선택*아래에서 Azure 데이터 공유 계정의 이름을 입력 합니다.
1. *Save*를 클릭합니다.

SQL 기반 원본의 경우 Azure 데이터 공유 계정과 동일한 이름을 사용 하 여 데이터를 공유 하는 SQL 데이터베이스의 외부 공급자에서 사용자를 만들어야 합니다. SQL 기반 공유의 다른 필수 구성 요소와 함께 샘플 스크립트는 [데이터 공유](share-your-data.md) 자습서에서 찾을 수 있습니다. 

### <a name="data-consumers"></a>데이터 소비자
데이터를 받으려면 데이터 소비자 데이터 공유 리소스 관리 id를 sql 데이터베이스의 저장소 Blob 데이터 참가자 역할 및/또는 dbo 역할에 추가 해야 합니다. 

저장소의 경우 사용자가 Azure를 통해 데이터 집합을 추가 하 고 저장소 계정의 소유자 이거나 Microsoft. 권한 부여/역할 할당/쓰기 권한이 있는 사용자 지정 역할의 멤버인 경우 Azure 데이터 공유 서비스에서 자동으로이 작업을 수행 합니다. 지정. 

또는 사용자가 Azure 관리자에 게 저장소 Blob 데이터 참가자 역할에 데이터 공유 리소스 관리 id를 수동으로 추가할 수 있습니다. 관리자가 수동으로이 역할 할당을 만들면 저장소 계정의 소유자 이거나 사용자 지정 역할을 할당 하는 것이 무효가 됩니다. 이는 Azure Storage 또는 Azure Data Lake Gen2으로 공유 되는 데이터에 적용 됩니다. Azure Data Lake Gen1에 대 한 데이터 수신은 지원 되지 않습니다. 

데이터 공유 리소스의 관리 되는 Id에 대 한 역할 할당을 수동으로 만들려면 다음 단계를 수행 합니다.

1. 저장소 계정으로 이동 합니다.
1. **Access Control(IAM)** 을 선택합니다.
1. **역할 할당 추가를**선택 합니다.
1. *역할*아래에서 *저장소 Blob 데이터 참가자*를 선택 합니다. 
1. *선택*아래에서 Azure 데이터 공유 계정의 이름을 입력 합니다.
1. *Save*를 클릭합니다.

REST Api를 사용 하 여 데이터를 공유 하는 경우의 데이터 공유 계정을 적절 한 역할에 추가 하 여 이러한 역할 할당을 수동으로 만들어야 합니다. 

SQL 기반 원본으로 데이터를 수신 하는 경우 Azure 데이터 공유 계정과 동일한 이름의 외부 공급자에서 새 사용자를 만들었는지 확인 합니다. [데이터 적용 및 수신](subscribe-to-data-share.md) 자습서의 필수 구성 요소를 참조 하세요. 

역할 할당을 추가 하는 방법에 대 한 자세한 내용은 Azure 리소스에 역할 할당을 추가 하는 방법을 설명 하는 [이 설명서](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) 를 참조 하세요. 

## <a name="resource-provider-registration"></a>리소스 공급자 등록 

Azure 데이터 공유 초대를 수락 하는 경우 구독에 DataShare 리소스 공급자를 수동으로 등록 해야 합니다. Azure 구독에 DataShare 리소스 공급자를 등록 하려면 다음 단계를 수행 합니다. 

1. Azure Portal에서 **구독**으로 이동 합니다.
1. Azure 데이터 공유에 사용 중인 구독을 선택 합니다.
1. **리소스 공급자**를 클릭 합니다.
1. DataShare를 검색 합니다.
1. **등록**을 클릭합니다.

## <a name="next-steps"></a>다음 단계

- Azure - [역할 정의 이해](../role-based-access-control/role-definitions.md)에서 역할에 대해 자세히 알아봅니다.

