---
title: Azure PowerShell 脚本示例 - 创建 Log Analytics 工作区 | Azure Docs
description: Azure PowerShell 脚本示例 - 创建 Log Analytics 工作区
services: log-analytics
documentationcenter: ''
author: lingliw
manager: digimobile
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: log-analytics
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2019
ms.author: v-lingwu
ms.openlocfilehash: 520f7bf2c8133cd0aa2dd7162bfc3da338fb596e
ms.sourcegitcommit: e78670855b207c6084997f747ad8e8c3afa3518b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68513989"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>使用 PowerShell 创建 Log Analytics 工作区

利用此脚本，可使用 Azure Log Analytics 工作区快速启动和运行；必须使用此工作区才能开始收集、分析和在数据上执行操作。  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令在订阅中创建新的 Log Analytics 工作区。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzureRmOperationalInsightsWorkspace](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace) | 获取现有工作区的相关信息。 |
| [New-AzureRmOperationalInsightsWorkspace](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace) | 在指定的资源组和位置中创建一个工作区。 |


## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。





