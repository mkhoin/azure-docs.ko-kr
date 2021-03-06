---
title: Azure Functions 1.x에 대 한 호스트 json 참조
description: V1 runtime을 사용 하는 Azure Functions 호스트 json 파일에 대 한 참조 설명서입니다.
ms.topic: conceptual
ms.date: 10/19/2018
ms.openlocfilehash: 256cd47fa0f309bef46c7f72951810d5f76d0fba
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74975468"
---
# <a name="hostjson-reference-for-azure-functions-1x"></a>Azure Functions 1.x에 대 한 호스트 json 참조

> [!div class="op_single_selector" title1="사용 중인 Azure Functions 런타임 버전을 선택 합니다. "]
> * [1-es verzió](functions-host-json-v1.md)
> * [2-es verzió](functions-host-json.md)

*Host. json* 메타 데이터 파일에는 함수 앱의 모든 함수에 영향을 주는 전역 구성 옵션이 포함 되어 있습니다. 이 문서에서는 v1 런타임에 사용할 수 있는 설정을 나열 합니다. JSON 스키마는 http://json.schemastore.org/host 에 있습니다.

> [!NOTE]
> 이 문서는 1. x Azure Functions입니다.  함수 2.x의 호스트나에 대 한 참조는 [Azure Functions 2.x에 대 한 호스트 json 참조](functions-host-json.md)를 참조 하세요.

다른 함수 앱 구성 옵션은 [앱 설정](functions-app-settings.md)에서 관리 됩니다.

