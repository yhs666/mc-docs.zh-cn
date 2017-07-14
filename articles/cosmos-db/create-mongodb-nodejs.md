---
title: "使用 Node.js 将 MongoDB 应用连接到 Azure Cosmos DB | Azure"
description: "了解如何将现有的 Node.js MongoDB 应用连接到 Azure Cosmos DB"
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
ms.devlang: nodejs
ms.topic: hero-article
origin.date: 06/19/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: 9666285d60abd5b61984e47b27f972ed2dde7800
ms.sourcegitcommit: b15d77b0f003bef2dfb9206da97d2fe0af60365a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# Azure Cosmos DB：迁移现有的 Node.js MongoDB Web 应用
<a id="azure-cosmos-db-migrate-an-existing-nodejs-mongodb-web-app" class="xliff"></a> 

Azure Cosmos DB 由 Microsoft 提供，是全球分布的多模型数据库服务。 可快速创建和查询文档、键/值和图形数据库，所有这些都受益于 Azure Cosmos DB 核心的全球分布和水平缩放功能。 

本快速入门演示如何使用以 Node.js 编写的现有 [MongoDB](mongodb-introduction.md) 应用，并将其连接到支持 MongoDB 客户端连接的 Azure Cosmos DB 数据库。 换而言之，Node.js 应用程序仅知道它要使用 MongoDB API 连接到某个数据库。 应用程序完全知道数据存储在 Azure Cosmos DB 中。

