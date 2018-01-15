---
title: "什么是 Azure 自动化 | Azure"
description: "了解 Azure 自动化的重要性和常见问题的答案，为创建和使用 Runbook 做准备。"
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "什么是自动化, azure 自动化, azure 自动化示例"
ms.assetid: 0cf1f3e8-dd30-4f33-b52a-e148e97802a9
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 05/10/2016
ms.date: 01/03/2017
ms.author: v-dazen
ms.openlocfilehash: 61353a655f9efc1ed118e4e18efad2acc948b5aa
ms.sourcegitcommit: 40b20646a2d90b00d488db2f7e4721f9e8f614d5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2018
---
# <a name="azure-automation-overview"></a>Azure 自动化概述
借助 Azure 自动化，用户可以自动完成通常要在云环境和企业环境中执行的手动、长时间进行、易出错且重复性高的任务。 它可以节省时间，可以提高常规管理任务的可靠性，甚至可以将这些任务安排成按特定的时间间隔自动执行。 可以使用 Runbook 实现这些过程的自动化，或者使用 Desired State Configuration 实现配置管理的自动化。 本文概述了 Azure 自动化并回答了一些常见问题。 可以参考此库中的其他文章，以了解有关不同主题的更多详细信息。

## <a name="automating-processes-with-runbooks"></a>使用 Runbook 实现过程的自动化
Runbook 是 Azure 自动化中执行某些自动化过程的一组任务。 它可以是一个简单的过程（如启动虚拟机和创建日志项目），也可以是一个复杂的 Runbook，组合其他较小的 Runbook 来执行复杂的过程，这些复杂的过程可以跨多个资源甚至多个云来执行，也可以在本地环境中执行。  

例如，如果 SQL 数据库即将达到最大大小，用户可以通过现有的手动过程来截断该数据库，手动过程包括多个步骤，例如连接到服务器、连接到数据库、获取数据库的当前大小、查看是否超过了阈值，然后截断该数据库并通知用户。 您无需手动执行每个步骤，而是可以创建一个将所有这些任务作为单个过程执行的 Runbook。 用户可以启动 Runbook，提供所需的信息，如 SQL 服务器名称、数据库名称、收件人电子邮件，然后无需干预，等待该过程自动完成即可。 

## <a name="what-can-runbooks-automate"></a>Runbook 可以自动化哪些任务？
Azure 自动化中的 Runbook 均基于 Windows PowerShell 或 Windows PowerShell 工作流，因此它们能够执行 PowerShell 可以完成的任何工作。 如果应用程序或服务具有一个 API，Runbook 可以使用它。 如果有一个用于该应用程序的 PowerShell 模块，可以将该模块加载到 Azure 自动化中，并在 Runbook 中包括这些 cmdlet。 Azure 自动化 Runbook 在 Azure 云中运行，因此可以访问任何云资源或者那些可从云中访问的外部资源。 

## <a name="getting-runbooks-from-the-community"></a>从社区获取 Runbook
[Runbook 库](automation-runbook-gallery.md#runbooks-in-runbook-gallery) 包含来自 Microsoft 和社区的 Runbook，既可以在环境中使用未经修改的 Runbook，也可以根据自己的目的对其进行自定义。 您还可以将它们作为学习如何创建自己的 Runbook 的参考。 甚至可以将自己认为对其他用户有用的 Runbook 分享到库中。 

## <a name="creating-runbooks-with-azure-automation"></a>使用 Azure 自动化创建 Runbook
可以从头开始[创建自己的 Runbook](automation-creating-importing-runbook.md)，根据自己的需要修改 [Runbook 库](http://msdn.microsoft.com/library/azure/dn781422.aspx)中的 Runbook。 有四种不同的 [Runbook 类型](automation-runbook-types.md)，可根据自身需求和 PowerShell 体验从中进行选择。 如果想要直接使用 PowerShell 代码，则可以使用 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) 或 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks)，这两种可以脱机编辑或使用 Azure 门户中的[文本编辑器](http://msdn.microsoft.com/library/azure/dn879137.aspx)进行编辑。 如果想要在不公开给底层代码的情况下编辑 Runbook，则可以使用 Azure 门户中的[图形编辑器](automation-graphical-authoring-intro.md)创建[图形 Runbook](automation-runbook-types.md#graphical-runbooks)。 

更喜欢观看视频而不喜欢阅读文档？ 请看一下下面的来自 2015 年 5 月 Microsoft Ignite 会话的视频。 请注意：虽然此视频中所述的概念和功能是正确的，但 Azure 自动化自录制此视频以来已取得很大进展，它现在具有 Azure 门户中更广泛的 UI，并支持其他功能。

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3451/player]
> 
> 


## <a name="getting-modules-and-configurations"></a>获取模块和配置
可以获取包含 cmdlet 的 [PowerShell 模块](automation-runbook-gallery.md#modules-in-powershell-gallery)，这些 cmdlet 可以用于 [PowerShell 库](http://www.powershellgallery.com/)中的 Runbook 和 DSC 配置。 可以从 Azure 门户启动此库，并将模块直接导入到 Azure 自动化中，也可以下载并手动导入这些模块。 不能直接从 Azure 门户安装这些模块，但可以在下载之后进行安装，就像使用其他模块一样。 

## <a name="example-practical-applications-of-azure-automation"></a>Azure 自动化实际应用示例
以下是一些使用 Azure 自动化的自动化方案种类的示例。 

* 在不同的 Azure 订阅中创建和复制虚拟机。 
* 计划文件复制操作，以便将文件从本地计算机复制到 Azure Blob 存储容器。 
* 自动执行各种安全功能，例如在检测到拒绝服务攻击时拒绝来自某个客户端的请求。 
* 确保计算机始终符合配置的安全策略。
* 通过管理确保在云和本地基础结构中对应用程序代码进行连续的部署。 
* 针对实验室环境在 Azure 中构建 Active Directory 林。 
* 如果 SQL 数据库即将达到其最大大小，则截断数据库中的表。 
* 远程更新 Azure 网站的环境设置。 

## <a name="where-can-i-get-more-information"></a>从哪里可以获得详细信息？
我们提供了多种资源，以帮助了解有关 Azure 自动化的信息以及学习如何创建自己的 Runbook。 

* “Azure 自动化库”是当前所处的位置。 该库中的文章提供了有关配置和管理 Azure 自动化以及创作自己的 Runbook 的完整文档。 
* [Azure PowerShell cmdlet](http://msdn.microsoft.com/library/jj156055.aspx) 提供了有关使用 Windows PowerShell 自动完成 Azure 操作的信息。 Runbook 使用这些 cmdlet 来处理 Azure 资源。 
* [自动化论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azureautomation%2Cazureautomation) 允许提出有关 Azure 自动化的问题，Microsoft 和自动化社区将提供解答。 
* [Azure 自动化 Cmdlet](https://msdn.microsoft.com/library/dn690262.aspx) 提供有关管理任务自动化的信息。 它包含的 cmdlet 可用于管理自动化帐户、资产、Runbook。

## <a name="can-i-provide-feedback"></a>我是否可以提供反馈？
**欢迎提供反馈！** 如果正在寻找 Azure 自动化 Runbook 解决方案或集成模块，请在脚本中心发布脚本请求。