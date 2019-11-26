---
title: Durable Functions 中的扇出/扇入方案 - Azure
description: 了解如何在 Azure Functions 的 Durable Functions 扩展中实现扇出/扇入方案。
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
origin.date: 11/02/2019
ms.date: 11/18/2019
ms.author: v-junlch
ms.openlocfilehash: 4392868652ed43d5a15b6417c9ab8e82c996dd58
ms.sourcegitcommit: a4b88888b83bf080752c3ebf370b8650731b01d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2019
ms.locfileid: "74178997"
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Durable Functions 中的扇出/扇入方案 - 云备份示例

“扇出/扇入”是指同时执行多个函数，然后针对结果执行某种聚合的模式。  本文讲解一个使用 [Durable Functions](durable-functions-overview.md) 实现扇入/扇出方案的示例。 该示例是一个持久函数，可将应用的全部或部分站点内容备份到 Azure 存储中。

[!INCLUDE [v1-note](../../../includes/functions-durable-v1-tutorial-note.md)]

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>方案概述

在此示例中，函数会将指定目录下的所有文件以递归方式上传到 Blob 存储。 它们还会统计已上传的字节总数。

可以编写单个函数来处理所有这些操作。 会遇到的主要问题是**可伸缩性**。 单个函数执行只能在单个 VM 上运行，因此，吞吐量会受到该 VM 的吞吐量限制。 另一个问题是**可靠性**。 如果中途失败或者整个过程花费的时间超过 5 分钟，则备份可能以部分完成状态失败。 然后，需要重新开始备份。

更可靠的方法是编写两个正则函数：一个函数枚举文件并将文件名添加到队列，另一个函数从队列读取数据并将文件上传到 Blob 存储。 此方法可以提高吞吐量和可靠性，但需要预配和管理队列。 更重要的是，如果想要执行其他任何操作，例如报告已上传的字节总数，则这种做法会明显增大**状态管理**和**协调**的复杂性。

Durable Functions 方法提供前面所述的所有优势，并且其系统开销极低。

## <a name="the-functions"></a>函数

本文介绍示例应用中的以下函数：

* `E2_BackupSiteContent`
* `E2_GetFileList`
* `E2_CopyFileToBlob`

以下部分介绍用于 C# 脚本的配置和代码。 文章末尾展示了用于 Visual Studio 开发的代码。

## <a name="the-cloud-backup-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>云备份业务流程（Visual Studio Code 和 Azure 门户的示例代码）

`E2_BackupSiteContent` 函数对业务流程协调程序函数使用标准的 *function.json*。

```json
{
  "bindings": [
    {
      "name": "backupContext",
      "type": "orchestrationTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

下面的代码可实现业务流程协调程序函数：

### <a name="c"></a>C#

```c#
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static async Task<long> Run(DurableOrchestrationContext backupContext)
{
    string rootDirectory = Environment.ExpandEnvironmentVariables(backupContext.GetInput<string>() ?? "");
    if (string.IsNullOrEmpty(rootDirectory))
    {
        throw new ArgumentException("A directory path is required as an input.");
    }

    if (!Directory.Exists(rootDirectory))
    {
        throw new DirectoryNotFoundException($"Could not find a directory named '{rootDirectory}'.");
    }

    string[] files = await backupContext.CallActivityAsync<string[]>(
        "E2_GetFileList",
        rootDirectory);

    var tasks = new Task<long>[files.Length];
    for (int i = 0; i < files.Length; i++)
    {
        tasks[i] = backupContext.CallActivityAsync<long>(
            "E2_CopyFileToBlob",
            files[i]);
    }

    await Task.WhenAll(tasks);

    long totalBytes = tasks.Sum(t => t.Result);
    return totalBytes;
}
```

### <a name="javascript-functions-20-only"></a>JavaScript（仅限 Functions 2.0）

```JavaScript
const df = require("durable-functions");

