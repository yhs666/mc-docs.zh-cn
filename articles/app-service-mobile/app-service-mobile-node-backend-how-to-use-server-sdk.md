---
title: "如何使用用于移动应用的 Node.js 后端服务器 SDK"
description: "了解如何使用适用于 Azure 应用服务移动应用的 Node.js 后端服务器 SDK。"
services: app-service\mobile
documentationcenter: 
author: elamalani
manager: elamalani
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
origin.date: 10/01/2016
ms.author: v-yiso
ms.date: 03/12/2018
ms.openlocfilehash: 18c968369e5443d710e7bcc3ef80bdb3f7171e3d
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="how-to-use-the-mobile-apps-nodejs-sdk"></a>如何使用移动应用 Node.js SDK
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

本文提供详细的信息和示例，说明如何在 Azure 应用服务的移动应用功能中使用 Node.js 后端。

## <a name="Introduction"></a>介绍
使用移动应用可将移动优化的数据访问 Web API 添加到 Web 应用程序。 提供的移动应用 SDK 适用于 ASP.NET 和 Node.js Web 应用程序。 此 SDK 提供以下操作：

* 数据访问的表操作（读取、插入、更新、删除）
* 自定义 API 操作

这两种操作都可用于 Azure 应用服务所允许的所有标识提供者（包括 Facebook、Twitter、Google 和 Microsoft 等社交标识提供者，以及用于企业标识的 Azure Active Directory）之间的身份验证。

可以 [在 GitHub 上的示例目录]中找到每种用例的示例。

## <a name="supported-platforms"></a>支持的平台
移动应用 Node.js SDK 支持 Node 的当前 LTS 版本及更高版本。 目前，最新 LTS 版本为 Node v4.5.0。 其他 Node 版本可能有效，但不受支持。

Azure 移动应用 Node.js SDK 支持两个数据库驱动程序： 

* node-mssqll 驱动程序支持 Azure SQL 数据库和本地 SQL Server 实例。  
* sqlite3 驱动程序仅支持单个实例上的 SQLite 数据库。

### <a name="howto-cmdline-basicapp"></a>使用命令行创建基本 Node.js 后端
每个移动应用 Node.js 后端都以 ExpressJS 应用程序的形式启动。 在适用于 Node.js 的 Web 服务框架中，ExpressJS 最广为使用。 可按以下方式创建基本的 [Express] 应用程序：

1. 在命令窗口或 PowerShell 窗口中，为项目创建目录。

    ```
    mkdir basicapp
    ```

2. 运行 npm init 初始化包结构。

    ```
    cd basicapp
    npm init
    ```

    npm init 命令将提出一系列问题以初始化项目。  查看示例输出：

    ![npm init 输出][0]

3. 从 npm 存储库安装 express 和 azure-mobile-apps 库。

    ```
    npm install --save express azure-mobile-apps
    ```

4. 创建 app.js 文件，实现基本移动服务器。

    ```
    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define a TodoItem table
    mobile.tables.add('TodoItem');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);
    ```

此应用程序创建具有单个终结点 (`/tables/TodoItem`) 的移动优化 Web API，让用户使用动态架构访问基础 SQL 数据存储，而无需经过身份验证。 它适用于以下客户端库快速入门：

- [Android 客户端快速入门]
- [Apache Cordova 客户端快速入门]
- [iOS 客户端快速入门]
- [Windows 应用商店客户端快速入门]
- [Xamarin.iOS 客户端快速入门]
- [Xamarin.Android 客户端快速入门]
- [Xamarin.Forms 客户端快速入门]

可以在 [GitHub 上的 basicapp 示例]中找到此基本应用程序的代码。

### <a name="howto-vs2015-basicapp"></a>使用 Visual Studio 2015 创建 Node.js 后端
Visual Studio 2015 需要使用一个扩展在 IDE 中开发 Node.js 应用程序。 首先，请安装 [Node.js Tools 1.1 for Visual Studio]。 完成安装后，创建 Express 4.x 应用程序：

1. 打开“新建项目”对话框（从“文件” > “新建” > “项目”）。
2. 展开“模板” > “JavaScript” > “Node.js”。
3. 选择“基本 Azure Node.js Express 4 应用程序”。
4. 填写项目名称。 选择“确定” 。

   ![Visual Studio 2015 中的“新建项目”][1]
