---
title: 포함 파일
description: 포함 파일
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 11/05/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: bc4ac68cb415a43ac34d36afc2adc30307e6d37c
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795292"
---
| 리소스 | Basic | Standard | Premium |
|---|---|---|---|
| Storage<sup>1</sup> | 10GiB | 100GiB| 500GiB |
| 최대 이미지 계층 크기 | 200 GiB | 200 GiB | 200 GiB |
| 분당 ReadOps<sup>2, 3</sup> | 1,000 | 3,000 | 10,000 |
| 분당 WriteOps<sup>2, 4</sup> | 100 | 500 | 2, 000 |
| 다운로드 대역폭 MBps<sup>2</sup> | 30 | 60 | 100 |
| 업로드 대역폭 MBps<sup>2</sup> | 10 | 20 | 50 |
| Webhook | 2 | 10 | 100 |
| 지역에서 복제 | 해당 없음 | 해당 없음 | [지원됨][geo-replication] |
| 콘텐츠 신뢰 | 해당 없음 | 해당 없음 | [지원됨][content-trust] |
| 가상 네트워크 액세스 | 해당 없음 | 해당 없음 | [미리 보기][vnet] |
| 리포지토리 범위 권한 | 해당 없음 | 해당 없음 | [미리 보기][token]|
| 토큰 &bull; | 해당 없음 | 해당 없음 | 20,000 |
| &bull; 범위 맵 | 해당 없음 | 해당 없음 | 20,000 |
| 범위 맵 당 &bull; 리포지토리 | 해당 없음 | 해당 없음 | 500 |


<sup>1</sup> 지정 된 저장소 제한은 각 계층에 대해 *포함* 된 저장소의 크기입니다. 이러한 한도 초과 이미지 스토리지에 대해 GiB당 추가 일별 요금이 청구됩니다. 요금 정보는 [Azure Container Registry 가격 책정][pricing]을 참조 하세요.

<sup>2</sup>*readops*, *writeops*및 *대역폭* 은 최소 추정치입니다. Azure Container Registry은 사용에 필요한 성능 개선을 위해 노력 하 고 있습니다.

<sup>3</sup> [Docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) 는 이미지의 레이어 수와 매니페스트 검색을 기준으로 여러 읽기 작업으로 변환 됩니다.

<sup>4</sup> [Docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) 는 푸시 해야 하는 계층 수를 기준으로 여러 쓰기 작업으로 변환 됩니다. `docker push`에는 기존 이미지에 대해 매니페스트를 검색할 *ReadOps*가 포함되어 있습니다.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
[vnet]: ../articles/container-registry/container-registry-vnet.md
[token]: ../articles/container-registry/container-registry-repository-scoped-permissions.md