module.exports = df(function*(context){
    const rootDirectory = context.df.getInput();
    if (!rootDirectory) {
        throw new Error("A directory path is required as an input.");
    }

    const files = yield context.df.callActivityAsync("E2_GetFileList", rootDirectory);

    // Backup Files and save Promises into array
    const tasks = [];
    for (const file of files) {
        tasks.push(context.df.callActivityAsync("E2_CopyFileToBlob", file));
    }

    // wait for all the Backup Files Activities to complete, sum total bytes
    const results = yield context.df.Task.all(tasks);
    const totalBytes = results.reduce((prev, curr) => prev + curr, 0);

    // return results;
    return totalBytes;
});
```

本质上，该业务流程协调程序函数执行以下操作：

1. 采用 `rootDirectory` 值作为输入参数。
2. 调用某个函数来获取 `rootDirectory` 下的文件的递归列表。
3. 发出多个并行函数调用，以将每个文件上传到 Azure Blob 存储。
4. 等待所有上传完成。
5. 返回已上传到 Azure Blob 存储的总字节数。

请注意 `await Task.WhenAll(tasks);` (C#) 和 `yield context.df.Task.all(tasks);` (JavaScript) 所在的行。 对 `E2_CopyFileToBlob` 函数的所有单个调用都未处于等待状态，这使它们可以并行运行。  将此任务数组传递给 `Task.WhenAll` (C#) 或 `context.df.Task.all` (JavaScript) 时，会获得所有复制操作完成之前不会完成的任务。  如果熟悉 .NET 中的任务并行库 (TPL) 或 JavaScript 中的 [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)，则对此过程也不会陌生。 差别在于，这些任务可在多个 VM 上同时运行，Durable Functions 扩展可确保端到端执行能够弹性应对进程回收。

> [!NOTE]
> 虽然任务在概念上类似于 JavaScript 承诺，但业务流程协调程序函数应使用 `context.df.Task.all` 和 `context.df.Task.any`（而不是 `Promise.all` 和 `Promise.race`）来管理任务并行化。

完成 `Task.WhenAll` 并进入等待中状态（或从 `context.df.Task.all` 输出）后，我们知道所有函数调用都已完成，并已收到返回值。 每次调用 `E2_CopyFileToBlob` 都会返回已上传字节数，因此，将所有这些返回值相加就能计算出字节数总和。

## <a name="helper-activity-functions"></a>帮助器活动函数

与其他示例一样，帮助器活动函数无非是使用 `activityTrigger` 触发器绑定的正则函数。 例如，`E2_GetFileList` 的 *function.json* 文件如下所示：

```json
{
  "bindings": [
    {
      "name": "rootDirectory",
      "type": "activityTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

下面是实现：

### <a name="c"></a>C#

```c#
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static string[] Run(string rootDirectory, TraceWriter log)
{
    string[] files = Directory.GetFiles(rootDirectory, "*", SearchOption.AllDirectories);
    log.Info($"Found {files.Length} file(s) under {rootDirectory}.");

    return files;
}
```

### <a name="javascript-functions-20-only"></a>JavaScript（仅限 Functions 2.0）

```JavaScript
const readdirp = require("readdirp");

module.exports = function (context, rootDirectory) {
    context.log(`Searching for files under '${rootDirectory}'...`);
    const allFilePaths = [];

    readdirp(
        {root: rootDirectory, entryType: 'all'},
        function (fileInfo) {
            if (!fileInfo.stat.isDirectory()) {
                allFilePaths.push(fileInfo.fullPath);
            }
        },
        function (err, res) {
            if (err) {
                throw err;
            }

            context.log(`Found ${allFilePaths.length} under ${rootDirectory}.`);
            context.done(null, allFilePaths);
        }
    );
};
```

`E2_GetFileList` 的 JavaScript 实现使用 `readdirp` 模块以递归方式读取目录结构。

> [!NOTE]
> 你可能会疑惑，为何不直接将此代码放入业务流程协调程序函数？ 可以这样做，不过，这会破坏业务流程协调程序函数的基本规则，即，它们不得执行 I/O，包括本地文件系统的访问。

`E2_CopyFileToBlob` 的 *function.json* 文件同样也很简单：

```json
{
  "bindings": [
    {
      "name": "filePath",
      "type": "activityTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C# 实现也很简单。 本示例恰好使用了 Azure Functions 绑定的某些高级功能（即使用了 `Binder` 参数），但对于本演练，无需考虑这些细节。

### <a name="c"></a>C#

```c#
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static async Task<long> Run(
    string filePath,
    Binder binder,
    TraceWriter log)
{
    long byteCount = new FileInfo(filePath).Length;

    // strip the drive letter prefix and convert to forward slashes
    string blobPath = filePath
        .Substring(Path.GetPathRoot(filePath).Length)
        .Replace('\\', '/');
    string outputLocation = $"backups/{blobPath}";

    log.Info($"Copying '{filePath}' to '{outputLocation}'. Total bytes = {byteCount}.");

    // copy the file contents into a blob
    using (Stream source = File.Open(filePath, FileMode.Open, FileAccess.Read, FileShare.ReadWrite))
    using (Stream destination = await binder.BindAsync<CloudBlobStream>(
        new BlobAttribute(outputLocation)))
    {
        await source.CopyToAsync(destination);
    }
    
    return byteCount;
}
```

### <a name="javascript-functions-20-only"></a>JavaScript（仅限 Functions 2.0）

JavaScript 实现无法访问 Azure Functions 的 `Binder` 功能，因此[用于 Node 的 Azure 存储 SDK](https://github.com/Azure/azure-storage-node) 将取而代之。

```JavaScript
const fs = require("fs");
const path = require("path");
const storage = require("azure-storage");

module.exports = function (context, filePath) {
    const container = "backups";
    const root = path.parse(filePath).root;
    const blobPath = filePath
        .substring(root.length)
        .replace("\\", "/");
    const outputLocation = `backups/${blobPath}`;
    const blobService = storage.createBlobService();

    blobService.createContainerIfNotExists(container, (error) => {
        if (error) {
            throw error;
        }

        fs.stat(filePath, function (error, stats) {
            if (error) {
                throw error;
            }
            context.log(`Copying '${filePath}' to '${outputLocation}'. Total bytes = ${stats.size}.`);

            const readStream = fs.createReadStream(filePath);

            blobService.createBlockBlobFromStream(container, blobPath, readStream, stats.size, function (error) {
                if (error) {
                    throw error;
                }

                context.done(null, stats.size);
            });
        });
    });
};
```

实现从磁盘加载文件，并以异步方式将内容流式传输到“backups”容器中同名的 Blob。 返回值为已复制到存储的字节数，业务流程协调程序函数随后会使用此数字来计算总和。

> [!NOTE]
> 这是一个演示如何将 I/O 操作移入 `activityTrigger` 函数的极佳示例。 这样，不仅可以在许多不同的 VM 上分配工作，而且还能获得设置进度检查点的优势。 如果主机进程出于任何原因终止，你就知道哪些上传操作已完成。

## <a name="run-the-sample"></a>运行示例

可以通过发送以下 HTTP POST 请求来启动业务流程。

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

> [!NOTE]
> 调用的 `HttpStart` 函数只会处理 JSON 格式的内容。 为此，`Content-Type: application/json` 标头是必需的，目录路径已编码为 JSON 字符串。 此外，HTTP 代码片段假定 `host.json` 文件中有一个条目，该条目从所有 HTTP 触发器函数 URL 中删除默认的 `api/` 前缀。 可以在示例的 `host.json` 文件中找到此配置的标记。

此 HTTP 请求会触发 `E2_BackupSiteContent` 业务流程协调程序，并将字符串 `D:\home\LogFiles` 作为参数传递。 响应提供了一个链接，可使用该链接获取备份操作的状态：

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

根据函数应用中包含的日志文件数，此操作可能需要几分钟才能完成。 可以通过查询上述 HTTP 202 响应的 `Location` 标头中的 URL 来获取最新状态。

```
GET http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 202 Accepted
Content-Length: 148
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":"D:\\home\\LogFiles","output":null,"createdTime":"2019-06-29T18:50:55Z","lastUpdatedTime":"2019-06-29T18:51:16Z"}
```

在本例中，函数仍在运行。 可以查看已保存到业务流程协调程序状态中的输入，以及上次更新时间。 可以继续使用 `Location` 标头值来轮询完成状态。 当状态为“Completed”时，会看到如下所示的 HTTP 响应值：

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2019-06-29T18:50:55Z","lastUpdatedTime":"2019-06-29T18:51:26Z"}
```

现在，可以看到业务流程已完成，以及完成它大约花费的时间。 另外，还会看到 `output` 字段的值，指示已上传大约 450 KB 的日志。

## <a name="visual-studio-sample-code"></a>Visual Studio 示例代码

下面是 Visual Studio 项目中以单个 C# 文件形式提供的业务流程：

> [!NOTE]
> 需要安装 `Microsoft.Azure.WebJobs.Extensions.Storage` NuGet 包才能运行下面的示例代码。


```c#
// Copyright (c) .NET Foundation. All rights reserved.
// Licensed under the MIT License. See LICENSE in the project root for license information.

using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.WindowsAzure.Storage.Blob;

namespace VSSample
{
    public static class BackupSiteContent
    {
        [FunctionName("E2_BackupSiteContent")]
        public static async Task<long> Run(
            [OrchestrationTrigger] DurableOrchestrationContext backupContext)
        {
            string rootDirectory = backupContext.GetInput<string>()?.Trim();
            if (string.IsNullOrEmpty(rootDirectory))
            {
                rootDirectory = Directory.GetParent(typeof(BackupSiteContent).Assembly.Location).FullName;
            }

            string[] files = await backupContext.CallActivityAsync<string[]>(
                "E2_GetFileList",
                rootDirectory);

            var tasks = new Task<long>[files.Length];
            for (int i = 0; i < files.Length; i++)
            {
                tasks[i] = backupContext.CallActivityAsync<long>(
                    "E2_CopyFileToBlob",
                    files[i]);
            }

            await Task.WhenAll(tasks);

            long totalBytes = tasks.Sum(t => t.Result);
            return totalBytes;
        }

        [FunctionName("E2_GetFileList")]
        public static string[] GetFileList(
            [ActivityTrigger] string rootDirectory, 
            TraceWriter log)
        {
            log.Info($"Searching for files under '{rootDirectory}'...");
            string[] files = Directory.GetFiles(rootDirectory, "*", SearchOption.AllDirectories);
            log.Info($"Found {files.Length} file(s) under {rootDirectory}.");

            return files;
        }

        [FunctionName("E2_CopyFileToBlob")]
        public static async Task<long> CopyFileToBlob(
            [ActivityTrigger] string filePath,
            Binder binder,
            TraceWriter log)
        {
            long byteCount = new FileInfo(filePath).Length;

            // strip the drive letter prefix and convert to forward slashes
            string blobPath = filePath
                .Substring(Path.GetPathRoot(filePath).Length)
                .Replace('\\', '/');
            string outputLocation = $"backups/{blobPath}";

            log.Info($"Copying '{filePath}' to '{outputLocation}'. Total bytes = {byteCount}.");

            // copy the file contents into a blob
            using (Stream source = File.Open(filePath, FileMode.Open, FileAccess.Read, FileShare.Read))
            using (Stream destination = await binder.BindAsync<CloudBlobStream>(
                new BlobAttribute(outputLocation, FileAccess.Write)))
            {
                await source.CopyToAsync(destination);
            }

            return byteCount;
        }
    }
}
```

## <a name="next-steps"></a>后续步骤

此示例说明了如何实现扇出/扇入模式。 下一个示例演示如何使用[持久计时器](durable-functions-timers.md)实现监视模式。

<!-- Update_Description: wording update -->