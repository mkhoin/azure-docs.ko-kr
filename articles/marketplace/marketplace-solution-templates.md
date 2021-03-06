---
title: Azure 응용 프로그램 솔루션 템플릿 제품 게시 가이드 | Azure Marketplace
description: 이 문서에서는 솔루션 템플릿을 Azure Marketplace에 게시하기 위한 요구 사항을 설명합니다.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: ellacroi
manager: nunoc
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 9/25/2019
ms.author: ellacroi
ms.openlocfilehash: 934a5e050e190c9a1f90bb3a22c2d1323a3ccecf
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73808294"
---
# <a name="azure-applications-solution-template-offer-publishing-guide"></a>Azure 애플리케이션: 솔루션 템플릿 제품 게시 가이드

솔루션 템플릿은 Marketplace에서 솔루션을 게시하는 주요 방법 중 하나입니다. 이 가이드를 사용하여 이 제품에 대한 요구 사항을 이해합니다. 

솔루션에 단일 VM 이외의 추가 배포 및 구성 자동화가 필요한 경우, Azure 앱: 솔루션 템플릿 제품 유형을 사용합니다. Azure 앱: 솔루션 템플릿을 사용하여 하나 이상의 VM 프로비전을 자동화할 수 있습니다. 네트워킹 및 스토리지 리소스를 프로비전할 수도 있습니다. Azure 앱: 솔루션 템플릿 제품 형식은 단일 VM 및 전체 IaaS 기반 솔루션에 대한 자동화 이점을 제공합니다.

이러한 솔루션 템플릿은 트랜잭션 제안이 아니라 Microsoft의 상용 marketplace를 통해 청구 되는 유료 VM 제품을 배포 하는 데 사용할 수 있습니다. 사용자에게 표시되는 작업에 대한 호출은 "지금 가져오기"입니다.


## <a name="requirements-for-solution-templates"></a>솔루션 템플릿에 대한 요구 사항

| **요구 사항** | **세부 정보**  |
| ---------------  | -----------  |
|청구 및 계량    |  리소스는 고객의 Azure 구독에서 프로 비전 됩니다. 종 량 제 (PAYGO) 가상 머신은 고객의 Azure 구독을 통해 청구 되는 Microsoft를 통해 고객에 게 거래 됩니다 (PAYGO).  <br/> BYOL(사용자 라이선스 필요)의 경우 Microsoft가 고객 구독에서 발생하는 인프라 비용을 청구하는 한편, 소프트웨어 라이선스 요금은 고객과 직접 거래합니다.   |
|Azure 호환 VHD(가상 하드 디스크)  |   VM은 Windows 또는 Linux에서 빌드해야 합니다.  자세한 내용은 [Azure 호환 VHD 만들기](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md)를 참조하세요. |
| 고객 사용량 특성 | 고객 사용량 특성 활성화는 Azure Marketplace에 게시된 모든 솔루션 템플릿에 필요합니다. 고객 사용량 특성과 활성화 방법에 대한 자세한 내용은 [Azure 파트너 고객 사용량 특성](./azure-partner-customer-usage-attribution.md)을 참조하세요.  |
| Managed Disks 사용 | [Managed Disks](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) 은 Azure에서 IaaS vm의 지속형 디스크에 대 한 기본 옵션입니다. 솔루션 템플릿에서 Managed Disks를 사용 해야 합니다. <br> <br> 1. Azure ARM 템플릿에서 Managed Disks를 사용 하 여 솔루션 템플릿을 업데이트 하는 방법에 대 한 [지침](https://docs.microsoft.com/azure/virtual-machines/windows/using-managed-disks-template-deployments) 및 [샘플](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md) 을 따릅니다. <br> <br> 2. 아래 지침에 따라 Managed Disks의 기본 VHD를 저장소 계정으로 가져와 Marketplace에 이미지로 VHD를 게시할 수 있습니다. <br> <ul> <li> [PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd?toc=%2fpowershell%2fmodule%2ftoc.json) </li> <li> [CLI](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd?toc=%2fcli%2fmodule%2ftoc.json) </li> </ul> |

## <a name="next-steps"></a>다음 단계
아직 수행하지 않은 경우 Marketplace에 [등록](https://azuremarketplace.microsoft.com/sell)합니다.

이미 등록되어 있으며 새로운 제품을 만들거나 기존 제품으로 작업하는 경우 [Cloud 파트너 포털](https://cloudpartner.azure.com)에 로그인하여 제품을 만들거나 완료합니다.
