---
title: Video Indexer BCDR (비즈니스 연속성 및 재해 복구)-Azure | Microsoft Docs
description: 지역 데이터 센터 중단 또는 오류가 발생 하는 경우 보조 Video Indexer 계정으로 장애 조치 (failover) 하는 방법을 알아봅니다.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 07/29/2019
ms.author: juliako
ms.openlocfilehash: ce788b4640f0a6c6f25b3280ce4f52fd018d1699
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668310"
---
# <a name="handle-video-indexer-business-continuity-and-disaster-recovery"></a>Video Indexer 비즈니스 연속성 및 재해 복구를 처리 합니다.

Azure Media Services Video Indexer는 지역 데이터 센터 중단 또는 오류로 인 한 서비스의 즉각적인 장애 조치 (failover)를 제공 하지 않습니다. 이 문서에서는 장애 조치 (failover)를 위한 환경을 구성 하 여 응용 프로그램에 대 한 최적의 가용성을 보장 하 고 재해 발생 시 복구 시간을 최소화 하는 방법을 설명 합니다

Azure의 격리 및 가용성 정책을 활용하려면 지역 쌍 간에 BCDR(비즈니스 연속성 및 재해 복구)을 구성하는 것이 좋습니다. 자세한 내용은 [Azure 쌍을 이루는 지역](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)을 참조하세요.

## <a name="prerequisites"></a>전제 조건 

Azure 구독. Azure 구독이 아직 없는 경우 [azure 무료 평가판](https://azure.microsoft.com/free/)에 등록 합니다.

## <a name="failover-to-a-secondary-account"></a>보조 계정으로 장애 조치 (Failover)

BCDR를 구현 하려면 중복성을 처리 하기 위해 두 개의 Video Indexer 계정이 있어야 합니다.

1. Azure에 연결 된 두 개의 Video Indexer 계정 ( [계정 만들기](connect-to-azure.md)참조)을 만듭니다. 하나는 주 지역에 대 한 것이 고 다른 하나는 페어링된 azure 지역에 대 한 것입니다. 
1. 주 지역에 오류가 있는 경우 보조 계정을 사용 하 여 인덱싱으로 전환 합니다.

> [!TIP]
> [서비스 알림에 대 한 활동 로그 경고 만들기에](../../service-health/alerts-activity-log-service-notifications.md)따라 서비스 상태 알림에 대 한 활동 로그 경고를 설정 하 여 BCDR를 자동화할 수 있습니다.

다중 테 넌 트 사용에 대 한 자세한 내용은 [다중 테 넌 트 관리](manage-multiple-tenants.md)를 참조 하세요. BCDR를 구현 하려면 다음 두 옵션 중 하나를 선택 합니다. 테 넌 트 [당 계정](manage-multiple-tenants.md#video-indexer-account-per-tenant) 또는 [테 넌 트 당 Azure 구독](manage-multiple-tenants.md#azure-subscription-per-tenant)을 Video Indexer 합니다.

## <a name="next-steps"></a>다음 단계

[Azure에 연결 된 Video Indexer 계정 관리](manage-account-connected-to-azure.md)
