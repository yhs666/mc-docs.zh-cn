---
title: "Azure Service Fabric 中的可靠并发队列"
description: "可靠并发队列是一种高吞吐量队列，它允许并行排队和并行取消排队。"
services: service-fabric
documentationcenter: .net
author: sangarg
manager: timlt
editor: raja,tyadam,masnider,vturecek
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/01/2017
ms.author: v-johch
ms.openlocfilehash: 61988680ff09f928d0d29ccd02ad9d3c7e093432
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# Azure Service Fabric 中的可靠并发队列简介
<a id="introduction-to-reliableconcurrentqueue-in-azure-service-fabric" class="xliff"></a>
可靠并发队列是一种被复制的事务性异步队列。 该队列允许“并发排队”操作和“并发取消排队”操作来处理该队列。 此数据结构旨在通过放松严格的 FIFO 约束来提供高吞吐量，并为项提供尽力排序。

## 提供的 API
<a id="apis-offered" class="xliff"></a>

|并发队列                |可靠并发队列                                         |
|--------------------------------|------------------------------------------------------------------|
| void Enqueue(T item)           | Task EnqueueAsync(ITransaction tx, T item)                       |
| bool TryDequeue(out T result)  | Task< ConditionalValue < T > > TryDequeueAsync(ITransaction tx)  |
| int Count()                    | long Count()                                                     |

## 与[可靠队列](https://msdn.microsoft.com/library/azure/dn971527.aspx)比较
<a id="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx" class="xliff"></a>

可靠并发队列作为[可靠队列](https://msdn.microsoft.com/library/azure/dn971527.aspx)的替代推出， 其使用范围局限于不需要严格 FIFO 排序的情况，这种情况下，在尽量保证 FIFO 的同时，还需要考虑到并发性。  [可靠队列](https://msdn.microsoft.com/library/azure/dn971527.aspx)使用锁来强制实施 FIFO 排序，一次最多允许一个事务排队和/或取消排队。 相比之下，可靠并发队列会放松排序约束。 它允许任何数目的并发事务交错其排队和取消排队操作。 可靠并发队列的原则是“尽力排序”，但无法保证两个值的相对顺序。

在任何有多个执行排队和/或取消排队操作的并发事务的情况下，可靠并发队列相对于[可靠队列](https://msdn.microsoft.com/library/azure/dn971527.aspx)而言都可以提高吞吐量并降低延迟。   

可靠并发队列的一个不错的用例是[消息队列](https://en.wikipedia.org/wiki/Message_queue)方案。 在此方案中，有一个消息生产者，它创建一条消息并将其添加到独立于消息使用者的队列。 然后，消息使用者可以从队列中拉取消息并对其进行处理。 我们可能有多个相互独立工作的生产者和使用者，因此需要允许并发事务来处理该队列。

## 使用指南
<a id="usage-guidelines" class="xliff"></a>
* 队列期望项的保留期较短，就是说项停留在队列中的时间不宜过长。
* 队列不保证严格的 FIFO 排序。
* 队列不读取自己的写入。 项在事务中排队时，该项对于同一事务中的取消排队者来说为不可见。
* 取消排队不是相互隔离的。 如果项 A 在事务 txnA 中取消排队，则即使 txnA 尚未提交，项 A 也不会对并发事务 txnB 可见。
* 可以先使用 TryDequeueAsync，然后中止事务，从而实施 TryPeekAsync 行为。 “编程模式”部分提供了一个这样的代码片段。
* 计数是非事务性的。 它应该用于了解队列中的元素数。 它不保证队列计数。
* 如果处理取消排队项的代价较高，则必须将该处理延迟到以后进行，从而避免出现长时间运行的事务。

## 代码片段
<a id="code-snippets" class="xliff"></a>
让我们看一些这样的代码片段及预期的输出。 本部分忽略异常处理。

### - EnqueueAsync
<a id="--enqueueasync" class="xliff"></a>
下面是一些用于使用 EnqueueAsync 的代码片段，后跟预期的输出。

- 案例 1：单个排队任务

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken).ConfigureAwait(false);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken).ConfigureAwait(false);

    await txn.CommitAsync().ConfigureAwait(false);
}
```

假定该任务已成功完成，并且没有任何处理该队列的并行任务。 用户可以预期队列中的项将采用以下任一顺序：

> 10, 20

>20, 10


- 案例 2：并行排队任务

```
// Parallel Task 1
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken).ConfigureAwait(false);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken).ConfigureAwait(false);

    await txn.CommitAsync().ConfigureAwait(false);
}

// Parallel  Task 2
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 30, cancellationToken).ConfigureAwait(false);
    await this.Queue.EnqueueAsync(txn, 40, cancellationToken).ConfigureAwait(false);

    await txn.CommitAsync().ConfigureAwait(false);
}
```

假定没有任何其他处理该队列的并行任务，并且任务 1 和任务 2 并行运行。 那么，队列中项的顺序将无法推断。 就此代码片段来说，项的显示顺序可能是 4! 个 顺序中的任何一个。


### - DequeueAsync
<a id="--dequeueasync" class="xliff"></a>
下面是使用 TryDequeueAsync 的一些代码片段，后跟预期的输出。 假定已在队列中填充以下项：
> 10, 20, 30, 40, 50, 60

- 案例 1：单个取消排队任务

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false);
    await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false);
    await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false);

    await txn.CommitAsync().ConfigureAwait(false);
}
```

假定没有任何其他处理该队列的并行任务。 项将按以下顺序取消排队：
> 10, 20, 30

- 案例 2：并行取消排队任务

```
// Parallel Task 1
List<int> dequeue1;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false)).val;
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false)).val;

    await txn.CommitAsync().ConfigureAwait(false);
}

// Parallel Task 2
List<int> dequeue2;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false)).val;
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false)).val;

    await txn.CommitAsync().ConfigureAwait(false);
}
```

