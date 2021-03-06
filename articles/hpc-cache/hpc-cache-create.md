---
title: Azure HPC Cache 만들기
description: Azure HPC Cache 인스턴스를 만드는 방법
author: ekpgh
ms.service: hpc-cache
ms.topic: tutorial
ms.date: 11/11/2019
ms.author: rohogue
ms.openlocfilehash: 07aba1b1536635e414fc5fab4ece148683909188
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74168614"
---
# <a name="create-an-azure-hpc-cache"></a>Azure HPC Cache 만들기

Azure Portal을 사용하여 캐시를 만듭니다.

![맨 아래에 만들기 단추가 있는, Azure Portal의 캐시 개요 스크린샷](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>기본 세부 정보 정의

![Azure Portal의 프로젝트 세부 정보 페이지 스크린샷](media/hpc-cache-create-basics.png)

**프로젝트 세부 정보**에서 캐시를 호스트할 구독 및 리소스 그룹을 선택합니다. 구독이 [액세스](hpc-cache-prereqs.md#azure-subscription) 목록에 있는지 확인합니다.

**서비스 세부 정보**에서 캐시 이름과 다음 기타 특성을 설정합니다.

* 위치 - [지원되는 지역](hpc-cache-overview.md#region-availability) 중 하나를 선택합니다.
* 가상 네트워크 - 기존 가상 네트워크를 선택하거나 새 가상 네트워크를 만들 수 있습니다.
* 서브넷 - 이 Azure HPC Cache 인스턴스에만 사용되고 IP 주소가 64개 이상(/24)인 서브넷을 선택하거나 만듭니다.

## <a name="set-cache-capacity"></a>캐시 용량 설정
<!-- referenced from GUI - update aka.ms link if you change this header text -->

**캐시** 페이지에서 캐시의 용량을 설정해야 합니다. 여기에 설정된 값은 캐시가 보유할 수 있는 데이터의 양과 클라이언트 요청을 얼마나 빠르게 처리할 수 있는지를 결정됩니다.

공개 미리 보기 기간 이후에는 용량이 캐시 비용에도 영향을 줍니다.

다음 두 값을 설정하여 용량을 선택합니다.

* 캐시의 최대 데이터 전송률(처리량)(GB/초)
* 캐시된 데이터에 할당되는 스토리지의 양(TB)

사용 가능한 처리량 값과 캐시 스토리지 크기 중 하나를 선택합니다.

실제 데이터 전송률은 워크로드, 네트워크 속도 및 스토리지 대상 유형에 따라 달라집니다. 선택한 값은 전체 캐시 시스템의 최대 처리량을 설정하지만 일부는 오버헤드 작업에 사용됩니다. 예를 들어 클라이언트가 캐시에 아직 저장되지 않은 파일을 요청하거나 파일이 오래된 것으로 표시된 경우 캐시는 일부 처리량을 사용하여 백 엔드 스토리지에서 가져옵니다.

Azure HPC Cache는 캐시 적중률을 최대화하기 위해 캐시되고 미리 로드되는 파일을 관리합니다. 캐시 콘텐츠는 지속적으로 평가되며, 자주 액세스하지 않는 파일은 장기 스토리지로 이동됩니다. 메타데이터와 기타 오버헤드를 위한 추가 공간과 함께 활성 작업 파일 세트를 여유 있게 저장할 수 있는 캐시 스토리지 크기를 선택합니다.

![캐시 크기 페이지 스크린샷](media/hpc-cache-create-capacity.png)

## <a name="add-resource-tags-optional"></a>리소스 태그 추가(선택 사항)

**태그** 페이지에서는 Azure HPC Cache 인스턴스에 [리소스 태그](https://go.microsoft.com/fwlink/?linkid=873112)를 추가할 수 있습니다.

## <a name="finish-creating-the-cache"></a>캐시 만들기 완료

새 캐시를 구성한 후 **검토 + 만들기** 탭을 클릭합니다. 포털에서 선택 항목의 유효성을 검사하고 사용자가 선택 항목을 검토할 수 있도록 합니다. 모든 선택 항목이 올바르면 **만들기**를 클릭합니다.

캐시를 만드는 데 10분 정도 걸립니다. Azure Portal의 알림 패널에서 진행 상황을 추적할 수 있습니다.

![포털의 캐시 만들기 "배포 진행 중" 및 "알림" 페이지의 스크린샷](media/hpc-cache-deploy-status.png)

만들기가 완료되면 새 Azure HPC Cache 인스턴스에 대한 링크가 포함된 알림이 표시되고, 해당 캐시가 구독의 **리소스** 목록에 표시됩니다.
<!-- double check on notification -->

![Azure Portal의 Azure HPC Cache 인스턴스 스크린샷](media/hpc-cache-new-overview.png)

## <a name="next-steps"></a>다음 단계

캐시가 **리소스** 목록에 표시되면 데이터 원본에 대한 캐시 액세스를 제공할 스토리지 대상을 정의합니다.

* [스토리지 대상 추가](hpc-cache-add-storage.md)
