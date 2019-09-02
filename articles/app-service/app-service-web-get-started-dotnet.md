---
title: 创建 C# ASP.NET Core Web 应用 - Azure 应用服务 | Azure Docs
description: 了解如何通过部署默认的 C# ASP.NET Core Web 应用，在 Azure 应用服务中运行 Web 应用。
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
origin.date: 09/05/2018
ms.date: 01/21/2019
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: 2bb53e569a0b905638ea89fae78a0c6300118bdf
ms.sourcegitcommit: 0cb57e97931b392d917b21753598e1bd97506038
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2019
ms.locfileid: "54906017"
---
# <a name="create-an-aspnet-core-web-app-in-azure"></a>在 Azure 中创建 ASP.NET Core Web 应用

[Azure 应用服务](overview.md)提供高度可缩放、自修补的 Web 托管服务。  本快速入门演示如何将第一个 ASP.NET Core Web 应用部署到 Azure 应用服务中。 完成后，将拥有一个资源组，该资源组包含一个应用服务计划和一个部署了 Web 应用程序的应用服务应用。

![](./media/app-service-web-get-started-dotnet/web-app-running-live.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件

若要完成本教程，请安装带有 ASP.NET 和 Web 开发工作负荷的 <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017</a>。

如果已安装 Visual Studio 2017：

- 单击“帮助” > “检查更新”，在 Visual Studio 中安装最新更新。
- 单击“工具” > “获取工具和功能”添加工作负荷。

## <a name="create-an-aspnet-core-web-app"></a>创建一个 ASP.NET Core Web 应用

在 Visual Studio 中，通过依次选择“文件”>“新建”>“项目”创建项目。 

在“新建项目”对话框中，选择“Visual C#”>“Web”>“ASP.NET Core Web 应用程序”。

将应用程序命名为 _myFirstAzureWebApp_，然后选择“确定”。
   
![“新建项目”对话框](./media/app-service-web-get-started-dotnet/new-project.png)

可将任何类型的 ASP.NET Core Web 应用部署到 Azure。 对于本快速入门，请选择“Web 应用程序”模板，确保身份验证设置为“无身份验证”，不要选择其他选项。
      
选择“确定” 。

![“新建 ASP.NET 项目”对话框](./media/app-service-web-get-started-dotnet/razor-pages-aspnet-dialog.png)

在菜单中，选择“调试>启动但不调试”以在本地运行 Web 应用。

![在本地运行应用](./media/app-service-web-get-started-dotnet/razor-web-app-running-locally.png)

## <a name="launch-the-publish-wizard"></a>启动发布向导

在“解决方案资源管理器”中右键单击“myFirstAzureWebApp”项目，然后选择“发布”。

![从解决方案资源管理器发布](./media/app-service-web-get-started-dotnet/right-click-publish.png)

发布向导将自动启动。 选择“应用服务” > “发布”以打开“创建应用服务”对话框。

![从项目概述页发布](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

## <a name="sign-in-to-azure"></a>登录 Azure

在“创建应用服务”对话框中单击“添加帐户”，然后登录到 Azure 订阅。 如果已登录，请从下拉列表中选择所需的帐户。

> [!NOTE]
> 如果已经登录，请先不要选择“创建”。
>
   
![登录 Azure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a>创建资源组

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

在“资源组”旁边，选择“新建”。

将资源组命名为 **myResourceGroup**，然后选择“确定”。

## <a name="create-an-app-service-plan"></a>创建应用服务计划

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

在“托管计划”旁边，选择“新建”。 

在“配置托管计划”对话框中，使用该屏幕截图下面的表中的设置。

![创建应用服务计划](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| 设置 | 建议的值 | 描述 |
|-|-|-|
|应用服务计划| myAppServicePlan | 应用服务计划的名称。 |
| 位置 | 西欧 | 托管 Web 应用的数据中心。 |
| 大小 | 免费 | [定价层](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)确定托管功能。 |

选择“确定” 。

## <a name="create-and-publish-the-web-app"></a>创建并发布 Web 应用

在“应用名称”中，键入唯一的应用名称（有效字符为 `a-z`、`0-9` 和 `-`），或接受自动生成的唯一名称。 Web 应用的 URL 为 `http://<app_name>.chinacloudsites.cn`，其中 `<app_name>` 是应用名称。

选择“创建”开始创建 Azure 资源。

![配置应用名称](./media/app-service-web-get-started-dotnet/web-app-name.png)

向导完成后，它会将 ASP.NET Core Web 应用发布到 Azure，然后在默认浏览器中启动该应用。

![已在 Azure 中发布的 ASP.NET Web 应用](./media/app-service-web-get-started-dotnet/web-app-running-live.png)

将[创建和发布步骤](#create-and-publish-the-web-app)中指定的应用名称用作 `http://<app_name>.chinacloudsites.cn` 格式的 URL 前缀。

恭喜，ASP.NET Core Web 应用已在 Azure 应用服务中实时运行！

## <a name="update-the-app-and-redeploy"></a>更新应用并重新部署

在“解决方案资源管理器”中打开“Pages/Index.cshtml”。

将两个 `<div>` 标记替换为以下代码：

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we've built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

若要重新部署到 Azure，请在“解决方案资源管理器”中右键单击“myFirstAzureWebApp”项目，然后选择“发布”。

在发布摘要页中选择“发布”。
![Visual Studio 发布摘要页](./media/app-service-web-get-started-dotnet/publish-summary-page.png)

发布完成后，Visual Studio 将启动浏览器并转到 Web 应用的 URL。

![已在 Azure 中更新的 ASP.NET Web 应用](./media/app-service-web-get-started-dotnet/web-app-running-live-updated.png)

## <a name="manage-the-azure-app"></a>管理 Azure 应用

转到 <a href="https://portal.azure.cn" target="_blank">Azure 门户</a>管理 Web 应用。

从左侧菜单中选择“应用服务”，然后选择 Azure Web 应用的名称。

![在门户中导航到 Azure Web 应用](./media/app-service-web-get-started-dotnet/access-portal.png)

随后会显示 Web 应用的概述页。 在此处可以执行基本的管理任务，例如浏览、停止、启动、重启和删除。 

![Azure 门户中的“应用服务”边栏选项卡](./media/app-service-web-get-started-dotnet/web-app-blade.png)

左侧菜单提供用于配置应用的不同页面。 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [将 ASP.NET Core 与 SQL 数据库配合使用](app-service-web-tutorial-dotnetcore-sqldb.md)
