---
title: Windows VM용 Azure Disk Storage 관리 디스크 개요
description: Azure Windows VM 사용 시 스토리지 계정을 처리하는 Azure 관리 디스크에 대한 개요
author: roygara
ms.service: virtual-machines-windows
ms.topic: overview
ms.date: 08/15/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: c23cfbc418cca82393a0a66b0ceace622b2833f5
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74038154"
---
# <a name="introduction-to-azure-managed-disks"></a>Azure 관리 디스크 소개

Azure Managed Disk는 VHD(가상 하드 디스크)입니다. 온-프레미스 서버의 물리적 디스크처럼 생각할 수 있지만 가상화된 디스크입니다. Azure Managed Disks는 Azure의 무작위 IO 스토리지 개체인 페이지 Blob으로 저장됩니다. 관리 디스크를 ‘관리’로 부르는 것은 페이지 Blob, Blob 컨테이너 및 Azure Storage 계정에 대한 추상이기 때문입니다. 관리 디스크를 사용할 경우 디스크를 프로비저닝하기만 하면 Azure에서 나머지 작업을 처리합니다.

워크로드에 Azure Managed Disks를 사용하도록 선택하면 Azure에서 해당 디스크를 만들고 관리합니다. 사용 가능한 디스크 유형은 Ultra 디스크, 프리미엄 SSD(반도체 드라이브), 표준 SSD 및 표준 HDD(하드 디스크 드라이브)입니다. 각 개별 디스크 유형에 대한 자세한 내용은 [IaaS VM의 디스크 유형 선택](disks-types.md)을 참조하세요.

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [IaaS VM의 디스크 유형 선택](disks-types.md)