5. 右键单击“npm”节点，选择“安装新的 npm 包”。
6. 创建第一个 Node.js 应用程序时，可能需要刷新 npm 目录。 根据需要选择“刷新”。
7. 在搜索框中输入 **azure-mobile-apps** 。 选择 **azure-mobile-apps 2.0.0** 包，然后选择“安装包”。

   ![安装新的 npm 包][2]
8. 选择“关闭” 。
9. 打开 app.js 文件，添加对移动应用 SDK 的支持。 在库 `require` 语句底部的第 6 行，添加以下代码：

    ```
    var bodyParser = require('body-parser');
    var azureMobileApps = require('azure-mobile-apps');
    ```

    在其他 app.use 语句之后大约第 27 行，添加以下代码：

    ```
    app.use('/users', users);

        // Mobile Apps initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Save the file.

10. Either run the application locally (the API is served on http://localhost:3000) or publish to Azure.

### <a name="create-node-backend-portal"></a>Create a Node.js back end by using the Azure portal
You can create a Mobile Apps back end right in the [Azure portal]. You can either complete the following steps or
create a client and server together by following the [Create a mobile app](app-service-mobile-ios-get-started.md)
tutorial. The tutorial contains a simplified version of these instructions and is best for proof-of-concept projects.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Back in the **Get started** pane, under **Create a table API**, choose **Node.js** as your back-end language.
Select the box for **I acknowledge that this will overwrite all site contents**, and then select
**Create TodoItem table**.

### <a name="download-quickstart"></a>Download the Node.js back-end quickstart code project by using Git
When you create a Node.js Mobile Apps back end by using the portal's **Quick start** pane, a Node.js project
is created for you and deployed to your site. In the portal, you can add tables and APIs, and edit code files for the Node.js
back end. You can also use various deployment tools to download the back-end project so that you
can add or modify tables and APIs, and then republish the project. For more information, see the
[Azure App Service deployment guide]. 

The following procedure uses a Git repository to download the quickstart
project code:

1. Install Git, if you haven't already done so. The steps required to install Git vary between operating systems. For operating system-specific distributions and installation guidance, see [Installing Git](http://git-scm.com/book/en/Getting-Started-Installing-Git).
2. Follow the steps in [Enable the App Service app repository](../app-service/app-service-deploy-local-git.md#Step3) to enable the Git repository for your back-end site. Make a note of the deployment username and password.
3. In the pane for your Mobile Apps back end, make a note of the **Git clone URL** setting.
4. Execute the `git clone` command by using the Git clone URL. Enter your password when required, as in the
   following example:

    ```
    $ git clone https://username@todolist.scm.azurewebsites.cn:443/todolist.git
    ```

5. Browse to the local directory (`/todolist` in the preceding example), and notice that project files have been
   downloaded. Locate the todoitem.json file in the `/tables` directory. This file defines permissions on the
   table. Also find the todoitem.js file in the same directory. It defines the CRUD operation scripts for
   the table.
6. After you make changes to project files, run the following commands to add, commit, and then upload the
   changes to the site:

    ```
    $ git commit -m "updated the table script"  $ git push origin master
    ```

   When you add new files to the project, you first need to run the `git add .` command.

The site is republished every time a new set of commits is pushed to the site.

### <a name="howto-publish-to-azure"></a>Publish your Node.js back end to Azure
Azure provides many mechanisms for publishing your Mobile Apps Node.js back end to
the Azure service. These mechanisms include deployment tools integrated into Visual Studio, command-line tools,
and continuous deployment options based on source control. For more information, see the
[Azure App Service deployment guide].

Azure App Service has specific advice for Node.js applications that you should review before you publish the back end:

* How to [specify the Node version]
* How to [use Node modules]

### <a name="howto-enable-homepage"></a>Enable a home page for your application
Many applications are a combination of web and mobile apps. You can use the ExpressJS framework to combine the
two facets. Sometimes, however, you might want to only implement a mobile interface. It's useful to provide a
home page to ensure that the app service is up and running. You can either provide your own home page or enable
a temporary home page. To enable a temporary home page, use the following code to instantiate Mobile Apps:

```
var mobile = azureMobileApps({ homePage: true });
```

If you only want this option available when developing locally, you can add this setting to your `azureMobile.js` 
file.

## <a name="TableOperations"></a>Table operations
The azure-mobile-apps Node.js Server SDK provides mechanisms to expose data tables stored in Azure SQL Database
as a Web API. It provides five operations:

| Operation | Description |
| --- | --- |
| GET /tables/*tablename* |Get all records in the table |
| GET /tables/*tablename*/:id |Get a specific record in the table |
| POST /tables/*tablename* |Create a record in the table |
| PATCH /tables/*tablename*/:id |Update a record in the table |
| DELETE /tables/*tablename*/:id |Delete a record in the table |

This WebAPI supports [OData] and extends the table schema to support [offline data sync].

### <a name="howto-dynamicschema"></a>Define tables by using a dynamic schema
Before you can use a table, you must define it. You can define tables by using a static schema (where you define
the columns in the schema) or dynamically (where the SDK controls the schema based on incoming
requests). In addition, you can control specific aspects of the Web API by adding JavaScript code
to the definition.

As a best practice, you should define each table in a JavaScript file in the `tables` directory, and then use the
`tables.import()` method to import the tables. Extending the basic-app sample, you would adjust the app.js file:

```
var express = require('express'), azureMobileApps = require('azure-mobile-apps');

