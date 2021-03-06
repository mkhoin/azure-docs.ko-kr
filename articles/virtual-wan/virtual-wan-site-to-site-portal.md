---
title: 'Azure Virtual WAN: 사이트 간 연결 만들기'
description: 이 자습서에서는 Azure Virtual WAN을 사용하여 Azure에 사이트 간 VPN 연결을 만드는 방법을 알아봅니다.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: e17205af1ede845ea77b04f6f2b4c6babf3bc450
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74482134"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan"></a>자습서: Azure Virtual WAN을 사용하여 사이트 간 연결 만들기

이 자습서에서는 가상 WAN을 사용하여 IPsec/IKE(IKEv1 및 IKEv2) VPN 연결을 통해 Azure에서 리소스를 연결하는 방법을 보여줍니다. 이 연결 유형은 할당된 외부 연결 공용 IP 주소를 갖고 있는 온-프레미스에 있는 VPN 디바이스를 필요로 합니다. Virtual WAN에 대한 자세한 내용은 [Virtual WAN 개요](virtual-wan-about.md)를 참조하세요.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 가상 WAN 만들기
> * 허브 만들기
> * 사이트 만들기
> * 허브에 사이트 연결
> * 허브에 VPN 사이트 연결
> * 허브에 VNet 연결
> * 구성 파일 다운로드
> * 가상 WAN 보기

> [!NOTE]
> 사이트가 많은 경우 일반적으로 [가상 WAN 파트너](https://aka.ms/virtualwan)를 사용하여 이러한 구성을 만듭니다. 하지만 네트워킹에 익숙하고 자신의 VPN 디바이스를 구성하는 데 능숙한 경우에는 구성을 직접 만들 수 있습니다.
>

![Virtual WAN 다이어그램](./media/virtual-wan-about/virtualwan.png)

## <a name="before-you-begin"></a>시작하기 전에

구성을 시작하기 전에 다음 기준을 충족하는지 확인합니다.

* 연결하려는 가상 네트워크가 있습니다. 온-프레미스 네트워크의 어떤 서브넷도 연결하려는 가상 네트워크 서브넷과 중첩되지 않는지 확인합니다. Azure Portal에서 가상 네트워크를 만들려면 [빠른 시작](../virtual-network/quick-create-portal.md)을 참조하세요.

* 가상 네트워크에 가상 네트워크 게이트웨이가 없습니다. 가상 네트워크에 게이트웨이(VPN 또는 ExpressRoute)가 있으면 모든 게이트웨이를 제거해야 합니다. 이 구성을 사용하려면 가상 네트워크가 Virtual WAN 허브 게이트웨이에 연결되어야 합니다.

* 허브 지역의 IP 주소 범위를 확보합니다. 허브는 Virtual WAN에서 만들고 사용하는 가상 네트워크입니다. 허브에 지정하는 주소 범위는 연결하는 기존 가상 네트워크와 겹칠 수 없습니다. 온-프레미스에 연결하는 주소 범위와도 겹칠 수 없습니다. 온-프레미스 네트워크 구성에 있는 IP 주소 범위를 잘 모른다면 세부 정보를 알고 있는 다른 사람의 도움을 받으세요.

* Azure 구독이 아직 없는 경우 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

## <a name="openvwan"></a>가상 WAN 만들기

브라우저에서 Azure Portal로 이동하고 Azure 계정으로 로그인합니다.

1. Virtual WAN 페이지로 이동합니다. 포털에서 **+리소스 만들기**를 클릭합니다. 검색 상자에 **Virtual WAN**을 입력하고 Enter를 선택합니다.
2. 결과에서 **Virtual WAN**을 선택합니다. Virtual WAN 페이지에서 **만들기**를 클릭하여 WAN 만들기 페이지를 엽니다.
3. **WAN 만들기** 페이지의 **기본 사항** 탭에서 다음 필드를 입력합니다.

   ![가상 WAN](./media/virtual-wan-site-to-site-portal/vwan.png)

   * **구독** - 사용할 Azure 구독을 선택합니다.
   * **리소스 그룹** - 새로 만들거나 기존 항목을 사용합니다.
   * **리소스 그룹 위치** - 드롭다운에서 리소스 위치를 선택합니다. WAN은 전역 리소스이며 특정 지역에 상주하지 않습니다. 하지만 만든 WAN 리소스를 보다 쉽게 관리하고 찾으려면 지역을 선택해야 합니다.
   * **이름** - WAN을 호출할 이름을 입력합니다.
   * **유형:** 기본 또는 표준. 기본 WAN을 만드는 경우 기본 허브만 만들 수 있습니다. 기본 허브는 VPN 사이트 간 연결만 가능합니다.
4. 필드 작성을 완료한 후 **검토 + 만들기**를 선택합니다.
5. 유효성 검사를 통과하면 **만들기**를 선택하여 가상 WAN을 만듭니다.

## <a name="hub"></a>허브 만들기

허브는 사이트 간, Express 경로 또는 지점 및 사이트 간 기능을 위한 게이트웨이를 포함할 수 있는 가상 네트워크입니다. 허브가 생성되면 사이트를 연결하지 않아도 허브 요금이 청구됩니다. 가상 허브에서 사이트 간 VPN 게이트웨이를 만드는 데 30분이 소요됩니다.

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-s2s-hub-include.md)]

