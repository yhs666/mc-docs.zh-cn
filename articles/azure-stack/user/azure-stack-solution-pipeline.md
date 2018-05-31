---
title: 将应用部署到 Azure 和 Azure Stack | Microsoft Docs
description: 了解如何使用混合 CI/CD 管道将应用部署到 Azure 和 Azure Stack。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
origin.date: 05/15/2018
ms.date: 05/23/2018
ms.author: v-junlch
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 47f9331d6936949050ccf3a10be06839050b59e2
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475077"
---
# <a name="tutorial-deploy-apps-to-azure-and-azure-stack"></a>教程：将应用部署到 Azure 和 Azure Stack

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

使用混合持续集成/持续交付 (CI/CD) 管道可以生成、测试应用并将其部署到多个云中。  在本教程中，我们将构建一个示例环境来完成以下任务：
 
> [!div class="checklist"]
> * 根据 Visual Studio Team Services (VSTS) 存储库的代码提交启动新的生成。
> * 自动将新生成的代码部署到 Azure 公有云进行用户验收测试。
> * 代码通过测试后，可自动部署到 Azure Stack。

### <a name="about-the-hybrid-delivery-build-pipe"></a>关于混合交付生成管道

应用程序部署持续性、安全性和可靠性对于组织和开发团队而言至关重要。 使用混合 CI/CD 管道，可跨本地环境与公有云整合管道。 无需切换应用程序即可直接更改位置。

使用此方法还能维护一致的开发工具集。 跨 Azure 公有云与本地 Azure Stack 环境维护一致的工具意味着执行 CI/CD 开发实践的难度会大大降低。 在 Azure 或 Azure Stack 中部署的应用和服务可以互换，相同的代码可在任一位置运行，并可利用本地和公有云的特性与功能。

