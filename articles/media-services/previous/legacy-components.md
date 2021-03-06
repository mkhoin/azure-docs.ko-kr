---
title: Azure Media Services 레거시 구성 요소 | Microsoft Docs
description: 이 항목에서는 레거시 구성 요소 Azure Media Services 설명 합니다.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2019
ms.author: juliako
ms.openlocfilehash: 6a61c868da7b979b1ac5654f4be93b5245179105
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74423923"
---
# <a name="azure-media-services-legacy-components"></a>Azure Media Services 레거시 구성 요소

시간이 지남에 따라 Media Service 구성 요소에 대 한 향상 된 기능과 향상 된 기능이 있습니다. 따라서 일부 레거시 구성 요소는 사용이 중지 되었습니다. 다음 문서에서는 레거시 구성 요소에서 현재 구성 요소로 응용 프로그램을 마이그레이션하는 방법에 대 한 지침을 찾을 수 있습니다.
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>레거시 구성 요소 및 마이그레이션 지침의 사용 중지 계획

*Windows Azure Media Encoder* (wame) 및 *Azure Media Encoder* (AME) 미디어 프로세서의 사용 중단을 발표 하 고 있습니다. 이러한 프로세서는 2020 년 3 월 31 일에 사용 중지 됩니다.

* [Windows Azure Media Encoder에서 Media Encoder Standard로 마이그레이션](migrate-windows-azure-media-encoder.md)
* [Azure Media Encoder에서 Media Encoder Standard로 마이그레이션](migrate-azure-media-encoder.md)

또한 다음 미디어 분석 미디어 프로세서를 사용 중지 하는 것을 발표 하 고 있습니다. 
 
|미디어 프로세서 이름|사용 중지 날짜|추가적인 참고 사항|
|---|---|
|[Azure Media Indexer 2](media-services-process-content-with-indexer2.md)| 2020의 1 월 1 일|이 미디어 프로세서는 [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)으로 교체 됩니다. 자세한 내용은 [Azure Media Indexer 2에서 Azure Media Services Video Indexer로 마이그레이션](migrate-indexer-v1-v2.md)을 참조 하세요.|
|[Azure Media Indexer](media-services-index-content.md)|2020의 10 월 1 일|이 미디어 프로세서는 [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/)으로 교체 됩니다. 자세한 내용은 [Azure Media Indexer에서 Azure Media Services로 마이그레이션](migrate-indexer-v1-v2.md) 을 참조 하세요 Video Indexer

## <a name="next-steps"></a>다음 단계

[Media Services v2에서 v3로 이동하기 위한 마이그레이션 지침](../latest/migrate-from-v2-to-v3.md)
