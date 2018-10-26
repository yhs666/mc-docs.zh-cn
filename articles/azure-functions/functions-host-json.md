---
title: Azure Functions 的 host.json 参考
description: Azure Functions host.json 文件的参考文档。
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
origin.date: 09/08/2018
ms.date: 10/19/2018
ms.author: v-junlch
ms.openlocfilehash: 7a260085b9afb7d8f523f392cfc1e81bd0bc0af2
ms.sourcegitcommit: 2d33477aeb0f2610c23e01eb38272a060142c85d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49453604"
---
# <a name="hostjson-reference-for-azure-functions"></a>Azure Functions 的 host.json 参考

*host.json* 元数据文件包含对函数应用的所有函数产生影响的全局配置选项。 本文列出可用的设置。 JSON 架构位于 http://json.schemastore.org/host。

> [!NOTE]
> Azure Functions 运行时版本 v1 和 v2 之间的 *host.json* 存在显著差异。 针对 v2 运行时的函数应用需要 `"version": "2.0"`。

其他函数应用配置选项在[应用设置](functions-app-settings.md)中进行管理。

[local.settings.json](functions-run-local.md#local-settings-file) 文件中的某些 host.json 设置仅在本地运行时才使用。

## <a name="sample-hostjson-file"></a>示例 host.json 文件

以下示例 *host.json* 文件指定了所有可能的选项。

### <a name="version-2x"></a>版本 2.x

```json
{
    "version": "2.0",
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "extensions": {
        "eventHubs": {
          "maxBatchSize": 64,
          "prefetchCount": 256,
          "batchCheckpointFrequency": 1
        },
        "http": {
            "routePrefix": "api",
            "maxConcurrentRequests": 100,
            "maxOutstandingRequests": 30
        },
        "queues": {
            "visibilityTimeout": "00:00:10",
            "maxDequeueCount": 3
        },
        "sendGrid": {
            "from": "Azure Functions <samples@functions.com>"
        },
        "serviceBus": {
          "maxConcurrentCalls": 16,
          "prefetchCount": 100,
          "autoRenewTimeout": "00:05:00"
        }
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
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logging": {
        "fileLoggingMode": "debugOnly",
        "logLevel": {
          "Function.MyFunction": "Information",
          "default": "None"
        },
        "applicationInsights": {
            "sampling": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 5
            }
        }
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "watchDirectories": [ "Shared", "Test" ]
}
```

### <a name="version-1x"></a>版本 1.x

```json
{
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

本文的以下各部分解释了每个顶级属性。 除非另有说明，否则其中的所有属性都是可选的。


## <a name="eventhub"></a>eventHub

[事件中心触发器和绑定](functions-bindings-event-hubs.md)的配置设置。 在版本 2.x 中，这是[扩展](#extensions)的子级。

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="extensions"></a>扩展

*仅限版本 2.x*。

返回包含所有特定于绑定的设置的对象的属性，例如 [http](#http) 和 [eventHub](#eventhub)。

## <a name="functions"></a>functions

作业主机运行的函数列表。 空数组表示运行所有函数。 仅供在[本地运行](functions-run-local.md)时使用。 在 Azure 的函数应用中，应改为按照[如何在 Azure Functions 中禁用函数](disable-function.md)中的步骤禁用特定函数，而不是使用此设置。

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

指示所有函数的超时持续时间。 在应用服务计划中，没有总体限制，默认值取决于运行时版本。 在版本 2.x 中，应用服务计划的默认值为 30 分钟。 在版本 1.x 中，它为 *null*，表示无超时。

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

[主机运行状况监视器](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor)的配置设置。

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

|属性  |默认 | 说明 |
|---------|---------|---------| 
|Enabled|是|指定是否已启用该功能。 | 
|healthCheckInterval|10 秒|定期后台运行状况检查之间的时间间隔。 | 
|healthCheckWindow|2 分钟|与 `healthCheckThreshold` 设置结合使用的滑动时间窗口。| 
|healthCheckThreshold|6|在启动主机回收之前，运行状况检查可以失败的最大次数。| 
|counterThreshold|0.80|性能计数器将被视为不正常的阈值。| 

## <a name="http"></a>http

[http 触发器和绑定](functions-bindings-http-webhook.md)的配置设置。 在版本 2.x 中，这是[扩展](#extensions)的子级。

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>id

*仅限版本 1.x*。

作业宿主的唯一 ID。 可以是不带短划线的小写 GUID。 在本地运行时必须指定。 在 Azure 中运行时，我们建议你不要设置 ID 值。 省略 `id` 时，Azure 中会自动生成 ID。 使用版本 2.x 运行时时，无法设置自定义函数应用 ID。

如果跨多个函数应用共享存储帐户，请确保每个函数应用都有不同的 `id`。 可省略 `id` 属性或手动将每个函数应用的 `id` 设置为不同的值。 计时器触发器使用存储锁来确保当函数应用横向扩展到多个实例时将只有一个计时器实例。 如果两个函数应用共享相同的 `id` 且每个都使用计时器触发器，则只会运行一个计时器。

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>logger

*仅限版本 1.x*。
控制 ILogger 对象或 context.log 写入的日志的筛选。

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

|属性  |默认 | 说明 |
|---------|---------|---------| 
|categoryFilter|不适用|指定按类别进行筛选| 
|defaultLevel|信息|对于 `categoryLevels` 数组中未指定的任何类别，将在此级别发送日志。| 
|categoryLevels|不适用|一个类别数组，指定每个类别的最低日志级别。 此处指定的类别控制以相同值开头的所有类别，较长的值优先。 在前面的示例 *host.json* 文件中，将在 `Information` 级别记录以“Host.Aggregator”开头的所有类别的日志。 在 `Error` 级别记录以“Host”开头的其他所有类别（例如“Host.Executor”）的日志。| 

## <a name="queues"></a>queues

[存储队列触发器和绑定](functions-bindings-storage-queue.md)的配置设置。 在版本 2.x 中，这是[扩展](#extensions)的子级。

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="servicebus"></a>serviceBus

[服务总线触发器和绑定](functions-bindings-service-bus.md)的配置设置。 在版本 2.x 中，这是[扩展](#extensions)的子级。

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

## <a name="singleton"></a>singleton

单一实例锁行为的配置设置。 有关详细信息，请参阅[有关单一实例支持的 GitHub 问题](https://github.com/Azure/azure-webjobs-sdk-script/issues/912)。

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

|属性  |默认 | 说明 |
|---------|---------|---------| 
|lockPeriod|00:00:15|占用函数级锁的时间段。 锁自动续订。| 
|listenerLockPeriod|00:01:00|占用侦听器锁的时间段。| 
|listenerLockRecoveryPollingInterval|00:01:00|在启动时无法获取侦听器锁的情况下，用于恢复侦听器锁的时间间隔。| 
|lockAcquisitionTimeout|00:01:00|运行时尝试获取锁的最长时间。| 
|lockAcquisitionPollingInterval|不适用|尝试获取锁的间隔时间。| 

## <a name="tracing"></a>tracing

版本 1.x

使用 `TraceWriter` 对象创建的日志的配置设置。 请参阅 [C# 日志记录](functions-reference-csharp.md#logging)和 [Node.js 日志记录](functions-reference-node.md#writing-trace-output-to-the-console)。 在版本 2.x 中，所有日志行为都由日志记录控制。

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|属性  |默认 | 说明 |
|---------|---------|---------| 
|consoleLevel|info|控制台日志记录的跟踪级别。 选项包括：`off`、`error`、`warning`、`info` 和 `verbose`。|
|fileLoggingMode|debugOnly|文件日志记录的跟踪级别。 选项包括 `never`、`always` 和 `debugOnly`。| 

## <a name="version"></a>版本

版本 2.x

针对 v2 运行时的函数应用需要版本字符串 `"version": "2.0"`。

## <a name="watchdirectories"></a>watchDirectories

应该监视其更改情况的一组[共享代码目录](functions-reference-csharp.md#watched-directories)。  确保当这些目录中的代码发生更改时，函数会拾取这些更改。

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何更新 host.json 文件](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [查看环境变量中的全局设置](functions-app-settings.md)

<!-- Update_Description: wording update -->