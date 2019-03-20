---
title: Azure Automation State Configuration 概述
description: 对 Azure Automation State Configuration (DSC) 及其术语和已知问题的概述
keywords: powershell dsc, desired state configuration, powershell dsc azure
services: automation
ms.service: automation
ms.subservice: dsc
author: WenJason
ms.author: v-jay
origin.date: 11/06/2018
ms.date: 03/18/2019
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: 50a85499745bc62872f4c45a1c916571d48750a0
ms.sourcegitcommit: c5646ca7d1b4b19c2cb9136ce8c887e7fcf3a990
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2019
ms.locfileid: "57988091"
---
# <a name="azure-automation-state-configuration-overview"></a>Azure Automation State Configuration 概述

Azure Automation State Configuration 是一种 Azure 服务，允许编写、管理和编译 PowerShell Desired State Configuration (DSC) [配置](https://docs.microsoft.com/powershell/dsc/configurations)，导入 [DSC 资源](https://docs.microsoft.com/powershell/dsc/resources)，并将配置分配给目标节点，所有操作均在云中进行。

## <a name="why-use-azure-automation-state-configuration"></a>为何使用 Azure Automation State Configuration

与在 Azure 之外使用 DSC 相比，Azure Automation State Configuration 具有多项优势。

### <a name="built-in-pull-server"></a>内置拉取服务器

Azure Automation State Configuration 提供了类似于 [Windows 功能 DSC 服务](https://docs.microsoft.com/powershell/dsc/pullserver)的 DSC 拉取服务器，这样目标节点可自动接收配置，符合所需状态，并报告回其符合性。 Azure 自动化中的内置拉取服务器消除了设置和维护你自己的拉取服务器的需要。 Azure 自动化的目标可以是云中或本地的虚拟机，或物理 Windows 或 Linux 计算机。

### <a name="management-of-all-your-dsc-artifacts"></a>管理所有 DSC 项目

Azure Automation State Configuration 向 [PowerShell Desired State Configuration](https://docs.microsoft.com/powershell/dsc/overview) 提供的管理层与 Azure 自动化为 PowerShell 脚本提供的相同。

从 Azure 门户，或从 PowerShell，你可以管理所有的 DSC 配置、资源和目标节点。

![“Azure 自动化”页的屏幕截图](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-azure-monitor-logs"></a>将报表数据导入到 Azure Monitor 日志中

使用 Azure Automation State Configuration 进行管理的节点将详细的报表状态数据发送到内置拉取服务器。 可以将 Azure Automation State Configuration 配置为将此数据发送到 Log Analytics 工作区。 若要了解如何将 State Configuration 状态数据发送到 Log Analytics 工作区，请参阅[将 Azure Automation State Configuration 报表数据转发到 Azure Monitor 日志](automation-dsc-diagnostics.md)。

## <a name="prerequisites"></a>先决条件

在使用 Azure Automation State Configuration (DSC) 时，请考虑以下要求。

### <a name="operating-system-requirements"></a>操作系统要求

运行 Windows 的节点支持以下版本：

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012R2
- Windows Server 2012
- Windows Server 2008 R2 SP1
- Windows 10
- Windows 8.1
- Windows 7

运行 Linux 的节点支持以下发行版/版本：

DSC Linux 扩展支持所有[在 Azure 上认可的](/virtual-machines/linux/endorsed-distros) Linux 发行版，除了以下这些：

分发 | 版本
-|-
Debian  | 所有版本
Ubuntu  | 18.04

### <a name="dsc-requirements"></a>DSC 要求

对于在 Azure 中运行的所有 Windows 节点，[WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure) 将在载入时安装。  对于运行 Windows Server 2012 和 Windows 7 的节点，[将会启用 WinRM](https://docs.microsoft.com/powershell/dsc/troubleshooting/troubleshooting#winrm-dependency)。

对于在 Azure 中运行的所有 Linux 节点，[PowerShell DSC for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) 将在载入时安装。

## <a name="next-steps"></a>后续步骤

- 有关入门信息，请参阅 [Azure Automation State Configuration 入门](automation-dsc-getting-started.md)
- 要了解如何登记节点，请参阅[登记由 Azure Automation State Configuration 管理的计算机](automation-dsc-onboarding.md)
- 若要了解如何编译 DSC 配置，以便将它们分配给目标节点，请参阅[在 Azure Automation State Configuration 中编译配置](automation-dsc-compile.md)
- 有关 PowerShell cmdlet 参考，请参阅 [Azure Automation State Configuration cmdlet](https://docs.microsoft.com/powershell/module/azurerm.automation/#automation)
- 有关定价信息，请参阅 [Azure Automation State Configuration 定价](https://azure.cn/pricing/details/automation/)
- 若要查看在持续部署管道中使用 Azure Automation State Configuration 的示例，请参阅[使用 Azure Automation State Configuration 和 Chocolatey 进行持续部署](automation-dsc-cd-chocolatey.md)