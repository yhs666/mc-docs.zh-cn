---
title: 适用于 Azure 的 MongoDB、Angular 和 Node 教程 - 第 5 部分 | Azure
description: 本教程系列介绍如何通过 Angular 和 Node 在 Azure Cosmos DB 上创建 MongoDB 应用（所使用的 API 与用于 MongoDB 的 API 完全相同）；这是本教程系列的第 5 部分
services: cosmos-db
author: rockboyfor
manager: digimobile
editor: ''
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
origin.date: 09/05/2017
ms.date: 12/03/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 525d9614958af9fe75a5913d188d3b6675405db0
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52675638"
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-5-use-mongoose-to-connect-to-azure-cosmos-db"></a>通过 Angular 和 Azure Cosmos DB 创建 MongoDB 应用 - 第 5 部分：使用 Mongoose 连接到 Azure Cosmos DB

本教程包含多个部分，演示了如何通过 Express、Angular 创建 Node.js 应用，然后将其连接到 [Azure Cosmos DB MongoDB API](mongodb-introduction.md) 帐户。

本教程的第 5 部分基于[第 4 部分](tutorial-develop-mongodb-nodejs-part4.md)，涵盖以下任务：

> [!div class="checklist"]
> * 使用 Mongoose 连接到 Azure Cosmos DB
> * 获取你的 Cosmos DB 连接字符串信息
> * 创建 hero 模型
> * 创建 hero 服务，以获取 hero 数据
> * 在本地运行应用

<!-- Not Available ## Video walkthrough-->

## <a name="prerequisites"></a>先决条件

开始教程的此部分之前，请确保已完成教程[第 4 部分](tutorial-develop-mongodb-nodejs-part4.md)的步骤。

