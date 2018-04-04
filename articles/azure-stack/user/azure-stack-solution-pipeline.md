---
title: 将应用部署到 Azure 和 Azure Stack | Microsoft Docs
description: 了解如何使用混合 CI/CD 管道将应用部署到 Azure 和 Azure Stack。
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
origin.date: 02/21/2018
ms.date: 03/09/2018
ms.author: v-junlch
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: f2857a354b22a6ce481727474e82e11bf9e3d49a
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="deploy-apps-to-azure-and-azure-stack"></a>将应用部署到 Azure 和 Azure Stack
*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

使用混合[持续集成](https://www.visualstudio.com/learn/what-is-continuous-integration/)/[持续交付](https://www.visualstudio.com/learn/what-is-continuous-delivery/) (CI/CD) 管道可以生成、测试应用并将其部署到多个云中。  在本教程中，我们将创建一个示例环境，以了解混合 CI/CD 管道的作用：
 
> [!div class="checklist"]
> * 根据 Visual Studio Team Services (VSTS) 存储库的代码提交启动新的生成。
> * 自动将新生成的代码部署到 Azure 进行用户验收测试。
> * 代码通过测试后，可自动部署到 Azure Stack。 


## <a name="prerequisites"></a>先决条件
生成混合 CI/CD 管道所需的几个组件，可能需要一段时间来准备。  如果已准备好其中的某些组件，请确保它们符合要求，然后开始操作。

本主题还假设你对 Azure 和 Azure Stack 有一定的了解。 若要在了解详情之后再继续，请务必从以下主题着手：

- [Azure 简介](/fundamentals-introduction-to-azure)
- [Azure Stack 的重要概念](../azure-stack-key-features.md)

### <a name="azure"></a>Azure
 - 如果没有 Azure 订阅，请在开始之前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。
 - 创建一个 Web 应用，并将其配置为用于 [FTP 发布](../../app-service/app-service-deploy-ftp.md)。  记下新 Web 应用的 URL，因为稍后需要用到。


### <a name="azure-stack"></a>Azure Stack
 - [部署 Azure Stack](../azure-stack-run-powershell-script.md)。  安装过程通常需要几个小时才能完成，因此请相应地做好规划。
 - 将[应用服务](../azure-stack-app-service-deploy.md) PaaS 服务部署到 Azure Stack。
 - 创建一个 Web 应用，并将其配置为用于 [FTP 发布](../azure-stack-app-service-enable-ftp.md)。  记下新 Web 应用的 URL，因为稍后需要用到。  

### <a name="developer-tools"></a>开发人员工具
 - 创建 [VSTS 工作区](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)。  注册过程将创建名为“MyFirstProject”的项目。  
 - [安装 Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) 并[登录到 VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#connect-and-share-code-from-visual-studio)
 - 连接到项目并在[本地克隆](https://www.visualstudio.com/docs/git/gitquickstart)。
 - 在 VSTS 中创建[代理池](https://www.visualstudio.com/docs/build/concepts/agents/pools-queues#creating-agent-pools-and-queues)。
 - 安装 Visual Studio 并将 [VSTS 生成代理](https://www.visualstudio.com/docs/build/actions/agents/v2-windows)部署到 Azure Stack 上的虚拟机。 
 

## <a name="create-app--push-to-vsts"></a>创建应用并推送到 VSTS

### <a name="create-application"></a>创建应用程序
在本部分，我们将创建一个简单的 ASP.NET 应用程序并将其推送到 VSTS。  这些步骤代表正常的开发人员工作流，可根据开发人员工具和语言进行调整。 

1.  打开 Visual Studio。
2.  在团队资源管理器空间和“解决方案...”区域中，单击“新建”。
3.  选择“Visual C#” > “Web” > “ASP.NET Web 应用程序(.NET Framework)”。
4.  提供应用程序的名称，并单击“确定”。
5.  在下一个屏幕上保留默认值（“Web窗体”），并单击“确定”。

### <a name="commit-and-push-changes-to-vsts"></a>将更改提交并推送到 VSTS
1.  使用 Visual Studio 中的团队资源管理器选择下拉表，并单击“更改”。
2.  提供提交消息，选择“全部提交”。 系统可能会提示保存解决方案文件。请单击“是”以全部保存。
3.  提交后，Visual Studio 会要求将更改同步到项目。 选择“同步”。

    ![完成提交后显示提交屏幕的图像](./media/azure-stack-solution-pipeline/image1.png)

4.  在“同步”选项卡中的“传出”下，可以看到新的提交内容。  选择“推送”，将更改同步到 VSTS。

    ![显示同步步骤的图像](./media/azure-stack-solution-pipeline/image2.png)

### <a name="review-code-in-vsts"></a>查看 VSTS 中的代码
将更改提交并推送到 VSTS 之后，请从 VSTS 门户检查代码。  选择“代码”，然后从下拉菜单中选择“文件”。  可以看到已创建的解决方案。

## <a name="create-build-definition"></a>创建生成定义
生成过程将定义应用程序的生成和打包方式，以便根据提交的每项代码更改进行部署。 在本示例中，尽管可以根据应用程序调整此配置，但我们仍使用了随附的模板来配置 ASP.NET 应用的生成过程。

1.  从 Web 浏览器登录到 VSTS 工作区。
2.  从标题中选择“生成和发布”，然后选择“生成”。
3.  单击“+ 新建定义”。
4.  从模板列表中选择“ASP.NET (预览版)”，然后选择“应用”。
5.  将“生成解决方案”步骤中的“MSBuild 参数”字段修改为：

    `/p:DeployOnBuild=True /p:WebPublishMethod=FileSystem /p:DeployDefaultTarget=WebPublish /p:publishUrl="$(build.artifactstagingdirectory)\\"`

6.  选择“选项”选项卡，然后选择部署到 Azure Stack 上虚拟机的生成代理的代理队列。 
7.  选择“触发器”选项卡，启用“持续集成”。
7.  单击“保存并排队”，并从下拉列表中选择“保存”。 

## <a name="create-release-definition"></a>创建发布定义
发布过程会定义如何将上一步骤中创建的生成部署到环境。  在本教程中，我们使用 FTP 将 ASP.NET 应用发布到 Azure Web 应用。 若要配置发布到 Azure，请使用以下步骤：

1.  从 VSTS 标题中选择“生成和发布”，然后选择“发布”。
2.  单击绿色的“+ 新建定义”。
3.  选择“空白”，单击“下一步”。
4.  选中“持续部署”对应的框，单击“创建”。

创建空白的发布定义并将其链接到生成后，请添加 Azure 环境的相应步骤：

1.  单击绿色的 **+** 添加任务。
2.  选择“所有”，从列表中添加“FTP 上传”，并选择“关闭”。
3.  选择刚刚添加的“FTP 上传”任务，并配置以下参数：
    
    | 参数 | 值 |
    | ----- | ----- |
    |身份验证方法| 输入凭据|
    |服务器 URL | 从 Azure 门户检索到的 Web 应用 FTP URL |
    |用户名 | 创建 Web 应用的 FTP 凭据时配置的用户名 |
    |密码 | 创建 Web 应用的 FTP 凭据时创建的密码|
    |源目录 | $(System.DefaultWorkingDirectory)\**\ |
    |远程目录 | /site/wwwroot/ |
    |保留的文件路径 | 已启用（已选中）|

4.  单击“保存” 

最后，使用以下步骤配置发布定义，以使用包含所部署的代理的代理池：
1.  选择发布定义，单击“编辑”。
2.  在中间列中选择“在代理上运行”。  在右侧列中，选择包含在 Azure Stack 上运行的生成代理的代理队列。  
    ![显示如何配置发布定义以使用特定队列的图像](./media/azure-stack-solution-pipeline/image3.png)


## <a name="deploy-your-app-to-azure"></a>将应用部署到 Azure
此步骤使用新生成的 CI/CD 管道将 ASP.NET 应用部署到 Azure 上的 Web 应用。 

1.  从 VSTS 的标题中选择“生成和发布”，然后选择“生成”。
2.  在前面创建的生成定义中单击“...”，并选择“将新的生成排队”。
3.  接受默认值，单击“确定”。  生成随即开始，并显示进度。
4.  生成完成后，可以通过依次选择“生成和发布”、“发布”来跟踪状态。
5.  生成完成后，使用创建 Web 应用时记下的 URL 来访问网站。    


## <a name="add-azure-stack-to-pipeline"></a>将 Azure Stack 添加到管道
通过部署到 Azure 测试 CI/CD 管道后，可将 Azure Stack 添加到管道。  以下步骤创建一个新环境并添加 FTP 上传任务，以便将应用部署到 Azure Stack。  也可以添加一个发布审批者，以此模拟“签收”到 Azure Stack 的代码发布。  

1.  在发布定义中，选择“+ 添加环境”和“创建新环境”。
2.  选择“空白”，单击“下一步”。
3.  选择“特定用户”并指定自己的帐户。  选择“创建” 。
4.  选择现有名称并键入 *Azure Stack*，以重命名环境。
5.  现在，请选择 Azure Stack 环境，然后选择“添加任务”。
6.  依次选择“FTP 上传”任务、“添加”、“关闭”。


### <a name="configure-ftp-task"></a>配置 FTP 任务
创建发布后，请配置发布到 Azure Stack 上的 Web 应用所要执行的步骤。  就像为 Azure 配置“FTP 上传”任务一样，为 Azure Stack 配置相应的任务：

1.  选择刚刚添加的“FTP 上传”任务，并配置以下参数：
    
    | 参数 | 值 |
    | -----     | ----- |
    |身份验证方法| 输入凭据|
    |服务器 URL | 从 Azure Stack 门户检索到的 Web 应用 FTP URL |
    |用户名 | 创建 Web 应用的 FTP 凭据时配置的用户名 |
    |密码 | 创建 Web 应用的 FTP 凭据时创建的密码|
    |源目录 | $(System.DefaultWorkingDirectory)\**\ |
    |远程目录 | /site/wwwroot/|
    |保留的文件路径 | 已启用（已选中）|

2.  单击“保存” 

最后，使用以下步骤配置发布定义，以使用包含所部署的代理的代理池：
1.  选择发布定义，单击“编辑”
2.  在中间列中选择“在代理上运行”。 在右侧列中，选择包含在 Azure Stack 上运行的生成代理的代理队列。  
    ![显示如何配置发布定义以使用特定队列的图像](./media/azure-stack-solution-pipeline/image3.png)

## <a name="deploy-new-code"></a>部署新代码
现在可以测试混合 CI/CD 管道，最后发布到 Azure Stack。  在本部分，我们将修改站点的页脚，并通过管道开始部署。  完成后，将会看到更改已部署到 Azure 供审查；批准发布后，更改将发布到 Azure Stack。

1. 在 Visual Studio 中打开 *site.master* 文件，并将以下行：
    
    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application</p>
    `

    更改为：

    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application delivered by VSTS, Azure, and Azure Stack</p>
    `
3.  将更改提交并同步到 VSTS。  
4.  在 VSTS 工作区中，选择“生成和发布” > “生成”来检查生成状态
5.  将会看到生成正在进行。  双击该状态可以观察生成进度。  控制台中显示“已完成生成”后，请通过“生成和发布” > “发布”继续检查发布。  双击发布项。
6.  将会收到一条有关发布需要审查的通知。 检查 Web 应用 URL 并确认新的更改是否出现。  审批 VSTS 中的发布。
    ![显示如何配置发布定义以使用特定队列的图像](./media/azure-stack-solution-pipeline/image4.png)
    
7.  使用创建 Web 应用时记下的 URL 访问网站，以验证发布到 Azure Stack 的操作是否已完成。
    ![显示 ASP.NET 应用以及已更改的页脚的图像](./media/azure-stack-solution-pipeline/image5.png)


现在，可以使用新的混合 CI/CD 管道作为其他混合云模式的构建基块。

## <a name="next-steps"></a>后续步骤
本教程已介绍如何生成一个具有以下用途的混合 CI/CD 管道：

> [!div class="checklist"]
> * 根据 Visual Studio Team Services (VSTS) 存储库的代码提交启动新的生成。
> * 自动将新生成的代码部署到 Azure 进行用户验收测试。
> * 代码通过测试后，可自动部署到 Azure Stack。 

创建混合 CI/CD 管道后，请继续学习如何开发适用于 Azure Stack 的应用。

> [!div class="nextstepaction"]
> [Azure Stack 开发](azure-stack-developer.md)



