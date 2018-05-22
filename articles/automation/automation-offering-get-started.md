---
title: Azure 自动化入门 | Azure
description: 本文概述了 Azure 自动化服务，探讨了 Azure Marketplace 产品/服务的载入准备过程涉及的设计和实现细节。
services: automation
author: yunan2016
manager: digimobile
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/16/2018
ms.date: 05/14/2018
ms.author: v-nany
ms.openlocfilehash: fc2aaa5c4fe4361c1a719eb68cbb7a0fb357b317
ms.sourcegitcommit: 6f08b9a457d8e23cf3141b7b80423df6347b6a88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2018
---
# <a name="get-started-with-azure-automation"></a>Azure 自动化入门

本文介绍了与 Azure 自动化部署相关的核心概念。 不管是 Azure 自动化新手，还是已有自动化工作流软件（例如 System Center Orchestrator）使用经验，都可以了解如何准备和载入自动化。 阅读本文后便可开始开发 Runbook，以支持过程自动化需求。 


## <a name="automation-architecture-overview"></a>自动化体系结构概述

![Azure 自动化概述](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Azure 自动化是一种服务型软件 (SaaS) 应用程序，提供可缩放且可靠的多租户环境，用户可在该环境中使用 Runbook 实现过程自动化。 可在 Azure、其他云服务或本地环境中使用 Desired State Configuration (DSC) 管理对 Windows 和 Linux 系统的配置更改。 自动化帐户中的实体（包括 Runbook、资产和运行方式帐户）与订阅以及其他订阅中的其他自动化帐户相互隔离。  

Azure 中运行的 Runbook 在自动化沙盒中执行。 这些沙盒托管在 Azure 平台即服务 (PaaS) 型虚拟机中。 

自动化沙盒针对 Runbook 执行的所有方面（包括模块、存储、内存、网络通信和作业流）提供租户隔离。 此角色由服务管理。 不能从 Azure 或自动化帐户访问或管理此角色。         



## <a name="prerequisites"></a>先决条件

### <a name="automation-dsc"></a>自动化 DSC
Automation DSC 可用于管理以下计算机：

* 运行 Windows 或 Linux 的 Azure 虚拟机（经典）。
* 运行 Windows 或 Linux 的 Azure 虚拟机。
* 运行 Windows 或 Linux 的 Amazon Web Services (AWS) 虚拟机。
* 位于本地或者 Azure 或 AWS 以外的云中的物理和虚拟 Windows 计算机。
* 位于本地或者 Azure 或 AWS 以外的云中的物理和虚拟 Linux 计算机。

对于 Windows 计算机，必须安装最新版的 Windows Management Framework (WMF) 5。 对于 Linux 计算机，必须安装最新版的 [PowerShell DSC agent for Linux](https://www.microsoft.com/en-us/download/details.aspx?id=49150)。 PowerShell DSC 代理使用 WMF 5 与自动化通信。 

### <a name="hybrid-runbook-worker"></a>混合 Runbook 辅助角色  
指定某台计算机运行混合 Runbook 作业时，该计算机必须满足以下先决条件：

* Windows Server 2012 或更高版本。
* Windows PowerShell 4.0 或更高版本。 为了提高可靠性，建议安装 Windows PowerShell 5.0。 可以从 Microsoft 下载中心[下载新版本](https://www.microsoft.com/download/details.aspx?id=50395)。
* .NET Framework 4.6.2 或更高版本。
* 至少双核。
* 至少 4 GB RAM。

### <a name="permissions-required-to-create-an-automation-account"></a>创建自动化帐户所需的权限
若要创建或更新自动化帐户，并完成本文所述的任务，必须具有以下特权和权限：   
 
* 若要创建自动化帐户，必须将 Azure Active Directory (Azure AD) 用户帐户添加到一个角色，该角色的权限相当于 **Microsoft.Automation** 资源的所有者角色。 有关详细信息，请参阅 [Azure 自动化中基于角色的访问控制](automation-role-based-access-control.md)。  
* 在 Azure 门户的“Azure Active Directory” > “管理” > “应用注册”下，如果“应用注册”设置为“是”，则 Azure AD 租户中的非管理员用户可以[注册 Active Directory 应用程序](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions)。 如果“应用注册”设置为“否”，则执行此操作的用户必须是 Azure AD 中的全局管理员。 

如果在被添加到订阅的全局管理员/共同管理员角色之前不是订阅的 Active Directory 实例的成员，则将作为来宾添加到 Active Directory。 在这种情况下，“添加自动化帐户”页中会显示此消息：“你无权创建”。 

如果用户已被添加到全局管理员/共同管理员角色，可以先将其从订阅的 Active Directory 实例中删除，然后重新添加到 Active Directory 中的完整用户角色。

若要验证用户角色，请执行以下操作：
1. 在 Azure 门户中，转到“Azure Active Directory”窗格。
2. 选择“用户和组”。
3. 选择“所有用户”。 
4. 选择特定的用户后，选择“配置文件”。 用户配置文件下的“用户类型”属性值不应为“来宾”。

## <a name="authentication-planning"></a>身份验证规划
在 Azure 自动化中，可以针对 Azure 资源、本地资源以及其他云服务资源自动执行任务。 为了使 Runbook 执行所需操作，Runbook 必须有权安全地访问资源。 它必须具有订阅中所需的最小权限。  

### <a name="what-is-an-automation-account"></a>什么是自动化帐户 
所有使用 Azure 自动化中的 cmdlet 针对资源执行的自动化任务在向 Azure 进行身份验证时，都使用基于 Azure AD 组织标识凭据的身份验证。  自动化帐户独立于用来登录到门户，对 Azure 资源进行配置和使用的帐户。 

自动化帐户附带以下资源：

* **证书**。 包含用于从 Runbook 或 DSC 配置进行身份验证的证书。 还可以添加证书。
* **连接**。 包含从 Runbook 或 DSC 配置连接到外部服务或应用程序所需的身份验证和配置信息。
* **凭据**。 包含 **PSCredential** 对象，该对象包含用户名和密码等安全凭据。 从 Runbook 或 DSC 配置进行身份验证时需要这些凭据。
* **集成模块**。 Azure 自动化帐户中包含的 PowerShell 模块。 PowerShell 模块用于在 Runbook 和 DSC 配置中运行 cmdlet。
* **计划**。 包含在指定时间启动或停止 Runbook（包括重复频率）的计划。
* **变量**。 包含来自于 Runbook 或 DSC 配置的值。
* **DSC 配置**。 属于 PowerShell 脚本，说明如何配置操作系统功能或设置，或者如何在 Windows 或 Linux 计算机上安装应用程序。  
* **Runbook**。 基于 Windows PowerShell 在自动化中执行自动化过程的一组任务。    

每个自动化帐户的自动化资源都与单个 Azure 区域相关联。 但是，可以使用自动化帐户管理订阅中的所有资源。 如果策略要求将数据和资源隔离到特定的区域，请在不同区域中创建自动化帐户。

在 Azure 门户中创建自动化帐户时，会自动创建两个身份验证实体：

* **运行方式帐户**。 此帐户执行以下任务：
  - 在 Azure AD 中创建服务主体。
  - 创建证书。
  - 向参与者分配基于角色的访问控制 (RBAC)，以便使用 Runbook 管理 Azure 资源管理器资源。
* **经典运行方式帐户**。 此帐户会上传一个管理证书。 该证书用于通过 Runbook 管理经典资源。

可在资源管理器中使用 RBAC 向 Azure AD 用户帐户和运行方式帐户授予允许的操作。 RBAC 还可用于对该服务主体进行身份验证。 有关详细信息以及如何帮助开发用于管理自动化权限的模型，请参阅 [Azure 自动化中基于角色的访问控制](automation-role-based-access-control.md)一文。  

#### <a name="authentication-methods"></a>身份验证方法
下表总结了 Azure 自动化所支持的可用于每个环境的身份验证方法。

| 方法 | 环境 
| --- | --- | 
| Azure 运行方式帐户和经典运行方式帐户 |Azure 资源管理器部署和 Azure 经典部署 |  
| Azure AD 用户帐户 |Azure 资源管理器部署和 Azure 经典部署 |  
| Windows 身份验证 |使用混合 Runbook 辅助角色的本地数据中心或其他云服务提供程序 |  

以下文章概述了如何为这些环境配置身份验证，并提供了相应的实现步骤。 它们介绍了如何使用专用于该环境的现有帐户和新帐户。 
* [创建独立的 Azure 自动化帐户](automation-create-standalone-account.md)
* [使用 Azure 经典部署和资源管理器部署对 Runbook 进行身份验证](automation-create-aduser-account.md)
* [更新自动化运行方式帐户](automation-create-runas-account.md)

对于 Azure 运行方式帐户和经典运行方式帐户，[更新自动化运行方式帐户](automation-create-runas-account.md)介绍了如何从门户使用运行方式帐户更新现有的自动化帐户。 它还介绍了如何在最初没有为自动化帐户配置运行方式帐户或经典运行方式帐户的情况下使用 PowerShell。 可以使用企业证书颁发机构 (CA) 颁发的证书创建运行方式帐户和经典运行方式帐户。 请查看[更新自动化运行方式帐户](automation-create-runas-account.md)，了解如何使用此配置创建帐户。     
 
## <a name="next-steps"></a>后续步骤
* 验证新的自动化帐户能否对 Azure 资源进行身份验证。 有关详细信息，请参阅[测试 Azure 自动化运行方式帐户身份验证](automation-verify-runas-authentication.md)。
* 在开始创作前，先了解如何开始创建 Runbook 和相关注意事项。 有关详细信息，请参阅[自动化 Runbook 类型](automation-runbook-types.md)。


