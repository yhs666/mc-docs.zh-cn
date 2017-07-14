---
title: "将 mongoimport 和 mongorestore 与 Azure Cosmos DB 的 API for MongoDB 配合使用 | Azure"
description: "了解如何使用 mongoimport 和 mongorestore 将数据导入到 API for MongoDB 帐户"
keywords: "mongoimport，mongorestore"
services: cosmos-db
author: rockboyfor
manager: digimobile
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/12/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 8f7c2a270774bf3e5b6df8de4d5a5cf597816c02
ms.sourcegitcommit: b15d77b0f003bef2dfb9206da97d2fe0af60365a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# Azure Cosmos DB：如何导入 MongoDB 数据？
<a id="azure-cosmos-db-how-to-import-mongodb-data" class="xliff"></a> 

若要将数据从 MongoDB 迁移到 Azure Cosmos DB 帐户以与 MongoDB API 一起使用，必须：

* 从 [MongoDB 下载中心](https://www.mongodb.com/download-center)下载 *mongoimport.exe* 或 *mongorestore.exe*。
* 获取 [API for MongoDB 连接字符串](connect-mongodb-account.md)。

如果要从 MongoDB 导入数据，并且计划将其与 DocumentDB API 一起使用，应使用数据迁移工具导入数据。 有关详细信息，请参阅[数据迁移工具](import-data.md)。

本教程涵盖以下任务：

> [!div class="checklist"]
> * 检索连接字符串
> * 使用 mongoimport 导入 MongoDB 数据
> * 使用 mongorestore 导入 MongoDB 数据

## 先决条件
<a id="prerequisites" class="xliff"></a>

* 增加吞吐量：数据迁移的持续时间取决于为集合设置的吞吐量。 请确保对于较大的数据迁移增加吞吐量。 完成迁移后，减少吞吐量以节约成本。 有关在 [Azure 门户](https://portal.azure.cn)中增加吞吐量的详细信息，请参阅 [Azure Cosmos DB 中的性能级别和定价层](performance-levels.md)。

* 启用 SSL：Azure Cosmos DB 具有严格的安全要求和标准。 请确保在与帐户进行交互时启用 SSL。 本文其余部分的步骤包括如何为 *mongoimport* 和 *mongorestore* 启用 SSL。

## 查找连接字符串信息（主机、端口、用户名和密码）
<a id="find-your-connection-string-information-host-port-username-and-password" class="xliff"></a>

1. 在 [Azure 门户](https://portal.azure.cn)的左侧窗格中，单击“Azure Cosmos DB”条目。
2. 在“订阅”窗格中，选择帐户名称。
3. 在“连接字符串”边栏选项卡中，单击“连接字符串”。  
右侧窗格中包含成功连接到帐户所需的所有信息。

    ![“连接字符串”边栏选项卡](./media/mongodb-migrate/ConnectionStringBlade.png)

## 使用 mongoimport 将数据导入到 MongoDB API
<a id="import-data-to-mongodb-api-with-mongoimport" class="xliff"></a>

若要将数据导入 Azure Cosmos DB 帐户，请使用以下模板执行导入。 使用特定于帐户的值填写“主机”、“用户名”和“密码”。  

模板：

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

示例：  

    mongoimport.exe --host anhoh-host.documents.azure.cn:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## 使用 mongorestore 将数据导入到 MongoDB 的 API
<a id="import-data-to-api-for-mongodb-with-mongorestore" class="xliff"></a>

若要将数据还原到 API for MongoDB 帐户，请使用以下模板执行导入。 使用特定于帐户的值填写“主机”、“用户名”和“密码”。

模板：

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

示例：

    mongorestore.exe --host anhoh-host.documents.azure.cn:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07

## 成功迁移指南
<a id="guide-for-a-successful-migration" class="xliff"></a>

1. 预先创建和缩放集合

    * 默认情况下，Azure Cosmos DB 将预配包含 1,000 RU 的新 MongoDB 集合。 使用 mongoimport、mongorestore 或 mongomirror 迁移之前，请通过 [Azure 门户](https://portal.azure.cn)或 MongoDB 驱动程序、工具等预先创建所有集合。如果集合大于 10GB，请确保使用相应的分片键创建[分片/分区集合](partition-data.md)。

    * 在 [Azure 门户](https://portal.azure.cn)中，在单个分区集合 1,000 RU/一个分片集合 2,500 RU 的基础上增大集合的吞吐量，这只是为了迁移。 设置更高的吞吐量可以避免限制，并在更短的时间内完成迁移。 根据 Azure Cosmos DB 的每小时计费，可以在迁移后立即降低吞吐量以节省成本。

2. 计算单次文档写入的近似 RU  费用

    * 从 MongoDB Shell 连接到 Azure Cosmos DB MongoDB 数据库。 可在[此处](connect-mongodb-account.md)找到说明。

    * 在 MongoDB Shell 中使用示例文档之一运行示例插入命令

        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```

    * 插入后，请运行 ```db.runCommand({getLastRequestStatistics: 1})```，随后你将收到如下所示的响应
        ```
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
        ```

    * 记下请求费用

3. 确定从计算机到 Azure Cosmos DB 云服务的延迟。

    * 在 MongoDB Shell 中使用以下命令启用详细日志记录：```setVerboseShell(true)```

    * 针对数据库运行一个简单查询：```db.coll.find().limit(1)```，然后你将收到如下所示的响应
        ```
        Fetched 1 record(s) in 100(ms)
        ```

4. 迁移之前请务必删除所插入的文档，确保没有重复的文档。 可以使用 ```db.coll.remove({})``` 删除文档。

5. 计算 *batchSize* 和 *numInsertionWorkers* 的近似值

    * 对于 *batchSize*，请将预配 RU 总数除以步骤 3 中单次文档写入消耗的 RU 数。

    * 如果计算出的 *batchSize* <= 24，请使用该数字作为 *batchSize*

    * 如果计算出的 *batchSize* > 24，应将 *batchSize* 设置为 24。

    * 对于 *numInsertionWorkers*，请使用以下公式：*numInsertionWorkers = (配置吞吐量 * 以秒为单位的延迟) / (批大小 * 单次写入消耗的 RU 数)*

    |属性|值|
    |--------|-----|
    |batchSize| 24 |
    |预配的 RU 数 | 10000 |
    |延迟 | 0.100 秒 |
    |1 次文档写入收取的 RU 费 | 10 RU |
    |numInsertionWorkers | ? |

    *numInsertionWorkers = (10000 RU x 0.1 秒) / (24 x 10 RU) = 4.1666*

6. 最终的迁移命令：

```
mongoimport.exe --host anhoh-mongodb.documents.azure.cn:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
```

## 后续步骤
<a id="next-steps" class="xliff"></a>

在本教程中，已完成以下内容：

> [!div class="checklist"]
> * 已检索连接字符串
> * 已使用 mongoimport 导入 MongoDB 数据
> * 已使用 mongorestore 导入 MongoDB 数据

现在可以继续学习下一教程，了解如何使用 Azure Cosmos DB 查询 MongoDB 数据。 

> [!div class="nextstepaction"]
>[如何查询 MongoDB 数据？](../cosmos-db/tutorial-query-mongodb.md)