var app = express(), mobile = azureMobileApps();

// 定义公开的数据库架构 mobile.tables.import('./tables');

// 提供静态定义的任何表的初始化 mobile.tables.initialize().then(function () { // 添加移动 API，使其可作为 Web API 访问 app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);
});
```

Define the table in ./tables/TodoItem.js:

```
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// 此处为表的其他配置

module.exports = table;
```

Tables use a dynamic schema by default. To turn off the dynamic schema globally, set the `MS_DynamicSchema` app setting to false in the Azure portal.

You can find a complete example in the [todo sample on GitHub].

### <a name="howto-staticschema"></a>Define tables by using a static schema
You can explicitly define the columns to expose via the Web API. The azure-mobile-apps Node.js SDK automatically
adds any extra columns required for offline data sync to the list that you provide. For example, the
quickstart client applications require a table with two columns: `text` (a string) and `complete` (a Boolean).  
The table can be defined in the table definition JavaScript file (located in the `tables` directory) as follows:

```
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// 定义表中的列 table.columns = { "text": "string", "complete": "boolean" };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

module.exports = table;
```

If you define tables statically, you must also call the `tables.initialize()` method to create the database
schema on startup. The `tables.initialize()` method returns a [promise] so that the web service does not serve
requests before the database is initialized.

### <a name="howto-sqlexpress-setup"></a>Use SQL Server Express as a development data store on your local machine
The Mobile Apps Node.js SDK provides three options for serving data out of the box:

* Use the **memory** driver to provide a non-persistent example store.
* Use the **mssql** driver to provide a SQL Server Express data store for development.
* Use the **mssql** driver to provide an Azure SQL Database data store for production.

The Mobile Apps Node.js SDK uses the [mssql Node.js package] to establish and use a connection to both SQL Server
Express and SQL Database. This package requires that you enable TCP connections on your SQL Server Express instance.

> [!TIP]
> The memory driver does not provide a complete set of facilities for testing. If you want to test
> your back end locally, we recommend the use of a SQL Server Express data store and the mssql driver.
>
>

1. Download and install [Microsoft SQL Server 2014 Express]. Ensure that you install the SQL Server 2014 Express
   with Tools edition. Unless you explicitly require 64-bit support, the 32-bit version consumes less memory
   when running.

2. Run SQL Server 2014 Configuration Manager:

   a. Expand the **SQL Server Network Configuration** node in the tree menu.

   b. Select **Protocols for SQLEXPRESS**.

   c. Right-click **TCP/IP** and select **Enable**. Select **OK** in the pop-up dialog box.

   d. Right-click **TCP/IP** and select **Properties**.

   e. Select the **IP Addresses** tab.

   f. Find the **IPAll** node. In the **TCP Port** field, enter **1433**.

      ![Configure SQL Server Express for TCP/IP][3]

   g. Select **OK**. Select **OK** in the pop-up dialog box.

   h. Select **SQL Server Services** in the tree menu.

   i. Right-click **SQL Server (SQLEXPRESS)** and select **Restart**.

   j. Close SQL Server 2014 Configuration Manager.
3. Run SQL Server 2014 Management Studio and connect to your local SQL Server Express instance:

   1. Right-click your instance in Object Explorer and select **Properties**.
   2. Select the **Security** page.
   3. Ensure that **SQL Server and Windows Authentication mode** is selected.
   4. Select **OK**.

      ![Configure SQL Server Express authentication][4]
   5. Expand **Security** > **Logins** in Object Explorer.
   6. Right-click **Logins** and select **New Login**.
   7. Enter a login name. Select **SQL Server authentication**. Enter a password, and then enter the same password
      in **Confirm password**. The password must meet Windows complexity requirements.
   8. Select **OK**.

      ![Add a new user to SQL Server Express][5]
   9. Right-click your new login and select **Properties**.
   10. Select the **Server Roles** page.
   11. Select the check box for the **dbcreator** server role.
   12. Select **OK**.
   13. Close SQL Server 2015 Management Studio.

Be sure to record the username and password that you selected. You  might need to assign additional server roles or
permissions, depending on your database requirements.

The Node.js application reads the `SQLCONNSTR_MS_TableConnectionString` environment variable for
the connection string for this database. You can set this variable in your environment. For example,
you can use PowerShell to set this environment variable:

```
$env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"
```

Access the database through a TCP/IP connection. Provide a username and password for the connection.

### <a name="howto-config-localdev"></a>Configure your project for local development
Mobile Apps reads a JavaScript file called *azureMobile.js* from the local file system. Do not use this
file to configure the Mobile Apps SDK in production. Instead, use **App settings** in the [Azure portal]. 

The azureMobile.js file should export a configuration object. The most common settings are:

* Database settings
* Diagnostic logging settings
* Alternate CORS settings

This example azureMobile.js file implements the preceding database settings:

```
module.exports = { cors: { origins: [ 'localhost' ] }, data: { provider: 'mssql', server: '127.0.0.1', database: 'mytestdatabase', user: 'azuremobile', password: 'T3stPa55word' }, logging: { level: 'verbose' } };
```

We recommend that you add azureMobile.js to your .gitignore file (or other source code control ignore file)
to prevent passwords from being stored in the cloud. Always configure production settings in **App settings** within
the [Azure portal].

### <a name="howto-appsettings"></a>Configure app settings for your mobile app
Most settings in the azureMobile.js file have an equivalent app setting in the [Azure portal]. Use the following
list to configure your app in **App settings**:

| App setting | azureMobile.js setting | Description | Valid values |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |name |Name of the app |string |
| **MS_MobileLoggingLevel** |logging.level |Minimum log level of messages to log |error, warning, info, verbose, debug, silly |
| **MS_DebugMode** |debug |Enables or disables debug mode |true, false |
| **MS_TableSchema** |data.schema |Default schema name for SQL tables |string (default: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Enables or disables debug mode |true, false |
| **MS_DisableVersionHeader** |version (set to undefined) |Disables the X-ZUMO-Server-Version header |true, false |
| **MS_SkipVersionCheck** |skipversioncheck |Disables the client API version check |true, false |

To set an app setting:

1. Sign in to the [Azure portal].
2. Select **All resources** or **App Services**, and then select the name of your mobile app.
3. The **Settings** pane opens by default. If it doesn't, select **Settings**.
4. On the **GENERAL** menu, select **Application settings**.
5. Scroll to the **App settings** section.
6. If your app setting already exists, select the value of the app setting to edit the value.
   If your app setting does not exist, enter the app setting in the **Key** box and the value in the **Value** box.
8. Select **Save**.

Changing most app settings requires a service restart.

### <a name="howto-use-sqlazure"></a>Use SQL Database as your production data store
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Using Azure SQL Database as a data store is identical across all Azure App Service application types. If you have
not done so already, follow these steps to create a Mobile Apps back end:

1. Sign in to the [Azure portal].
2. In the upper left of the window, select the **+NEW** button > **Web + Mobile** > **Mobile App**, and then provide a
   name for your Mobile Apps back end.
3. In the **Resource Group** box, enter the same name as your app.
4. The default App Service plan is selected. If you want to change your App Service plan:

   a. Select **App Service Plan** > **+Create New**. 
   
   b. Provide a name of the new App Service plan and select an
   appropriate location. 
   
   c. Select an appropriate pricing tier for the service. Select
   **View all** to view more pricing options, such as **Free** and **Shared**. 
   
   d. Click the **Select** button. 
   
   e. Back in the **App Service plan** pane, select **OK**.
5. Select **Create**. 

Provisioning a Mobile Apps back end can take a couple of minutes. After the Mobile Apps
back end is provisioned, the portal opens the **Settings** pane for the Mobile Apps back end.

You can choose to either connect an existing SQL database to your
Mobile Apps back end or create a new SQL database. In this section, we create a SQL database.

> [!NOTE]
> If you already have a database in the same location as the Mobile Apps back end, you can
> instead select **Use an existing database** and then select that database. We don't recommend the use of a database in a different location because of higher latencies.
>
>

1. In the new Mobile Apps back end, select **Settings** > **Mobile App** > **Data** > **+Add**.
2. In the **Add data connection** pane, select **SQL Database - Configure required settings** > **Create a new database**. Enter the name of the new database in the **Name** box.
3. Select **Server**. In the **New server** pane, enter a unique server name in the **Server name** box,
   and provide a suitable server admin login and password. Ensure that **Allow azure services to access server**
   is selected. Select **OK**.

   ![Create an Azure SQL database][6]
4. In the **New database** pane, select **OK**.
5. Back in the **Add data connection** pane, select **Connection string**, and enter the login and password that
   you provided when you created the database. If you use an existing database, provide the login credentials
   for that database. Select **OK**.
6. Back in the **Add data connection** pane again, select **OK** to create the database.

<!--- END OF ALTERNATE INCLUDE -->

Creation of the database can take a few minutes. Use the **Notifications** area to monitor the progress of
the deployment. Do not progress until the database is deployed successfully. After the database is deployed,
a connection string is created for the SQL Database instance in your Mobile Apps back-end app settings. You can
see this app setting in **Settings** > **Application settings** > **Connection strings**.

### <a name="howto-tables-auth"></a>Require authentication for access to tables
If you want to use App Service Authentication with the `tables` endpoint, you must configure App Service
Authentication in the [Azure portal] first. For more information, see the configuration guide for the identity provider that you intend to use:
- [How to configure Azure Active Directory Authentication]
- [How to configure Microsoft Authentication]

Each table has an access property that you can use to control access to the table. The following sample shows
a statically defined table with authentication required.

```
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// 定义表中的列 table.columns = { "text": "string", "complete": "boolean" };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

