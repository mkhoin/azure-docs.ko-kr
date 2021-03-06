---
title: 쿼리 결과에서 Apache Hive 뷰 시간 초과-Azure HDInsight
description: Azure HDInsight에서 쿼리 결과를 인출 하는 동안 Apache Hive 보기 시간 초과
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 86523a7564220191de252b362e45569116ff3111
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73494192"
---
# <a name="scenario-apache-hive-view-times-out-when-fetching-a-query-result-in-azure-hdinsight"></a>시나리오: Azure HDInsight에서 쿼리 결과를 인출 하는 동안 Apache Hive 뷰 시간 초과

이 문서에서는 Azure HDInsight 클러스터에서 대화형 쿼리 구성 요소를 사용 하는 경우 문제 해결 단계 및 가능한 해결 방법을 설명 합니다.

## <a name="issue"></a>문제

Apache Hive 뷰에서 특정 쿼리를 실행 하는 경우 다음과 같은 오류가 발생할 수 있습니다.

```
result fetch timed out
java.util.concurrent.TimeoutException: deadline passed
```

## <a name="cause"></a>원인

Hive 뷰 기본 시간 제한 값은 실행 중인 쿼리에 적합 하지 않을 수 있습니다. 지정 된 기간이 너무 짧아서 Hive 뷰에서 쿼리 결과를 가져올 수 없습니다.

## <a name="resolution"></a>해상도

`/etc/ambari-server/conf/ambari.properties`에서 다음 속성을 설정 하 여 Apache Ambari Hive 뷰 시간 제한을 늘립니다.

```
views.ambari.request.read.timeout.millis=300000
views.request.read.timeout.millis=300000
views.ambari.hive<HIVE_VIEW_INSTANCE_NAME>.result.fetch.timeout=300000
```

`HIVE_VIEW_INSTANCE_NAME` 값은 Hive 뷰 URL의 끝에 있습니다.

## <a name="next-steps"></a>다음 단계

문제가 표시되지 않거나 문제를 해결할 수 없는 경우 다음 채널 중 하나를 방문하여 추가 지원을 받으세요.

* Azure [커뮤니티 지원을](https://azure.microsoft.com/support/community/)통해 azure 전문가 로부터 답변을 받으세요.

* [@AzureSupport](https://twitter.com/azuresupport) 연결-Azure 커뮤니티를 적절 한 리소스 (답변, 지원 및 전문가)에 연결 하 여 고객 환경을 개선 하기 위한 공식 Microsoft Azure 계정입니다.

* 도움이 더 필요한 경우 [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)에서 지원 요청을 제출할 수 있습니다. 메뉴 모음에서 **지원** 을 선택 하거나 **도움말 + 지원** 허브를 엽니다. 자세한 내용은 [Azure 지원 요청을 만드는 방법](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)을 검토하세요. 구독 관리 및 청구 지원에 대한 액세스 권한은 Microsoft Azure 구독에 포함되어 있으며, [Azure 지원 플랜](https://azure.microsoft.com/support/plans/) 중 하나를 통해 기술 지원이 제공됩니다.
