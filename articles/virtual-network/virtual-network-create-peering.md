---
title: "创建 Azure 虚拟网络对等互连 - 资源管理器 - 同一订阅 | Azure"
description: "了解如何在同一 Azure 订阅中通过资源管理器创建的虚拟网络之间创建虚拟网络对等互连。"
services: virtual-network
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 09/25/2017
ms.date: 03/12/2018
ms.author: v-yeche
ms.openlocfilehash: feac8b81e7e65238fd917f33cae578146b6f1a8b
ms.sourcegitcommit: ad7accbbd1bc7ce0aeb2b58ce9013b7cafa4668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a>创建虚拟网络对等互连 - 资源管理器，同一订阅

本教程介绍如何在通过资源管理器创建的虚拟网络间创建虚拟网络对等互连。 这两个虚拟网络在同一个订阅中。 在两个虚拟网络之间建立对等互连可让不同虚拟网络中的资源以相同的带宽和延迟彼此通信，就像这些资源位于同一个虚拟网络中一样。 了解有关[虚拟网络对等互连](virtual-network-peering-overview.md)的详细信息。 

创建虚拟网络对等互连的步骤有所不同，具体取决于虚拟网络是否位于相同订阅，以及创建虚拟网络的 [ Azure 部署模型](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fvirtual-network%2ftoc.json)。 单击下表中的方案，了解如何采用其他方案创建虚拟网络对等互连：

|Azure 部署模型  | Azure 订阅  |
|--------- |---------|
|[都是 Resource Manager 模型](create-peering-different-subscriptions.md) |不同|
|[一个为资源管理器模型，一个为经典模型](create-peering-different-deployment-models.md) |相同|
|[一个为 Resource Manager 模型，一个为经典模型](create-peering-different-deployment-models-subscriptions.md) |不同|

不能在通过经典部署模型部署的两个虚拟网络之间创建对等互连。 如需连接两个通过经典部署模型创建的虚拟网络，可使用 Azure [VPN 网关](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fvirtual-network%2ftoc.json)来连接它们。 