// 要求进行身份验证以访问表 table.access = 'authenticated';

module.exports = table;
```

The access property can take one of three values

  - *anonymous* indicates that the client application is allowed to read data without authentication
  - *authenticated* indicates that the client application must send a valid authentication token with the request
  - *disabled* indicates that this table is currently disabled

If the access property is undefined, unauthenticated access is allowed.

### <a name="howto-tables-getidentity"></a>Use authentication claims with your tables
You can set up various claims that are requested when authentication is set up. These claims are not normally
available through the `context.user` object. However, you can retrieve them by using the `context.user.getIdentity()`
method.  The `getIdentity()` method returns a Promise that resolves to an object.  The object is keyed by the 
authentication method (microsoftaccount, or aad).

For example, if you set up Microsoft Account authentication and request the email addresses claim, you can add 
the email address to the record with the following table controller:

```
var azureMobileApps = require('azure-mobile-apps');

// 创建新的表定义 var table = azureMobileApps.table();

table.columns = { "emailAddress": "string", "text": "string", "complete": "boolean" }; table.dynamicSchema = false; table.access = 'authenticated';

/**
* 将上下文查询限制为包含经过身份验证的用户电子邮件地址的这些记录
* @param {Context} context the operation context
* @returns {Promise} context execution Promise */
function queryContextForEmail(context) {   return context.user.getIdentity().then((data) => {       context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });       return context.execute();   }); }

