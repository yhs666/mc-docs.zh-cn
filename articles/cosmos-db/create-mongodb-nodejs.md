---
title: 使用 Node.js 将 MongoDB 应用连接到 Azure Cosmos DB | Azure
description: 了解如何将现有的 Node.js MongoDB 应用连接到 Azure Cosmos DB
services: cosmos-db
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.custom: quick start connect, mvc, devcenter
ms.devlang: nodejs
ms.topic: quickstart
origin.date: 06/19/2017
ms.date: 11/05/2018
ms.author: v-yeche
ms.openlocfilehash: ca895ee0be0a7d17953d76b1e64ff5db88877838
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52655255"
---
# <a name="azure-cosmos-db-migrate-an-existing-nodejs-mongodb-web-app"></a>Azure Cosmos DB：迁移现有的 Node.js MongoDB Web 应用 

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB 是 21Vianet 提供的多区域分布式多模型数据库服务。 可快速创建和查询文档数据库，这些数据库受益于 Azure Cosmos DB 核心的多区域分布和水平缩放功能。 

<!-- Not Available on Key/Value and Graph -->
<!-- Notice: globally TO multiple-region -->

本快速入门演示如何使用以 Node.js 编写的现有 MongoDB 应用，并使用 [MongoDB API](mongodb-introduction.md) 将其连接到支持 MongoDB 客户端连接的 Azure Cosmos DB 数据库。 换而言之，Node.js 应用程序仅知道它要使用 MongoDB API 连接到某个数据库。 应用程序完全知道数据存储在 Azure Cosmos DB 中。

完成本教程后，[Azure Cosmos DB](https://www.azure.cn/home/features/cosmos-db/) 中会运行一个 MEAN（MongoDB、Express、Angular 和 Node.js）应用程序。 

![在 Azure 应用服务中运行的 MEAN.js 应用](./media/create-mongodb-nodejs/meanjs-in-azure.png)

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。 

## <a name="prerequisites"></a>先决条件 
如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

除 Azure CLI 之外，还需要在本地安装 [Node.js](https://nodejs.org/) 和 [Git](http://www.git-scm.com/downloads)，以运行 `npm` 和 `git` 命令。

应具备 Node.js 的实践知识。 本快速入门并未介绍有关开发 Node.js 应用程序的一般信息。

## <a name="clone-the-sample-application"></a>克隆示例应用程序

运行以下命令克隆示例存储库。 此示例存储库包含默认的 [MEAN.js](http://meanjs.org/) 应用程序。

1. 打开命令提示符，新建一个名为“git-samples”的文件夹，然后关闭命令提示符。

    ```bash
    md "C:\git-samples"
    ```

2. 打开诸如 git bash 之类的 git 终端窗口，并使用 `cd` 命令更改为要安装示例应用的新文件夹。

    ```bash
    cd "C:\git-samples"
    ```

3. 运行下列命令以克隆示例存储库。 此命令在计算机上创建示例应用程序的副本。 

    ```bash
    git clone https://github.com/prashanthmadi/mean
    ```

## <a name="run-the-application"></a>运行应用程序

安装所需的包并启动应用程序。

```bash
cd mean
npm install
npm start
```
应用程序将尝试连接到 MongoDB 源，并在输出返回“[MongoError: connect ECONNREFUSED 127.0.0.1:27017]”时失败，继续下一步退出应用程序。

## <a name="log-in-to-azure"></a>登录 Azure

如果使用已安装的 Azure CLI，请使用 [az login](https://docs.azure.cn/zh-cn/cli/reference-index?view=azure-cli-latest#az-login) 命令登录到 Azure 订阅，按屏幕说明操作。 如果使用 Azure Cloud Shell，可以跳过此步骤。

```azurecli
az cloud set -n AzureChinaCloud
az login
#az cloud set -n AzureCloud  //means return to global azure 
``` 

## <a name="add-the-azure-cosmos-db-module"></a>添加 Azure Cosmos DB 模块

如果使用已安装的 Azure CLI，请运行 `az` 命令，检查是否已安装 `cosmosdb` 组件。 如果 `cosmosdb` 在基本命令列表中，请继续执行下一个命令。

<!-- Not Available on Azure Cloud Shell-->

如果 `cosmosdb` 不在基本命令列表中，请重装 [Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-create) 创建[资源组](../azure-resource-manager/resource-group-overview.md)。 Azure 资源组是在其中部署和管理 Azure 资源（例如 Web 应用、数据库和存储帐户）的逻辑容器。 

以下示例在中国北部区域中创建一个资源组。 选择资源组的唯一名称。

<!-- Not Avaialbe on Azure Cloud Shell -->

```azurecli
az group create --name myResourceGroup --location "China North"
```

## <a name="create-an-azure-cosmos-db-account"></a>创建 Azure Cosmos DB 帐户

使用 [az cosmosdb create](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-create) 命令创建 Azure Cosmos DB 帐户。

在以下命令中，请将 `<cosmosdb-name>` 占位符替换成自己的唯一 Azure Cosmos DB 帐户名。 此唯一名称将用作 Azure Cosmos DB 终结点 (`https://<cosmosdb-name>.documents.azure.cn/`) 的一部分，因此需要在 Azure 中的所有 Azure Cosmos DB 帐户之间保持唯一。 

```azurecli
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

`--kind MongoDB` 参数启用 MongoDB 客户端连接。

创建 Azure Cosmos DB 帐户后，Azure CLI 会显示类似于以下示例的信息： 

> [!NOTE]
> 此示例使用 JSON 作为 Azure CLI 输出格式，此为默认设置。 若要使用其他输出格式，请参阅 [Azure CLI 命令的输出格式](https://docs.azure.cn/zh-cn/cli/format-output-azure-cli?view=azure-cli-latest)。

```json
{
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb-name>.documents.azure.cn:443/",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Document
DB/databaseAccounts/<cosmosdb-name>",
  "kind": "MongoDB",
  "location": "China North",
  "name": "<cosmosdb-name>",
  "readLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-chinanorth.documents.azure.cn:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-chinanorth",
      "locationName": "China North",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-chinanorth.documents.azure.cn:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-chinanorth",
      "locationName": "China North",
      "provisioningState": "Succeeded"
    }
  ]
} 
```

## <a name="connect-your-nodejs-application-to-the-database"></a>将 Node.js 应用程序连接至数据库

此步骤使用 MongoDB 连接字符串将 MEAN.js 示例应用程序连接到刚刚创建的 Azure Cosmos DB 数据库。 

<a name="devconfig"></a>
## <a name="configure-the-connection-string-in-your-nodejs-application"></a>在 Node.js 应用程序中配置连接字符串

在 MEAN.js 存储库中打开 `config/env/local-development.js`。

请将此文件的内容替换为以下代码。 另外，请务必将两个 `<cosmosdb-name>` 占位符替换为 Azure Cosmos DB 帐户名。

```javascript
'use strict';

module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.cn:10255/mean-dev?ssl=true&sslverifycertificate=false'
  }
};
```

## <a name="retrieve-the-key"></a>检索密钥

若要连接到 Azure Cosmos DB 数据库，需要使用数据库密钥。 使用 [az cosmosdb list-keys](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest#list-keys) 命令检索主键。

```azurecli
az cosmosdb list-keys --name <cosmosdb-name> --resource-group myResourceGroup --query "primaryMasterKey"
```

Azure CLI 输出类似于以下示例的信息。 

```json
"RUayjYjixJDWG5xTqIiXjC..."
```

复制 `primaryMasterKey` 的值。 粘贴此值并覆盖 `local-development.js` 中的 `<primary_master_key>`。

保存所做更改。

### <a name="run-the-application-again"></a>再次运行应用程序。

再次运行 `npm start`。 

```bash
npm start
```

此时应会显示一条控制台消息，告知开发环境已启动且正在运行。 

在浏览器中导航到 `http://localhost:3000`。 在顶部菜单中单击“注册”，并尝试创建两个虚构的用户。 

