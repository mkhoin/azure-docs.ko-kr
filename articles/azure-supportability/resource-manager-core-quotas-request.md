---
title: Azure Resource Manager vCPU 할당량 증가 요청 | Microsoft Docs
description: Azure Resource Manager vCPU 할당량 증가 요청
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: c3248b10b4ad343e8776056d42ec153130f0061f
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72248647"
---
# <a name="quota-increase-requests"></a>할당량 증가 요청

가상 머신 및 가상 머신 확장 집합에 대 한 리소스 관리자 vCPU 할당량은 각 지역의 각 구독에 대해 두 개의 계층으로 적용 됩니다. 

첫 번째 계층은 총 지역 vCPUs 한도 (모든 VM 시리즈에서)이 고 두 번째 계층은 VM 시리즈 vCPUs 제한 (예: D 시리즈 vCPUs)입니다. 새 VM을 배포할 때마다 해당 VM 시리즈에 대 한 새로운 및 기존 vCPUs 사용량의 합계는 특정 VM 시리즈에 대해 승인 된 Vcpus 할당량을 초과 하지 않아야 합니다. 또한 모든 VM 시리즈에서 배포 된 새로운 총 및 기존 vCPU 수는 해당 구독에 대해 승인 된 총 지역 Vcpu 할당량을 초과할 수 없습니다. 이러한 할당량 중 하나가 초과되면 VM 배포가 허용되지 않습니다.
Azure Portal에서 VM 시리즈에 대 한 vCPUs 할당량 한도 증가를 요청할 수 있습니다. VM 시리즈 할당량을 늘리면 전체 지역 vCPUs 한도가 같은 양만큼 자동으로 증가 합니다. 

새 구독을 만들 때 기본 총 지역 vCPUs가 모든 개별 VM 시리즈에 대 한 기본 Vcpus 할당량의 합계와 같지 않을 수 있습니다. 이로 인해 배포 하려는 각 개별 VM 시리즈에 대 한 할당량이 충분 하지만 모든 배포에 대 한 총 지역 vCPUs 할당량은 충분 하지 않은 구독을 만들 수 있습니다. 이 경우 전체 지역 vCPUs 제한을 명시적으로 늘리기 위해 요청을 제출 해야 합니다. 총 지역 vCPUs 제한은 해당 지역에 대 한 모든 VM 시리즈에서 승인 된 할당량의 합계를 초과할 수 없습니다.

[가상 머신 vCPU 할당량 페이지](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) 및 [Azure 구독 및 서비스 제한](https://aka.ms/quotalimits) 페이지에서 할당량에 대해 자세히 알아보세요. 