假定没有任何其他处理该队列的并行任务，并且任务 1 和任务 2 并行运行。 用户可以预期事务中的顺序会被保留，但项不会跨事务进行排序。 列表 dequeue1 的顺序和 dequeue2 的顺序可能是以下任何一个：
> 10, 20

> 10, 30

> 10, 40

> 20, 30

> 20, 40

> 30, 40

同一元素不会同时出现在这两个列表中。 因此，如果 dequeue1 包含 10、30，则 dequeue2 就会包含 20、40。

- 案例 3：中止事务时的取消排队排序

中止正在执行“取消排队”操作的事务，项就会重新回到队列头。 项回到队列头的顺序是不确定的。 请看以下代码：

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false);
    await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false);

    // Abort the transaction
    await txn.AbortAsync().ConfigureAwait(false);
}
```
假定项按以下顺序取消排队：
> 10, 20

中止事务后，项会添加回队列头。 由于“取消排队”操作未成功完成，项可按以下任何顺序添加回队列：
> 10, 20

> 20, 10

事务未成功提交的所有案例均是如此。

## 编程模式
<a id="programming-patterns" class="xliff"></a>
此部分介绍一些编程模式，在使用可靠并发队列时可能会用到这些模式。

### 对取消排队进行批处理
<a id="batch-dequeues" class="xliff"></a>
建议的编程模式是，通过使用者任务对取消排队进行批处理，而不是一次执行一个“取消排队”操作。 用户可以选择限制每个批处理之间的延迟，或者限制批处理大小。 以下代码片段显示了此编程模型。

```
int batchSize = 5;
long delayMs = 100;

while(!cancellationToken.IsCancellationRequested)
{
    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        for(int i = 0; i < batchSize; ++i)
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false);

            if (ret.HasValue)
            {
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
            else
            {
                // else break the for loop
                break;
            }
        }

        await txn.CommitAsync().ConfigureAwait(false);
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }

    int delayFactor = batchSize - processItems.Count;
    await Task.Delay(TimeSpan.FromMilliseconds(delayMs * delayFactor), cancellationToken).ConfigureAwait(false);
}
```

### 基于通知的处理
<a id="notification-based-processing" class="xliff"></a>
另一种有趣的编程模式是使用计数 API。 在这里，我们可以为队列实施基于通知的处理。 可以使用队列计数来限制排队或取消排队任务。

```
int threshold = 5;
long delayMs = 1000;

while(!cancellationToken.IsCancellationRequested)
{
    while (this.Queue.Count < threshold)
    {
        cancellationToken.ThrowIfCancellationRequested();

        // If the queue does not have the threshold number of items, delay the task and check again
        await Task.Delay(TimeSpan.FromMilliseconds(delayMs), cancellationToken).ConfigureAwait(false);
    }

    // If there are approximately threshold number of items, try and process the queue

    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false);

            if (ret.HasValue)
            {
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
        } while (processItems.Count < threshold && ret.HasValue);

        await txn.CommitAsync().ConfigureAwait(false);
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
}
```

### 尽力 DrainQueueAsync
<a id="best-effort-drainqueueasync" class="xliff"></a>
考虑到数据结构的并发特性，我们无法保证队列会被清空。 若要执行 DrainQueueAsync 任务，用户必须等待处理该队列的所有并行任务完成。 由于提交的排队和中止的取消排队均会将项添加到队列，因此应完成所有正在进行的排队和取消排队。 如果队列中的项数量很大，用户应选择对取消排队进行批处理。 所有并行任务都完成后，可以按以下方式实现 DrainQueueAsync：

```
int numItemsDequeued;
int batchSize = 5;

ConditionalValue ret;

do
{
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false);

            if(ret.HasValue)
            {
                // Buffer the dequeues
                processItems.Add(ret.Value);
            }
        } while (ret.HasValue && processItems.Count < batchSize);

        await txn.CommitAsync().ConfigureAwait(false);
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
} while (ret.HasValue);
```

### TryPeekAsync
<a id="trypeekasync" class="xliff"></a>
可靠并发队列不支持 TryPeekAsync API。 用户可以先使用 TryDequeueAsync，然后中止事务，从而获取同一行为。 让我们看看这样的代码片段。 在此示例中，仅当项的值大于 10 时，才会处理“取消排队”操作；

```
using (var txn = this.StateManager.CreateTransaction())
{
    ConditionalValue ret = await this.Queue.TryDequeueAsync(txn, cancellationToken).ConfigureAwait(false);
    bool valueProcessed = false;

    if (ret.HasValue)
    {
        if (ret.Value > 10)
        {
            // Process the item
            Console.WriteLine("Value : " + ret.Value);
            valueProcessed = true;
        }
    }

    if (valueProcessed)
    {
        await txn.CommitAsync().ConfigureAwait(false);    
    }
    else
    {
        await txn.AbortAsync().ConfigureAwait(false);
    }
}
```

## 必读
<a id="must-read" class="xliff"></a>
* [Reliable Services 快速启动](service-fabric-reliable-services-quick-start.md)
* [使用可靠集合](service-fabric-work-with-reliable-collections.md)
* [Reliable Services 通知](service-fabric-reliable-services-notifications.md)
* [Reliable Services 备份和还原（灾难恢复）](service-fabric-reliable-services-backup-restore.md)
* [可靠状态管理器和配置](service-fabric-reliable-services-configuration.md)
* [Service Fabric Web API 服务入门](service-fabric-reliable-services-communication-webapi.md)
* [Reliable Services 编程模型的高级用法](service-fabric-reliable-services-advanced-usage.md)
* [Reliable Collections 的开发人员参考](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