일부 host. json 설정은 [로컬. 설정 json](functions-run-local.md#local-settings-file) 파일에서 로컬로 실행 하는 경우에만 사용 됩니다.

## <a name="sample-hostjson-file"></a>샘플 호스트 json 파일

다음 샘플 *호스트 json* 파일에는 모든 가능한 옵션이 지정 되어 있습니다.


```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "documentDB": {
        "connectionMode": "Gateway",
        "protocol": "Https",
        "leaseOptions": {
            "leasePrefix": "prefix"
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "sendGrid": {
        "from": "Contoso Group <admin@contoso.com>"
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

이 문서의 다음 섹션에서는 각 최상위 속성에 대해 설명 합니다. 별도로 지정 하지 않는 한 모두 선택 사항입니다.

## <a name="aggregator"></a>aggregator

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>Applicationinsights.config

[!INCLUDE [applicationInsights](../../includes/functions-host-json-applicationinsights.md)]

## <a name="documentdb"></a>Dokumentum-adatbázis

[Azure Cosmos DB 트리거 및 바인딩에](functions-bindings-cosmosdb.md)대 한 구성 설정입니다.

```json
{
    "documentDB": {
        "connectionMode": "Gateway",
        "protocol": "Https",
        "leaseOptions": {
            "leasePrefix": "prefix1"
        }
    }
}
```

|Tulajdonság  |Alapértelmezett | Leírás |
|---------|---------|---------|
|게이트웨이 모드|Átjáró|Azure Cosmos DB 서비스에 연결할 때 함수에서 사용 하는 연결 모드입니다. 옵션은 `Direct` `Gateway`|
|Protocol (Protokoll)|http|Azure Cosmos DB 서비스에 연결할 때 함수에서 사용 하는 연결 프로토콜입니다.  [두 모드에 대 한 설명을 보려면 여기를 참조 하세요](../cosmos-db/performance-tips.md#networking) .|
|leasePrefix|–|앱의 모든 함수에서 사용할 임대 접두사입니다.|

## <a name="durabletask"></a>Microsoft.azure.webjobs.extensions.durabletask

[!INCLUDE [durabletask](../../includes/functions-host-json-durabletask.md)]

## <a name="eventhub"></a>eventHub

[이벤트 허브 트리거 및 바인딩에](functions-bindings-event-hubs.md)대 한 구성 설정입니다.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>functions

작업 호스트에서 실행 하는 함수 목록입니다. 빈 배열은 모든 함수를 실행 함을 의미 합니다. [로컬로 실행](functions-run-local.md)하는 경우에만 사용할 목적입니다. Azure의 함수 앱에서 대신 [Azure Functions에서 함수를 사용 하지 않도록 설정 하는 방법](disable-function.md) 의 단계를 수행 하 여이 설정을 사용 하는 대신 특정 기능을 사용 하지 않도록 설정 해야 합니다.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

모든 함수의 제한 시간을 나타냅니다. 서버를 사용 하지 않는 소비 계획에서 유효한 범위는 1 초에서 10 분 사이 이며 기본값은 5 분입니다. App Service 계획에서는 전체 제한이 없으며 기본값은 _null_이며 시간 제한이 없음을 나타냅니다.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

[호스트 상태 모니터](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor)에 대 한 구성 설정입니다.

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Tulajdonság  |Alapértelmezett | Leírás |
|---------|---------|---------| 
|enabled|igaz|기능이 사용 되는지 여부를 지정 합니다. | 
|healthCheckInterval|10 másodperc|정기적인 백그라운드 상태 검사 간의 시간 간격입니다. | 
|healthCheckWindow|2 분|`healthCheckThreshold` 설정과 함께 사용 되는 슬라이딩 시간 창입니다.| 
|healthCheckThreshold|6|호스트 재활용이 시작 되기 전에 상태 검사가 실패할 수 있는 최대 횟수입니다.| 
|counterThreshold|0,80|성능 카운터가 비정상으로 간주 되는 임계값입니다.| 

## <a name="http"></a>http

[Http 트리거 및 바인딩에](functions-bindings-http-webhook.md)대 한 구성 설정입니다.

```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 200,
        "maxConcurrentRequests": 100,
        "dynamicThrottlesEnabled": true
    }
}
```

|Tulajdonság  |Alapértelmezett | Leírás |
|---------|---------|---------| 
|dynamicThrottlesEnabled|hamis|이 설정을 사용 하는 경우이 설정을 사용 하면 요청 처리 파이프라인이 연결/스레드/프로세스/메모리/cpu/등의 시스템 성능 카운터를 주기적으로 확인 하 고, 해당 카운터가 기본 제공 높은 임계값 (80%)을 초과 하는 경우에는 카운터가 정상 수준으로 돌아올 때까지 429 "사용량이 많음" 응답으로 요청이 거부 됩니다.|
|maxConcurrentRequests|바인딩되지 않음 (`-1`)|병렬로 실행 될 http 함수의 최대 수입니다. 이렇게 하면 리소스 사용률을 관리 하는 데 도움이 되는 동시성을 제어할 수 있습니다. 예를 들어 많은 시스템 리소스 (메모리/c p u/소켓)를 사용 하는 http 함수를 사용 하 여 동시성이 너무 높을 때 문제를 발생 시킬 수 있습니다. 또는 타사 서비스에 아웃 바운드 요청을 하는 함수를 사용 하 고 해당 호출에 대 한 요금이 제한 되어야 할 수 있습니다. 이러한 경우 제한을 적용 하면 도움이 될 수 있습니다.|
|maxOutstandingRequests|바인딩되지 않음 (`-1`)|지정 된 시간에 유지 되는 미해결 요청의 최대 수입니다. 이 제한에는 대기 중이지만 실행이 시작 되지 않은 요청과 진행 중인 모든 실행이 포함 됩니다. 이 제한을 초과 하는 들어오는 요청은 429 "사용량이 많음" 응답으로 거부 됩니다. 이를 통해 호출자는 시간 기반 재시도 전략을 사용 하 고 최대 요청 대기 시간을 제어할 수 있습니다. 이는 스크립트 호스트 실행 경로 내에서 발생 하는 큐만 제어 합니다. ASP.NET request 큐와 같은 다른 큐는 여전히 적용 되 고이 설정에 의해 영향을 받지 않습니다.|
|routePrefix|api-t|모든 경로에 적용 되는 경로 접두사입니다. 빈 문자열을 사용 하 여 기본 접두사를 제거 합니다. |

## <a name="id"></a>id

작업 호스트의 고유 ID입니다. 대시가 제거 된 소문자 GUID 일 수 있습니다. 로컬로 실행 하는 경우 필수 사항입니다. Azure에서 실행 하는 경우 ID 값을 설정 하지 않는 것이 좋습니다. `id`이 생략 되 면 Azure에서 ID가 자동으로 생성 됩니다. 

여러 함수 앱에서 저장소 계정을 공유 하는 경우 각 함수 앱에 다른 `id`있는지 확인 합니다. `id` 속성을 생략 하거나 각 함수 앱의 `id`를 다른 값으로 수동으로 설정할 수 있습니다. 타이머 트리거는 함수 앱이 여러 인스턴스로 확장 될 때 하나의 타이머 인스턴스만 갖도록 하기 위해 저장소 잠금을 사용 합니다. 두 함수 앱이 동일한 `id`을 공유 하 고 각각 타이머 트리거를 사용 하는 경우 하나의 타이머만 실행 됩니다.

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>로

[ILogger 개체](functions-monitoring.md#write-logs-in-c-functions) 또는 [컨텍스트별](functions-monitoring.md#write-logs-in-javascript-functions)에서 작성 한 로그에 대 한 필터링을 제어 합니다.

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Tulajdonság  |Alapértelmezett | Leírás |
|---------|---------|---------| 
|categoryFilter|–|범주별 필터링을 지정 합니다.| 
|defaultLevel|Információ|`categoryLevels` 배열에 지정 되지 않은 범주에 대해이 수준 이상의 로그를 Application Insights으로 보냅니다.| 
|categoryLevels|–|각 범주에 대해 Application Insights에 보낼 최소 로그 수준을 지정 하는 범주 배열입니다. 여기에 지정 된 범주는 동일한 값으로 시작 하는 모든 범주를 제어 하 고 긴 값이 우선 적용 됩니다. 위의 샘플 *호스트 json* 파일에서 "host-a"로 시작 하는 모든 범주는 `Information` 수준에서 로그 합니다. "Host"로 시작 하는 다른 모든 범주 (예: "Host. Executor")는 `Error` 수준으로 기록 합니다.| 

## <a name="queues"></a>쿼리

[저장소 큐 트리거 및 바인딩에](functions-bindings-storage-queue.md)대 한 구성 설정입니다.

```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Tulajdonság  |Alapértelmezett | Leírás |
|---------|---------|---------| 
|maxPollingInterval|60000|큐 폴링 사이의 최대 간격 (밀리초)입니다.| 
|visibilityTimeout|0|메시지 처리에 실패 하는 경우 재시도 사이의 시간 간격입니다.| 
|batchSize|16|함수 런타임이 동시에 검색 하 고 병렬로 처리 하는 큐 메시지 수입니다. 처리 되는 수가 `newBatchThreshold`으로 감소 하면 런타임은 다른 일괄 처리를 가져와 해당 메시지의 처리를 시작 합니다. 따라서 함수 당 처리 되는 최대 동시 메시지 수는 `batchSize` 및 `newBatchThreshold`입니다. 이 제한은 큐 트리거 함수에 개별적으로 적용 됩니다. <br><br>한 큐에서 받은 메시지에 대 한 병렬 실행을 방지 하려면 `batchSize`를 1로 설정 합니다. 그러나이 설정은 함수 앱이 단일 VM (가상 컴퓨터)에서 실행 되는 동안에만 동시성을 제거 합니다. 함수 앱이 여러 Vm으로 확장 되는 경우 각 VM은 각 큐 트리거 함수의 인스턴스 하나를 실행할 수 있습니다.<br><br>최대 `batchSize`은 32입니다. | 
|maxDequeueCount|5|메시지를 포이즌 큐로 이동 하기 전에 처리를 시도 하는 횟수입니다.| 
|newBatchThreshold|batchSize/2|동시에 처리 되는 메시지 수가이 수로 감소 하면 런타임은 다른 일괄 처리를 검색 합니다.| 

