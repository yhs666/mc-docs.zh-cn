---
title: "Azure 自动化概述 | Microsoft Docs"
description: "了解如何使用 Azure 自动化自动完成基础结构和应用程序的生命周期。"
services: automation
author: yunan2016
documentationcenter: 
keywords: "azure 自动化, DSC, powershell, desired state configuration, 更新管理, 更改跟踪, 清单, runbook, python, 图形"
ms.assetid: 0cf1f3e8-dd30-4f33-b52a-e148e97802a9
ms.service: automation
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 12/13/2017
ms.date: 01/17/2018
ms.author: v-nany
ms.custom: mvc
ms.openlocfilehash: 2a6cde27eb07a8a1a088e3e3fa4afae0097baba5
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="an-introduction-to-azure-automation"></a>Azure 自动化简介

Azure 自动化提供基于云的自动化和配置服务，用于跨 Azure 环境和非 Azure 环境进行一致的管理。 它包含流程自动化、更新管理和配置功能。 可以通过 Azure 自动化在工作负荷和资源的部署、操作和解除授权过程中进行完全的控制。
本文概述了 Azure 自动化并回答了一些常见问题。 有关不同功能的详细信息，请访问本概述中提供的链接。

## <a name="azure-automation-capabilities"></a>Azure 自动化功能

![自动化功能概述](./media/automation-overview/automation-overview.png)

### <a name="process-automation"></a>流程自动化

可以通过 Azure 自动化自动完成频繁进行的、耗时的、易出错的云管理任务。 有了这样的自动化，你就可以专注于能够让业务增值的工作。 还可以通过自动化来减少错误和提升效率，从而降低运营成本。 可以将 Azure 服务和其他公用系统集成，这些系统是部署、配置和管理端到端流程所必需的。 可以在 PowerShell 或 Python 中使用此服务以图形方式[创作 Runbook](automation-runbook-types.md)。 可以使用混合 Runbook 辅助角色跨本地环境进行协调，实现统一管理。 可以通过 [Webhook](automation-webhooks.md) 从 ITSM、DevOps 和监视系统触发自动化，从而满足相关请求并确保持续交付和操作。


### <a name="shared-capabilities"></a>共享功能

Azure 自动化包含一组共享资源，方便用户大规模地完成环境的自动化操作和配置。

* **[基于角色的访问控制](automation-role-based-access-control.md)** - 通过自动化操作员角色控制帐户访问权限，这样就可以在不提供创作功能的情况下运行任务。
* **[变量](automation-variables.md)** - 通过变量来保存那些可以跨 Runbook 和配置使用的内容。 可以更改值而不需修改引用这些值的 Runbook 和配置。
* **[凭据](automation-credentials.md)** - 安全地存储可供 Runbook 和配置在运行时使用的敏感信息。
* **[证书](automation-certificates.md)** - 存储证书，使之在运行时可供用于身份验证，确保已部署资源的安全。
* **[连接](automation-connections.md)** - 以名称/值对的形式存储信息。在连接资源中连接到系统时，需要使用其中包含的常用信息。 连接由模块作者定义，在运行时的 Runbook 和配置中使用。
* **[计划](automation-schedules.md)** - 用在服务中，在预定义的时间触发自动化。
* **[PowerShell 模块](automation-integration-modules.md)** -  可以使用模块来管理 Azure 和其他系统。 请将其导入到适用于 Microsoft、第三方、社区或自定义 cmdlet 和 DSC 资源的自动化帐户中。

### <a name="windows-and-linux"></a>Windows 和 Linux

根据设计，Azure 自动化可以在混合云环境中使用，且同时适用于 Windows 和 Linux。 可以通过它对部署的工作负荷及其运行时所依赖的操作系统进行一致的自动化操作和配置。

### <a name="community-gallery"></a>社区库

以浏览方式查找[自动化库](automation-runbook-gallery.md)中的 Runbook 和模块，以便通过 PowerShell 库和 Microsoft 脚本中心快速启动流程的集成和创作。

## <a name="common-scenarios-for-automation"></a>常用自动化方案

Azure 自动化可以在基础结构和应用程序的整个生命周期中进行管理。 可以将有关组织如何交付和维护工作负荷的知识传输到系统中； 可以使用 PowerShell、Desired State Configuration、Python、图形 Runbook 等常用语言进行创作； 可以获取已部署资源的完整清单，以便进行针对性操作、完成相关报告并了解符合性情况； 确定哪些更改可能导致配置错误，哪些更改可以改进操作符合性。

* **生成/部署资源** - 使用 Runbook 和 Azure 资源管理器模板在混合环境中部署 VM。 可以集成到 Jenkins 和 Visual Studio Team Services 之类的开发工具中。
* **配置 VM** - 使用基础结构和应用程序所需的配置评估和配置 Windows 和 Linux 计算机。
* **监视** - 确定计算机上那些导致问题的更改，进行相应的补救，或者将其升级到管理系统。
* **保护** - 在已引发安全警报的情况下隔离 VM。 设置来宾内要求。
* **管控** - 为团队设置基于角色的访问控制。 恢复未使用的资源。

## <a name="pricing-for-automation"></a>自动化定价

可以在[定价](https://www.azure.cn/pricing/details/automation/)页查看 Azure 自动化的价格。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [创建自动化帐户](automation-quickstart-create-account.md)

