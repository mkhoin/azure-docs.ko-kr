---
title: Azure Powershell을 사용 하 여 요새 호스트 만들기 | Microsoft Docs
description: 이 문서에서는 Azure 방호 호스트를 만드는 방법에 대해 알아봅니다.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: 6cae6d258da2ddf0c3bfaade65ae74f1201b67b7
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74121066"
---
# <a name="create-an-azure-bastion-host-using-azure-powershell"></a>Azure PowerShell를 사용 하 여 Azure 방호 호스트 만들기

이 문서에서는 Azure 방호 호스트를 만드는 방법을 보여 줍니다. 가상 네트워크에서 Azure 방호 서비스를 프로 비전 한 후에는 동일한 가상 네트워크의 모든 Vm에서 원활한 RDP/SSH 환경을 사용할 수 있습니다. 이 배포의 단위는 구독/계정 또는 가상 머신이 아니라 가상 네트워크입니다.

## <a name="before-you-begin"></a>시작하기 전에

Azure 구독이 있는지 확인합니다. Azure 구독이 아직 없는 경우 [MSDN 구독자 혜택](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details)을 활성화하거나 [무료 계정](https://azure.microsoft.com/pricing/free-trial)에 등록할 수 있습니다.

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="createhost"></a>요새 호스트 만들기

이 섹션은 Azure PowerShell를 사용 하 여 새 Azure 방호 리소스를 만드는 데 도움이 됩니다.

1. 가상 네트워크 및 Azure 방호 서브넷을 만듭니다. 이름 값 **AzureBastionSubnet**을 사용 하 여 Azure 방호 서브넷을 만들어야 합니다. 이 값을 통해 Azure는 요새 리소스를 배포할 서브넷을 알 수 있습니다. 이는 게이트웨이 서브넷과는 다릅니다. 적어도 a/27 이상의 서브넷 (/27,/26 등)의 서브넷을 사용 해야 합니다. 경로 테이블 또는 위임 없이 **AzureBastionSubnet** 를 만듭니다. **AzureBastionSubnet**에서 네트워크 보안 그룹을 사용 하는 경우 [nsgs 작업](bastion-nsg.md)을 참조 하세요.

   ```azurepowershell-interactive
   $subnetName = "AzureBastionSubnet"
   $subnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   $vnet = New-AzVirtualNetwork -Name "myVnet" -ResourceGroupName " myBastionRG " -Location "westeurope" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

2. Azure 방호에 대 한 공용 IP 주소를 만듭니다. 공용 IP는 RDP/SSH를 액세스할 수 있는 방호 리소스 (443 포트)에 대 한 공용 IP 주소입니다. 공용 IP 주소는 만들려는 방호 리소스와 동일한 지역에 있어야 합니다.

   ```azurepowershell-interactive
   $publicip = New-AzPublicIpAddress -ResourceGroupName "myBastionRG" -name "myPublicIP" -location "westeurope" -AllocationMethod Static -Sku Standard
   ```

3. 가상 네트워크의 AzureBastionSubnet에서 새 Azure 방호 리소스를 만드세요. 요새 리소스를 만들고 배포 하는 데 약 5 분이 걸립니다.

   ```azurepowershell-interactive
   $bastion = New-AzBastion -ResourceGroupName " myBastionRG " -Name "myBastion" -PublicIpAddress $publicip -VirtualNetwork $vnet
   ```

## <a name="next-steps"></a>다음 단계

[요새 FAQ](bastion-faq.md)를 읽어 보세요.