/**
* 将声明中的电子邮件地址添加到上下文项 - 用于
* 插入操作
* @param {Context} context the operation context
* @returns {Promise} context execution Promise */
function addEmailToContext(context) {   return context.user.getIdentity().then((data) => {       context.item.emailAddress = data.microsoftaccount.claims.emailaddress;       return context.execute();   }); }

    // 配置客户端执行请求时的特定代码。
    // 读取：只返回属于已经过身份验证用户的记录。
    table.read(queryContextForEmail);

    // 创建：基于经过身份验证的用户添加或重写 userId。
    table.insert(addEmailToContext);

    // 更新：只允许更新属于已经过身份验证用户的记录。
    table.update(queryContextForEmail);

    // 删除：只允许删除属于已经过身份验证用户的记录。
table.delete(queryContextForEmail);

module.exports = table;
```

To see what claims are available, use a web browser to view the `/.auth/me` endpoint of your site.

### <a name="howto-tables-disabled"></a>Disable access to specific table operations
In addition to appearing on the table, the access property can be used to control individual operations. There
are four operations:

* `read` is the RESTful GET operation on the table.
* `insert` is the RESTful POST operation on the table.
* `update` is the RESTful PATCH operation on the table.
* `delete` is the RESTful DELETE operation on the table.

For example, you might want to provide a read-only unauthenticated table:

```
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// 只读表 - 只允许读取操作 table.read.access = 'anonymous'; table.insert.access = 'disabled'; table.update.access = 'disabled'; table.delete.access = 'disabled';

