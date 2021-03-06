---
title: 포함 파일
description: 포함 파일
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b7fddce8f682bc341b361a47f8e083cc281e90aa
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309581"
---
ZRS(영역 중복 스토리지)는 단일 지역에 있는 3개의 스토리지 클러스터에서 데이터를 동기적으로 복제합니다. 각 스토리지 클러스터는 다른 스토리지 클러스터와 물리적으로 분리되어 있으며 자체 AZ(가용성 영역)에 위치합니다. 각 가용성 영역&mdash; 및 그&mdash; 안에 포함된 ZRS 클러스터는 자율적으로 사용되며, 별도의 유틸리티와 네트워킹 기능을 포함합니다. ZRS 저장소 계정에 대 한 쓰기 요청은 3 개 클러스터의 모든 복제본에 데이터가 기록 된 후에만 성공적으로 반환 됩니다.

ZRS 복제를 사용하여 스토리지 계정에 데이터를 저장하는 경우 가용성 영역을 사용할 수 없으면 데이터 액세스 및 관리를 계속할 수 있습니다. ZRS는 뛰어난 성능과 짧은 대기 시간을 제공하며, [LRS(로컬 중복 스토리지)](../articles/storage/common/storage-redundancy-lrs.md)와 동일한 [확장성 대상](../articles/storage/common/storage-scalability-targets.md)을 제공합니다.

일관성, 내구성 및 고가용성이 필요한 시나리오의 경우 ZRS가 좋습니다. 가동 중단 또는 자연 재해가 가용성 영역을 사용할 수 없게 하더라도 ZRS는 지정된 연도 동안 최소 99.9999999999%(12 9)의 스토리지 개체에 대한 내구성을 제공합니다.

GZRS (지역 중복 저장소) (미리 보기)는 기본 지역의 세 Azure 가용성 영역에서 데이터를 동기적으로 복제 한 다음, 데이터를 보조 지역에 비동기적으로 복제 합니다. GZRS는 최고 내구성과 함께 고가용성을 제공 합니다. GZRS는 지정 된 연도 동안 최소 99.99999999999999% (16 9의) 개체 내 구성을 제공 하도록 설계 되었습니다. 보조 지역의 데이터에 대 한 읽기 액세스의 경우 읽기 액세스 지리적 영역 중복 저장소 (RA-GZRS)를 사용 하도록 설정 합니다. GZRS에 대 한 자세한 내용은 고가용성 [및 최대 내구성 (미리 보기)을 위한 지리적 영역 중복 저장소](../articles/storage/common/storage-redundancy-gzrs.md)를 참조 하세요.

가용성 영역에 대한 자세한 내용은 [가용성 영역 개요](https://docs.microsoft.com/azure/availability-zones/az-overview)를 참조하세요.