## <a name="sendgrid"></a>SendGrid

[SendGrind 출력 바인딩에](functions-bindings-sendgrid.md) 대 한 구성 설정

```json
{
    "sendGrid": {
        "from": "Contoso Group <admin@contoso.com>"
    }
```

|Tulajdonság  |Alapértelmezett | Leírás |
|---------|---------|---------| 
|from|–|모든 함수에서 보낸 사람의 전자 메일 주소입니다.| 

## <a name="servicebus"></a>serviceBus

[Service Bus 트리거 및 바인딩에](functions-bindings-service-bus.md)대 한 구성 설정입니다.

```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Tulajdonság  |Alapértelmezett | Leírás |
|---------|---------|---------| 
|MaxConcurrentCalls|16|메시지 펌프가 시작 해야 하는 콜백에 대 한 최대 동시 호출 수입니다. 기본적으로 함수 런타임은 여러 메시지를 동시에 처리 합니다. 런타임에 단일 큐 또는 토픽 메시지만 처리 하도록 지시 하려면 `maxConcurrentCalls`를 1로 설정 합니다. | 
|prefetchCount|–|기본 MessageReceiver에서 사용할 기본 PrefetchCount입니다.| 
|autoRenewTimeout|00:05:00|메시지 잠금이 자동으로 갱신 되는 최대 기간입니다.| 

## <a name="singleton"></a>단일

Singleton 잠금 동작에 대 한 구성 설정입니다. 자세한 내용은 [singleton 지원에 대 한 GitHub 문제](https://github.com/Azure/azure-webjobs-sdk-script/issues/912)를 참조 하세요.

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Tulajdonság  |Alapértelmezett | Leírás |
|---------|---------|---------| 
|lockPeriod|00:00:15|함수 수준 잠금이 수행 되는 기간입니다. 에서 자동 갱신을 잠급니다.| 
|listenerLockPeriod|00:01:00|수신기 잠금이 수행 되는 기간입니다.| 
|listenerLockRecoveryPollingInterval|00:01:00|시작 시 수신기 잠금을 획득할 수 없는 경우 수신기 잠금 복구에 사용 되는 시간 간격입니다.| 
|lockAcquisitionTimeout|00:01:00|런타임이 잠금을 획득 하려고 시도 하는 최대 시간입니다.| 
|lockAcquisitionPollingInterval|–|잠금 획득 시도 사이의 간격입니다.| 

## <a name="tracing"></a>추적할

*버전 1.x*

`TraceWriter` 개체를 사용 하 여 만든 로그의 구성 설정입니다. [ C# 로깅](functions-reference-csharp.md#logging) 및 [node.js 로깅](functions-reference-node.md#writing-trace-output-to-the-console)을 참조 하세요.

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Tulajdonság  |Alapértelmezett | Leírás |
|---------|---------|---------| 
|consoleLevel|정보|콘솔 로깅의 추적 수준입니다. 옵션은 `off`, `error`, `warning`, `info`및 `verbose`입니다.|
|fileLoggingMode|debugOnly|파일 로깅의 추적 수준입니다. 옵션은 `never`, `always`, `debugOnly`입니다.| 

## <a name="watchdirectories"></a>watchDirectories

변경 내용을 모니터링 해야 하는 [공유 코드 디렉터리](functions-reference-csharp.md#watched-directories) 집합입니다.  이러한 디렉터리의 코드가 변경 될 때 함수에 의해 변경 내용이 선택 되도록 합니다.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [호스트 json 파일을 업데이트 하는 방법을 알아봅니다.](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [환경 변수의 전역 설정 보기](functions-app-settings.md)
