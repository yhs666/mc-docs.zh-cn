---
title: "VM 重新启动或大小调整问题 | Azure"
description: "排查在 Azure 中重新启动现有 Windows 虚拟机或调整其大小时遇到的经典部署问题"
services: virtual-machines-windows
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: required
origin.date: 06/13/2017
ms.date: 07/03/2017
ms.devlang: na
ms.author: v-dazen
ms.openlocfilehash: ba4ec759e2405bb2a701c9b74aef996d6fb1ef60
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 排查在 Azure 中重新启动现有 Windows 虚拟机或调整其大小时遇到的经典部署问题
<a id="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure" class="xliff"></a>
> [!div class="op_single_selector"]
> * [经典](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [Resource Manager](../restart-resize-error-troubleshooting.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

尝试启动已停止的 Azure 虚拟机 (VM)，或调整现有 Azure VM 的大小时，经常遇到的错误是分配失败。 当群集或区域没有可用的资源或无法支持所请求的 VM 大小时，就会发生此错误。

> [!IMPORTANT]
> Azure 具有用于创建和处理资源的两个不同的部署模型：[Resource Manager 和经典](../../../azure-resource-manager/resource-manager-deployment-model.md)。  本文介绍使用经典部署模型的情况。 Azure 建议大多数新部署使用 Resource Manager 模型。
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## 收集审核日志
<a id="collect-audit-logs" class="xliff"></a>
若要开始故障排除，请收集审核日志，以识别与问题相关的错误。

在 Azure 门户中，单击“浏览” > “虚拟机” >  *Windows 虚拟机*  > “设置” > “审核日志”。

## 问题：启动已停止的 VM 时发生错误
<a id="issue-error-when-starting-a-stopped-vm" class="xliff"></a>
尝试启动已停止的 VM，但出现分配失败错误。

### 原因
<a id="cause" class="xliff"></a>
必须在托管云服务的原始群集上尝试发出启动已停止 VM 的请求。 但是，群集没有可用的空间来完成该请求。

### 解决方法
<a id="resolution" class="xliff"></a>
* 创建新的云服务，并将其与区域或基于区域的虚拟网络（而不是地缘组）关联。
* 删除已停止的 VM。
* 使用磁盘在新的云服务中重新创建 VM。
* 启动重新创建的 VM。

如果在尝试创建新的云服务时收到错误，请稍后重试，或更改云服务的区域。

> [!IMPORTANT]
> 新的云服务将有新的名称和 VIP，因此需要针对现有云服务中依赖于此信息的所有项目更改该信息。
> 
> 

## 问题：调整现有 VM 的大小时发生错误
<a id="issue-error-when-resizing-an-existing-vm" class="xliff"></a>
尝试调整现有 VM 的大小，但出现分配失败错误。

### 原因
<a id="cause" class="xliff"></a>
必须在托管云服务的原始群集上尝试发出调整 VM 大小的请求。 但是，群集不支持请求的 VM 大小。

### 解决方法
<a id="resolution" class="xliff"></a>
减小请求的 VM 大小，然后重试调整大小请求。

* 单击“浏览全部” > “虚拟机(经典)” >  *你的虚拟机*  > “设置” > “大小”。 有关详细步骤，请参阅[调整虚拟机的大小](https://msdn.microsoft.com/library/dn168976.aspx)。

如果无法减小 VM 大小，请遵循以下步骤：

* 创建新的云服务，确保它不链接到地缘组，并且未与链接到地缘组的虚拟网络相关联。
* 在其中创建更大的新 VM。

可以在同一个云服务中合并所有 VM。 如果现有的云服务与基于区域的虚拟网络相关联，则可以将新的云服务连接到现有虚拟网络。

如果现有的云服务未与基于区域的虚拟网络相关联，则必须删除现有云服务中的 VM，并在新云服务中从其磁盘重新创建 VM。 然而，请务必记住，新的云服务将有新的名称和 VIP，因此需要为所有目前将此信息用于现有云服务的依赖项更新该信息。

## 后续步骤
<a id="next-steps" class="xliff"></a>
如果在 Azure 中创建 Windows VM 时遇到问题，请参阅[排查在 Azure 中创建 Windows 虚拟机时遇到的部署问题](../troubleshoot-deployment-new-vm.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。
