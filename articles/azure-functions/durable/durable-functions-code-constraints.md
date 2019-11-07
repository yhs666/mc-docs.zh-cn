---
title: 持久性业务流程协调程序代码约束 - Azure Functions
description: 适用于 Azure Durable Functions 的业务流程函数重播和代码约束。
author: cgillum
manager: gwallace
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
origin.date: 08/18/2019
ms.date: 09/24/2019
ms.author: v-junlch
ms.openlocfilehash: 9b75971bdc2a288370b2fb16e7ab080e26290d79
ms.sourcegitcommit: 73a8bff422741faeb19093467e0a2a608cb896e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2019
ms.locfileid: "71673619"
---
# <a name="orchestrator-function-code-constraints"></a>业务流程协调程序函数代码约束

Durable Functions 是 [Azure Functions](../functions-overview.md) 的一个扩展，用于构建有状态应用程序。 可以使用业务流程协调程序函数来协调函数应用中其他持久函数的执行。[](durable-functions-orchestrations.md) 业务流程协调程序函数带有状态且可靠，可以长时间运行。

## <a name="orchestrator-code-constraints"></a>业务流程协调程序代码约束

业务流程协调程序函数使用事件溯源来确保执行的可靠性，并保留本地变量状态。 业务流程协调程序代码的[重播行为](durable-functions-orchestrations.md#reliability)针对可在业务流程协调程序函数中编写的代码类型创建约束。 例如，业务流程协调程序代码必须是确定性的  。  业务流程协调程序函数会被重播多次，每次必须生成相同的结果。

以下部分提供了一些简单的准则，用于确保代码是确定性的。

### <a name="using-deterministic-apis"></a>使用确定性 API

业务流程协调程序函数可以根据需要采用目标语言自由地调用任何 API。 但是，必须确保业务流程协调程序函数只调用确定性  API。 确定性 API  是一种在输入相同时始终返回同一值的 API，不管何时调用，也不管调用频率如何。

下面是应该避免使用的 API 的一些示例，因为它们不是确定性的。  这些限制仅适用于业务流程协调程序函数。 其他函数类型没有此类限制。

| API 类别 | Reason | 解决方法 |
| ------------ | ------ | ---------- |
| 日期/时间  | 返回当前日期或时间的 API  是非确定性的，因为每次重播时它们返回的值都不相同。 | 使用可安全重播的 [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) (.NET) 或 `currentUtcDateTime` (JavaScript) API。 |
| GUID/UUID  | 返回随机 GUID/UUID 的 API  是非确定性的，因为每次重播时它们生成的值都不相同。 | 使用 [NewGuid](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_NewGuid) (.NET) 或 `newGuid` (JavaScript) 安全地生成随机 GUID。 |
| 随机数 | 返回随机数的 API 是非确定性的，因为每次重播时它们生成的值都不相同。 | 使用活动函数将随机数返回给业务流程。 就重播来说，活动函数的返回值始终是安全的。 |
| 绑定 | 输入和输出绑定通常会执行 I/O 操作，是非确定性的。 即使是[业务流程客户端](durable-functions-bindings.md#orchestration-client)和[实体客户端](durable-functions-bindings.md#entity-client)绑定，也不得由业务流程协调程序函数直接使用。 | 在客户端或活动函数中使用输入和输出绑定。 |
| 网络 | 网络调用涉及外部系统，是非确定性的。 | 使用活动函数进行网络调用。 如果需要从业务流程协调程序函数进行 HTTP 调用，则也可选择使用[持久性 HTTP API](durable-functions-http-features.md#consuming-http-apis)。 |
| 阻止 API | `Thread.Sleep` (.NET) 之类的阻止 API 或其他类似的 API 可能导致业务流程协调程序函数出现性能和缩放问题，应该避免使用。 在 Azure Functions 消耗计划中，它们甚至可能导致不必要的执行时收费。 | 在适用的情况下使用阻止 API 的替代方案（例如 `CreateTimer`），在业务流程执行过程中引入延迟。 [持久计时器](durable-functions-timers.md)延迟不计入业务流程协调程序函数的执行时间。 |
| 异步 API | 除非使用 [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) API 或 `context.df` 对象的 API，否则业务流程协调程序不得发起任何异步操作  。 例如，.NET 中没有 `Task.Run`、`Task.Delay` 或 `HttpClient.SendAsync`，JavaScript 中没有 `setTimeout()` 和 `setInterval()`。 Durable Task Framework 在单个线程上执行业务流程协调程序代码，不能与可由其他异步 API 计划的其他任何线程交互。 | 业务流程协调程序函数只应进行持久性异步调用。  应该通过活动函数进行任何其他的异步 API 调用。 |
| 异步 JavaScript 函数 | JavaScript 业务流程协调程序函数不能是 `async`，因为 node.js 运行时不保证异步函数是确定性的。 | JavaScript 业务流程协调程序函数必须声明为同步的生成器函数。 |
| 线程 API | Durable Task Framework 在单个线程上执行业务流程协调程序代码，不能与任何其他线程交互。 将新线程引入业务流程的执行中可能导致非确定性执行或死锁。 | 大多数情况下，不应在业务流程协调程序函数中使用线程 API。 如果必须使用它们，则应将其局限于活动函数。 |
| 静态变量 | 非常数静态变量应该避免在业务流程协调程序函数中使用，因为其值可能随时间而变，导致非确定性的执行行为。 | 使用常数，或者将静态变量的使用局限于活动函数。 |
| 环境变量 | 请勿在业务流程协调程序函数中使用环境变量。 其值可能随时间而变，导致非确定性的执行行为。 | 环境变量只能在客户端函数或活动函数内部引用。 |
| 无限循环 | 在业务流程协调程序函数中，应避免无限循环。 Durable Task Framework 在业务流程函数的执行过程中会保存执行历史记录，因此无限循环可能会导致业务流程协调程序实例耗尽内存。 | 对于无限循环方案，可使用 [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) (.NET) 或 `continueAsNew` (JavaScript) 等 API 来重启函数执行，并丢弃以前的执行历史记录。 |

尽管这些约束在乍看之下让人心虚，但其实并不难遵守。 Durable Task Framework 会尝试检测上述规则的冲突，并引发 `NonDeterministicOrchestrationException`。 但是，这种检测行为是尽力而为的，请不要对它有依赖。

## <a name="versioning"></a>版本控制

持久性业务流程可能会连续执行数天、数月、数年，甚至会[永久](durable-functions-eternal-orchestrations.md)执行。 对 Durable Functions 应用程序进行代码更新时，如果此类更新影响尚未完成的业务流程，则可能会中断其重播行为。 因此，在更新代码时请仔细进行计划，这很重要。 有关如何进行代码版本控制的更详细说明，请参阅[版本控制](durable-functions-versioning.md)一文。

## <a name="durable-tasks"></a>持久任务

> [!NOTE]
> 本部分介绍 Durable Task Framework 的内部实现详细信息。 在不了解这些信息的情况下也可以使用 Durable Functions。 本部分旨在帮助读者了解重播行为。

可在业务流程协调程序函数中安全等待的任务有时称为“持久任务”。  这些任务由 Durable Task Framework 创建和管理。 示例是由 .NET 业务流程协调程序函数中的 `CallActivityAsync`、`WaitForExternalEvent` 和 `CreateTimer` 返回的任务。

可以在 .NET 中使用 `TaskCompletionSource` 对象的列表对这些持久任务进行内部管理。  在重播期间，这些任务在业务流程协调程序代码执行过程中予以创建，并在调度程序枚举相应历史记录事件时完成。 将会通过单个线程以同步方式执行，直至重播完所有历史记录。 在历史记录重播结束时尚未完成的持久任务会执行相应的操作。例如，可能会将一条消息排队，以调用活动函数。

借助此处所述的执行行为，我们应该可以理解业务流程协调程序函数代码为何不得将非持久任务置于 `await` 或 `yield` 状态：调度程序线程无法等待该任务完成，并且该任务发出的任何回调可能会破坏业务流程协调程序函数的跟踪状态。 进行某些运行时检查是为了检测此类违规行为。

如需有关 Durable Task Framework 如何执行业务流程协调程序函数的详细信息，最好是查阅 [GitHub 上的持久任务源代码](https://github.com/Azure/durabletask)。 具体而言，请参阅 [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) 和[TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何调用子业务流程](durable-functions-sub-orchestrations.md)

> [!div class="nextstepaction"]
> [了解如何处理版本控制](durable-functions-versioning.md)
