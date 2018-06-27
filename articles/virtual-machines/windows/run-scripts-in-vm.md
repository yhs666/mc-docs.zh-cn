---
title: 在 Azure Windows VM 中运行脚本
description: 本主题介绍如何在 Windows 虚拟机中运行脚本
services: automation
ms.service: automation
author: rockboyfor
ms.author: v-yeche
origin.date: 05/02/2018
ms.date: 06/25/2018
ms.topic: article
manager: digimobile
ms.openlocfilehash: 13b6f78b59e9d26c35873beaf6e90282b11c3334
ms.sourcegitcommit: 092d9ef3f2509ca2ebbd594e1da4048066af0ee3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36315385"
---
# <a name="run-scripts-in-your-windows-vm"></a>在 Windows VM 中运行脚本

可能需要在 VM 中运行命令来自动完成任务或排查问题。 下文概述了可以用来在 VM 中运行脚本和命令的功能。

## <a name="custom-script-extension"></a>自定义脚本扩展

[自定义脚本扩展](../extensions/custom-script-windows.md)主要用于部署后配置和软件安装。

* 在 Azure 虚拟机中下载并运行脚本。
* 可以使用 Azure 资源管理器模板、Azure CLI、REST API、PowerShell 或 Azure 门户来运行。
* 脚本文件可以从 Azure 存储或 GitHub 下载，也可以从电脑获取（通过 Azure 门户运行时）。
* 在 Windows 计算机中运行 PowerShell 脚本，在 Linux 计算机中运行 Bash 脚本。
* 适用于部署后配置、软件安装以及其他配置或管理任务。

<!-- Not Available on ## Run command-->


<!-- Not Available on ## Hybrid Runbook Worker -->

## <a name="serial-console"></a>串行控制台

可以通过[串行控制台](serial-console.md)直接访问 VM，类似于将键盘连接到 VM。

* 在 Azure 虚拟机中运行命令。
* 可以在 Azure 门户中使用计算机的基于文本的控制台来运行。
* 使用本地用户帐户登录到计算机。
* 适用于需要访问虚拟机的情况（不管计算机的网络或操作系统状态如何）。

## <a name="next-steps"></a>后续步骤

详细了解可以用来在 VM 中运行脚本和命令的不同功能。

* [自定义脚本扩展](../extensions/custom-script-windows.md)
<!-- Not Available on * [Run Command](run-command.md)-->
<!-- Not Available on * [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md)-->
* [串行控制台](serial-console.md)
<!-- Update_Description: update meta properties -->