> [!TIP]
> 本教程介绍生成应用程序的各个步骤。 若要下载完成的项目，可从 GitHub 上的 [angular-cosmosdb 存储库](https://github.com/Azure-Samples/angular-cosmosdb)获取完成的应用程序。

## <a name="use-mongoose-to-connect-to-azure-cosmos-db"></a>使用 Mongoose 连接到 Azure Cosmos DB

1. 安装 mongoose npm 模块，此模块为 API，用于与 MongoDB 通信。

    ```bash
    npm i mongoose --save
    ```

2. 现在，请在 server 文件夹中创建名为“mongo.js”的新文件。 需将 Cosmos DB 帐户的连接详细信息添加到此文件。

3. 将以下代码复制到 mongo.js 中。 此代码：

    * 需要 Mongoose。

    * 替代 Mongo 约定，使用内置到 ES6/ES2015 及更高版本中的基本约定。

    * 针对 env 文件进行调用，以便根据自己是处在过渡期、生产期还是开发期来设置某些项目。 将在下一部分创建该文件。

    * 包括 MongoDB 连接字符串（在 env 文件中设置）。

    * 创建调用 Mongoose 的 connect 函数。

    ```javascript
    const mongoose = require('mongoose');
    /**
     * Set to Node.js native promises
     * Per http://mongoosejs.com/docs/promises.html
     */
    mongoose.Promise = global.Promise;

    const env = require('./env/environment');

    // eslint-disable-next-line max-len
    const mongoUri = `mongodb://${env.accountName}:${env.key}@${env.accountName}.documents.azure.cn:${env.port}/${env.databaseName}?ssl=true`;

    function connect() {
     mongoose.set('debug', true);
     return mongoose.connect(mongoUri, { useMongoClient: true });
    }

    module.exports = {
      connect,
      mongoose
    };
    ```

4. 在“资源管理器”窗格的 server 下创建名为 environment 的文件夹，然后在 environment 文件夹中创建名为 environment.js 的新文件。

5. 从 mongo.js 文件可以知道，需包括 `dbName`、`key` 和 `cosmosPort`，因此请将以下代码复制到 environment.js 中。

    ```javascript
    // TODO: replace if yours are different
    module.exports = {
      accountName: 'your-cosmosdb-account-name-goes-here',
      databaseName: 'admin', 
      key: 'your-key-goes-here',
      port: 10255
    };
    ```

## <a name="get-the-connection-string-information"></a>获取连接字符串信息

1. 在 environment.js 中，将 `port` 的值更改为 10255。 （可以在 Azure 门户中查找 Cosmos DB 端口）

    ```javascript
    const port = 10255;
    ```

2. 在 environment.js 中，将 `accountName` 的值更改为在[步骤 4](tutorial-develop-mongodb-nodejs-part4.md) 中创建的 Azure Cosmos DB 帐户的名称。 

3. 在 Terminal 窗口中使用以下 CLI 命令，检索 Azure Cosmos DB 帐户的主密钥： 

    ```Azure CLI  az cosmosdb list-keys --name <cosmosdb-name> -g myResourceGroup
    ```    

    * `<cosmosdb-name>` is the name of the Azure Cosmos DB account you created in [Step 4](tutorial-develop-mongodb-nodejs-part4.md).

4. Copy the primary key into the environment.js file as the `key` value.

    Your app now has all the information it needs to connect to Azure Cosmos DB. This information can also be retrieved in the portal. For more information, see [Get the MongoDB connection string to customize](connect-mongodb-account.md#GetCustomConnection). The Username in the portal equates to the dbName in environments.js. 

## Create a Hero model

1. In the Explorer pane, create the file **hero.model.js** under the **server** folder.

2. Copy the following code into **hero.model.js**. This code provides the following functionality:

   * Requires Mongoose.
   * Creates a new schema with an ID, name, and saying.
   * Creates a model using the schema.
   * Exports the model. 
   * Name the collection Heroes (instead of Heros, which would be the default name of the collection based on Mongoose plural naming rules).

   ```javascript
   const mongoose = require('mongoose');

   const Schema = mongoose.Schema;

   const heroSchema = new Schema(
     {
       id: { type: Number, required: true, unique: true },
       name: String,
       saying: String
     },
     {
       collection: 'Heroes'
     }
   );

   const Hero = mongoose.model('Hero', heroSchema);

   module.exports = Hero;
   ```

## <a name="create-a-hero-service"></a>创建 Hero 服务

1. 在“资源管理器”窗格的 server 文件夹下，创建 hero.service.js 文件。

2. 将以下代码复制到 hero.service.js 中。 此代码：

   * 获取刚创建的模型
   * 连接到数据库
   * 创建 docquery 变量，以便使用 hero.find 方法定义一个返回所有 hero 的查询。
   * 通过 docquery.exec 运行查询，使用约定来获取其中的响应状态为 200 的所有 hero 的列表。 
   * 如果状态为 500，则发回错误消息
   * 由于使用的是模块，因此可以获取 hero。 

   ```javascript
   const Hero = require('./hero.model');

   require('./mongo').connect();

   function getHeroes() {
     const docquery = Hero.find({});
     docquery
       .exec()
       .then(heroes => {
         res.status(200).json(heroes);
       })
       .catch(error => {
         res.status(500).send(error);
         return;
       });
   }

   module.exports = {
     getHeroes
   };
   ```

## <a name="add-the-hero-service-to-routesjs"></a>将 hero 服务添加到 routes.js

1. 在 Visual Studio Code 中，注释掉 routes.js 中用于发送示例 hero 数据的 `res.send` 函数，然后添加一行代码，改为调用 `heroService.getHeroes` 函数。

    ```javascript
    router.get('/heroes', (req, res) => {
      heroService.getHeroes(req, res);
    //  res.send(200, [
    //      {"id": 10, "name": "Starlord", "saying": "oh yeah"}
    //  ])
    });
    ```

2. 在 routes.js 中，需要 hero 服务：

    ```javascript
    const heroService = require('./hero.service'); 
    ```

3. 在 hero.service.js 中更新使用 `req` 和 `res` 参数的 getHeroes 函数，如下所示：

    ```javascript
    function getHeroes(req, res) {
    ```

    请花点时间查看并演练此处的调用链。 首先看看 `index.js`，此文件用于设置节点服务器。注意，此文件会设置和定义路由。 然后，routes.js 文件会与 hero 服务通信，要求该服务获取 getHeroes 之类的函数，并传递请求和响应。 在这里，hero.service.js 会获取模型并连接到 Mongo，然后在用户调用时执行 getHeroes，并返回响应 200。 然后通过链退出。 

## <a name="run-the-app"></a>运行应用程序

1. 现在，请再次运行该应用。 在 Visual Studio Code 中，保存所有更改，单击左侧的“调试”按钮 ![Visual Studio Code 中的“调试”图标](./media/tutorial-develop-mongodb-nodejs-part5/debug-button.png)，然后单击“开始调试”按钮 ![Visual Studio Code 中的“开始调试”图标](./media/tutorial-develop-mongodb-nodejs-part5/start-debugging-button.png)。

3. 现在请切换到浏览器，打开开发人员工具和“网络”选项卡，然后导航到 http://localhost:3000，其中有我们的应用程序。

    ![Azure 门户中的新 Azure Cosmos DB 帐户](./media/tutorial-develop-mongodb-nodejs-part5/azure-cosmos-db-heroes-app.png)

   尚无 hero 存储在应用中，但我们会在本教程的下一步添加放置、推送和删除功能，以便通过与 Azure Cosmos DB 数据库的 Mongoose 连接从 UI 中添加、更新和删除 hero。 

## <a name="next-steps"></a>后续步骤

在本教程的此部分，你已完成以下任务：

> [!div class="checklist"]
> * 使用 Mongoose API 将 heroes 应用连接到 Azure Cosmos DB 
> * 向应用添加获取 hero 功能

可以转到本教程的下一部分，了解如何向应用添加 Post、Put 和 Delete 函数。

> [!div class="nextstepaction"]
> [向应用添加 Post、Put 和 Delete 函数](tutorial-develop-mongodb-nodejs-part6.md)

<!-- Update_Description: update meta properties, wording update -->