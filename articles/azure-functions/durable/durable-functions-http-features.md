---
title: Durable Functions 中的 HTTP 功能 - Azure Functions
description: 了解 Azure Functions 的 Durable Functions 扩展中的集成式 HTTP 功能。
author: cgillum
manager: gwallace
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
origin.date: 09/04/2019
ms.date: 09/24/2019
ms.author: v-junlch
ms.openlocfilehash: ada2d9747ab021d8bd91cbce438abe307e786544
ms.sourcegitcommit: 73a8bff422741faeb19093467e0a2a608cb896e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2019
ms.locfileid: "71673615"
---
# <a name="http-features"></a>HTTP 功能

Durable Functions 提供多个功能来方便用户将持久性业务流程和实体整合到 HTTP 工作流中。 本文将会详细介绍其中的某些功能。

## <a name="exposing-http-apis"></a>公开 HTTP API

可以使用 HTTP 请求来调用和管理业务流程与实体。 Durable Functions 扩展公开内置的 HTTP API，并提供相应的 API 用于从 HTTP 触发的函数内部来与业务流程和实体交互。

### <a name="built-in-http-apis"></a>内置的 HTTP API

Durable Functions 扩展自动将一组 HTTP API 添加到 Azure Functions 宿主。 使用这些 API，无需编写任何代码即可与业务流程和实体交互并对其进行管理。

支持以下内置 HTTP API。