了解有关以下方面的详细信息：
 - [什么是持续集成？](https://www.visualstudio.com/learn/what-is-continuous-integration/)
 - [什么是持续交付？](https://www.visualstudio.com/learn/what-is-continuous-delivery/)


## <a name="prerequisites"></a>先决条件

需要准备好几个组件才能生成混合 CI/CD 管道。 准备这些组件需要一段时间。
 
 - Azure OEM/硬件合作伙伴可以部署生产 Azure Stack，所有用户都可以部署 Azure Stack 开发工具包 (ASDK)。 
 - 此外，Azure Stack 操作员必须部署应用服务、创建计划和产品/服务、创建租户订阅，并添加 Windows Server 2016 映像。

如果已准备好其中的某些组件，请确保它们符合要求，然后开始操作。

本主题还假设你对 Azure 和 Azure Stack 有一定的了解。 若要在了解详情之后再继续，请务必从以下主题着手：


本教程还假设读者对 Azure 和 Azure Stack 有一定的了解。 

若要在了解详情之后再继续，可从以下主题着手：
 - [Azure 简介](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure Stack 的重要概念](/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure

 - 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

 - 在 Azure 中创建 [Web 应用](/app-service/app-service-web-overview)。 记下新 Web 应用的 URL，因为稍后需要用到。

Azure Stack
 - 使用 Azure Stack 集成系统，或参考以下链接部署 Azure Stack 开发工具包 (ASDK)：
    - 可在[教程：使用安装程序部署 ASDK](/azure-stack/asdk/asdk-deploy) 中找到有关部署 ASDK 的详细说明
    - 可以使用 PowerShell 脚本 [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ) 将许多的 ASDK 部署后步骤自动化。

    > [!Note]  
    > ASDK 安装过程需要七个小时才能完成，因此请相应地做好规划。

 - 将[应用服务](/azure-stack/azure-stack-app-service-deploy) PaaS 服务部署到 Azure Stack。 
 - 在 Azure Stack 环境中创建[计划/产品/服务](/azure-stack/azure-stack-plan-offer-quota-overview)。 
 - 在 Azure Stack 环境中创建[租户订阅](/azure-stack/azure-stack-subscribe-plan-provision-vm)。 
 - 在租户订阅中创建 Web 应用。 记下新 Web 应用的 URL，供稍后使用。
 - 部署 VSTS 虚拟机（仍在租户订阅中）。
 - 需要装有 .NET 3.5 的 Windows Server 2016 VM。 将在 Azure Stack 上生成此 VM，作为专用的生成代理。 

### <a name="developer-tools"></a>开发人员工具

 - 创建 [VSTS 工作区](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)。 注册过程将创建名为 **MyFirstProject** 的项目。
 - [安装 Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) 并[登录到 VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services)。
 - 连接到项目并在[本地克隆](https://www.visualstudio.com/docs/git/gitquickstart)。
 
 > [!Note]  
 > 需要在 Azure Stack 上创建适当的合成映像用于运行 Windows Server 和 SQL，并需要部署应用服务。
 
## <a name="prepare-the-private-build-and-release-agent-for-visual-studio-team-services-integration"></a>准备用于 Visual Studio Team Services 集成的专用生成和发布代理

### <a name="prerequisites"></a>先决条件

Visual Studio Team Services (VSTS) 使用服务主体对 Azure 资源管理器进行身份验证。 VSTS 需处于“参与者”状态才能在 Azure Stack 订阅中预配资源。

下面是为了启用此类身份验证而需要配置的概要步骤：

1. 应创建服务主体，或使用现有的服务主体。
2. 需要创建服务主体的身份验证密钥。
3. 需要通过基于角色的访问控制来验证 Azure Stack 订阅，使 SPN 能够成为“参与者”角色的一部分。
4. 必须使用 Azure Stack 终结点和 SPN 信息在 VSTS 中创建新的服务定义。

### <a name="service-principal-creation"></a>创建服务主体

请参阅[创建服务主体](/active-directory/develop/active-directory-integrating-applications)中的说明来创建服务主体，并为“应用程序类型”选择“Web 应用/API”。

### <a name="access-key-creation"></a>创建访问密钥

服务主体需要使用密钥进行身份验证，请遵循本部分中的步骤生成密钥。


1. 从 Azure Active Directory 中的“应用注册”，选择应用程序。

    ![替换文字](./media/azure-stack-solution-hybrid-pipeline/000_01.png)

2.  记下“应用程序 ID”的值。 在 VSTS 中配置服务终结点时，将要使用该值。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_02.png)

3. 若要生成身份验证密钥，请选择“设置”。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_03.png)

4. 若要生成身份验证密钥，请选择“密钥” 。
 
    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_04.png)

5. 提供密钥说明和密钥持续时间。 完成后，选择“保存” 。
 
    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_05.png)

    保存密钥后，密钥的值显示。 复制此值，因为稍后不能检索密钥。 提供用于登录到应用程序的**密钥值**和应用程序 ID。 将密钥值存储在应用程序可检索的位置。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_06.png)

### <a name="get-tenant-id"></a>获取租户 ID

在配置服务终结点的过程中，VSTS 要求提供与 Azure Stack 堆栈所部署到的 AAD 目录相对应的**租户 ID**。 请遵循本部分所述的步骤收集租户 ID。

1. 选择“Azure Active Directory” 。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_07.png)

2. 若要获取租户 ID，请选择 Azure AD 租户的“属性”。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_08.png)
 
3. 复制“目录 ID” 。 此值即为租户 ID。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>授予在 Azure Stack 订阅中部署资源的服务主体权限 

要访问订阅中的资源，必须将应用程序分配到角色。 决定哪个角色表示应用程序的相应权限。 若要了解有关可用角色的信息，请参阅 [RBAC：内置角色](/role-based-access-control/built-in-roles)。

可将作用域设置为订阅、资源组或资源级别。 较低级别的作用域会继承权限。 例如，将某个应用程序添加到资源组的“读取者”角色意味着该应用程序可以读取该资源组及其包含的所有资源。

