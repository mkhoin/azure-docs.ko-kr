---
title: Azure Application Insights 스마트 검색 – 기본 알림 수신자의 예정된 변경 | Microsoft Docs
description: Azure Application Insights에서 애플리케이션 추적을 모니터링하여 추적 원격 분석의 비정상적인 패턴을 확인합니다.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: harelbr
ms.author: harelbr
ms.date: 03/13/2019
ms.reviewer: mbullwin
ms.openlocfilehash: fa1f98b5a9ee592a4c702e87e365eff7941194d4
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72820617"
---
# <a name="smart-detection-e-mail-notification-change"></a>스마트 검색 메일 알림 변경

고객 피드백에 따라, 2019년 4월 1일에 스마트 검색에서 메일 알림을 받는 기본 역할이 변경됩니다.

## <a name="what-is-changing"></a>변경되는 내용

현재 스마트 검색 메일 알림은 기본적으로 ‘Subscription Owner’, ‘Subscription Contributor’ 및 ‘Subscription Reader’ 역할에 전송됩니다. 모니터링에 적극적으로 참여하지 않는 사용자가 해당 역할에 포함되는 경우가 많으며, 이로 인해 많은 사용자가 불필요하게 알림을 받게 됩니다. 이 환경을 개선하기 위해 메일 알림이 기본적으로 [Monitoring Reader](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) 및 [Monitoring Contributor](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) 역할로만 이동하도록 변경됩니다.

## <a name="scope-of-this-change"></a>이 변경의 범위

이 변경 사항은 다음을 제외한 모든 스마트 검색 규칙에 적용됩니다.

* 미리 보기로 표시된 스마트 검색 규칙. 이러한 스마트 검색 규칙은 현재 메일 알림을 지원하지 않습니다.

* 오류 이상 규칙. 이 규칙은 클래식 경고에서 통합 경고 플랫폼으로 마이그레이션된 후 새 기본 역할에 적용되기 시작합니다(자세한 내용은 [여기](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement) 참조).

## <a name="how-to-prepare-for-this-change"></a>이 변경을 준비하는 방법

스마트 검색의 전자 메일 알림이 관련 사용자에 게 전송 되도록 하려면 해당 사용자가 구독의 [모니터링 독자](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) 또는 [모니터링 참가자](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) 역할에 할당 되어야 합니다.

Azure Portal을 통해 Monitoring Reader 또는 Monitoring Contributor 역할에 사용자를 할당하려면 [역할 할당 추가](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) 문서에 설명된 단계를 수행합니다. 사용자가 할당되는 역할로 ‘Monitoring Reader’ 또는 ‘Monitoring Contributor’를 선택해야 합니다.

> [!NOTE]
> 규칙 설정의 ‘추가 메일 수신자’ 옵션을 사용하여 구성된 스마트 검색 알림의 특정 수신자는 이 변경의 영향을 받지 않습니다. 해당 수신자는 메일 알림을 계속 수신합니다.

이 변경에 대한 질문이나 우려 사항이 있으면 [Microsoft에 문의](mailto:smart-alert-feedback@microsoft.com)하시기 바랍니다.

## <a name="next-steps"></a>다음 단계

스마트 검색에 대해 자세히 알아봅니다.

- [오류 잘못된 부분](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [메모리 누수](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [성능 이상](../../azure-monitor/app/proactive-performance-diagnostics.md)