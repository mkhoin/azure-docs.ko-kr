---
title: 연결 아키텍처-Azure Database for MySQL
description: Azure Database for MySQL 서버에 대 한 연결 아키텍처를 설명 합니다.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 22c77bee95533606156ec6cc337af1d743018005
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74765327"
---
# <a name="connectivity-architecture-in-azure-database-for-mysql"></a>Azure Database for MySQL의 연결 아키텍처
이 문서에서는 Azure 내부 및 외부의 클라이언트에서 Azure Database for MySQL 인스턴스로 트래픽이 전송 되는 방법 뿐만 아니라 Azure Database for MySQL 연결 아키텍처에 대해 설명 합니다.

## <a name="connectivity-architecture"></a>연결 아키텍처
들어오는 연결을 클러스터에 있는 서버의 실제 위치로 라우팅하는 게이트웨이를 통해 Azure Database for MySQL에 대 한 연결이 설정 됩니다. 다음 다이어그램은 트래픽 흐름을 보여 줍니다.

![연결 아키텍처 개요](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

클라이언트는 데이터베이스에 연결할 때 게이트웨이에 연결 되는 연결 문자열을 가져옵니다. 이 게이트웨이에는 3306 포트를 수신 하는 공용 IP 주소가 있습니다. 데이터베이스 클러스터 내에서 트래픽이 적절 한 Azure Database for MySQL 전달 됩니다. 따라서 회사 네트워크에서와 같이 서버에 연결 하려면 클라이언트 쪽 방화벽을 열어 아웃 바운드 트래픽이 게이트웨이에 도달할 수 있도록 해야 합니다. 아래에서 지역별 게이트웨이에서 사용 하는 IP 주소의 전체 목록을 찾을 수 있습니다.

## <a name="azure-database-for-mysql-gateway-ip-addresses"></a>게이트웨이 IP 주소 Azure Database for MySQL
다음 표에서는 모든 데이터 영역에 대 한 Azure Database for MySQL 게이트웨이의 기본 및 보조 Ip를 나열 합니다. 주 IP 주소는 게이트웨이의 현재 IP 주소이 고 두 번째 IP 주소는 주 복제본의 오류가 발생 하는 경우 장애 조치 (failover) IP 주소입니다. 앞서 언급 했 듯이, 고객은 두 IP 주소에 대 한 아웃 바운드를 허용 해야 합니다. 두 번째 IP 주소는 연결을 허용 하기 위해 Azure Database for MySQL에 의해 활성화 될 때까지 서비스에서 수신 하지 않습니다.

| **영역 이름** | **기본 IP 주소** | **보조 IP 주소** |
|:----------------|:-------------|:------------------------|
| 오스트레일리아 동부 | 13.75.149.87 | 40.79.161.1 |
| 오스트레일리아 동남부 | 191.239.192.109 | 13.73.109.251 |
| 브라질 남부 | 104.41.11.5 | |
| 캐나다 중부 | 40.85.224.249 | |
| 캐나다 동부 | 40.86.226.166 | |
| 미국 중부 | 23.99.160.139 | 13.67.215.62 |
| 중국 동부 1 | 139.219.130.35 | |
| 중국 동부 2 | 40.73.82.1 | |
| 중국 북부 1 | 139.219.15.17 | |
| 중국 북부 2 | 40.73.50.0 | |
| 동아시아 | 191.234.2.139 | 52.175.33.150 |
| 미국 동부 1 | 191.238.6.43 | 40.121.158.30 |
| 미국 동부 2 | 191.239.224.107 | 40.79.84.180 * |
| 프랑스 중부 | 40.79.137.0 | 40.79.129.1 |
| 독일 중부 | 51.4.144.100 | |
| 인도 중부 | 104.211.96.159 | |
| 인도 남부 | 104.211.224.146 | |
| 인도 서부 | 104.211.160.80 | |
| 일본 동부 | 191.237.240.43 | 13.78.61.196 |
| 일본 서부 | 191.238.68.11 | 104.214.148.156 |
| 한국 중부 | 52.231.32.42 | |
| 한국 남부 | 52.231.200.86 |  |
| 미국 중북부 | 23.98.55.75 | 23.96.178.199 |
| 북유럽 | 191.235.193.75 | 40.113.93.91 |
| 미국 중남부 | 23.98.162.75 | 13.66.62.124 |
| 동남아시아 | 23.100.117.95 | 104.43.15.0 |
| 남아프리카 공화국 북부 | 102.133.152.0 | |
| 남아프리카 공화국 서부 | 102.133.24.0 | |
| 아랍에미리트 북부 | 65.52.248.0 | |
| 영국 남부 | 51.140.184.11 | |
| 영국 서부 | 51.141.8.11| |
| 서유럽 | 191.237.232.75 | 40.68.37.158 |
| 미국 서부 1 | 23.99.34.75 | 104.42.238.205 |
| 미국 서부 2 | 13.66.226.202 | |
||||

> [!NOTE]
> *미국 동부 2* 에는 `52.167.104.0`의 3 차 IP 주소도 있습니다.

## <a name="next-steps"></a>다음 단계

* [Azure Portal을 사용한 MySQL용 Azure Database 방화벽 규칙 만들기 및 관리](./howto-manage-firewall-using-portal.md)
* [Azure CLI를 사용 하 여 Azure Database for MySQL 방화벽 규칙 만들기 및 관리](./howto-manage-firewall-using-cli.md)