* [启动新业务流程](durable-functions-http-api.md#start-orchestration)
* [查询业务流程实例](durable-functions-http-api.md#get-instance-status)
* [终止业务流程实例](durable-functions-http-api.md#terminate-instance)
* [将外部事件发送到业务流程](durable-functions-http-api.md#raise-event)
* [清除业务流程历史记录](durable-functions-http-api.md#purge-single-instance-history)
* [将操作事件发送到实体](durable-functions-http-api.md#signal-entity)
* [查询实体的状态](durable-functions-http-api.md#query-entity)

有关 Durable Functions 扩展公开的所有内置 HTTP API 的完整说明，请参阅 [HTTP API](durable-functions-http-api.md) 一文。

### <a name="http-api-url-discovery"></a>HTTP API URL 发现

[业务流程客户端绑定](durable-functions-bindings.md#orchestration-client)公开可用于生成便捷 HTTP 响应有效负载的 API。 例如，它可以创建一个响应，其中包含特定业务流程实例的管理 API 的链接。 以下 HTTP 触发器函数示例演示如何对新的业务流程实例使用此 API：

#### <a name="precompiled-c"></a>预编译 C#

```C#
// Copyright (c) .NET Foundation. All rights reserved.
// Licensed under the MIT License. See LICENSE in the project root for license information.

using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;

namespace VSSample
{
    public static class HttpStart
    {
        [FunctionName("HttpStart")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
            [OrchestrationClient] DurableOrchestrationClientBase starter,
            string functionName,
            ILogger log)
        {
            // Function input comes from the request content.
            dynamic eventData = await req.Content.ReadAsAsync<object>();
            string instanceId = await starter.StartNewAsync(functionName, eventData);

            log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

            return starter.CreateCheckStatusResponse(req, instanceId);
        }
    }
}
```

#### <a name="c-script"></a>C# 脚本

```C#
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"
#r "Microsoft.Extensions.Logging"
#r "Newtonsoft.Json"

using System.Net;
using System.Net.Http.Headers;

public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);
    
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
    
    return starter.CreateCheckStatusResponse(req, instanceId);
}
```
#### <a name="javascript-functions-2x-only"></a>JavaScript（仅限 Functions 2.x）

```JavaScript
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);
    const instanceId = await client.startNew(req.params.functionName, undefined, req.body);

    context.log(`Started orchestration with ID = '${instanceId}'.`);

    return client.createCheckStatusResponse(context.bindingData.req, instanceId);
};
```
#### <a name="functionjson"></a>Function.json

```JavaScript
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "route": "orchestrators/{functionName}",
      "methods": ["get", "post"]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    }
  ]
}
```

可以通过任何 HTTP 客户端启动使用上面所示 HTTP 触发器函数的业务流程协调程序函数。 以下 cURL 命令启动名为 `DoWork` 的业务流程协调程序函数。

```bash
curl -X POST https://localhost:7071/orchestrators/DoWork -H "Content-Length: 0" -i
```

下面是业务流程的示例响应，其中包含 `abc123` 作为该业务流程的 ID（为简明起见，此处删除了一些细节）：

```http
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX
Retry-After: 10

{
    "id": "abc123",
    "purgeHistoryDeleteUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX",
    "sendEventPostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123/raiseEvent/{eventName}?code=XXX",
    "statusQueryGetUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX",
    "terminatePostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123/terminate?reason={text}&code=XXX"
}
```

以上示例中的每个 `~Uri` 字段对应于内置的 HTTP API。 可以使用这些 API 来管理目标业务流程实例。

> [!NOTE]
> Webhook URL 的格式可能会有所不同，具体取决于所运行 Azure Functions 主机的版本。 上面的示例适用于 Azure Functions 2.0 主机。

有关所有内置 HTTP API 的介绍，请参阅 [HTTP API 参考](durable-functions-http-api.md)文档。

### <a name="async-operation-tracking"></a>异步操作跟踪

前面提到的 HTTP 响应旨在通过 Durable Functions 实现长时间运行的 HTTP 异步 API。 此模式有时称为“轮询使用者模式”。  客户端/服务器流工作方式如下：

1. 客户端发出 HTTP 请求，启动长时间运行的进程，例如业务流程协调程序函数。
2. 目标 HTTP 触发器返回 HTTP 202 响应，其中包含带有 `statusQueryGetUri` 值的 `Location` 标头。
3. 客户端轮询 `Location` 标头中的 URL。 可继续看到包含 `Location` 标头的 HTTP 202 响应。
4. 实例完成（或失败）后，`Location` 标头中的终结点返回 HTTP 200。

此协议允许通过外部客户端或支持轮询 HTTP 终结点并遵循 `Location` 标头的服务协调长时间运行的进程。 此模式的客户端和服务器实现内置于 Durable Functions HTTP API 中。

> [!NOTE]
> 默认情况下，[Azure 逻辑应用](https://www.azure.cn/home/features/logic-apps/)提供的所有基于 HTTP 的操作都支持标准异步操作模式。 使用此功能，可在逻辑应用工作流中嵌入长时间运行的持久函数。 有关异步 HTTP 模式的逻辑应用支持的更多详细信息，请参阅 [Azure 逻辑应用工作流操作和触发器文档](../../logic-apps/logic-apps-workflow-actions-triggers.md)。

> [!NOTE]
> 可以从任何函数类型（而不仅仅是 HTTP 触发的函数）来与业务流程交互。

有关如何使用客户端 API 管理业务流程和实体的详细信息，请参阅[实例管理](durable-functions-instance-management.md)一文。

## <a name="consuming-http-apis"></a>使用 HTTP API

根据[协调程序函数代码约束](durable-functions-code-constraints.md)中所述，不允许业务流程协调程序函数直接执行 I/O。 业务流程协调程序函数通常调用执行 I/O 操作的[活动函数](durable-functions-types-features-overview.md#activity-functions)。

从 Durable Functions 2.0 开始，业务流程原生可以通过[业务流程触发器绑定](durable-functions-bindings.md#orchestration-trigger)来使用 HTTP API。

> [!NOTE]
> 在 JavaScript 中，目前无法直接从业务流程协调程序函数调用 HTTP 终结点。

以下示例代码演示了一个使用 `CallHttpAsync` .NET API 发出出站 HTTP 请求的 C# 业务流程协调程序函数：

```csharp
[FunctionName("CheckSiteAvailable")]
public static async Task CheckSiteAvailable(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    Uri url = context.GetInput<Uri>();

    // Makes an HTTP GET request to the specified endpoint
    DurableHttpResponse response = 
        await context.CallHttpAsync(HttpMethod.Get, url);

    if (response.StatusCode >= 400)
    {
        // handling of error codes goes here
    }
}
```

使用“调用 HTTP”操作可以在业务流程协调程序函数中实现以下目的：

* 直接从业务流程函数调用 HTTP API（但存在后面所述的一些限制）。
* 自动支持客户端 HTTP 202 状态轮询模式。
* 使用 [Azure 托管标识](../../active-directory/managed-identities-azure-resources/overview.md)对其他 Azure 终结点发出授权的 HTTP 调用。

直接从业务流程协调程序函数使用 HTTP API 的功能旨在为一组特定的常见方案提供便利。 你可以使用活动函数自行实现所有这些功能。 在许多情况下，活动函数可以提供更大的灵活性。

### <a name="http-202-handling"></a>HTTP 202 处理

“调用 HTTP”API 可自动实现客户端“轮询使用者模式”。  如果被调用的 API 返回带有 `Location` 标头的 HTTP 202 响应，则业务流程协调程序函数将自动轮询 `Location` 资源，直到返回非 202 响应。 非 202 响应是返回到业务流程协调程序函数代码的响应。

> [!NOTE]
> 业务流程协调程序函数原生还支持服务器端“轮询使用者模式”，如[异步操作跟踪](#async-operation-tracking)中所述。  这意味着，一个函数应用中的业务流程可以轻松协调其他函数应用中的业务流程协调程序函数。 这类似于[子业务流程](durable-functions-sub-orchestrations.md)的概念，但支持跨应用的通信。 这对于微服务式的应用程序开发特别有用。

### <a name="managed-identities"></a>托管标识

Durable Functions 原生支持调用接受 Azure AD 授权令牌的 API。 此项支持使用 [Azure 托管标识](../../active-directory/managed-identities-azure-resources/overview.md)来获取这些令牌。

以下代码是 .NET 业务流程协调程序函数的一个示例，该函数发出经过身份验证的调用来使用 Azure 资源管理器[虚拟机 REST API](https://docs.microsoft.com/rest/api/compute/virtualmachines) 重启虚拟机。

```csharp
[FunctionName("RestartVm")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string subscriptionId = "mySubId";
    string resourceGroup = "myRG";
    string vmName = "myVM";
    
    // Automatically fetches an Azure AD token for resource = https://management.core.chinacloudapi.cn
    // and attaches it to the outgoing Azure Resource Manager API call.
    var restartRequest = new DurableHttpRequest(
        HttpMethod.Post, 
        new Uri($"https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Compute/virtualMachines/{vmName}/restart?api-version={apiVersion}"),
        tokenSource: new ManagedIdentityTokenSource("https://management.core.chinacloudapi.cn"));
    DurableHttpResponse restartResponse = await context.CallHttpAsync(restartRequest);
    if (restartResponse.StatusCode != HttpStatusCode.OK)
    {
        throw new ArgumentException($"Failed to restart VM: {restartResponse.StatusCode}: {restartResponse.Content}");
    }
}
```

在以上示例中，`tokenSource` 参数配置为获取 [Azure 资源管理器](../../azure-resource-manager/resource-group-overview.md)的 Azure AD 令牌（由“资源 URI”`https://management.core.chinacloudapi.cn` 标识）。 该示例假设当前函数应用在本地运行，或者已使用托管标识部署为 Azure Functions 应用。 假设本地标识或托管标识有权管理指定资源组 `myRG` 中的 VM。

在运行时，配置的令牌源会自动返回 OAuth 2.0 访问令牌，并将其作为持有者令牌添加到传出请求的 `Authorization` 标头中。 相比于将授权标头手动添加到 HTTP 请求，此模型是一种改进，原因如下：

* 可自动处理令牌刷新。 无需担心令牌过期。
* 永远不会以持久性业务流程状态存储令牌。
* 无需编写任何代码即可处理令牌获取。

可以在[预编译的 C#“RestartVMs”示例](https://github.com/Azure/azure-functions-durable-extension/blob/v2/samples/v2/precompiled/RestartVMs.cs)中找到更完整的示例。

托管标识并不局限于 Azure 资源管理。 托管标识可用于访问接受 Azure AD 持有者令牌的任何 API，包括第一方 Azure 服务或第三方 Web 应用程序。 第三方 Web 应用甚至可以是其他函数应用。 有关支持使用 Azure AD 进行身份验证的第一方 Azure 服务的列表，请参阅[支持 Azure AD 身份验证的 Azure 服务](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)。

### <a name="limitations"></a>限制

内置的 HTTP API 调用支持只是一项便利功能，并不适合用于所有方案。 业务流程协调程序函数发送的 HTTP 请求及其响应将序列化并保存为队列消息。 此排队行为确保 HTTP 调用在[业务流程重播中可靠并安全](durable-functions-orchestrations.md#reliability)。 但是，此排队行为也意味着：

* 与本机 HTTP 客户端相比，每个 HTTP 请求会造成更大的延迟。
* 无法装入队列消息的较大请求或响应消息可能会明显降低业务流程的性能。 潜在性能降低的原因是将消息有效负载分散到 Blob 存储会产生开销。
* 不支持流式处理、分块和二进制有效负载。
* 自定义 HTTP 客户端行为的功能受到限制。

如果上述任何限制可能会影响你的用例，请考虑改用活动函数和特定于语言的 HTTP 客户端库来发出出站 HTTP 调用。

> [!NOTE]
> 如果你是 .NET 开发人员，可能想要知道此功能为何使用 `DurableHttpRequest` 和 `DurableHttpResponse` 类型，而不是内置的 .NET `HttpRequestMessage` 和 `HttpResponseMessage`。 这种选择是设计使然。 主要原因在于，自定义类型有助于确保用户不会对内部 HTTP 客户端支持的行为做出错误的假设。 使用特定的持久类型还可以简化 API 设计，并更轻松地实现[托管标识集成](#managed-identities)和[轮询使用者模式](#http-202-handling)等特殊功能。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解持久实体](durable-functions-entities.md)