<!-- PENDING on Creating Peer Virtual Network in different regions -->
本教程将在同一区域中的虚拟网络之间建立对等互连。 在不同区域的虚拟网络之间建立对等互连的功能目前处于预览版状态。 首先完成[注册全局虚拟网络对等互连](#register)中的步骤，然后再尝试在不同区域中的虚拟网络之间建立对等互连，否则对等互连的建立将失败。 使用 Azure [VPN 网关](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fvirtual-network%2ftoc.json)在不同区域连接虚拟网络的功能现已公开发布且无需注册。
<!-- PENDING on Creating Peer Virtual Network in different regions -->

可以使用 [Azure 门户](#portal)、Azure [命令行接口](#cli) (CLI)、Azure [PowerShell](#powershell)、或 [Azure 资源管理器模板](#template)创建虚拟网络对等互连。 单击以前的任何工具链接直接转到使用所选工具创建虚拟网络对等互连的步骤。

## <a name="portal"></a>创建对等互连 - Azure 门户

1. 登录到 [Azure 门户](https://portal.azure.cn)。 用于登录的帐户必须拥有创建虚拟网络对等互连的必要权限。 有关详细信息，请参阅本文的[权限](#permissions)部分。
2. 依次单击“+ 新建”、“网络”、“虚拟网络”。
3. 在“创建虚拟网络”边栏选项卡中，为以下设置输入或选择值，然后单击“创建”：
    - 名称：myVnet1
    - 地址空间：10.0.0.0/16
    - **子网名称**：默认值
    - 子网地址范围：10.0.0.0/24
    - 订阅：选择订阅
    - 资源组：选择“新建”并输入 myResourceGroup
    - 位置：中国东部
4. 再次完成步骤 2-3，在步骤 3 中指定以下值：
    - **名称**：*myVnet2*
    - 地址空间：10.1.0.0/16
    - 子网名称：默认值
    - 子网地址范围：10.1.0.0/24
    - 订阅：选择订阅
    - 资源组：选择“使用现有资源组”，然后选择“myResourceGroup”
    - 位置：中国东部
5. 在门户顶部的“搜索资源”框中键入 myResourceGroup。 当“myResourceGroup”出现在搜索结果中时，请单击它。 随后将显示 myresourcegroup 资源组的边栏选项卡。 该资源组包含前面步骤中创建的两个虚拟网络。
6. 单击“myVNet1”。
7. 在显示的“myVnet1”边栏选项卡中，单击左侧垂直选项列表中的“对等互连”。
8. 在显示的“myVnet1 - 对等互连”边栏选项卡中，单击“+ 添加”
9. 在显示的“添加对等互连”边栏选项卡中，输入或选择以下选项，然后单击“确定”：
     - 名称：myVnet1ToMyVnet2
     - **虚拟网络部署模型**：选择“Resource Manager”。 
     - 订阅：选择订阅
     - 虚拟网络：单击“选择虚拟网络”，然后单击“myVnet2”。
     - 允许虚拟网络访问：确保选择“启用”。
    本教程不使用其他任何设置。 若要了解所有对等互连设置，请参阅[管理虚拟网络对等互连](virtual-network-manage-peering.md#create-a-peering)。
10. 在上一步骤中单击“确定”后，“添加对等互连”边栏选项卡将会关闭，并再次出现“myVnet1 - 对等互连”边栏选项卡。 几秒钟后，创建的对等互连将显示在该边栏选项卡中。 所创建的 **myVnet1ToMyVnet2** 对等互连的“对等互连状态”列中列出了“已启动”。 已将 Vnet1 对等互连到 Vnet2，但现在必须将 myVnet2 对等互连到 myVnet1。 必须朝两个方向创建对等互连才能让虚拟网络中的资源相互通信。
11. 针对 myVnet2 再次完成步骤 5-10。 将对等互连命名为 *myVnet2ToMyVnet1*。
12. 单击“确定”创建 MyVnet2 的对等互连几秒钟后，将会列出刚刚创建的 myVnet2ToMyVnet1 对等互连，“对等互连状态”列中显示“已连接”。
13. 针对 MyVnet1 再次完成步骤 5-7。 **myVnet1ToVNet2** 对等互连的“对等互连状态”现在也显示为“已连接”。 对等互连中两个虚拟网络的“对等互连状态”列都显示为“已连接”后，即表示已成功建立对等互连。
14. 可选：尽管本教程未介绍如何创建虚拟机，但你可以在每个虚拟网络中创建一个虚拟机并将其相互连接，以验证连接性。
15. 可选：若要删除在本教程中创建的资源，请完成本文[删除资源](#delete-portal)部分所述步骤。

在任一虚拟网络中创建的任何 Azure 资源现在都可通过其 IP 地址相互通信。 如果为虚拟网络使用默认的 Azure 名称解析，则虚拟网络中的资源无法跨虚拟网络解析名称。 若要跨对等互连中的虚拟网络解析名称，必须创建自己的 DNS 服务器。 了解如何[使用自己的 DNS 服务器进行名称解析](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server)。

## <a name="cli"></a>创建对等互连 - Azure CLI

以下脚本：

- 需要 Azure CLI 2.0.4 或更高版本。 若要查找版本，请运行 `az --version` 命令。 如果需要进行升级，请参阅[安装 Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-azure-cli?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest)。
- 可以在 Bash shell 中使用。 有关在 Windows 客户端上运行 Azure CLI 脚本的选项，请参阅[在 Windows 中运行 Azure CLI](../virtual-machines/windows/cli-options.md?toc=%2fvirtual-network%2ftoc.json)。 

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]
<!-- Not Available on Azure Cloud Shell in Azure.cn-->

1. 创建一个资源组和两个虚拟网络。

    ```azurecli
    #!/bin/bash

    # Variables for common values used throughout the script.
    rgName="myResourceGroup"
    location="chinaeast"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network 1.
    az network vnet create \
      --name myVnet1 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Create virtual network 2.
    az network vnet create \
      --name myVnet2 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.1.0.0/16
    ```

2. 在两个虚拟网络之间创建虚拟网络对等互连。

    ```azurecli
    # Get the id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get the id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 to VNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. 执行该脚本后，查看每个虚拟网络的对等互连。 

    ```azurecli
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```

    再次运行上述命令，但请将 myVnet1 替换为 myVnet2。 这两条命令的输出将在 PeeringState 列中显示 Connected。

    在任一虚拟网络中创建的任何 Azure 资源现在都可通过其 IP 地址相互通信。 如果为虚拟网络使用默认的 Azure 名称解析，则虚拟网络中的资源无法跨虚拟网络解析名称。 若要跨对等互连中的虚拟网络解析名称，必须创建自己的 DNS 服务器。 了解如何[使用自己的 DNS 服务器进行名称解析](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server)。

4. 可选：尽管本教程未介绍如何创建虚拟机，但你可以在每个虚拟网络中创建一个虚拟机并将其相互连接，以验证连接性。
5. 可选：若要删除在本教程中创建的资源，请完成本文[删除资源](#delete-cli)部分所述步骤。

## <a name="powershell"></a>创建对等互连 - PowerShell

1. 安装最新版本的 PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) 模块。 如果不熟悉 Azure PowerShell，请参阅 [Azure PowerShell 概述](https://docs.microsoft.com/powershell/azure/overview?toc=%2fvirtual-network%2ftoc.json)。
2. 若要启动 PowerShell 会话，请转到“开始”，输入“powershell”，然后单击“PowerShell”。
3. 在 PowerShell 中，输入 `login-azurermaccount` 命令登录 Azure。 用于登录的帐户必须拥有创建虚拟网络对等互连的必要权限。 有关详细信息，请参阅本文的[权限](#permissions)部分。
4. 创建一个资源组和两个虚拟网络。 若要执行脚本，请复制以下脚本，将其粘贴到 PowerShell，在屏幕上显示最后一行后按 `Enter`：

    ```powershell
    # Variables for common values used throughout the script.
    $rgName='myResourceGroup'
    $location='chinaeast'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network 1.
    $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location

    # Create virtual network 2.
    $vnet2 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet2' `
      -AddressPrefix '10.1.0.0/16' `
      -Location $location
    ```

5. 在两个虚拟网络之间创建虚拟网络对等互连。 复制以下脚本，将其粘贴到 PowerShell，在屏幕上显示最后一行后按 `Enter`：
    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 to VNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. 若要查看虚拟网络的子网，请复制以下命令，将其粘贴到 PowerShell，按 `Enter`：

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    再次运行上述命令，但请将 myVnet1 替换为 myVnet2。 这两条命令的输出将在 PeeringState 列中显示 Connected。
7. 可选：尽管本教程未介绍如何创建虚拟机，但你可以在每个虚拟网络中创建一个虚拟机并将其相互连接，以验证连接性。
8. 可选：若要删除在本教程中创建的资源，请完成本文[删除资源](#delete-powershell)部分所述步骤。

在任一虚拟网络中创建的任何 Azure 资源现在都可通过其 IP 地址相互通信。 如果为虚拟网络使用默认的 Azure 名称解析，则虚拟网络中的资源无法跨虚拟网络解析名称。 若要跨对等互连中的虚拟网络解析名称，必须创建自己的 DNS 服务器。 了解如何[使用自己的 DNS 服务器进行名称解析](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server)。

## <a name="template"></a>创建对等互连 - Resource Manager 模板

1. 请参考[创建虚拟网络对等互连](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-to-vnet-peering) Resource Manager 模板。 此文提供了相关说明，以及用于通过 Azure 门户、PowerShell 或 Azure CLI 部署模板的模板。 登录到所选的任何工具，以便使用拥有创建虚拟网络对等互连所需的权限的帐户来部署模板。 有关详细信息，请参阅本文的[权限](#permissions)部分。
2. **可选**：尽管本教程未介绍如何创建虚拟机，但你可以在每个虚拟网络中创建一个虚拟机并将其相互连接，以验证连接性。
3. 可选：若要删除在本教程中创建的资源，请使用 Azure 门户、PowerShell 或 Azure CLI 完成本文[删除资源](#delete)部分所述步骤。

## <a name="permissions"></a>权限

用于创建虚拟网络对等互连的帐户必须具有所需的角色或权限。 例如，如果在分别名为 VNet1 和 VNet2 的两个虚拟网络之间创建了对等互连，则必须为帐户分配针对每个虚拟网络的最小角色或权限：

|虚拟网络|角色|权限|
|---|---|---|
|VNet1|[网络参与者](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
|VNet2|[网络参与者](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|

详细了解[内置角色](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)以及将特定的权限分配到[自定义角色](../active-directory/role-based-access-control-custom-roles.md?toc=%2fvirtual-network%2ftoc.json)（仅限 Resource Manager）。

## <a name="delete"></a>删除资源
完成本教程后，可能需要删除本教程中创建的资源，以免产生使用费。 删除资源组会删除其中包含的所有资源。

### <a name="delete-portal"></a>Azure 门户

1. 在门户的搜索框中，输入 myResourceGroup。 在搜索结果中，单击“myResourceGroup”。
2. 在“myResourceGroup”边栏选项卡中，单击“删除”图标。
3. 若要确认删除，请在“键入资源组名称”框中输入 myResourceGroup，然后单击“删除”。

### <a name="delete-cli"></a>Azure CLI

输入以下命令：

```azurecli
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

输入以下命令：

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

<!--PENDING ## <a name="register"></a>Register for the global virtual network peering preview-->
## <a name="register"></a>注册全局虚拟网络对等互连（预览版）

在同一区域中的虚拟网络之间建立对等互连的功能已推出正式版。 在不同区域的虚拟网络之间建立对等互连目前处于预览版状态。 有关可用区域，请参阅[虚拟网络更新](https://www.azure.cn/what-is-new/)。 若要跨区域在虚拟网络之间建立对等互连，必须先通过使用 Azure PowerShell 或 Azure CLI 完成以下步骤（在要对其建立对等互连的每个虚拟网络所在的订阅中执行）来注册预览版：

### <a name="powershell"></a>PowerShell

1. 安装最新版本的 PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) 模块。 如果不熟悉 Azure PowerShell，请参阅 [Azure PowerShell 概述](https://docs.microsoft.com/powershell/azure/overview?toc=%2fvirtual-network%2ftoc.json)。
2. 使用 `Login-AzureRmAccount -EnvironmentName AzureChinaCloud` 命令启动 PowerShell 会话并登录到 Azure。
3. 通过输入以下命令，注册要对其建立对等互连的每个虚拟网络所在订阅的预览版：

    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network

    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
4. 输入以下命令，确认已针对预览版进行了注册：

    ```powershell    
    Get-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    ```

    输入之前的命令，在收到的两个订阅的“RegistrationState”输出为“已注册”后，才能完成本文中在门户、Azure CLI、PowerShell 或资源管理器模板部分中进行的步骤。

### <a name="azure-cli"></a>Azure CLI

1. [安装并配置 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?toc=%2Fvirtual-network%2Ftoc.json?view=azure-cli-latest)。
2. 输入 `az --version` 命令，确保使用的是版本 2.0.18 或更高版本的 Azure CLI。 如果不是，请安装最新版本。
3. 使用 `az login` 命令登录到 Azure。
4. 输入以下命令，针对预览版进行注册：

    ```azurecli
    az feature register --name AllowGlobalVnetPeering --namespace Microsoft.Network
    az provider register --name Microsoft.Network
    ```

5. 输入以下命令，确认已针对预览版进行了注册：

    ```azurecli
    az feature show --name AllowGlobalVnetPeering --namespace Microsoft.Network
    ```

    输入之前的命令，在收到的两个订阅的“RegistrationState”输出为“已注册”后，才能完成本文中在门户、Azure CLI、PowerShell 或资源管理器模板部分中进行的步骤。
<!--PENDING ## <a name="register"></a>Register for the global virtual network peering preview-->
## <a name="next-steps"></a>后续步骤

- 在针对生产用途创建虚拟网络对等互连之前，请充分熟悉重要的[虚拟网络对等互连约束和行为](virtual-network-manage-peering.md#requirements-and-constraints)。
- 了解所有的[虚拟网络对等互连设置](virtual-network-manage-peering.md#create-a-peering)。
- 了解如何使用虚拟网络对等互连[创建中心辐射型网络拓扑](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fvirtual-network%2ftoc.json#vnet-peering)。

<!--Update_Description: update meta properties, wording update, update link -->