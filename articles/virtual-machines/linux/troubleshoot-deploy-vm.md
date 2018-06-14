---
title: 排查 Azure 中的 Linux 虚拟机部署问题 | Azure
description: 排查 Azure 资源管理器部署模型中的 Linux 虚拟机部署问题。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/11/2018
ms.date: 06/04/2018
ms.author: v-yeche
ms.openlocfilehash: 2efcf4056bcd811c656957b233efb178408478fb
ms.sourcegitcommit: 6f42cd6478fde788b795b851033981a586a6db24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2018
ms.locfileid: "34702693"
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a>排查 Azure 中的 Linux 虚拟机部署问题

若要排查 Azure 中虚拟机 (VM) 的部署问题，请查看[常见问题](#top-issues)了解常见故障和解决方法。

如果对本文中的任何观点存在疑问，可以联系 [MSDN Azure 和 CSDN Azure](https://www.azure.cn/support/forums/)上的 Azure 专家。 或者，也可以提出 Azure 支持事件。 请转到 [Azure 支持站点](https://www.azure.cn/support/contact/)并选择“获取支持”。

## <a name="top-issues"></a>常见问题
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

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

<!--Not Available on BizSpart, GPU, N-Series VB -->
## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>重设 VM 大小时，看不到所需的 VM 大小系列。

当 VM 正在运行时，将其部署到物理服务器。 Azure 区域中的物理服务器被分在常见物理硬件群集组中。 需要将 VM 移到其他硬件群集才能重设 VM 大小，具体操作因部署 VM 所用部署模型而异。

- 对于在经典部署模型中部署的 VM，必须删除并重新部署云服务部署，才能将 VM 大小更改为其他大小系列。

- 对于在资源管理器部署模型中部署的 VM，必须先停止可用性集中的所有 VM，然后才能更改可用性集中任何 VM 的大小。

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>在可用性集中部署时，列出的 VM 大小不受支持。

请选择可用性集的群集支持的大小。 建议在创建可用性集时选择所需的最大 VM 大小，并将它率先部署到可用性集。

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a>Azure 支持哪些 Linux 发行版/版本？

有关列表，可以参阅 [Azure 认可的 Linux 发行版](endorsed-distros.md)。

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>能否将现有经典 VM 添加到可用性集？

是的。 可以将现有经典 VM 添加到新的或现有的可用性集。 有关详细信息，请参阅[将现有虚拟机添加到可用性集](../windows/classic/configure-availability-classic.md#addmachine)。

## <a name="next-steps"></a>后续步骤
如果对本文中的任何观点存在疑问，可以联系 [MSDN Azure 和 CSDN Azure](https://www.azure.cn/support/forums/)上的 Azure 专家。

或者，也可以提出 Azure 支持事件。 请转到 [Azure 支持站点](https://www.azure.cn/support/contact/)并选择 **获取支持**。

<!--Update_Description: wording update, update link  -->