## <a name="site"></a>사이트 만들기

이제 실제 위치에 해당하는 사이트를 만들 준비가 되었습니다. 물리적 위치에 해당하는 사이트를 필요한 만큼 만듭니다. 예를 들어 NY에 지사가 있고, 런던에 지사가 있고, LA에 지사가 있는 경우 3개의 사이트를 별도로 만들 수 있습니다. 이 사이트에는 온-프레미스 VPN 디바이스 엔드포인트가 포함됩니다. 가상 WAN에서 가상 허브당 최대 1,000개의 사이트를 만들 수 있습니다. 허브가 여러 개 있는 경우 해당 허브당 1,000개의 사이트를 만들 수 있습니다. 가상 WAN 파트너(링크 삽입) CPE 디바이스가 있는 경우 Azure에 대한 Automation 상황을 확인하세요. 일반적으로 Automation은 대규모 분기 정보를 Azure로 내보내고 CPE에서 Azure 가상 WAN VPN 게이트웨이로의 연결을 설정하는 간단한 클릭 환경을 의미합니다(Azure에서 CPE 파트너에 대한 Automation 지침 링크에 대해서는 아래 참조).

[!INCLUDE [Create a site](../../includes/virtual-wan-tutorial-s2s-site-include.md)]

## <a name="connectsites"></a>허브에 VPN 사이트 연결

이 단계에서는 VPN 사이트를 허브에 연결합니다.

[!INCLUDE [Connect VPN sites](../../includes/virtual-wan-tutorial-s2s-connect-vpn-site-include.md)]

## <a name="vnet"></a>허브에 VNet 연결

이 단계에서는 허브와 VNet 간에 연결을 만듭니다. 연결하려는 각 VNet에 대해 이 단계를 반복합니다.

1. 가상 WAN에 대한 페이지에서 **가상 네트워크 연결**을 클릭합니다.
2. 가상 네트워크 연결 페이지에서 **+연결 추가**를 클릭합니다.
3. **연결 추가** 페이지에서 다음 필드를 채웁니다.

    * **연결 이름** - 연결의 이름을 지정합니다.
    * **허브** - 이 연결과 연결할 허브를 선택합니다.
    * **구독** - 구독을 확인합니다.
    * **가상 네트워크** - 이 허브에 연결할 가상 네트워크를 선택합니다. 가상 네트워크에는 기존의 가상 네트워크 게이트웨이를 사용할 수 없습니다.
4. **확인**을 클릭하여 가상 네트워크 연결을 만듭니다.

## <a name="device"></a>VPN 구성 다운로드

VPN 디바이스 구성을 사용하여 온-프레미스 VPN 디바이스를 구성합니다.

