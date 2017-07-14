---
title: "NoSQL 教程：Azure DocumentDB Java SDK | Azure"
description: "使用 DocumentDB Java SDK 创建联机数据库和 Java 控制台应用程序的 NoSQL 教程。 Azure DocumentDB 是用于 JSON 的 NoSQL 数据库。"
keywords: "nosql 教程, 联机数据库, java 控制台应用程序"
services: cosmos-db
documentationcenter: Java
author: rockboyfor
manager: digimobile
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
origin.date: 05/22/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: eb1ed2dd74b26bcfc20291cec788e89f0e65668b
ms.sourcegitcommit: b15d77b0f003bef2dfb9206da97d2fe0af60365a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# NoSQL 教程：构建 DocumentDB Java 控制台应用程序
<a id="nosql-tutorial-build-a-documentdb-java-console-application" class="xliff"></a>
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [用于 MongoDB 的 Node.js](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

欢迎使用 Azure DocumentDB Java SDK 的 NoSQL 教程！ 学习本教程后，你将拥有一个可创建并查询 DocumentDB 资源的控制台应用程序。

我们介绍：

* 创建并连接到 Azure Cosmos DB 帐户
* 配置 Visual Studio 解决方案
* 创建联机数据库
* 创建集合
* 创建 JSON 文档
* 查询集合
* 创建 JSON 文档
* 查询集合
* 替换文档
* 删除文档
* 删除数据库

现在，让我们开始吧！

## 先决条件
<a id="prerequisites" class="xliff"></a>
确保具有以下内容：

* 有效的 Azure 帐户。 如果没有，可以注册[免费帐户](https://www.azure.cn/pricing/1rmb-trial/)。 另外，对于本教程，也可以使用 [Azure Cosmos DB 模拟器](local-emulator.md)。
* [Git](https://git-scm.com/downloads)
* [Java 开发工具包 (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)。
* [Maven](http://maven.apache.org/download.cgi)。

## 步骤 1：创建 Azure Cosmos DB 帐户
<a id="step-1-create-an-azure-cosmos-db-account" class="xliff"></a>
创建 Azure Cosmos DB 帐户。 如果已有一个可用的帐户，可以直接跳到[克隆 GitHub 项目](#GitClone)。 如果使用 Azure Cosmos DB 模拟器，请遵循 [Azure Cosmos DB 模拟器](local-emulator.md)中的步骤设置该模拟器，然后直接跳到[克隆 GitHub 项目](#GitClone)。

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>步骤 2：克隆 GitHub 项目
首先，可以根据 [Get Started with Azure Cosmos DB and Java](https://github.com/Azure-Samples/documentdb-java-getting-started)（Azure Cosmos DB 和 Java 入门）中所述克隆 GitHub 存储库。 例如，从本地目录运行以下命令，在本地检索示例项目。

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

该目录包含项目的 `pom.xml`，以及含有 Java 源代码的 `src` 文件夹，其中包括 `Program.java`，说明如何使用 Azure DocumentDB 执行简单的操作，例如创建文档以及查询集合中的数据。 `pom.xml` 包括一个依赖项，依赖于 [Maven 上的 DocumentDB Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb)。

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>步骤 3：连接到 Azure Cosmos DB 帐户
接下来，回到 [Azure 门户](https://portal.azure.cn) ，检索终结点和主要主密钥。 Azure Cosmos DB 终结点和主密钥是必需的，可让应用程序知道要连接的对象，使 Azure Cosmos DB 信任应用程序的连接。

在 Azure 门户中，导航到 Azure Cosmos DB 帐户，然后单击“密钥”。 从门户复制 URI，并将其粘贴到 Program.java 文件的 `<your endpoint URI>` 中。 然后从门户复制主密钥，并将其粘贴到 `<your key>`中。

    this.client = new DocumentClient(
        "<your endpoint URI>",
        "<your key>"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![NoSQL 教程创建 Java 控制台应用程序时使用的 Azure 门户的屏幕截图。 显示了一个 Azure Cosmos DB 帐户，在“Azure Cosmos DB 帐户”边栏选项卡上突出显示了“ACTIVE”中心、“密钥”按钮，在“密钥”边栏选项卡上突出显示了 URI、主密钥、辅助密钥的值][keys]

## 第 4 步：创建数据库
<a id="step-4-create-a-database" class="xliff"></a>
可以使用 **DocumentClient** 类的 [createDatabase](https://docs.microsoft.com/zh-cn/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) 方法创建 Azure Cosmos DB [数据库](documentdb-resources.md#databases)。 数据库是跨集合分区的 JSON 文档存储的逻辑容器。

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>步骤 5：创建集合
> [!WARNING]
> **createCollection** 将创建一个具有保留吞吐量的新集合，它牵涉定价。 有关详细信息，请访问[定价页](https://www.azure.cn/pricing/details/cosmos-db/)。
> 
> 

可以使用 **DocumentClient** 类的 [createCollection](https://docs.microsoft.com/zh-cn/java/api/com.microsoft.azure.documentdb._document_client.createcollection) 方法创建[集合](documentdb-resources.md#collections)。 集合是 JSON 文档和相关联的 JavaScript 应用程序逻辑的容器。

    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>步骤 6：创建 JSON 文档
可以使用 **DocumentClient** 类的 [createDocument](https://docs.microsoft.com/zh-cn/java/api/com.microsoft.azure.documentdb._document_client.createdocument) 方法创建[文档](documentdb-resources.md#documents)。 文档是用户定义的（任意）JSON 内容。 现在，我们可以插入一个或多个文档。 如果已有要在数据库中存储的数据，则可使用 DocumentDB 的 [数据迁移工具](import-data.md)将数据导入数据库。

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![说明 NoSQL 教程创建 Java 控制台应用程序所用帐户、联机数据库、集合和文档的层次关系的图表](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>步骤 7：查询 Azure Cosmos DB 资源
Azure Cosmos DB 支持对存储在每个集合中的 JSON 文档进行[各种查询](documentdb-sql-query.md)。  以下示例代码演示了如何将 SQL 语法与 [queryDocuments](https://docs.microsoft.com/zh-cn/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) 方法一起使用来查询 Azure Cosmos DB 中的文档。

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>步骤 8：替换 JSON 文档
Azure Cosmos DB 支持使用 [replaceDocument](https://docs.microsoft.com/zh-cn/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) 方法更新 JSON 文档。

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>步骤 9：删除 JSON 文档
Azure Cosmos DB 支持使用 [deleteDocument](https://docs.microsoft.com/zh-cn/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) 方法更新 JSON 文档。  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>步骤 10：删除数据库
删除已创建的数据库将删除该数据库及其所有子资源（集合、文档等）。

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>步骤 11：运行整个 Java 控制台应用程序！
若要从控制台运行应用程序，请导航到项目文件夹，然后使用 Maven 进行编译：

    mvn package

运行 `mvn package` 将从 Maven 下载最新的 Azure Cosmos DB 库，并生成 `GetStarted-0.0.1-SNAPSHOT.jar`。 然后，通过运行以下命令来运行该应用：

    mvn exec:java -D exec.mainClass=GetStarted.Program

祝贺你！ 你已经完成本 NoSQL 教程，并且获得了一个可正常使用的 Java 控制台应用程序！

## 后续步骤
<a id="next-steps" class="xliff"></a>
* 需要 Java Web 应用教程？ 请参阅[通过 Java 构建使用 Azure Cosmos DB 的 Web 应用程序](documentdb-java-application.md)。
* 了解如何[监视 Azure Cosmos DB 帐户](monitor-accounts.md)。
* 在 [Query Playground](https://www.documentdb.com/sql/demo)中对示例数据集运行查询。
* 在 [Azure Cosmos DB 文档页](/documentdb/)的“开发”部分了解有关编程模型的详细信息。

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png