module.exports = table;
```

### <a name="howto-tables-query"></a>Adjust the query that is used with table operations
A common requirement for table operations is to provide a restricted view of the data. For example, you can
provide a table that is tagged with the authenticated user ID such that you can only read or update your
own records. The following table definition provides this functionality:

```
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// 定义表的静态架构 table.columns = { "userId": "string", "text": "string", "complete": "boolean" }; table.dynamicSchema = false;

// 此表需要进行身份验证 table.access = 'authenticated';

// 确保仅检索已经过身份验证用户的记录 table.read(function (context) { context.query.where({ userId: context.user.id }); return context.execute(); });

// 添加记录时，添加或重写已经过身份验证用户的 userId table.insert(function (context) { context.item.userId = context.user.id; return context.execute(); });

module.exports = table;
```

Operations that normally run a query have a query property that you can adjust by using a `where` clause. The query
property is a [QueryJS] object that is used to convert an OData query to something that the data back end can
process. For simple equality cases (like the preceding one), you can use a map. You can also add specific SQL
clauses:

```
context.query.where('myfield eq ?', 'value');
```

### <a name="howto-tables-softdelete"></a>Configure a soft delete on a table
A soft delete does not actually delete records. Instead it marks them as deleted within the database by setting
the deleted column to true. The Mobile Apps SDK automatically removes soft-deleted records from results
unless the Mobile Client SDK uses `IncludeDeleted()`. To configure a table for a soft delete, set the `softDelete`
property in the table definition file:

```
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// 定义表中的列 table.columns = { "text": "string", "complete": "boolean" };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

// 启用软删除 table.softDelete = true;

// 要求进行身份验证以访问表 table.access = 'authenticated';

module.exports = table;
```

You should establish a mechanism for deleting records: a client application, a WebJob, an Azure
function, or a custom API.

### <a name="howto-tables-seeding"></a>Seed your database with data
When you're creating a new application, you might want to seed a table with data. You can do this within the table
definition JavaScript file as follows:

```
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// 定义表中的列 table.columns = { "text": "string", "complete": "boolean" }; table.seed = [ { text: 'Example 1', complete: false }, { text: 'Example 2', complete: true } ];

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

// 要求进行身份验证以访问表 table.access = 'authenticated';

