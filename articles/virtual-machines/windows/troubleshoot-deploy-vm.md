---
title: "排查 Azure 中的 Windows 虚拟机部署问题 | Azure"
description: "排查在 Azure 资源管理器部署模型中部署 Windows 虚拟机时遇到的问题。"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/22/2017
ms.date: 08/14/2017
ms.author: v-dazen
ms.openlocfilehash: 9883cefec1cf74b4c9368f18da0a11b531c68a92
ms.sourcegitcommit: f858adac6a7a32df67bcd5c43946bba5b8ec6afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2017
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a>排查 Azure 中的 Windows 虚拟机部署问题

若要排查 Azure 中虚拟机 (VM) 的部署问题，请查看[常见问题](#top-issues)了解常见故障和解决方法。

如果对本文中的任何观点存在疑问，可以联系 [MSDN Azure 和 CSDN Azure](https://www.azure.cn/support/forums/)上的 Azure 专家。 或者，也可以提交 Azure 支持事件。 请转到 [Azure 支持站点](https://www.azure.cn/support/contact/)并选择“获取支持”。

## <a name="top-issues"></a>常见问题
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a>群集不支持请求的 VM 大小
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- 使用更小的 VM 大小来重试请求。
- 如果无法更改请求的 VM 大小：
    - 停止可用性集中的所有 VM。 依次单击“资源组”> 资源组 >“资源”> 可用性集 >“虚拟机”> 虚拟机 >“停止”。
    - 在所有 VM 都停止后，创建相应大小的 VM。
    - 先启动新 VM，再选择所有已停止的 VM 并单击“启动”。

## <a name="the-cluster-does-not-have-free-resources"></a>群集没有可用资源
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- 请稍后重试请求。
- 如果新 VM 属于不同的可用性集
    - 在不同的可用性集（位于同一区域）中创建 VM。
    - 将新 VM 添加到同一虚拟网络。

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>重设 VM 大小时，看不到所需的 VM 大小系列。

当 VM 正在运行时，将其部署到物理服务器。 Azure 区域中的物理服务器被分在常见物理硬件群集组中。 需要将 VM 移到其他硬件群集才能重设 VM 大小，具体操作因部署 VM 所用部署模型而异。

- 对于在经典部署模型中部署的 VM，必须删除并重新部署云服务部署，才能将 VM 大小更改为其他大小系列。

- 对于在资源管理器部署模型中部署的 VM，必须先停止可用性集中的所有 VM，然后才能更改可用性集中任何 VM 的大小。

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>在可用性集中部署时，列出的 VM 大小不受支持。

请选择可用性集的群集支持的大小。 建议在创建可用性集时选择所需的最大 VM 大小，并将它率先部署到可用性集。

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>能否将现有经典 VM 添加到可用性集？

是的。 可以将现有经典 VM 添加到新的或现有的可用性集。 有关详细信息，请参阅[将现有虚拟机添加到可用性集](classic/configure-availability.md#a-idaddmachine-aoption-2-add-an-existing-virtual-machine-to-an-availability-set)。

## <a name="next-steps"></a>后续步骤
如果对本文中的任何观点存在疑问，可以联系 [MSDN Azure 和 CSDN Azure](https://www.azure.cn/support/forums/)上的 Azure 专家。

或者，也可以提交 Azure 支持事件。 请转到 [Azure 支持站点](https://www.azure.cn/support/contact/)并选择“获取支持”。