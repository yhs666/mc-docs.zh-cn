---
title: "Azure Cosmos DB：使用 Java 和 MongoDB API 生成控制台应用 | Azure"
description: "演示一个可以用来连接到 Azure Cosmos DB MongoDB API 并进行查询的 Java 代码示例"
services: cosmos-db
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
origin.date: 05/10/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: 6defeda770a8ee4e98ba82a9bc13bca83cf0a4cc
ms.sourcegitcommit: b15d77b0f003bef2dfb9206da97d2fe0af60365a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# Azure Cosmos DB：使用 Java 和 Azure 门户生成 MongoDB API 控制台应用
<a id="azure-cosmos-db-build-a-mongodb-api-console-app-with-java-and-the-azure-portal" class="xliff"></a>

Azure Cosmos DB 由 Microsoft 提供，是全球分布的多模型数据库服务。 可快速创建和查询文档、键/值和图形数据库，所有这些都受益于 Azure Cosmos DB 核心的全球分布和水平缩放功能。 

本快速入门教程演示如何使用 Azure 门户创建 Azure Cosmos DB 帐户、文档数据库和集合。 然后生成并部署基于 [MongoDB Java 驱动程序](https://docs.mongodb.com/ecosystem/drivers/java/)构建的控制台应用。 

## 先决条件
<a id="prerequisites" class="xliff"></a>

* 在运行此示例之前，必须具备以下先决条件：
   * JDK 1.7+（如果没有 JDK，请运行 `apt-get install default-jdk`）
   * Maven（如果没有 Maven，请运行 `apt-get install maven`）

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## 创建数据库帐户
<a id="create-a-database-account" class="xliff"></a>

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## 添加集合
<a id="add-a-collection" class="xliff"></a>

将新数据库命名为 **db**，将新集合命名为 **coll**。

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## 克隆示例应用程序
<a id="clone-the-sample-application" class="xliff"></a>

现在让我们从 github 克隆 MongoDB API 应用、设置连接字符串，并运行。 你将看到以编程方式处理数据是多么容易。 

1. 打开 git 终端窗口（例如 git bash）并使用 `cd` 切换到工作目录。  

2. 运行下列命令，克隆示例存储库。 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. 然后在 Visual Studio 中打开解决方案文件。 

## 查看代码
<a id="review-the-code" class="xliff"></a>

快速查看应用中发生的情况。 打开 `Program.cs` 文件，会发现以下代码行创建 Azure Cosmos DB 资源。 

* 将对 DocumentClient 进行初始化。

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* 将创建新数据库和集合。

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* 将使用 `MongoCollection.insertOne` 插入一些文档

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* 将使用 `MongoCollection.find` 执行一些查询

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## 更新连接字符串
<a id="update-your-connection-string" class="xliff"></a>

现在返回到 Azure 门户，获取连接字符串信息，并将其复制到应用。

1. 从帐户中选择“快速启动”，选择 Java，然后将连接字符串复制到剪贴板

2. 打开 `Program.java` 文件，将 MongoClientURI 构造函数的参数替换为该连接字符串。 现已使用与 Azure Cosmos DB 进行通信所需的所有信息更新应用。 

## 运行控制台应用
<a id="run-the-console-app" class="xliff"></a>

1. 在终端中运行 `mvn package`，安装所需的 npm 模块

2. 在终端中运行 `mvn exec:java -D exec.mainClass=GetStarted.Program`，启动 Java 应用程序。

现在可以使用 [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) 来查询、修改和处理此新数据。

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

在本快速入门教程中，已了解如何创建 Azure Cosmos DB 帐户、使用数据资源管理器创建集合和运行控制台应用。 现在可以将其他数据导入 Cosmos DB 帐户。 

> [!div class="nextstepaction"]
> [将 MongoDB 数据导入 Azure Cosmos DB](mongodb-migrate.md)