MEAN.js 示例应用程序将用户数据存储在数据库中。 如果上述操作成功并且 MEAN.js 可自动登录到已创建的用户，则表示 Azure Cosmos DB 连接可正常工作。 

![MEAN.js 成功连接到 MongoDB](./media/create-mongodb-nodejs/mongodb-connect-success.png)

## <a name="view-data-in-data-explorer"></a>在数据资源管理器中查看数据

可在 Azure 门户中查看、查询 Azure Cosmos DB 存储的数据并对其运行业务逻辑。

若要查看、查询和处理在上一步骤中创建的用户数据，请在 Web 浏览器中登录到 [Azure 门户](https://portal.azure.cn)。

在顶部搜索框中，键入“Azure Cosmos DB”。 打开 Cosmos DB 帐户边栏选项卡后，请选择 Cosmos DB 帐户。 在左侧导航栏中，单击“数据资源管理器”。 在“集合”窗格中展开集合，即可查看该集合中的文档，查询数据，甚至可以创建和运行存储过程、触发器与 UDF。 

![Azure 门户中的数据资源管理器](./media/create-mongodb-nodejs/cosmosdb-connect-mongodb-data-explorer.png)

## <a name="deploy-the-nodejs-application-to-azure"></a>将 Node.js 应用程序部署到 Azure

此步骤将已连接 MongoDB 的 Node.js 应用程序部署到 Azure Cosmos DB。

可能已注意到，前面更改的配置文件适用于开发环境 (`/config/env/local-development.js`)。 将应用程序部署到应用服务后，该应用程序默认在生产环境中运行。 因此，现在需要对相应的配置文件做出相同的更改。

在 MEAN.js 存储库中打开 `config/env/production.js`。

在 `db` 对象中，如以下示例所示替换 `uri` 的值。 请务必按上文所述替换占位符。

```javascript
'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.cn:10255/mean?ssl=true&sslverifycertificate=false',
```

> [!NOTE] 
> 由于 [Azure Cosmos DB 需要 SSL](connect-mongodb-account.md#connection-string-requirements)，因此 `ssl=true` 选项非常重要。 
>
>

在终端中，将所有更改提交到 Git。 可以复制这两个命令，以便同时运行。

```bash
git add .
git commit -m "configured MongoDB connection string"
```
## <a name="clean-up-resources"></a>清理资源

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>后续步骤

本快速入门教程已介绍如何创建 Azure Cosmos DB 帐户以及使用数据资源管理器创建 MongoDB 集合。 现在，可将 MongoDB 数据迁移到 Azure Cosmos DB。  

> [!div class="nextstepaction"]
> [将 MongoDB 数据导入 Azure Cosmos DB](mongodb-migrate.md)

<!--Update_Description: update meta properties, wording update, update link -->