完成本教程后，[Azure Cosmos DB](https://www.azure.cn/home/features/cosmos-db/) 中将会运行一个 MEAN（MongoDB、Express、AngularJS 和 Node.js）应用程序。 

![在 Azure 应用服务中运行的 MEAN.js 应用](./media/create-mongodb-nodejs/meanjs-in-azure.png)

## 先决条件
<a id="prerequisites" class="xliff"></a> 

本快速入门教程需要 Azure CLI 2.0。 可以在浏览器中使用 Azure Cloud Shell，或者在自己的计算机上[安装 Azure CLI 2.0](https://docs.microsoft.com/zh-cn/cli/azure/install-azure-cli) 以运行本教程中的代码块。

<!-- Not Available [!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)] -->

除 Azure CLI 之外，还需要在本地安装 [Node.js](https://nodejs.org/) 和 [Git](http://www.git-scm.com/downloads)，以运行 `npm` 和 `git` 命令。

你应该具备 Node.js 的实践知识。 本快速入门并未介绍有关开发 Node.js 应用程序的一般信息。

## 克隆示例应用程序
<a id="clone-the-sample-application" class="xliff"></a>

打开 git 终端窗口（例如 git bash）并使用 `cd` 切换到工作目录。  

运行以下命令克隆示例存储库。 此示例存储库包含默认的 [MEAN.js](http://meanjs.org/) 应用程序。 

```bash
git clone https://github.com/prashanthmadi/mean
```

## 运行应用程序
<a id="run-the-application" class="xliff"></a>

安装所需的包并启动应用程序。

```bash
cd mean
npm install
npm start
```

## 登录 Azure
<a id="log-in-to-azure" class="xliff"></a>

如果使用已安装的 Azure CLI，请使用 [az login](https://docs.microsoft.com/zh-cn/cli/azure/#login) 命令登录到 Azure 订阅，按屏幕说明操作。

```azurecli
az configure            # Azure CLI 2.0
// Update the cloud name with AzureChinaCloud in specific configureation file with default name of C:\Users\{USER NAME}\.azure\config
// [cloud]
// name = AzureChinaCloud
az login
``` 

## 添加 Azure Cosmos DB 模块
<a id="add-the-azure-cosmos-db-module" class="xliff"></a>

如果使用已安装的 Azure CLI，请运行 `az` 命令，检查是否已安装 `cosmosdb` 组件。 如果 `cosmosdb` 在基本命令列表中，请继续执行下一个命令。

如果 `cosmosdb` 不在基本命令列表中，请重装 [Azure CLI 2.0](https://docs.microsoft.com/zh-cn/cli/azure/install-azure-cli)。

## 创建资源组
<a id="create-a-resource-group" class="xliff"></a>

使用 [az group create](https://docs.microsoft.com/zh-cn/cli/azure/group#create) 创建[资源组](../azure-resource-manager/resource-group-overview.md)。 Azure 资源组是在其中部署和管理 Azure 资源（例如 Web 应用、数据库和存储帐户）的逻辑容器。 

以下示例在中国东部区域中创建一个资源组。 选择资源组的唯一名称。

如果使用 Azure Cloud Shell，请单击“试用”，按照屏幕提示登录，然后将命令复制到命令提示符中。
```azurecli-interactive
az group create --name myResourceGroup --location "China East"
```

## 创建 Azure Cosmos DB 帐户
<a id="create-an-azure-cosmos-db-account" class="xliff"></a>

使用 [az cosmosdb create](https://docs.microsoft.com/zh-cn/cli/azure/cosmosdb#create) 命令创建 Azure Cosmos DB 帐户。

在以下命令中，请将 `<cosmosdb_name>` 占位符替换为自己的唯一 Azure Cosmos DB 帐户名。 此唯一名称将用作 Azure Cosmos DB 终结点 (`https://<cosmosdb_name>.documents.azure.cn/`) 的一部分，因此需要在 Azure 中的所有 Azure Cosmos DB 帐户之间保持唯一。 

```azurecli-interactive
az cosmosdb create --name <cosmosdb_name> --resource-group myResourceGroup --kind MongoDB
```

`--kind MongoDB` 参数启用 MongoDB 客户端连接。

创建 Azure Cosmos DB 帐户后，Azure CLI 将显示类似于以下示例的信息。 

```json
{
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb_name>.documents.azure.cn:443/",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Document
DB/databaseAccounts/<cosmosdb_name>",
  "kind": "MongoDB",
  "location": "China East",
  "name": "<cosmosdb_name>",
  "readLocations": [
    {
      "documentEndpoint": "https://<cosmosdb_name>-chinaeast.documents.azure.cn:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb_name>-chinaeast",
      "locationName": "China East",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://<cosmosdb_name>-chinaeast.documents.azure.cn:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb_name>-chinaeast",
      "locationName": "China East",
      "provisioningState": "Succeeded"
    }
  ]
} 
```

## 将 Node.js 应用程序连接至数据库
<a id="connect-your-nodejs-application-to-the-database" class="xliff"></a>

此步骤使用 MongoDB 连接字符串将 MEAN.js 示例应用程序连接到刚刚创建的 Azure Cosmos DB 数据库。 

## 检索密钥
<a id="retrieve-the-key" class="xliff"></a>

若要连接到 Azure Cosmos DB 数据库，需要使用数据库密钥。 使用 [az cosmosdb list-keys](https://docs.microsoft.com/zh-cn/cli/azure/cosmosdb#list-keys) 命令检索主键。

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

Azure CLI 输出类似于以下示例的信息。 

```json
{
  "primaryMasterKey": "RUayjYjixJDWG5xTqIiXjC...",
  "primaryReadonlyMasterKey": "...",
  "secondaryMasterKey": "...",
  "secondaryReadonlyMasterKey": "..."
}
```

将 `primaryMasterKey` 的值复制到文本编辑器。 下一步骤需要用到此信息。

<a name="devconfig"></a>
## 在 Node.js 应用程序中配置连接字符串
<a id="configure-the-connection-string-in-your-nodejs-application" class="xliff"></a>

在 MEAN.js 存储库中打开 `config/env/local-development.js`。

将此文件的内容替换为以下代码。 确保将两个 `<cosmosdb_name>` 占位符替换为 Azure Cosmos DB 帐户名，将 `<primary_master_key>` 占位符替换为在上一步骤中复制的密钥。

```javascript
'use strict';

module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.cn:10250/mean-dev?ssl=true&sslverifycertificate=false'
  }
};
```

> [!NOTE] 
> 由于 [Azure Cosmos DB 需要 SSL](connect-mongodb-account.md#connection-string-requirements)，因此 `ssl=true` 选项非常重要。 
>
>

保存所做更改。

### 再次运行应用程序。
<a id="run-the-application-again" class="xliff"></a>

再次运行 `npm start`。 

```bash
npm start
```

此时应会显示一条控制台消息，告知开发环境已启动且正在运行。 

在浏览器中导航到 `http://localhost:3000`。 在顶部菜单中单击“注册”，然后尝试创建两个虚构的用户。 

MEAN.js 示例应用程序将用户数据存储在数据库中。 如果上述操作成功并且 MEAN.js 可自动登录到已创建的用户，则表示 Azure Cosmos DB 连接可正常工作。 

![MEAN.js 成功连接到 MongoDB](./media/create-mongodb-nodejs/mongodb-connect-success.png)

## 在数据资源管理器中查看数据
<a id="view-data-in-data-explorer" class="xliff"></a>

可在 Azure 门户中查看、查询 Azure Cosmos DB 存储的数据并对其运行业务逻辑。

若要查看、查询和处理在上一步骤中创建的用户数据，请在 Web 浏览器中登录到 [Azure 门户](https://portal.azure.cn)。

在顶部搜索框中，键入“Azure Cosmos DB”。 打开 Cosmos DB 帐户边栏选项卡后，请选择 Cosmos DB 帐户。 在左侧导航栏中，单击“数据资源管理器”。 在“集合”窗格中展开集合，然后即可查看该集合中的文档，查询数据，甚至可以创建和运行存储过程、触发器与 UDF。 

![Azure 门户中的数据资源管理器](./media/create-mongodb-nodejs/cosmosdb-connect-mongodb-data-explorer.png)

## 将 Node.js 应用程序部署到 Azure
<a id="deploy-the-nodejs-application-to-azure" class="xliff"></a>

此步骤将已连接 MongoDB 的 Node.js 应用程序部署到 Azure Cosmos DB。

你可能已注意到，前面更改的配置文件适用于开发环境 (`/config/env/local-development.js`)。 将应用程序部署到应用服务后，该应用程序默认在生产环境中运行。 因此，现在需要对相应的配置文件进行相同的更改。

在 MEAN.js 存储库中打开 `config/env/production.js`。

在 `db` 对象中，如以下示例所示替换 `uri` 的值。 请务必按上文所述替换占位符。

```javascript
'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.cn:10250/mean?ssl=true&sslverifycertificate=false',
```

在终端中，将所有更改提交到 Git。 可以复制这两个命令，以便同时运行。

```bash
git add .
git commit -m "configured MongoDB connection string"
```
## 清理资源
<a id="clean-up-resources" class="xliff"></a>

如果不打算继续使用此应用，请删除本快速入门教程在 Azure 门户中创建的所有资源，步骤如下：

1. 在 Azure 门户的左侧菜单中，单击“资源组”，然后单击已创建资源的名称。 
2. 在资源组页上单击“删除”，在文本框中键入要删除的资源的名称，然后单击“删除”。

## 后续步骤
<a id="next-steps" class="xliff"></a>

本快速入门教程已介绍如何创建 Azure Cosmos DB 帐户以及使用数据资源管理器创建 MongoDB 集合。 现在，可将 MongoDB 数据迁移到 Azure Cosmos DB。  

> [!div class="nextstepaction"]
> [将 MongoDB 数据导入 Azure Cosmos DB](mongodb-migrate.md)