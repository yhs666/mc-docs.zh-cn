---
title: 将数据批量导入到 Azure Cosmos DB SQL API 帐户时优化吞吐量
description: 了解如何生成用于优化将数据导入到 Azure Cosmos DB 所需的预配吞吐量 (RU/s) 的 .NET 控制台应用程序。
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: tutorial
origin.date: 11/04/2019
ms.date: 12/16/2019
ms.reviewer: sngun
ms.openlocfilehash: cee927fac9aa00cc15bff76d7eb2ee18719ac323
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348555"
---
# <a name="optimize-throughput-when-bulk-importing-data-to-azure-cosmos-db-sql-api-account"></a>将数据批量导入到 Azure Cosmos DB SQL API 帐户时优化吞吐量

本教程演示如何生成用于优化将数据导入到 Azure Cosmos DB 所需的预配吞吐量 (RU/s) 的 .NET 控制台应用程序。 在本文中，你将读取示例数据源中的数据，并将其导入到 Azure Cosmos 容器中。
本教程使用 [3.0+ 版](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) Azure Cosmos DB .NET SDK，后者以 .NET Framework 或 .NET Core 为目标。

本教程的内容包括：

> [!div class="checklist"]
> * 创建 Azure Cosmos 帐户
> * 配置项目
> * 连接到启用了批量支持的 Azure Cosmos 帐户
> * 通过并发创建操作执行数据导入

## <a name="prerequisites"></a>先决条件

在按照本文中的说明操作之前，请确保具备以下资源：

* 有效的 Azure 帐户。 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core)。 可以通过运行 `dotnet --version` 来验证你的环境中哪个版本可用。

## <a name="step-1-create-an-azure-cosmos-db-account"></a>步骤 1：创建 Azure Cosmos DB 帐户

从 Azure 门户[创建 Azure Cosmos DB SQL API 帐户](create-cosmosdb-resources-portal.md)或者可以通过使用 [Azure Cosmos DB 模拟器](local-emulator.md)来创建帐户。

## <a name="step-2-set-up-your-net-project"></a>步骤 2：设置 .NET 项目

从本地计算机打开 Windows 命令提示符或终端窗口。 你将从命令提示符或终端运行接下来的部分中的所有命令。 运行以下 dotnet 新命令，创建名为“bulk-import-demo”  的新应用。 `--langVersion` 参数在创建的项目文件中设置 LangVersion  属性。

   ```bash
   dotnet new console -langVersion:8 -n bulk-import-demo
   ```

将目录更改为新创建的应用文件夹。 可使用以下代码生成应用程序：

   ```bash
   cd bulk-import-demo
   dotnet build
   ```

内部版本的预期输出应如下所示：

   ```bash
   Restore completed in 100.37 ms for C:\Users\user1\Downloads\CosmosDB_Samples\bulk-import-demo\bulk-import-demo.csproj.
     bulk -> C:\Users\user1\Downloads\CosmosDB_Samples\bulk-import-demo \bin\Debug\netcoreapp2.2\bulk-import-demo.dll

   Build succeeded.
       0 Warning(s)
       0 Error(s)

   Time Elapsed 00:00:34.17
   ```

## <a name="step-3-add-the-azure-cosmos-db-package"></a>步骤 3：添加 Azure Cosmos DB 包

当仍在应用程序目录中时，使用 DotNet 添加包命令安装适用于 .NET 的 Azure Cosmos DB 客户端库。

   ```bash
   dotnet add package Microsoft.Azure.Cosmos
   ```

## <a name="step-4-get-your-azure-cosmos-account-credentials"></a>步骤 4：获取 Azure Cosmos 帐户凭据

此示例应用程序需对 Azure Cosmos 帐户进行身份验证。 为了进行身份验证，应将 Azure Cosmos 帐户凭据传递给应用程序。 按照以下步骤获取 Azure Cosmos 帐户凭据：