1. 导航到要将应用程序分配到的作用域级别。 例如，若要在订阅范围内分配角色，选择“订阅” 。 可改为选择资源组或资源。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_10.png)

2. 选择要将应用程序分配到的**订阅**（资源组或资源）。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_11.png)

3. 选择“访问控制(IAM)”。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_12.png)

4. 选择“设置” （应用程序对象和服务主体对象）。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_13.png)

5. 选择要分配到应用程序的角色。 下图显示了“所有者”角色。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_14.png)

6. 默认情况下，可用选项中不显示 Azure Active Directory 应用程序。 若要查找应用程序，必须在搜索字段中**提供名称**。 选择它。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_16.png)

7. 选择“保存”完成角色分配。 该应用程序会显示在分配到该范围角色的用户列表中。

### <a name="role-based-access-control"></a>基于角色的访问控制

Azure 基于角色的访问控制 (RBAC) 可用于对 Azure 进行细致的访问管理。 使用 RBAC，可以仅授予用户执行其作业所需的访问次数。 有关基于角色的访问控制的详细信息，请参阅[管理对 Azure 订阅资源的访问](/role-based-access-control/role-assignments-portal)。

### <a name="vsts-agent-pools"></a>VSTS 代理池

可在代理池中组织代理，而无需单独管理每个代理。 代理池定义该池中所有代理的共享边界。 在 VSTS 中，代理池的范围限定为 VSTS 帐户；因此，可在团队项目之间共享一个代理池。 有关如何创建 VSTS 代理池的详细信息和教程，请参阅[创建代理池和队列](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts)。

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>添加 Azure Stack 的个人访问令牌 (PAT)

1. 登录到 VSTS 帐户，并选择帐户配置文件名称。
2. 选择“管理安全性”以访问令牌创建页。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_17.png)

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_18.png)

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_18a.png)

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_18b.png)

3. 复制令牌。
    
    > [!Note]  
    > 获取令牌信息。 退出此屏幕后不再会显示此信息。 
    
    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_19.png)
    

### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>在 Azure Stack 托管的生成服务器上安装 VSTS 生成代理

1. 连接到 Azure Stack 主机上部署的生成服务器。

2. 下载生成代理，并使用个人访问令牌 (PAT) 将其部署为服务，然后以 VM 管理员帐户运行。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\010_downloadagent.png)

3. 转到提取的生成代理文件夹。 从特权提升的命令提示符运行 **run.cmd** 文件。 

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_20.png)

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\000_21.png)

4.  run.cmd 完成运行后，包含提取内容的文件夹应如下所示：

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\009_token_file.png)

    现在可以在 VSTS 文件夹中看到该代理。

## <a name="endpoint-creation-permissions"></a>终结点创建权限

用户可以创建终结点，使 VSTO 生成能够将 Azure 服务应用部署到堆栈。 VSTS 会连接到生成代理，而后者会连接到 Azure Stack。 

![替换文字](media\azure-stack-solution-hybrid-pipeline\012_securityendpoints.png)

1. 在“设置”菜单中，选择“安全性”。
2. 在左侧的“VSTS 组”列表中，选择“终结点创建者”。 

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\013_endpoint_creators.png)

3. 在“成员”选项卡上，选择“+ 添加”。 

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\014_members_tab.png)

4. 键入用户名，然后从列表中选择该用户。
5. 单击“保存更改”。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\015_save_endpoint.png)

6. 在左侧的“VSTS 组”列表中，选择“终结点管理员”。
7. 在“成员”选项卡上，选择“+ 添加”。
8. 键入用户名，然后从列表中选择该用户。
9. 单击“保存更改”。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\016_save_changes.png)

    Azure Stack 中的生成代理会从 VSTS 获取指令，然后，VSTS 会传达与 Azure Stack 通信所需的终结点信息。 
    
    从 VSTS 到 Azure Stack 的连接现已准备就绪。

