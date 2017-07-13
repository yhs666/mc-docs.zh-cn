---
title: "使用 SQL 数据库在 Azure 中生成 ASP.NET | Azure"
description: "了解如何在 Azure 中运行 ASP.NET 应用，同时使其连接到 SQL 数据库。"
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: article
origin.date: 06/09/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.custom: mvc
ms.openlocfilehash: a1497cb809ce90dc86b6442cec77fc2c3d36694b
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 SQL 数据库在 Azure 中生成 ASP.NET 应用
<a id="build-an-aspnet-app-in-azure-with-sql-database" class="xliff"></a>

[!INCLUDE [azure-sdk-developer-differences](../../includes/azure-sdk-developer-differences.md)]

[Azure Web 应用](/app-service-web/app-service-web-overview)提供高度可缩放、自修补的 Web 托管服务。 本教程演示如何在 Azure 中部署数据驱动的 ASP.NET Web 应用，以及如何将其连接到 [Azure SQL 数据库](../sql-database/sql-database-technical-overview.md)。 完成此流程后，你将能在 [Azure 应用服务](../app-service/app-service-value-prop-what-is.md)中运行 ASP.NET 应用，并将其连接到 SQL 数据库。

![Azure Web 应用中已发布 ASP.NET 应用程序](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 在 Azure 中创建 SQL 数据库
> * 将 ASP.NET 应用连接到 SQL 数据库
> * 将应用部署到 Azure
> * 更新数据模型并重新部署应用
> * 将日志从 Azure 流式传输到终端
> * 在 Azure 门户中管理应用

## 先决条件
<a id="prerequisites" class="xliff"></a>

若要完成本教程，需执行以下操作：

* 使用以下工作负荷安装 [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)：
  - **ASP.NET 和 Web 开发**
  - **Azure 开发**

  ![ASP.NET 和 Web 开发以及 Azure 开发（在 Web 和云下）](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## 下载示例
<a id="download-the-sample" class="xliff"></a>

[下载示例项目](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip)。

提取（解压缩）*dotnet-sqldb-tutorial-master.zip* 文件。

此示例项目包含一个基本的 [ASP.NET MVC](https://www.asp.net/mvc) CRUD（创建-读取-更新-删除）应用，它使用 [Entity Framework Code First](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)。

### 运行应用程序
<a id="run-the-app" class="xliff"></a>

在 Visual Studio 中打开 *dotnet-sqldb-tutorial-master/DotNetAppSqlDb.sln* 文件。 

键入 `Ctrl+F5`，在不调试的情况下运行该应用。 该应用在默认的浏览器中显示。 选择“新建”链接，创建多个待办事项。 

![“新建 ASP.NET 项目”对话框](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

测试“编辑”、“详细信息”和“删除”链接。

该应用使用数据库上下文与数据库进行连接。 在此示例中，数据库上下文使用名为 `MyDbConnection` 的连接字符串。 此连接字符串在 *Web.config* 文件中设置，在 *Models/MyDatabaseContext.cs* 文件中引用。 本教程稍后会用到该连接字符串，以便将 Azure Web 应用连接到 Azure SQL 数据库。 

## 使用 SQL 数据库发布到 Azure
<a id="publish-to-azure-with-sql-database" class="xliff"></a>

在“解决方案资源管理器”中，右键单击 “DotNetAppSqlDb”项目，然后选择“发布”。

![从解决方案资源管理器发布](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

确保已选择“Azure 应用服务”，然后单击“发布”。

![从项目概述页发布](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

发布时将打开“创建应用服务”对话框，帮助你创建所需的所有 Azure 资源，以便在 Azure 中运行 ASP.NET Web 应用。

### 登录 Azure
<a id="sign-in-to-azure" class="xliff"></a>

[!INCLUDE [azure-visual-studio-login-guide](../../includes/azure-visual-studio-login-guide.md)]

在“创建应用服务”对话框中单击“添加帐户”，然后登录到你的 Azure 订阅。 如果已登录到 Azure 帐户，请确保该帐户包含你的 Azure 订阅。 如果登录的 Azure 帐户不包含你的 Azure 订阅，请单击该帐户添加正确的帐户。

![登录 Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

登录后，可在此对话框中创建 Azure Web 应用所需的所有资源。

### 配置 Web 应用名称
<a id="configure-the-web-app-name" class="xliff"></a>

可以保留生成的 Web 应用名称，也可将其更改为其他唯一的名称。 Web 应用名称用作应用的默认 URL 的一部分 (`<app_name>.chinacloudsites.cn`)。 在 Azure 的所有应用中，Web 应用名称必须是唯一的。 

![“创建应用服务”对话框](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### 创建资源组
<a id="create-a-resource-group" class="xliff"></a>

[!INCLUDE [resource-group](../../includes/resource-group.md)]

在“资源组”旁边单击“新建”。

![在“资源组”旁边单击“新建”。](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

将资源组命名为 **myResourceGroup**。

> [!NOTE]
> 请勿单击“创建”。 首先需要设置后面步骤中的 SQL 数据库。

### 创建应用服务计划
<a id="create-an-app-service-plan" class="xliff"></a>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

在“应用服务计划”旁边单击“新建”。 

在“配置应用服务计划”对话框中，使用以下设置配置新的应用服务计划：

![创建应用服务计划](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| 设置  | 值 | 更多信息 |
| ----------------- | ------------ | ----|
|**应用服务计划**| myAppServicePlan | [应用服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|**位置**| 中国北部 | - |
|**大小**| 免费 | [定价层](https://www.azure.cn/pricing/details/app-service/)|

### 创建 SQL Server 实例
<a id="create-a-sql-server-instance" class="xliff"></a>

选择“浏览其他 Azure 服务”。

![配置 Web 应用名称](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

在“服务”选项卡中，单击“SQL 数据库”旁边的 **+** 图标。 

![在“服务”选项卡中，单击“SQL 数据库”旁边的 + 图标。](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

在“配置 SQL 数据库”对话框中，单击“SQL Server”旁的“新建”。 

此时会生成唯一的服务器名称。 该名称用作数据库服务器的默认 URL 的一部分 (`<server_name>.database.chinacloudapi.cn`)。 在 Azure 的所有 SQL 服务器实例中，该名称必须唯一。 可以更改服务器名称，但就本教程来说，请保留生成的值。

添加管理员用户名和密码，然后选择“确定”。

![创建 SQL Server 实例](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

此时会显示“配置 SQL 数据库”对话框： 

* 保留默认生成的数据库名称。
* 在“连接字符串名称”中，键入 *MyDbConnection*。 此名称必须与 *Models/MyDatabaseContext.cs* 中引用的连接字符串相匹配。
* 选择“确定” 。

![配置 SQL 数据库](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

“创建应用服务”对话框会显示所创建的资源。 单击“创建” 。 

![已创建的资源](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

向导在创建完 Azure 资源以后，会将 ASP.NET 应用发布到 Azure。 默认浏览器会使用已部署应用的 URL 启动。 

添加多个待办事项。

![Azure Web 应用中已发布 ASP.NET 应用程序](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

祝贺你！ 数据驱动 ASP.NET 应用程序当前在 Azure 应用服务中实时运行。

## 本地访问 SQL 数据库
<a id="access-the-sql-database-locally" class="xliff"></a>

使用 Visual Studio 可在“SQL Server 对象资源管理器”中轻松浏览和管理自己的新 SQL 数据库。

### 创建数据库连接
<a id="create-a-database-connection" class="xliff"></a>

在“视图”菜单中，选择“SQL Server 对象资源管理器”。

在“SQL Server 对象资源管理器”顶部，单击“添加 SQL Server”按钮。

### 配置数据库连接
<a id="configure-the-database-connection" class="xliff"></a>

在“连接”对话框中，展开“Azure”节点。 此处列出了 Azure 中的所有 SQL 数据库实例。

选择 `DotNetAppSqlDb` SQL 数据库。 底部将自动填充前面创建的连接。

键入先前创建的数据库管理员密码，然后单击“连接”。

![通过 Visual Studio 配置数据库连接](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### 允许来自你的计算机的客户端连接
<a id="allow-client-connection-from-your-computer" class="xliff"></a>

此时会打开“新建防火墙规则”对话框。 默认情况下，SQL 数据库实例仅允许来自 Azure 服务（例如 Azure Web 应用）的连接。 若要连接到数据库，请在 SQL 数据库实例中创建防火墙规则。 防火墙规则允许本地计算机的公共 IP 地址。

对话框中已填充了你的计算机的公共 IP 地址。

请确保选中“添加我的客户端 IP”，然后单击“确定”。 

![为 SQL 数据库实例设置防火墙](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

Visual Studio 成功为 SQL 数据库实例创建防火墙设置后，连接将立即显示在“SQL Server 对象资源管理器”中。

可在此处执行最常见的数据库操作，如运行查询、创建视图和存储过程等。 

右键单击 `Todoes` 表，然后选择“查看数据”。 

![探索 SQL 数据库对象](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## 使用 Code First 迁移更新应用
<a id="update-app-with-code-first-migrations" class="xliff"></a>

此步骤中将使用实体框架中的 Code First 迁移对数据库架构进行更改，并将其发布至 Azure。

### 更新数据模型
<a id="update-your-data-model" class="xliff"></a>

在代码编辑器中打开 _Models\Todo.cs_。 将以下属性添加到 `ToDo` 类：

```csharp
public bool Done { get; set; }
```

### 本地运行 Code First 迁移
<a id="run-code-first-migrations-locally" class="xliff"></a>

运行几个命令来更新本地数据库。 

在“工具”菜单中，单击“NuGet 包管理器” > “包管理器控制台”。

启用 Code First 迁移：

```PowerShell
Enable-Migrations
```

添加迁移：

```PowerShell
Add-Migration AddProperty
```

更新本地数据库：

```PowerShell
Update-Database
```

键入 `Ctrl+F5`，以便运行该应用。 测试“编辑”、“详细信息”和“创建”链接。

如果应用程序加载未出错，则 Code First 迁移成功。 但页面看上去仍没有变化，这是因为应用程序逻辑尚未使用新属性。 

### 使用新属性
<a id="use-the-new-property" class="xliff"></a>

为了使用 `Done` 属性，请对代码做一些更改。 简单起见，本教程中将仅更改 `Index` 和 `Create` 视图，以便在操作中查看属性。

打开 _Controllers\TodosController.cs_。

找到 `Create()` 方法，并将 `Done` 添加到 `Bind` 属性中的属性列表。 完成后，`Create()` 方法签名如以下代码所示：

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

打开 _Views\Todos\Create.cshtml_。

在 Razor 代码中，应依次看见使用 `model.Description` 的 `<div class="form-group">` 元素和使用 `model.CreatedDate` 的 `<div class="form-group">` 元素。 紧跟在这两个元素之后，添加另一个使用 `model.Done` 的 `<div class="form-group">` 元素：

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
    </div>
</div>
```

打开 _Views\Todos\Index.cshtml_。

搜索空的 `<th></th>` 元素。 在此元素的正上方，添加下列 Razor 代码：

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

查找包含 `Html.ActionLink()` 帮助器方法的 `<td>` 元素。 在此元素的正上方，添加下列 Razor 代码：

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

这就是要在 `Index` 和 `Create` 视图中查看更改所需的全部操作。 

键入 `Ctrl+F5`，以便运行该应用。

现在可以添加待办事项，然后单击“完成”。 然后，它应作为已完成项在主页中显示。 请记住，`Edit`视图不显示`Done`字段，因为没有更改`Edit`视图。

### 在 Azure 中启用 Code First 迁移
<a id="enable-code-first-migrations-in-azure" class="xliff"></a>

代码更改生效（包括数据库迁移）后，将其发布至 Azure Web 应用，并仍使用 Code First 迁移更新 SQL 数据库。

与先前的操作相同，右键单击项目，然后选择“发布”。

单击“设置”打开发布向导。

![打开发布设置](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

在向导中，单击“下一步”。

请确保在 **MyDatabaseContext (MyDbConnection)** 中填充 SQL 数据库的连接字符串。 可能需要从下拉列表中选择“myToDoAppDb”数据库。 

选择“执行 Code First 迁移(应用程序启动时运行)”，然后单击“保存”。

![在 Azure Web 应用中启用 Code First 迁移](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### 发布更改
<a id="publish-your-changes" class="xliff"></a>

现已在 Azure Web 应用中启用了 Code First 迁移，请发布代码更改。

在发布页中单击“发布”。

再次尝试添加待办事项并选择“完成”，然后，它们将作为已完成项显示在主页中。

![Code First 迁移后的 Azure Web 应用](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

所有现有待办事项仍将显示。 重新发布 ASP.NET 应用程序时，SQL 数据库中的现有数据不会丢失。 此外，Code First 迁移仅更改数据架构，而使现有数据保持不变。

## 流式传输应用程序日志
<a id="stream-application-logs" class="xliff"></a>

可直接通过 Azure Web 应用将跟踪消息流式传输到 Visual Studio。

打开 _Controllers\TodosController.cs_。

每个操作都以 `Trace.WriteLine()` 方法开头。 添加此代码的目的是演示如何将跟踪消息添加至 Azure Web 应用。

### 打开服务器资源管理器
<a id="open-server-explorer" class="xliff"></a>

在“视图”菜单中，选择“服务器资源管理器”。 可在“服务器资源管理器”中为 Azure Web 应用配置日志记录。 

### 启用日志流式传输
<a id="enable-log-streaming" class="xliff"></a>

在“服务器资源管理器”中，展开“Azure” > “应用服务”。

展开“myResourceGroup”资源组，该资源组在首次创建 Azure Web 应用时创建。

右键单击 Azure Web 应用，然后选择“查看流式传输日志”。

![启用日志流式传输](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

现在，日志已流式传输到“输出”窗口。 

![输出窗口中的日志流式传输](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

但还无法查看任何跟踪消息。 因为当首先选择“查看流式传输日志”时，Azure Web 应用将跟踪级别设置为 `Error`，此级别只记录错误事件（使用 `Trace.TraceError()` 方法）。

### 更改跟踪级别
<a id="change-trace-levels" class="xliff"></a>

若要更改跟踪级别以输出其他跟踪消息，请返回到“服务器资源管理器”。

再次右键单击 Azure Web 应用，然后选择“设置”。

在“应用程序日志记录(文件系统)”下拉列表中，选择“详细”。 单击“保存” 。

![将跟踪级别更改为“详细”](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> 可试验不同的跟踪级别，以查看每个级别分别显示哪些类型的消息。 例如，“信息”级别包括 `Trace.TraceInformation()`、`Trace.TraceWarning()` 和 `Trace.TraceError()` 创建的所有日志，但不包括 `Trace.WriteLine()` 创建的日志。
>
>

在浏览器中，尝试在 Azure 中的待办事项列表应用程序周围进行单击。 现在，跟踪消息已流式传输到 Visual Studio 中的“输出”窗口。

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```

### 停止日志流式传输
<a id="stop-log-streaming" class="xliff"></a>

若要停止日志流式传输服务，请单击“输出”窗口中的“停止监视”按钮。

![停止日志流式传输](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## 管理 Azure Web 应用
<a id="manage-your-azure-web-app" class="xliff"></a>

转到 [Azure 门户](https://portal.azure.cn)查看已创建的 Web 应用。 

从左侧菜单中单击“应用服务”，然后单击 Azure Web 应用的名称。

![在门户中导航到 Azure Web 应用](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

已进入 Web 应用的页面。 

默认情况下，门户将显示“概览”页。 在此页中可以查看应用的运行状况。 在此处还可以执行基本的管理任务，例如浏览、停止、启动、重新启动和删除。 页面左侧的选项卡显示可以打开的不同配置页。 

![Azure 门户中的“应用服务”页](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## 后续步骤
<a id="next-steps" class="xliff"></a>

在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 在 Azure 中创建 SQL 数据库
> * 将 ASP.NET 应用连接到 SQL 数据库
> * 将应用部署到 Azure
> * 更新数据模型并重新部署应用
> * 将日志从 Azure 流式传输到终端
> * 在 Azure 门户中管理应用

转到下一教程，了解如何向 Web 应用映射自定义 DNS 名称。

> [!div class="nextstepaction"]
> [将现有的自定义 DNS 名称映射到 Azure Web 应用](app-service-web-tutorial-custom-domain.md)