1. 登录到 [Azure 门户](https://portal.azure.cn/)。
1. 导航到 Azure Cosmos 帐户。
1. 打开“键”窗格，复制帐户的 URI 和主键    。

如果使用 Azure Cosmos DB 模拟器，请获取[本文中的模拟器凭据](local-emulator.md#authenticating-requests)。

## <a name="step-5-initialize-the-cosmosclient-object-with-bulk-execution-support"></a>步骤 5：初始化具有批量执行支持的 CosmosClient 对象

在代码编辑器中打开生成的 `Program.cs` 文件。 你将创建启用了批量执行的 CosmosClient 的新实例，并使用该实例对 Azure Cosmos DB 执行操作。 

首先，让我们覆盖默认 `Main` 方法，并定义全局变量。 这些全局变量将包含终结点和授权密钥、数据库的名称、你将创建的容器，以及将批量插入的项数。 确保根据环境替换终结点 URL 和授权密钥值。 

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Diagnostics;
   using System.IO;
   using System.Text.Json;
   using System.Threading.Tasks;
   using Microsoft.Azure.Cosmos;

   public class Program
   {
        private const string EndpointUrl = "https://<your-account>.documents.azure.cn:443/";
        private const string AuthorizationKey = "<your-account-key>";
        private const string DatabaseName = "bulk-tutorial";
        private const string ContainerName = "items";
        private const int ItemsToInsert = 300000;

        static async Task Main(string[] args)
        {

        }
   }
   ```

在 `Main` 方法中，添加以下代码以初始化 CosmosClient 对象：

```csharp

CosmosClient cosmosClient = new CosmosClient(EndpointUrl, AuthorizationKey, new CosmosClientOptions() { AllowBulkExecution = true });

```

启用批量执行后，CosmosClient 在内部将并发操作分组为单个服务调用。 通过这种方式，可以通过跨分区分发服务调用来优化吞吐量利用率，最后将各个结果分配给原始调用方。

然后可以创建一个容器来存储所有项。  将 `/pk` 定义为分区键，将 50000 RU/s 定义为预配的吞吐量，并定义将排除所有字段以优化写入吞吐量的自定义索引策略。 在 CosmosClient 初始化语句后面添加以下代码：

```csharp

Database database = await cosmosClient.CreateDatabaseIfNotExistsAsync(Program.DatabaseName);

await database.DefineContainer(Program.ContainerName, "/pk")
        .WithIndexingPolicy()
            .WithIndexingMode(IndexingMode.Consistent)
            .WithIncludedPaths()
                .Attach()
            .WithExcludedPaths()
                .Path("/*")
                .Attach()
        .Attach()
    .CreateAsync(50000);

```

## <a name="step-6-populate-a-list-of-concurrent-tasks"></a>步骤 6：填充并发任务的列表

若要利用批量执行支持，请基于数据源和要执行的操作创建异步任务的列表，并使用 `Task.WhenAll` 并发执行它们。
首先，让我们使用“假”数据从数据模型生成项目列表。 在实际应用程序中，项目会来自所需的数据源。

首先，使用 dotnet add package 命令将假包添加到解决方案中。

   ```bash
   dotnet add package Bogus
   ```

定义要保存的项的定义。 需要在 `Program.cs` 文件中定义 `Item` 类：

```csharp

public class Item
{
    public string id {get;set;}
    public string pk {get;set;}

    public string username{get;set;}
}

```

接下来，在 `Program` 类中创建一个 helper 函数。 此 helper 函数将获取你定义用于插入的项数并生成随机数据：

```csharp

private static IReadOnlyCollection<Item> GetItemsToInsert()
{
    return new Bogus.Faker<Item>()
    .StrictMode(true)
    //Generate item
    .RuleFor(o => o.id, f => Guid.NewGuid().ToString()) //id
    .RuleFor(o => o.username, f => f.Internet.UserName())
    .RuleFor(o => o.pk, (f, o) => o.id) //partitionkey
    .Generate(ItemsToInsert);
}

```

使用 `System.Text.Json` 类读取项并将其序列化为流实例。 由于自动生成的数据的性质，因此需要将数据序列化为流。 还可以直接使用项实例，但通过将其转换为流，可以利用 CosmosClient 中流 API 的性能。 通常，只要知道分区键，就可以直接使用数据。 

若要将数据转换为流实例，请在 `Main` 方法中，在创建容器后直接添加以下代码：

```csharp

Dictionary<PartitionKey, Stream> itemsToInsert = new Dictionary<PartitionKey, Stream>(ItemsToInsert);
foreach (Item item in Program.GetItemsToInsert())
{
    MemoryStream stream = new MemoryStream();
    await JsonSerializer.SerializeAsync(stream, item);
    itemsToInsert.Add(new PartitionKey(item.pk), stream);
}

```

接下来，使用数据流创建并发任务并填充任务列表以将项插入到容器中。 若要执行此操作，请将以下代码添加到 `Program` 类：

```csharp

Container container = database.GetContainer(ContainerName);
List<Task> tasks = new List<Task>(ItemsToInsert);
foreach (KeyValuePair<PartitionKey, Stream> item in itemsToInsert)
{
    tasks.Add(container.CreateItemStreamAsync(item.Value, item.Key)
        .ContinueWith((Task<ResponseMessage> task) =>
        {
            using (ResponseMessage response = task.Result)
            {
                if (!response.IsSuccessStatusCode)
                {
                    Console.WriteLine($"Received {response.StatusCode} ({response.ErrorMessage}) status code for operation {response.RequestMessage.RequestUri.ToString()}.");
                }
            }
        }));
}

// Wait until all are done
await Task.WhenAll(tasks);

```

所有这些并发点操作将一起执行（批量），如简介部分中所述。

## <a name="step-7-run-the-sample"></a>步骤 7：运行示例

若要运行示例，只需使用 `dotnet` 命令即可：

```bash
dotnet run
```

## <a name="get-the-complete-sample"></a>获取完整示例

如果没有时间完成本教程中的步骤，或者只需下载代码示例，则可从 [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-bulk-import-throughput-optimizer) 获取。

克隆项目后，请确保在 [Program.cs](https://github.com/Azure-Samples/cosmos-dotnet-bulk-import-throughput-optimizer/blob/master/src/Program.cs#L25) 中更新所需的凭据。

可以通过更改为存储库目录并使用 `dotnet` 来运行示例：

```bash
cd cosmos-dotnet-bulk-import-throughput-optimizer
dotnet run
```

## <a name="next-steps"></a>后续步骤

在本教程中，我们已完成以下步骤：

> [!div class="checklist"]
> * 创建 Azure Cosmos 帐户
> * 配置项目
> * 连接到启用了批量支持的 Azure Cosmos 帐户
> * 通过并发创建操作执行数据导入

你现在可以继续学习下一个教程：

> [!div class="nextstepaction"]
>[使用 SQL API 查询 Azure Cosmos DB](tutorial-query-sql-api.md)

<!-- Update_Description: new article about tutorial sql api dotnet bulk import -->
<!--NEW.date: 12/09/2019-->