module.exports = table;
```

Seeding of data happens only when you've used the Mobile Apps SDK to create the table. If the table already
exists in the database, no data is injected into the table. If the dynamic schema is turned on, the
schema is inferred from the seeded data.

We recommend that you explicitly call the `tables.initialize()` method to create the table when the service starts 
running.

### <a name="Swagger"></a>Enable Swagger support
Mobile Apps comes with built-in [Swagger] support. To enable Swagger support, first install
swagger-ui as a dependency:

```
npm install --save swagger-ui
```

You can then enable Swagger support in the Mobile Apps constructor:

```
var mobile = azureMobileApps({ swagger: true });
```

You probably only want to enable Swagger support in development editions. You can do this by using the
`NODE_ENV` app setting:

```
var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });
```

The swagger endpoint is located at http://_yoursite_.azurewebsites.cn/swagger.  You can access the Swagger 
UI via the `/swagger/ui` endpoint. If you choose to require authentication across your entire application,
Swagger produces an error. For best results, choose to allow unauthenticated requests in the Azure
App Service Authentication/Authorization settings, and then control authentication by using the `table.access`
property.

You can also add the Swagger option to your azureMobile.js file if you only want Swagger support for
developing locally.

## <a name="push">Push notifications
Mobile Apps integrates with Azure Notification Hubs so you can send targeted push notifications to millions
of devices across all major platforms. By using Notification Hubs, you can send push notifications to iOS, Android,
and Windows devices. To learn more about all that you can do with Notification Hubs, see the
[Notification Hubs overview](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Send push notifications
The following code shows how to use the `push` object to send a broadcast push notification to registered iOS
devices:

```
// 创建 APNS 有效负载。
var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured.
    if (context.push) {
        // Send a push notification by using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

通过从客户端创建模板推送注册，可以改为向所有受支持平台上的设备发送模板推送消息。 以下代码演示如何发送模板通知：

```
// Define the template payload.
var payload = '{"messageParam": "This is a template payload."}';

// Only do the push if configured
if (context.push) {
    // Send a template notification.
    context.push.send(null, payload, function (error) {
        if (error) {
            // Do something or log the error.
        }
    });
}
```

### <a name="push-user"></a>使用标记将推送通知发送到经过身份验证的用户
当经过身份验证的用户注册推送通知时，用户 ID 标记自动添加到注册中。 使用此标记可以向特定用户注册的所有设备发送推送通知。 以下代码获取发出请求的用户的 SID，并将模板推送通知发送到该用户的每个设备注册：

```
// Only do the push if configured
if (context.push) {
    // Send a notification to the current user.
    context.push.send(context.user.id, payload, function (error) {
        if (error) {
            // Do something or log the error.
        }
    });
}
```

在注册来自经过身份验证客户端的推送通知时，请确保在尝试注册之前身份验证已完成。

## <a name="CustomAPI"></a> 自定义 API
### <a name="howto-customapi-basic"></a>定义自定义 API
除了通过 `/tables` 终结点的数据访问 API 以外，移动应用还可提供自定义 API 覆盖范围。 自定义 API 以类似于表定义的方法定义，可访问所有相同的功能，包括身份验证。

若要将应用服务身份验证与自定义 API 配合使用，必须先在 [Azure 门户]中配置应用服务身份验证。 有关详细信息，请参阅要使用的标识提供者的配置指南：

- [如何配置 Azure Active Directory 身份验证]
- [如何配置 Microsoft 身份验证]

定义自定义 API 的方法与表 API 大致相同：

1. 创建 `api` 目录。
2. 在 `api` 目录中创建 API 定义 JavaScript 文件。
3. 使用 import 方法导入 `api` 目录。

下面是根据前面使用的基本应用示例所做的原型 API 定义：

```
var express = require('express'),
    azureMobileApps = require('azure-mobile-apps');

var app = express(),
    mobile = azureMobileApps();

// Import the Custom API
mobile.api.import('./api');

// Add the mobile API so it is accessible as a Web API
app.use(mobile);

// Start listening on HTTP
app.listen(process.env.PORT || 3000);
```

让我们使用一个通过 `Date.now()` 方法返回服务器日期的示例 API。 下面是 api/date.js 文件：

```
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    });
};

module.exports = api;
```

每个参数是标准的 RESTful 谓词之一：GET、POST、PATCH 或 DELETE。 此方法是发送所需输出的标准 [ExpressJS 中间件]函数。

### <a name="howto-customapi-auth"></a>要求在访问自定义 API 时进行身份验证
Azure 移动应用 SDK 对 `tables` 终结点和自定义 API 使用相同的方式实现身份验证。 若要在前一部分开发的 API 中添加身份验证，请添加 `access` 属性：

```
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    });
};
// All methods must be authenticated.
api.access = 'authenticated';

module.exports = api;
```

也可以指定对特定操作的身份验证：

```
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    }
};
// The GET methods must be authenticated.
api.get.access = 'authenticated';

module.exports = api;
```

对于要求身份验证的自定义 API，必须使用与 `tables` 终结点相同的令牌。

### <a name="howto-customapi-auth"></a>处理大型文件上传
移动应用 SDK 使用[正文分析器中间件](https://github.com/expressjs/body-parser)来接受和解码提交件的正文内容。 可以将正文分析器预先配置为接受大型文件上传：

```
var express = require('express'),
    bodyParser = require('body-parser'),
    azureMobileApps = require('azure-mobile-apps');

var app = express(),
    mobile = azureMobileApps();

// Set up large body content handling
app.use(bodyParser.json({ limit: '50mb' }));
app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

// Import the Custom API
mobile.api.import('./api');

// Add the mobile API so it is accessible as a Web API
app.use(mobile);

// Start listening on HTTP
app.listen(process.env.PORT || 3000);
```

传输前，该文件采用 Base-64 编码。 此编码会增加实际上传的大小（因此必须考虑该大小）。

### <a name="howto-customapi-sql"></a>执行自定义 SQL 语句
移动应用 SDK 允许通过请求对象访问整个上下文。 可以轻松针对定义的数据提供程序执行参数化的 SQL 语句：

```
    var api = {
        get: function (request, response, next) {
            // Check for parameters. If not there, pass on to a later API call.
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query. Anything that the mssql
            // driver can handle is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query. The context for Mobile Apps is available through
            // request.azureMobile. The data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;
```

## <a name="Debugging"></a>调试、简易表和简易 API
### <a name="howto-diagnostic-logs"></a>对移动应用进行调试、诊断和故障排除
Azure 应用服务提供多种适用于 Node.js 应用程序的调试和故障排除方法。
若要开始针对 Node.js 移动应用后端进行故障排除，请参阅以下文章：

* [监视 Azure 应用服务]
* [在 Azure 应用服务中启用诊断日志记录]
* [在 Visual Studio 中对 Azure 应用服务进行故障排除]

Node.js 应用程序可访问各种诊断日志工具。 在内部，移动应用 Node.js SDK 使用 [Winston] 进行诊断日志记录。 启用调试模式，或者在 [Azure 门户]中将 `MS_DebugMode` 应用设置指定为 true，即可自动启用日志记录。 生成的日志显示在 [Azure 门户]上的诊断日志中。

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>在 Azure 门户中使用简易表
使用简易表可以直接在门户中创建和使用表。 可以采用 CSV 格式将数据集上传到简易表。 请注意，不能使用与移动应用后端的系统属性名称冲突的属性名称（在 CSV 数据集中）。 系统属性名称包括：
* createdAt
* updatedAt
* deleted
* 版本

甚至可以使用应用服务编辑器来编辑表操作。 在后端站点设置中选择“简易表”时，可以添加、修改或删除表。 还可以查看表中的数据。

![使用简易表](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

表的命令栏中提供了以下命令：

* **更改权限**：修改在表中读取、插入、更新和删除操作的权限。
 选项包括允许匿名访问、要求身份验证，或禁用对操作的所有访问。
* **编辑脚本**：在应用服务编辑器中打开表的脚本文件。
* **管理架构**：添加或删除列，或者更改表索引。
* **清除表**：截断现有表可能会删除所有行，但架构保持不变。
* **删除行**：删除单个数据行。
* **查看流式处理日志**：连接到站点的流式处理日志服务。

### <a name="work-easy-apis"></a>在 Azure 门户中使用简易 API
使用简易 API 可以直接在门户中创建和使用自定义 API。 可以使用应用服务编辑器编辑 API 脚本。

在后端站点设置中选择“简易 API”时，可以添加、修改或删除自定义 API 终结点。

![使用简易 API](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

在门户中，可以更改 HTTP 操作的访问权限、在应用服务编辑器中编辑 API 脚本文件，或查看流式处理日志。

### <a name="online-editor"></a>在应用服务编辑器中编辑代码
使用 Azure 门户可在应用服务编辑器中编辑 Node.js 后端脚本文件，而无需将项目下载到本地计算机。 若要在在线编辑器中编辑脚本文件，请执行以下操作：

1. 在移动应用后端的窗格中，选择“所有设置”“简易表”或“简易 API”。 选择表或 API，并选择“编辑脚本”。 脚本文件会在应用服务编辑器中打开。

    ![应用服务编辑器](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. 在在线编辑器中更改代码文件。 键入内容时，更改会自动保存。

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android 客户端快速入门]: ./app-service-mobile-android-get-started.md
[Apache Cordova 客户端快速入门]: ./app-service-mobile-cordova-get-started.md
[iOS 客户端快速入门]: ./app-service-mobile-ios-get-started.md
[Xamarin.iOS 客户端快速入门]: ./app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android 客户端快速入门]: ./app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms 客户端快速入门]: ./app-service-mobile-xamarin-forms-get-started.md
[Windows 应用商店客户端快速入门]: ./app-service-mobile-windows-store-dotnet-get-started.md
[offline data sync]: ./app-service-mobile-offline-data-sync.md
[如何配置 Azure Active Directory 身份验证]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[如何配置 Microsoft 身份验证]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Azure App Service Deployment Guide]: ../app-service/app-service-deploy-local-git.md
[Monitoring an Azure App Service]: ../app-service/web-sites-monitor.md
[Enable Diagnostic Logging in Azure App Service（在 Azure 应用服务中启用诊断记录）]: ../app-service/web-sites-enable-diagnostic-log.md
[Troubleshoot an Azure App Service in Visual Studio]: ../app-service/web-sites-dotnet-troubleshoot-visual-studio.md
[specify the Node Version]: ../nodejs-specify-node-version-azure-apps.md
[use Node modules]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure Portal]: https://portal.azure.cn/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise
[GitHub 上的 basicapp 示例]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[在 GitHub 上的示例目录]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS 中间件]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston