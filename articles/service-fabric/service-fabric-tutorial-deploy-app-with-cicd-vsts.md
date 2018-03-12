---
title: "部署具有持续集成功能的 Azure Service Fabric 应用程序 (Team Services) | Azure"
description: "本教程介绍如何使用 Visual Studio Team Services 为 Service Fabric 应用程序设置持续集成和部署。  将应用程序部署到 Azure 中的 Service Fabric 群集。"
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 12/13/2017
ms.date: 03/12/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: bca30cab4a38236a6ee698fe64feb57cc6ce7774
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="tutorial-deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>教程：将具有 CI/CD 的应用程序部署到 Service Fabric 群集
本教程是一个系列的第三部分，介绍了如何使用 Visual Studio Team Services 为 Azure Service Fabric 应用程序设置持续集成和部署。  需要一个现有的 Service Fabric 应用程序，将使用在[生成 .NET 应用程序](service-fabric-tutorial-create-dotnet-app.md)中创建的应用程序作为示例。

在该系列的第三部分中，你会学习如何：

> [!div class="checklist"]
> * 向项目中添加源代码管理
> * 在 Team Services 中创建生成定义
> * 在 Team Services 中创建发布定义
> * 自动部署和升级应用程序

在此系列教程中，你将学习如何：
> [!div class="checklist"]
> * [构建 .NET Service Fabric 应用程序](service-fabric-tutorial-create-dotnet-app.md)
> * [将应用程序部署到远程群集](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * 使用 Visual Studio Team Services 配置 CI/CD
<!-- Not Available on > * [Set up monitoring and diagnostics for the application](service-fabric-tutorial-monitoring-aspnet.md)-->

## <a name="prerequisites"></a>先决条件
在开始学习本教程之前：
- 如果还没有 Azure 订阅，请创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)
- [安装 Visual Studio 2017](https://www.visualstudio.com/)，并安装 **Azure 开发**以及 **ASP.NET 和 Web 开发**工作负荷。
- [安装 Service Fabric SDK](service-fabric-get-started.md)
- 在 Azure 上创建一个 Windows Service Fabric 群集，例如[根据此教程](service-fabric-tutorial-create-vnet-and-windows-cluster.md)创建
- 创建一个 [Team Services 帐户](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)。

## <a name="download-the-voting-sample-application"></a>下载投票示例应用程序
如果未生成[本系列教程的第一部分](service-fabric-tutorial-create-dotnet-app.md)中的投票示例应用程序，可以下载它。 在命令窗口中，运行以下命令，将示例应用存储库克隆到本地计算机。

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>准备一个发布配置文件
你已[创建了一个应用程序](service-fabric-tutorial-create-dotnet-app.md)并已[将该应用程序部署到了 Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md)，现在可以设置持续集成了。  首先，在应用程序中准备一个发布配置文件，供要在 Team Services 中执行的部署进程使用。  应当将发布配置文件配置为以之前创建的群集为目标。  启动 Visual Studio 并打开一个现有的 Service Fabric 应用程序项目。  在“解决方案资源管理器”中，右键单击该应用程序并选择“发布...”。

在应用程序项目中选择一个要用于持续集成工作流的目标配置文件，例如 Cloud。  指定群集连接终结点。  选中“升级应用程序”复选框，以便应用程序针对 Team Services 中的每个部署进行升级。  单击“保存”超链接将设置保存到发布配置文件，然后单击“取消”关闭对话框。  

![推送配置文件][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>将 Visual Studio 解决方案共享到一个新的 Team Services Git 存储库
将应用程序源文件共享到 Team Services 中的一个团队项目，以便生成内部版本。  

通过在 Visual Studio 的右下角的状态栏中选择“添加到源代码管理” -> “Git”为项目创建一个新的本地 Git 存储库。 

在“团队资源管理器”中的“推送”视图中，在“推送到 Visual Studio Team Services”下选择“发布 Git 存储库”按钮。

![推送 Git 存储库][push-git-repo]

验证电子邮件并在“Team Services 域”下拉列表中选择帐户。 输入存储库名称并选择“发布存储库”。

![推送 Git 存储库][publish-code]

发布存储库时，会在帐户中创建一个与本地存储库同名的新团队项目。 若要在现有团队项目中创建存储库，请单击“存储库名称”旁边的“高级”并选择一个团队项目。 可以通过选择“在 Web 上查看”在 Web 上查看代码。

## <a name="configure-continuous-delivery-with-vsts"></a>配置“使用 VSTS 实现持续交付”
Team Services 生成定义所描述的工作流由一系列按顺序执行的生成步骤组成。 创建一个生成定义，以生成要部署到 Service Fabric 群集的 Service Fabric 应用程序包和其他项目。 详细了解 [Team Services 生成定义](https://www.visualstudio.com/docs/build/define/create)。 

Team Services 发布定义描述了将应用程序包部署到群集的工作流。 一起使用时，生成定义和发布定义将执行从开始到结束的整个工作流，即一开始只有源文件，而结束时群集中会有一个运行的应用程序。 详细了解 Team Services [发布定义](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition)。

### <a name="create-a-build-definition"></a>创建生成定义
打开 Web 浏览器并导航到新的团队项目：[https://&lt;myaccount&gt;.visualstudio.com/Voting/Voting%20Team/_git/Voting](https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting)。 

依次选择“生成和发布”选项卡、“生成”、“+ 新建定义”。  在“选择模板”中，选择“Azure Service Fabric 应用程序”模板，然后单击“应用”。 

![选择“生成模板”][select-build-template] 

在“任务”中，为“代理队列”输入“Hosted VS2017”。 

![选择任务][save-and-queue]

在“触发器”下，通过设置“触发器状态”启用持续集成。  选择“保存并排队”以手动启动生成。  

![选择触发器][save-and-queue2]

推送或签入时也会触发生成操作。 若要检查生成进度，请切换到“生成”选项卡。在验证生成成功执行后，定义用于将应用程序部署到群集的发布定义。 

### <a name="create-a-release-definition"></a>创建发布定义  

依次选择“生成和发布”选项卡、“发布”、“+ 新建定义”。  在“选择模板”中，从列表中选择“Azure Service Fabric 部署”模板，然后单击“应用”。  

![选择发布模板][select-release-template]

选择“任务”->“环境 1”，然后单击“+新建”添加新的群集连接。

![添加群集连接][add-cluster-connection]

在“添加新的 Service Fabric 连接”视图中，选择“基于证书”或“Azure Active Directory”身份验证。  指定连接名称“mysftestcluster”和群集终结点“tcp://mysftestcluster.chinaeast.cloudapp.chinacloudapi.cn:19000”（或要部署到的群集的终结点）。 

对于基于证书的身份验证，请添加用于创建群集的服务器证书的**服务器证书指纹**。  在“客户端证书”中，添加客户端证书文件的 base-64 编码。 查看有关该字段的帮助弹出窗口，了解有关如何获取该证书的 base-64 编码表示形式的信息。 另请添加证书的**密码**。  如果没有单独的客户端证书，可以使用群集或服务器证书。 

对于 Azure Active Directory 凭据，请添加用于创建群集的服务器证书的**服务器证书指纹**，并在“用户名”和“密码”字段中添加用于连接群集的凭据。 

单击“添加”保存群集连接。

接下来，将一个生成项目添加到管道，以便发布定义可以找到生成返回的输出。 依次选择“管道”和“项目”->“+添加”。  在“源(生成定义)”中，选择前面创建的生成定义。  单击“添加”保存生成项目。

![添加项目][add-artifact]

启用持续部署触发器，以便在生成完成时自动创建发布。 单击项目中的闪电图标启用触发器，然后单击“保存”以保存发布定义。

![启用触发器][enable-trigger]

选择“+ 发布” -> “创建发布” -> “创建”，手动创建发布。  验证部署是否已成功以及应用程序是否正在群集中运行。  打开 Web 浏览器并导航到 [http://mysftestcluster.chinaeast.cloudapp.chinacloudapi.cn:19080/Explorer/](http://mysftestcluster.chinaeast.cloudapp.chinacloudapi.cn:19080/Explorer/)。  记下应用程序版本，在本例中为“1.0.0.20170616.3”。 

## <a name="commit-and-push-changes-trigger-a-release"></a>提交并推送更改，触发发布
通过将一些代码更改签入到 Team Services 中来验证持续集成管道是否正常工作。    

在编写代码时，Visual Studio 会自动跟踪代码更改。 通过从右下角的状态栏中选择“挂起的更改”图标（![挂起的][pending]），将更改提交到本地 Git 存储库。

在“团队资源管理器”的“更改”视图中，添加一条消息来说明所做的更新，然后提交更改。

![全部提交][changes]

在“团队资源管理器”中选择“未发布的更改”状态栏图标（![未发布的更改][unpublished-changes]）或“同步”视图。 选择“推送”以更新 Team Services/TFS 中的代码。

![推送更改][push]

将更改推送到 Team Services 会自动触发生成。  当生成定义成功完成时，会自动创建一个发布，并开始升级群集上的应用程序。

若要检查生成进度，请在 Visual Studio 中切换到“团队资源管理器”中的“生成”选项卡。  在验证生成成功执行后，定义用于将应用程序部署到群集的发布定义。

验证部署是否已成功以及应用程序是否正在群集中运行。  打开 Web 浏览器并导航到 [http://mysftestcluster.chinaeast.cloudapp.chinacloudapi.cn:19080/Explorer/](http://mysftestcluster.chinaeast.cloudapp.chinacloudapi.cn:19080/Explorer/)。  记下应用程序版本，在本例中为“1.0.0.20170815.3”。

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>更新应用程序
在应用程序中进行代码更改。  按照前面的步骤保存并提交更改。

在应用程序升级开始后，可以在 Service Fabric Explorer 中观察升级进度：

![Service Fabric Explorer][sfx2]

应用程序升级可能要花费几分钟时间才能完成。 当升级完成后，应用程序会运行下一版本。  在本例中为“1.0.0.20170815.4”。

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>后续步骤
在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 向项目中添加源代码管理
> * 创建生成定义
> * 创建发布定义
> * 自动部署和升级应用程序

<!-- Not Available on > [Set up monitoring and diagnostics for the application](service-fabric-tutorial-monitoring-aspnet.md) -->

<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue.png
[save-and-queue2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue2.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/EnableTrigger.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png

<!-- Update_Description: update meta properties -->
