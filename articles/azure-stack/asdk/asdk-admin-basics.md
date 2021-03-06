---
title: ASDK 管理基础知识 | Microsoft Docs
description: 了解如何对 Azure Stack 开发工具包 (ASDK) 执行基本管理任务。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/19/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: e1ec727db5f85297b9bc54e05d8124193d388ab1
ms.sourcegitcommit: 7dfb76297ac195e57bd8d444df89c0877888fdb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020077"
---
# <a name="asdk-admin-basics"></a>ASDK 管理基础知识
如果你不熟悉 Azure Stack 开发工具包 (ASDK) 的管理，请参考本文。 本指南概述评估环境中 Azure Stack 操作员的角色。 熟悉此信息可确保测试用户能够尽快提高工作效率。

首先请查看[什么是 Azure Stack 开发工具包？](asdk-what-is.md)一文，确保了解 ASDK 的用途和限制。 应该将开发工具包作为“沙盒”使用，在其中对 Azure Stack 进行评估，并在非生产环境中开发和测试应用。 

与 Azure 类似，Azure Stack 会快速推出创新，因此我们会定期发布 ASDK 的新版本。 但是，无法像升级 Azure Stack 集成系统部署一样升级 ASDK。 因此，若要过渡到最新版本，必须彻底[重新部署 ASDK](asdk-redeploy.md)。 不能应用更新包。 此过程需要一定的时间，但好处是可以在最新功能可用时立即试用。 

## <a name="what-account-should-i-use"></a>我应使用什么帐户?
管理 Azure Stack 时，应该注意帐户方面的几个事项， 在使用 Windows Server Active Directory 联合身份验证服务 (AD FS) 而不是 Azure Active Directory (Azure AD) 作为标识提供者的部署中尤其如此。 以下帐户注意事项同时适用于 Azure Stack 集成系统和 ASDK 部署：

|帐户|Azure AD|AD FS|
|-----|-----|-----|
|本地管理员 (.\Administrator)|ASDK 主机管理员|ASDK 主机管理员|
|AzureStack\AzureStackAdmin|ASDK 主机管理员<br><br>可用于登录到 Azure Stack 管理员门户<br><br>拥有查看和管理 Service Fabric 环的访问权限|ASDK 主机管理员<br><br>无权访问 Azure Stack 管理员门户<br><br>拥有查看和管理 Service Fabric 环的访问权限<br><br>不再是默认提供程序订阅 (DPS) 的所有者|
|AzureStack\CloudAdmin|可在特权终结点中访问和运行允许的命令|可在特权终结点中访问和运行允许的命令<br><br>无法登录到 ASDK 主机<br><br>默认提供程序订阅 (DPS) 的所有者|
|Azure AD 全局管理员|安装期间使用<br><br>默认提供程序订阅 (DPS) 的所有者|不适用|
|

## <a name="what-tools-do-i-use-to-manage"></a>使用哪些工具进行管理？
可以使用 [Azure Stack 管理员门户](https://adminportal.local.azurestack.external)或 PowerShell 来管理 Azure Stack。 若要了解基本概念，最简单的方式是使用门户。 若要使用 PowerShell，需要安装[适用于 Azure Stack 的 PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) 并[从 GitHub 下载 Azure Stack 工具](asdk-post-deploy.md#download-the-azure-stack-tools)。

Azure Stack 使用 Azure 资源管理器作为其基础的部署、管理和组织机制。 若要管理 Azure Stack 并帮助支持用户，应了解 Azure 资源管理器。 有关详细信息，请参阅 [Azure 资源管理器入门白皮书](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf)。

## <a name="your-typical-responsibilities"></a>典型责任
用户需要使用服务， 从其角度来看，你的主要角色是向他们提供这些服务。 可以使用 ASDK 来了解需要提供哪些服务，以及如何通过[创建计划、套餐和配额](../operator/azure-stack-tutorial-tenant-vm.md)来提供这些服务。 此外还需向市场添加项，例如虚拟机 (VM) 映像。 最简单的方法是[将市场项从 Azure 下载到 Azure Stack](../operator/azure-stack-create-and-publish-marketplace-item.md)。

> [!NOTE]
> 如需测试计划、套餐和服务，应使用[用户门户](https://portal.local.azurestack.external)而不是[管理员门户](https://adminportal.local.azurestack.external)。

除了提供服务外，你还必须负责 Azure Stack 操作员的所有常规职责，使 ASDK 保持启动且正在运行。 这些职责包括以下事项：
- 为 Azure AD 或 AD FS 部署添加用户帐户。
- 分配基于角色的访问控制 (RBAC) 角色（这不仅限于管理员）。
- 监视基础结构运行状况。
- 管理网络和存储资源。
- 更换有故障的开发工具包主计算机硬件。

## <a name="where-to-get-support"></a>从何处获取支持
对于 ASDK，可以在 [Azure Stack MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home?sort=relevancedesc&brandIgnore=True&searchTerm=Azure+Stack)中提出与支持相关的问题。 

还可以通过单击管理员门户右上角的“帮助”  (问号) 来访问论坛。 然后单击“帮助 + 支持”  打开“帮助 + 支持”**概述**，其中包含指向论坛的链接。 我们会定期监视 MSDN 论坛。  

> [!IMPORTANT]
> 由于 ASDK 是一个评估环境，因此我们不会通过 Azure 客户支持服务 (CSS) 提供官方支持。

## <a name="next-steps"></a>后续步骤
[部署 ASDK](asdk-install.md)

