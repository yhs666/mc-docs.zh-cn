---
title: 更新 Azure 自动化中的 Azure 模块
description: 本文介绍现在如何才能更新在 Azure 自动化中默认提供的常用 Azure PowerShell 模块。
services: automation
ms.service: automation
ms.component: process-automation
author: WenJason
ms.author: v-jay
origin.date: 09/19/2018
ms.date: 11/05/2018
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: a05b45423dbe7232cb6d1bf942a4dc5a5fb8ad46
ms.sourcegitcommit: d26e5d0d625a61d6b130800d10c81f47c83fb1e0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "50745503"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>如何在 Azure 自动化中更新 Azure PowerShell 模块

默认情况下，每个自动化帐户中提供最常用的 Azure PowerShell 模块。 Azure 团队会定期更新 Azure 模块，因此在自动化帐户中，我们提供了一种方法，用于在门户中有新版本时更新帐户中的模块。

由于模块由产品组定期更新，所包含的 cmdlet 可能会发生更改，这可能会对 runbook 产生负面影响，具体要取决于更改类型（如重命名参数或完全弃用 cmdlet）。 为了避免影响 runbook 及其自动化过程，建议在继续操作之前进行测试和验证。 如果没有用于此目的专用自动化帐户，请考虑创建一个自动化帐户，以便可以在 runbook 开发期间测试许多不同的方案和排列，以及更新 PowerShell 模块等迭代更改。 在验证结果并且应用了所需的任何更改之后，请继续协调需要修改的任何 runbook 的迁移，然后按照以下生产环境中的描述执行更新。

> [!NOTE]
> 新自动化帐户可能不包含最新的模块。

## <a name="updating-azure-modules"></a>更新 Azure 模块

1. 在自动化帐户的“模块”页中有一个名为“更新 Azure 模块”的选项。 该选项始终处于启用状态。<br><br> ![“模块”页中的“更新 Azure 模块”选项](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > 在更新 Azure 模块之前，建议在一个测试自动化帐户中更新它们，以确保现有脚本按预期工作。
  >

2. 单击“更新 Azure 模块”，会出现一条询问是否要继续的确认通知。<br><br> ![“更新 Azure 模块”通知](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. 单击“是”，模块更新过程开始。 更新过程大约需要 15-20 分钟来更新以下模块：

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    如果模块已经是最新的，则该过程只需几秒钟即可完成。 更新过程完成后将收到通知。<br><br> ![“更新 Azure 模块”更新状态](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

    .NET core AzureRm 模块 (AzureRm.*.Core) 在 Azure 自动化中不受支持，并且无法导入。

> [!NOTE]
> 当运行新的计划作业时，Azure 自动化将在自动化帐户中使用最新模块。  

如果在 Runbook 中使用这些 Azure PowerShell 模块中的 cmdlet，需要大约每月运行一次此更新过程，以确保拥有最新的模块。 更新模块时，Azure 自动化使用 AzureRunAsConnection 连接进行身份验证，如果服务主体已过期或不再以订阅级别存在，模块更新将失败。

## <a name="next-steps"></a>后续步骤

* 若要详细了解集成模块以及如何创建自定义模块来进一步与其他系统、服务或解决方案集成自动化，请参阅 [集成模块](automation-integration-modules.md)。