## <a name="develop-your-application"></a>开发应用程序

设置混合 CI/CD，以将 Web 应用部署到 Azure 和 Azure Stack，并自动将更改推送到这两个云中。

> [!Note]  
> 需要在 Azure Stack 上创建适当的合成映像用于运行 Windows Server 和 SQL，并需要部署应用服务。 查看应用服务文档的“先决条件”部分，了解 Azure Stack 操作员的要求。

### <a name="add-code-to-vsts-project"></a>将代码添加到 VSTS 项目

1. 使用在 Azure Stack 上拥有项目创建权限的帐户登录到 Visual Studio。

    混合 CI/CD 可同时应用到应用程序代码和基础结构代码。 对这两个云使用 [Azure 资源管理器模板](https://azure.microsoft.com/resources/templates/)（例如 VSTS 中的 Web 应用代码）。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\017_connect_to_project.png)

2. 创建并打开默认 Web 应用以**克隆存储库**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>为这两个云中的应用服务创建独立的 Web 应用部署

1. 编辑 **WebApplication.csproj** 文件：选择 **Runtimeidentifier** 并添加 `win10-x64.`。有关详细信息，请参阅[独立部署](https://docs.microsoft.com/zh-cn/dotnet/core/deploying/#self-contained-deployments-scd)文档。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\019_runtimeidentifer.png)

2. 使用团队资源管理器将代码签入 VSTS。

3. 确认应用程序代码已签入 Visual Studio Team Services。 

### <a name="create-the-build-definition"></a>创建生成定义

1. 登录到 VSTS 以确认能够创建生成定义。

2. 添加 **-r win10-x64** 代码。 在 .Net Core 中触发独立部署时需要此代码。 

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\020_publish_additions.png)

3. 运行生成。 [独立部署生成](https://docs.microsoft.com/zh-cn/dotnet/core/deploying/#self-contained-deployments-scd)过程将发布可在 Azure 和 Azure Stack 上运行的项目。

### <a name="using-an-azure-hosted-agent"></a>使用 Azure 托管代理

在 VSTS 中使用托管代理是生成和部署 Web 应用的便捷做法。 Azure 会自动执行维护和升级，可实现持续不间断的开发、测试和部署。

### <a name="manage-and-configure-the-continuous-deployment-cd-process"></a>管理和配置持续部署 (CD) 过程

Visual Studio Team Services (VSTS) 和 Team Foundation Server (TFS) 提供高度可配置、可管理的管道用于发布到多个环境（例如开发、过渡、QA 和生产环境）；在特定的阶段要求审批。

### <a name="create-release-definition"></a>创建发布定义

![替换文字](media\azure-stack-solution-hybrid-pipeline\021a_releasedef.png)

1. 在 VSO 的“生成和发布”页的“发布”选项卡下，选择“\[ + ]”以添加新的发布。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\102.png)

2. 应用“Azure 应用服务部署”模板。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\103.png)

3. 在“添加项目”下拉菜单中，为 Azure 云生成应用**添加项目**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\104.png)

4. 在“管道”选项卡下选择“阶段”、环境的“任务”链接，并设置 Azure 云环境值。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\105.png)

5. 设置**环境名称**，并选择 Azure 云终结点的 Azure **订阅**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\106.png)

6. 在“环境名称”下，设置所需的 **Azure 应用服务名称**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\107.png)

7. 在 Azure 云托管环境的“代理队列”下输入 **Hosted VS2017**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\108.png)

8. 在“部署 Azure 应用服务”菜单中，为环境选择有效的**包或文件夹**。 选择**文件夹位置**旁边的“确定”。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\109.png)

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\110.png)

9. 保存所有更改并返回**发布管道**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\111.png)

10. 选择 Azure Stack 应用的生成以添加**新项目**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\112.png)

11. 额外添加一个应用 **Azure 应用服务部署**的环境。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\113.png)

12. 将新环境命名为 **Azure Stack**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\114.png)

