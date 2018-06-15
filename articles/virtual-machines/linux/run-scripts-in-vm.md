---
title: 在 Azure Linux VM 中运行脚本
description: 本主题介绍如何在虚拟机中运行脚本
services: automation
ms.service: automation
author: rockboyfor
ms.author: v-yeche
origin.date: 05/02/2018
ms.date: 06/04/2018
ms.topic: article
manager: digimobile
ms.openlocfilehash: 1364a4c08b900854ce0d9f0f740280ef47caa2d1
ms.sourcegitcommit: 6f42cd6478fde788b795b851033981a586a6db24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2018
ms.locfileid: "34702947"
---
# <a name="run-scripts-in-your-linux-vm"></a>在 Linux VM 中运行脚本

可能需要在 VM 中运行命令来自动完成任务或排查问题。 下文概述了可以用来在 VM 中运行脚本和命令的功能。

<!-- Not Available on ## Custom Script Extension -->
<!-- Confirm
The [Custom Script Extension](../extensions/custom-script-linux.md) is primarily used for post deployment configuration and software installation.

* Download and run scripts in Azure virtual machines.
* Can be run using Azure Resource Manager templates, Azure CLI, REST API, PowerShell, or Azure portal.
* Script files can be downloaded from Azure storage or GitHub, or provided from your PC when run from the Azure portal.
* Run PowerShell script in Windows machines and Bash script in Linux machines.
* Useful for post deployment configuration, software installation, and other configuration or management tasks.
-->
<!-- Not Available on ## Run command-->
<!-- Not Available on ## Hybrid Runbook Worker-->

## <a name="serial-console"></a>串行控制台

可以通过[串行控制台](serial-console.md)直接访问 VM，类似于将键盘连接到 VM。

* 在 Azure 虚拟机中运行命令。
* 可以在 Azure 门户中使用计算机的基于文本的控制台来运行。
* 使用本地用户帐户登录到计算机。
* 适用于需要访问虚拟机的情况（不管计算机的网络或操作系统状态如何）。

## <a name="next-steps"></a>后续步骤

详细了解可以用来在 VM 中运行脚本和命令的不同功能。

<!-- Not Available on * [Custom Script Extension](../extensions/custom-script-linux.md)-->
<!-- Not Available on * [Run Command](run-command.md)-->
<!-- Not Available on * [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md)-->
* [串行控制台](serial-console.md)
<!-- Update_Description: new articles on run scripts in vm -->
<!--ms.date: 06/04/2018-->