1. 가상 WAN에 대한 페이지에서 **개요**를 클릭합니다.
2. **허브 -> VPNSite** 페이지의 맨 위에서 **VPN 구성 다운로드**를 클릭합니다. Azure는 'microsoft-network-[location]' 리소스 그룹에 스토리지 계정을 만듭니다. 여기서 위치는 WAN의 위치입니다. VPN 디바이스에 구성을 적용한 후에는 이 스토리지 계정을 삭제할 수 있습니다.
3. 파일 만들기가 끝나면 링크를 클릭하여 다운로드할 수 있습니다.
4. 온-프레미스 VPN 디바이스에 구성을 적용합니다.

### <a name="understanding-the-vpn-device-configuration-file"></a>VPN 디바이스 구성 파일 이해

디바이스 구성 파일은 온-프레미스 VPN 디바이스를 구성할 때 사용할 설정을 포함합니다. 이 파일을 볼 때 다음 정보를 확인합니다.

* **vpnSiteConfiguration -** 이 섹션은 Virtual WAN에 연결된 사이트로 설정된 디바이스 정보를 나타냅니다. 여기에는 분기 디바이스의 이름 및 공용 IP 주소가 포함됩니다.
* **vpnSiteConnections -** 이 섹션에서는 다음 설정에 관한 정보를 제공합니다.

    * 가상 허브 VNet의 **주소 공간**<br>예제:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * 허브에 연결된 VNet의 **주소 공간**<br>예제:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * 가상 허브 vpngateway의 **IP 주소**. vpngateway의 각 연결은 활성-활성 구성의 2개 터널로 구성되므로 이 파일에 두 IP 주소가 모두 나열됩니다. 이 예제에서는 각 사이트에 대한 “Instance0” 및 “Instance1”이 표시됩니다.<br>예제:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * BGP, 미리 공유한 키 등의 **Vpngateway 연결 구성 세부 정보**. PSK는 자동으로 생성되는 미리 공유한 키입니다. 사용자 지정 PSK의 개요 페이지에서 연결을 언제든지 편집할 수 있습니다.
  
### <a name="example-device-configuration-file"></a>예제 디바이스 구성 파일

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.30.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

### <a name="configuring-your-vpn-device"></a>VPN 디바이스 구성

>[!NOTE]
> Virtual WAN 파트너 솔루션을 사용하여 작업하는 경우 VPN 디바이스 구성이 자동으로 수행됩니다. 디바이스 컨트롤러는 Azure에서 구성 파일을 가져오고 Azure에 대한 연결을 설정하는 디바이스에 적용합니다. 따라서 VPN 디바이스를 수동으로 구성하는 방법을 알 필요가 없습니다.
>

디바이스를 구성하는 지침이 필요한 경우에는 [VPN 디바이스 구성 스크립트 페이지](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts)의 지침을 다음 주의 사항과 함께 사용하면 됩니다.

* VPN 디바이스 페이지의 지침은 Virtual WAN용으로 작성되지 않았지만 구성 파일의 Virtual WAN 값을 사용하여 VPN 디바이스를 수동으로 구성할 수 있습니다. 
* VPN Gateway용 다운로드 가능한 디바이스 구성 스크립트는 구성이 다르기 때문에 Virtual WAN에서는 작동하지 않습니다.
* 새 가상 WAN은 IKEv1 및 IKEv2를 모두 지원할 수 있습니다.
* 가상 WAN은 정책 기반 및 경로 기반 VPN 디바이스와 디바이스 지침을 모두 사용할 수 있습니다.

## <a name="viewwan"></a>가상 WAN 보기

1. 가상 WAN 탭으로 이동합니다.
2. **개요** 페이지의 맵에 있는 각 점은 허브를 나타냅니다. 점 위로 마우스를 가져가면 허브 상태 요약, 연결 상태 및 송수신 바이트 수를 볼 수 있습니다.
3. 허브 및 연결 섹션에서 허브 상태, VPN 사이트 등을 볼 수 있습니다. 특정 허브 이름을 클릭하고 VPN 사이트로 이동하여 자세한 내용을 확인할 수 있습니다.

## <a name="next-steps"></a>다음 단계

가상 WAN에 대해 자세히 알아보려면 [가상 WAN 개요](virtual-wan-about.md) 페이지를 참조하세요.
