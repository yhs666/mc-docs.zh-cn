---
title: "使用图形 API 生成 Azure Cosmos DB .NET 应用程序 | Azure"
description: "演示一个可以用来连接和查询 Azure Cosmos DB 的 .NET 代码示例"
services: cosmos-db
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
origin.date: 05/21/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: 27a1298b48b3bddd05063b2e5aff6a9d58168cf4
ms.sourcegitcommit: b15d77b0f003bef2dfb9206da97d2fe0af60365a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# Azure Cosmos DB：使用图形 API 生成 .NET 应用程序
<a id="azure-cosmos-db-build-a-net-application-using-the-graph-api" class="xliff"></a>

Azure Cosmos DB 由 Microsoft 提供，是全球分布的多模型数据库服务。 可快速创建和查询文档、键/值和图形数据库，所有这些都受益于 Azure Cosmos DB 核心的全球分布和水平缩放功能。 

本快速入门教程演示如何使用 Azure 门户创建 Azure Cosmos DB 帐户、数据库和图形（容器）。 然后将生成并运行基于[图形 API](graph-sdk-dotnet.md)（预览版）构建的控制台应用。  

## 先决条件
<a id="prerequisites" class="xliff"></a>

如果尚未安装 Visual Studio 2017，可以下载并使用**免费的** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)。 在安装 Visual Studio 的过程中，请确保启用“Azure 开发”。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## 创建数据库帐户
<a id="create-a-database-account" class="xliff"></a>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## 添加图形
<a id="add-a-graph" class="xliff"></a>

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## 克隆示例应用程序
<a id="clone-the-sample-application" class="xliff"></a>

现在让我们从 github 克隆图形 API 应用、设置连接字符串，并运行。 你将看到以编程方式处理数据是多么容易。 

1. 打开 git 终端窗口（例如 git bash）并使用 `cd` 切换到工作目录。  

2. 运行下列命令，克隆示例存储库。 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. 然后在 Visual Studio 中打开解决方案文件。 

## 查看代码
<a id="review-the-code" class="xliff"></a>

快速查看应用中发生的情况。 打开 Program.cs 文件，会发现以下代码行创建 Azure Cosmos DB 资源。 

* 将对 DocumentClient 进行初始化。 在预览版中，我们已将图形扩展 API 添加到 Azure Cosmos DB 客户端。 我们正在努力开发从 Azure Cosmos DB 客户端和资源中解耦的独立图形客户端。

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* 将创建一个新数据库。

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* 将创建一个新图形。

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* 将使用 `CreateGremlinQuery` 方法执行一系列 Gremlin 步骤。

    ```csharp
    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, "g.V().count()");
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    ```

## 更新连接字符串
<a id="update-your-connection-string" class="xliff"></a>

现在返回到 Azure 门户，获取连接字符串信息，并将其复制到应用。

1. 在 [Azure 门户](http://portal.azure.cn/)的 Azure Cosmos DB 帐户的左侧导航栏中，单击“密钥”，然后单击“读写密钥”。 使用屏幕右侧的复制按钮将 URI 和主密钥复制到下一步的 `App.config` 文件中。

    ![在 Azure 门户的“密钥”边栏选项卡中查看并复制访问密钥](./media/create-graph-dotnet/keys.png)

2. 在 Visual Studio 2017 中，打开 `App.config` 文件。 

3. 从门户中复制 URI 值（使用复制按钮），并在 `App.config` 中将其设为终结点密钥的值。 

    `<add key="Endpoint" value="FILLME.documents.azure.cn:443" />`

4. 然后从门户复制“主密钥”的值，并在 `App.config` 中将其设为 authKey 的值。 

    `<add key="AuthKey" value="FILLME" />`

现已使用与 Azure Cosmos DB 进行通信所需的所有信息更新应用。 

## 运行控制台应用
<a id="run-the-console-app" class="xliff"></a>

1. 在 Visual Studio 中，右键单击**解决方案资源管理器**中的 **GraphGetStarted** 项目，然后单击“管理 NuGet 包”。 

2. 在 NuGet“浏览”框中，键入 *Microsoft.Azure.Graphs*，并选中“包括预发行版”框。 

3. 从结果中安装“Microsoft.Azure.Graphs”库。 这将安装 Azure Cosmos DB 图形扩展库包和所有依赖项。

4. 单击 Ctrl+F5 运行应用程序。

   控制台窗口将显示所添加到图形的顶点及边缘。 完成脚本后，按两次 ENTER 以关闭控制台窗口。 

## 使用数据资源管理器浏览
<a id="browse-using-the-data-explorer" class="xliff"></a>

现在可以返回到 Azure 门户中的数据资源管理器，浏览和查询新的图形数据。

* 在数据资源管理器中，新数据库将显示在“集合”窗格中。 依次展开 **graphdb**、**graphcoll**，然后单击“图形”。

    示例应用生成的数据将显示在“图形”窗格中。

## 在 Azure 门户中查看 SLA
<a id="review-slas-in-the-azure-portal" class="xliff"></a>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## 清理资源
<a id="clean-up-resources" class="xliff"></a>

如果不打算继续使用此应用，请删除本快速入门教程在 Azure 门户中创建的所有资源，步骤如下： 

1. 在 Azure 门户的左侧菜单中，单击“资源组”，然后单击已创建资源的名称。 
2. 在资源组页上单击“删除”，在文本框中键入要删除的资源的名称，然后单击“删除”。

## 后续步骤
<a id="next-steps" class="xliff"></a>

在本快速入门教程中，已了解如何创建 Azure Cosmos DB 帐户、使用数据资源管理器创建图形和运行应用。 现可使用 Gremlin 构建更复杂的查询，实现功能强大的图形遍历逻辑。 

> [!div class="nextstepaction"]
> [使用 Gremlin 查询](tutorial-query-graph.md)