13. 在“任务”选项卡下找到 Azure Stack 环境。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\115.png)

14. 选择 Azure Stack 终结点的**订阅**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\116.png)

15. 将 Azure Stack Web 应用名称设置为**应用服务名称**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\117.png)

16. 选择“Azure Stack 代理”。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\118.png)

17. 在“部署 Azure 应用服务”部分下，为环境选择有效的**包或文件夹**。 选择**文件夹位置**旁边的“确定”。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\119.png)

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\120.png)

18. 在“变量”选项卡下添加名为 **VSTS_ARM_REST_IGNORE_SSL_ERRORS** 的变量，并将其值设置为 **true**，将范围设置为 **Azure Stack**。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\121.png)

19. 选择两个项目中的“持续”部署触发器图标，并启用持续部署触发器。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\122.png)

20. 选择 Azure Stack 环境中的“部署前”条件图标，并将触发器设置为“发布后”。

21. 保存所有更改。

> [!Note]  
> 任务的某些设置可能已在从模板创建发布定义时自动定义为[环境变量](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables)。 无法在任务设置中修改这些设置；必须选择父环境项才能编辑这些设置。

## <a name="create-a-release"></a>创建发布

完成对发布定义的修改后，可以开始部署。 为此，请从发布定义创建发布。 可以自动创建发布；例如，发布定义中已设置持续部署触发器。 这意味着，修改源代码会启动新的生成，继而生成新的发布。 但在本部分，我们将手动创建新发布。

1. 打开“发布”下拉列表，并选择“创建发布”。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\200.png)
 
2. 输入发布的说明，检查是否选择了正确的项目，然后选择“创建”。 片刻之后，将会出现一个横幅，指出已创建新的发布。 选择链接（发布的名称）。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\201.png)
 
3. 此时会打开发布摘要页，其中显示了发布详细信息。 在“环境”部分，可以看到“QA”环境的部署状态已从“正在进行”更改为“成功”，并且此时出现了一个横幅，指出发布正在等待审批。 如果部署到环境的过程挂起或失败，将会显示一个蓝色的 (i) 信息图标。 指向该图标会显示一个包含原因的弹出窗口。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\202.png)

其他视图（例如发布列表）也会显示一个图标，指示正在等待审批。 指向该图标时，会显示一个包含环境名称和更多详细信息的弹出窗口。 管理员可以凭此轻松查看哪些发布正在等待审批，以及所有发布的总体进度。

### <a name="monitor-and-track-deployments"></a>监视和跟踪部署

本部分介绍如何通过上一部分创建的发布来监视和跟踪部署（在本示例中为两个 Azure 应用服务网站）。

1. 在发布摘要页中，选择“日志”链接。 在部署过程中，此页会显示来自代理的实时日志，左窗格会指示每个环境的部署过程中每项操作的状态。

    选择部署前或部署后审批活动的“操作”列中的图标，查看部署批准者（或拒绝者）的详细信息，以及该用户提供的消息。

2. 部署完成后，整个日志文件会显示在右窗格中。 在左窗格中选择任一**处理步骤**只会显示该步骤的日志文件内容。 这样，便可以更轻松地跟踪和调试整体部署的各个组成部分。 或者，可以通过页面中的图标和链接下载单个日志文件或包含所有日志文件的 zip 文件。

    ![替换文字](media\azure-stack-solution-hybrid-pipeline\203.png)
 
3. 打开“摘要”选项卡，查看发布的总体详细信息。 其中显示了该发布所部署到的生成和环境，以及部署状态和有关发布的其他信息。

4. 选择每个**环境链接**，查看与该特定环境的现有和挂起部署相关的详细信息。 可以使用这些页面来验证同一个生成是否已部署到这两个环境。

5. 在浏览器中打开**已部署的生产应用**。 例如，对于 Azure 应用服务网站，请打开 URL `http://[your-app-name].chinacloudsites.cn`。

<!-- Update_Description: wording update -->
