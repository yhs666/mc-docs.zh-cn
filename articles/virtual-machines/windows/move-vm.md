---
title: "在 Azure 中移动 Windows VM 资源 | Azure"
description: "在 Resource Manager 部署模型中将 Windows VM 移到其他 Azure 订阅或资源组。"
services: virtual-machines-windows
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 12/06/2017
ms.date: 01/08/2018
ms.author: v-yeche
ms.openlocfilehash: 19651ea087a1a8d08b69fade3954b6b674a0a06b
ms.sourcegitcommit: f02cdaff1517278edd9f26f69f510b2920fc6206
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2018
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>将 Windows VM 移到其他 Azure 订阅或资源组
本文逐步说明如何在资源组或订阅之间移动 Windows VM。 如果最初在个人订阅中创建了 VM，现在想要将其移到公司的订阅以继续工作，则在订阅之间移动 VM 可能很方便。

> [!IMPORTANT]
>不可在此时移动托管磁盘。 
>
>在移动过程中会创建新的资源 ID。 移动 VM 后，需要更新工具和脚本以使用新的资源 ID。 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>使用 PowerShell 移动 VM

要将虚拟机移到其他资源组，需确保同时移动所有依赖资源。 若要使用 Move-AzureRMResource cmdlet，需要提供每个资源的 ResourceId。 可以使用 [Find-AzureRMResource](https://docs.microsoft.com/powershell/module/azurerm.resources/find-azurermresource) cmdlet 获取 ResourceId 的列表。

```azurepowershell-interactive
Find-AzureRMResource -ResourceGroupNameContains <sourceResourceGroupName> | Format-table -Property ResourceId 
```

若要移动 VM，需要移动多个资源。 可以使用 Find-AzureRMResource 的输出创建 ResourceId 的逗号分隔列表，并将该列表传递给 [Move-AzureRMResource](https://docs.microsoft.com/powershell/module/azurerm.resources/move-azurermresource) 以将它们移动到目标。 

```azurepowershell-interactive

Move-AzureRmResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```

要将资源移到其他订阅，请包含 **-DestinationSubscriptionId** 参数的值。 

```azurepowershell-interactive
Move-AzureRmResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```

系统会要求确认需要移动指定资源。 

## <a name="next-steps"></a>后续步骤
可以在资源组和订阅之间移动许多不同类型的资源。 有关详细信息，请参阅[将资源移到新资源组或订阅](../../resource-group-move-resources.md)。
<!-- Update_Description: update meta properties, wording update -->