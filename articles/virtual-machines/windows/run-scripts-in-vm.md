---
title: 在 Azure Windows VM 中运行脚本
description: 本主题介绍了如何在 Windows 虚拟机中运行脚本
services: automation
ms.service: automation
author: rockboyfor
ms.author: v-yeche
origin.date: 05/02/2018
ms.date: 06/25/2018
ms.topic: article
manager: digimobile
ms.openlocfilehash: 59f1079678128f0df3678e520021f5f306af81b1
ms.sourcegitcommit: 5f2849d5751cb634f1cdc04d581c32296e33ef1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53028771"
---
# <a name="run-scripts-in-your-windows-vm"></a>在 Windows VM 中运行脚本

若要自动执行任务或解决问题，可能需要在 VM 中运行命令。 以下文章简要概述了可用于在 VM 中运行脚本和命令的功能。

## <a name="custom-script-extension"></a>自定义脚本扩展

[自定义脚本扩展](../extensions/custom-script-windows.md)主要用于发布部署配置和软件安装。

* 在 Azure 虚拟机中下载并运行脚本。
* 可以使用 Azure 资源管理器模板、Azure CLI、REST API、PowerShell 或 Azure 门户运行。
* 脚本文件可以从 Azure 存储或 GitHub 下载，或者在从 Azure 门户运行时从电脑提供。
* 在 Windows 计算机中运行 PowerShell 脚本，在 Linux 计算机中运行 Bash 脚本。
* 适用于部署后配置、软件安装和其他配置或管理任务。

<!-- Not Available on ## Run command-->


<!-- Not Available on ## Hybrid Runbook Worker -->

<!-- Not Available on ## Serial console-->
## <a name="next-steps"></a>后续步骤

详细了解可用于在 VM 中运行脚本和命令的不同功能。

* [自定义脚本扩展](../extensions/custom-script-windows.md)
<!-- Not Available on * [Run Command](run-command.md)-->
<!-- Not Available on * [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md)-->
<!-- Not Available on * [Serial console](serial-console.md)-->
<!-- Update_Description: update meta properties -->
