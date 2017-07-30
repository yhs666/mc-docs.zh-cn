---
title: "创建 Azure 虚拟网络对等互连 | Azure"
description: "了解如何在两个虚拟网络之间创建虚拟网络对等互连。"
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 06/06/2017
ms.date: 07/24/2017
ms.author: v-dazen
ms.openlocfilehash: 0fd9cef8fd1b92983767cd17a5eaac3d41adf7db
ms.sourcegitcommit: 2e85ecef03893abe8d3536dc390b187ddf40421f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="create-a-virtual-network-peering"></a>创建虚拟网络对等互连

本文介绍如何在两个虚拟网络之间创建虚拟网络对等互连。 在两个虚拟网络之间建立对等互连可让不同虚拟网络中的资源以相同的带宽和延迟彼此通信，就像这些资源位于同一个虚拟网络中一样。 只能在同一 Azure 区域中的两个虚拟网络之间创建虚拟网络对等互连。 若要了解有关虚拟网络对等互连的详细信息，请参阅[虚拟网络对等互连](virtual-network-peering-overview.md)概述文章。 

如果需要连接不同 Azure 区域中的虚拟网络，可以使用 Azure [VPN 网关](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fvirtual-network%2ftoc.json)进行连接。 

可以使用 [Azure 门户](#portal)、Azure [PowerShell](#powershell)、Azure [命令行界面](#cli) (CLI) 或 [Azure Resource Manager 模板](#template)创建虚拟网络对等互连。 单击以前的任何工具链接即可直接转到使用所选工具创建虚拟网络对等互连的步骤。

## <a name="portal"></a>创建对等互连 - Azure 门户

完成以下步骤即可在通过 Resource Manager 部署的两个虚拟网络（存在于同一订阅中）之间创建虚拟网络对等互连。 还可以[在不同订阅中的两个虚拟网络之间创建对等互连](#different-subscriptions)，或者[在一个 Resource Manager 虚拟网络与一个经典虚拟网络之间创建对等互连](#different-models)。 

1. 登录到 [Azure 门户](https://portal.azure.cn)。 用于登录的帐户必须具有创建虚拟网络对等互连的必要权限。 有关详细信息，请参阅本文的[权限](#permissions)部分。
2. 依次单击“+ 新建”、“网络”、“虚拟网络”。
3. 在“虚拟网络”边栏选项卡的“选择部署模型”框中，让“Resource Manager”处于选中状态，然后单击“创建”。
4. 在“创建虚拟网络”边栏选项卡中，为以下设置输入或选择值，然后单击“创建”：
    - 名称：myVnet1
    - 地址空间：10.0.0.0/16
    - 子网名称：默认值
    - 子网地址范围：10.0.0.0/24
    - 订阅：选择订阅
    - 资源组：选择“新建”并输入 myResourceGroup
    - 位置：中国东部
5. 再次完成步骤 2-4，在步骤 4 中指定以下值：
    - 名称：myVnet2
    - 地址空间：10.1.0.0/16
    - 子网名称：默认值
    - 子网地址范围：10.1.0.0/24
    - 订阅：选择订阅
    - 资源组：选择“使用现有资源组”，然后选择“myResourceGroup”
    - 位置：中国东部
6. 在门户顶部的“搜索资源”框中键入 myResourceGroup。 当“myResourceGroup”出现在搜索结果中时，请单击它。 随后将显示 myresourcegroup 资源组的边栏选项卡。 该资源组包含前面步骤中创建的两个虚拟网络。
7. 单击“myVNet1”。
8. 在显示的“myVnet1”边栏选项卡中，单击左侧垂直选项列表中的“对等互连”。
9. 在显示的“myVnet1 - 对等互连”边栏选项卡中，单击“+ 添加”
10. 在显示的“添加对等互连”边栏选项卡中，输入或选择以下选项，然后单击“确定”：
     - 名称：myVnet1ToMyVnet2
     - 虚拟网络部署模型：选择“Resource Manager”。 
     - 订阅：选择订阅
     - 虚拟网络：单击“选择虚拟网络”，然后单击“myVnet2”。
     - 允许虚拟网络访问：确保选择“启用”。
    本教程不使用其他任何设置。 若要了解所有对等互连设置，请阅读[管理虚拟网络对等互连](virtual-network-manage-peering.md#create-peering)。
11. 在上一步骤中单击“确定”后，“添加对等互连”边栏选项卡将会关闭，并再次出现“myVnet1 - 对等互连”边栏选项卡。 几秒钟后，创建的对等互连将显示在该边栏选项卡中。 所创建的 myVnet1ToMyVnet2 对等互连的“对等互连状态”列中列出了“已启动”。 已将 Vnet1 对等互连到 Vnet2，但现在必须将 Vnet2 对等互连到 Vnet1。 必须朝两个方向创建对等互连才能让虚拟网络中的资源相互通信。
12. 针对 myVnet2 再次完成步骤 6-11。  将对等互连命名为 myVnet2ToMyVnet1。
13. 单击“确定”创建 MyVnet2 的对等互连几秒钟后，将会列出刚刚创建的 myVnet2ToMyVnet1 对等互连，“对等互连状态”列中显示“已连接”。
14. 针对 MyVnet1 再次完成步骤 6-8。 myVnet1ToVNet2 对等互连的“对等互连状态”现在也显示为“已连接”。 对等互连中两个虚拟网络的“对等互连状态”列都显示为“已连接”后，即表示已成功建立对等互连。
15. 可选：尽管本教程未介绍如何创建虚拟机，但你可以在每个虚拟网络中创建一个虚拟机并将其相互连接，以验证连接性。
16. 可选：若要删除在本教程中创建的资源，请完成本文[删除资源](#delete-portal)部分所述步骤。

在任一虚拟网络中创建的任何 Azure 资源现在都可通过其 IP 地址相互通信。 如果为虚拟网络使用默认的 Azure 名称解析，则虚拟网络中的资源无法跨虚拟网络解析名称。 若要跨对等互连中的虚拟网络解析名称，必须创建自己的 DNS 服务器。 了解如何设置[使用你自己的 DNS 服务器的名称解析](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server)。

## <a name="cli"></a>创建对等互连 - Azure CLI

以下脚本：

- 需要 Azure CLI 2.0.4 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行升级，请参阅[安装 Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?toc=%2fvirtual-network%2ftoc.json)。
- 可以在 Bash shell 中使用。 有关在 Windows 客户端上运行 Azure CLI 脚本的选项，请参阅[在 Windows 中运行 Azure CLI](../virtual-machines/windows/cli-options.md?toc=%2fvirtual-network%2ftoc.json)。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

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
    #
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
    #
    ```
3. 执行该脚本后，查看每个虚拟网络的对等互连。 复制以下命令，然后将其粘贴到命令窗口中：

    ```azurecli
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    #
    ```
再次运行上述命令，但请将 myVnet1 替换为 myVnet2。 这两条命令的输出将在 PeeringState 列中显示 Connected。
4. 可选：尽管本教程未介绍如何创建虚拟机，但你可以在每个虚拟网络中创建一个虚拟机并将其相互连接，以验证连接性。
5. 可选：若要删除在本教程中创建的资源，请完成本文[删除资源](#delete-cli)部分所述步骤。

在任一虚拟网络中创建的任何 Azure 资源现在都可通过其 IP 地址相互通信。 如果为虚拟网络使用默认的 Azure 名称解析，则虚拟网络中的资源无法跨虚拟网络解析名称。 若要跨对等互连中的虚拟网络解析名称，必须创建自己的 DNS 服务器。 了解如何设置[使用你自己的 DNS 服务器的名称解析](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server)。

## <a name="powershell"></a>创建对等互连 - PowerShell

1. 安装最新版本的 PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) 模块。 如果不熟悉 Azure PowerShell，请参阅 [Azure PowerShell 概述](https://docs.microsoft.com/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json)。
2. 若要启动 PowerShell 会话，请转到“开始”，输入 powershell，然后单击“PowerShell”。
3. 在 PowerShell 窗口中，输入 `login-azurermaccount` 命令登录到 Azure。 用于登录的帐户必须具有创建虚拟网络对等互连的必要权限。 有关详细信息，请参阅本文的[权限](#permissions)部分。
4. 在浏览器中复制以下脚本，然后在 PowerShell 窗口中单击右键，执行用于创建一个资源组和两个虚拟网络的脚本：
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
    #
    ```
5. 在浏览器中复制以下脚本，然后在 PowerShell 窗口中单击右键，执行用于在两个虚拟网络之间创建虚拟网络对等互连的脚本：
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
    #
    ```
6. 若要查看虚拟网络的子网，请复制以下命令，然后将其粘贴到 PowerShell 窗口中：

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    #
    ```

再次运行上述命令，但请将 myVnet1 替换为 myVnet2。 这两条命令的输出将在 PeeringState 列中显示 Connected。
7. 可选：尽管本教程未介绍如何创建虚拟机，但你可以在每个虚拟网络中创建一个虚拟机并将其相互连接，以验证连接性。
8. 可选：若要删除在本教程中创建的资源，请完成本文[删除资源](#delete-powershell)部分所述步骤。

在任一虚拟网络中创建的任何 Azure 资源现在都可通过其 IP 地址相互通信。 如果为虚拟网络使用默认的 Azure 名称解析，则虚拟网络中的资源无法跨虚拟网络解析名称。 若要跨对等互连中的虚拟网络解析名称，必须创建自己的 DNS 服务器。 了解如何设置[使用你自己的 DNS 服务器的名称解析](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server)。

## <a name="template"></a>创建对等互连 - Resource Manager 模板

1. 请参考[创建虚拟网络对等互连](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-to-vnet-peering) Resource Manager 模板。 此文提供了相关说明，以及用于通过 Azure 门户、PowerShell 或 Azure CLI 部署模板的模板。 登录到所选的任何工具，以便使用有权创建虚拟网络对等互连的帐户来部署模板。 有关详细信息，请参阅本文的[权限](#permissions)部分。
2. 可选：尽管本教程未介绍如何创建虚拟机，但你可以在每个虚拟网络中创建一个虚拟机并将其相互连接，以验证连接性。
3. 可选：若要删除在本教程中创建的资源，请使用 Azure 门户、PowerShell 或 Azure CLI 完成本文[删除资源](#delete)部分所述步骤。

## <a name="different-models"></a>将通过不同部署模型创建的虚拟网络对等互连

前面部分中的步骤介绍了如何在通过 Resource Manager 部署模型创建的两个虚拟网络之间创建虚拟网络对等互连。 还可以在 Resource Manager 虚拟网络与经典虚拟网络之间创建对等互连。 不能在通过经典部署模型创建的两个虚拟网络之间创建虚拟网络对等互连。 若要详细了解 Azure 部署模型，请阅读[了解 Azure 部署模型](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fvirtual-network%2ftoc.json)一文。

若要创建虚拟网络对等互连，请完成本文[门户](#portal)部分所述的步骤 1-11，但在步骤 3 中创建 myVnet2 时要选择“经典”部署模型。

在 Resource Manager 虚拟网络与经典虚拟网络之间创建虚拟网络对等互连时，只需配置从 Resource Manager 虚拟网络到经典虚拟网络的对等互连。 创建 Resource Manager 虚拟网络的对等互连后，其对等互连状态为“正在更新”。 几秒钟后，状态将自动更改为“已连接”，因为不需要为经典虚拟网络创建对等互连。 

> [!NOTE]
> 尽管本教程未介绍相应的操作，但你可以根据本部分所述过程差异，调整本文 [Azure CLI](#cli) 和 [PowerShell](#powershell) 部分所述脚本。

## <a name="different-subscriptions"></a>为不同订阅中的虚拟网络建立对等互连

前面部分中的步骤介绍了如何在同一订阅中的两个虚拟网络之间创建虚拟网络对等互连。 还可以在不同订阅中的虚拟网络之间创建对等互连，前提是两个订阅与同一个 Azure Active Directory 租户相关联。 如果还没有 AD 租户，可以快速[创建一个](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fvirtual-network%2ftoc.json#start-from-scratch)。 如果订阅与不同的 Active Directory 租户相关联，则无法在不同订阅中的虚拟网络之间创建虚拟网络对等互连。 创建对等互连的步骤稍有不同，具体取决于创建虚拟网络时所用的部署模型，如下所述：

### <a name="different-subscriptions-same-deployment-model"></a>两个虚拟网络都是通过 Resource Manager 创建的

1. 完成本文[门户](#portal)部分所述的步骤 1-7，但在执行步骤 5 时需要为 myVnet2 选择不同的订阅（与同一 Azure Active Directory 租户关联的订阅），而不要选择执行步骤 4 时为 myVnet1 选择的订阅。
2. 在显示的“myVnet1”边栏选项卡中，单击左侧垂直选项列表中的“访问控制(IAM)”。
3. 在显示的“myVnet1 - 访问控制(IAM)”边栏选项卡中，单击“+ 添加”。
4. 在显示的“添加权限”边栏选项卡上的“角色”框中，选择“网络参与者”。
5. 在“选择”框中，选择一个用户名，或者键入一个电子邮件地址来搜索用户名。 显示的用户列表与要为其设置对等互连的虚拟网络来自同一个 Azure Active Directory 租户。
6. 单击“保存” 。
7. 在“myVnet1 - 访问控制(IAM)”边栏选项卡中，单击左侧垂直选项列表中的“属性”。 复制“资源 ID”，如以下示例所示：/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet1
8. 针对 myVnet2 完成本部分所述的步骤 1-7。
9. 为确保成功启用授权，请从 Azure 注销，然后重新登录。 如果为两个虚拟网络使用了不同的帐户，请注销这两个帐户，然后重新登录到这两个帐户。
10. 在门户顶部的“搜索资源”框中键入 myResourceGroup。 当“myResourceGroup”出现在搜索结果中时，请单击它。 随后将显示 myresourcegroup 资源组的边栏选项卡。 该资源组包含前面步骤中创建的两个虚拟网络。
11. 单击“myVNet1”。
12. 在显示的“myVnet1”边栏选项卡中，单击左侧垂直选项列表中的“对等互连”。
13. 在显示的“myVnet1 - 对等互连”边栏选项卡中，单击“+ 添加”
14. 在显示的“添加对等互连”边栏选项卡中，输入或选择以下选项，然后单击“确定”：
     - 名称：myVnet1ToMyVnet2
     - 虚拟网络部署模型：选择“Resource Manager”。 
     - 我知道我的资源 ID：选中此框。
     - 资源 ID：输入 myVnet2 的资源 ID。 仅当已选中“我知道我的资源 ID”复选框时，才显示此框。
     - 允许虚拟网络访问：确保选择“启用”。
    本教程不使用其他任何设置。 若要了解所有对等互连设置，请阅读[管理虚拟网络对等互连](virtual-network-manage-peering.md#create-peering)。
15. 在上一步骤中单击“确定”后，“添加对等互连”边栏选项卡将会关闭，并再次出现“myVnet1 - 对等互连”边栏选项卡。 几秒钟后，创建的对等互连将显示在该边栏选项卡中。 所创建的 myVnet1ToMyVnet2 对等互连的“对等互连状态”列中列出了“已启动”。 已将 Vnet1 对等互连到 Vnet2，但现在必须将 Vnet2 对等互连到 Vnet1。 必须朝两个方向创建对等互连才能让虚拟网络中的资源相互通信。
16. 针对 myVnet2 再次完成本部分所述的步骤 10-14。  将对等互连命名为 myVnet2ToMyVnet1，并在“资源 ID”中输入 myVnet1 的资源 ID。
17. 单击“确定”创建 MyVnet2 的对等互连几秒钟后，将会列出刚刚创建的 myVnet2ToMyVnet1 对等互连，“对等互连状态”列中显示“已连接”。
18. 针对 MyVnet1 再次完成本部分所述的步骤 10-11。 myVnet1ToVNet2 对等互连的“对等互连状态”现在也显示为“已连接”。 对等互连中两个虚拟网络的“对等互连状态”列都显示为“已连接”后，即表示已成功建立对等互连。
19. 可选：尽管本教程未介绍如何创建虚拟机，但你可以在每个虚拟网络中创建一个虚拟机并将其相互连接，以验证连接性。
20. 可选：若要删除在本教程中创建的资源，请完成本文[删除资源](#delete-portal)部分所述步骤。

### <a name="different-subscriptions-different-deployment-models"></a>一个 Resource Manager 虚拟网络，一个经典虚拟网络

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-preview.md)]

1. 注册预览版。 只有在注册预览版后，才可以为不同订阅中通过不同部署模型创建的虚拟网络创建对等互连。 无法使用门户或 Azure CLI 注册预览版。 只能使用 PowerShell 注册预览版。 若要注册预览版，请完成以下任务：
    - 安装最新版本的 PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) 模块。 如果不熟悉 Azure PowerShell，请参阅 [Azure PowerShell 概述](https://docs.microsoft.com/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json)。
    - 若要从 Windows 电脑启动 PowerShell 会话，请转到“开始”，输入 powershell，然后单击“PowerShell”。
    - 在 PowerShell 窗口中，输入 `login-azurermaccount` 命令登录到 Azure。 用于登录的帐户必须具有创建虚拟网络对等互连的必要权限。 有关详细信息，请参阅本文的[权限](#permissions)部分。
    - 为两个 Azure 订阅注册预览版功能，然后通过 PowerShell 输入以下命令： 

    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    #
    ```
    请在登录到每个订阅的情况下注册预览版。 只有在输入以下命令之后，收到的 RegistrationState 输出在两个订阅中显示为 Registered 时，才执行后续步骤。

    ```powershell
    Get-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    #
    ```

2. 完成本文[门户](#portal)部分所述步骤 1-7，但要进行以下更改：
    - 执行步骤 5 时需要为 myVnet2 选择不同的订阅（与同一 Azure Active Directory 租户关联的订阅），而不要选择执行步骤 4 时为 myVnet1 选择的订阅。
    - 创建 myVnet2 时，请在步骤 3 中选择“经典”。
3. 在门户顶部的“搜索资源”框中键入 myResourceGroup。 当“myResourceGroup”出现在搜索结果中时，请单击它。 随后将显示 myresourcegroup 资源组的边栏选项卡。 该资源组包含前面步骤中创建的两个虚拟网络。
4. 单击“myVNet1”。
5. 完成本文[两个虚拟网络都是通过 Resource Manager 创建的](#different-subscriptions-same-deployment-model)部分所述步骤 2-9。
6. 在门户顶部的“搜索资源”框中键入 myResourceGroup。 当“myResourceGroup”出现在搜索结果中时，请单击它。 随后将显示 myresourcegroup 资源组的边栏选项卡。 该资源组包含前面步骤中创建的两个虚拟网络。
7. 单击“myVNet1”。
8. 在显示的“myVnet1”边栏选项卡中，单击左侧垂直选项列表中的“对等互连”。
9. 在显示的“myVnet1 - 对等互连”边栏选项卡中，单击“+ 添加”
10. 在显示的“添加对等互连”边栏选项卡中，输入或选择以下选项，然后单击“确定”：
     - 名称：myVnet1ToMyVnet2
     - 虚拟网络部署模型：选择“经典”。 
     - 我知道我的资源 ID：选中此框。
     - 资源 ID：输入 myVnet2 的资源 ID。 仅当已选中“我知道我的资源 ID”复选框时，才显示此框。
     - 允许虚拟网络访问：确保选择“启用”。
    本教程不使用其他任何设置。 若要了解所有对等互连设置，请阅读[管理虚拟网络对等互连](virtual-network-manage-peering.md#create-peering)。

    在 Resource Manager 虚拟网络与经典虚拟网络之间创建虚拟网络对等互连时，只需配置从 Resource Manager 虚拟网络到经典虚拟网络的对等互连。 创建 Resource Manager 虚拟网络的对等互连后，其对等互连状态为“正在更新”。 几秒钟后，状态将自动更改为“已连接”，因为不需要为经典虚拟网络创建对等互连。 
11. 可选：尽管本教程未介绍如何创建虚拟机，但你可以在每个虚拟网络中创建一个虚拟机并将其相互连接，以验证连接性。
12. 可选：若要删除在本教程中创建的资源，请完成本文[删除资源](#delete-portal)部分所述步骤。

> [!NOTE]
> 尽管本教程未介绍相应的操作，但你可以根据本部分所述过程差异，调整本文 [Azure CLI](#cli) 和 [PowerShell](#powershell) 部分所述脚本。

## <a name="permissions"></a>权限

用于创建虚拟网络对等互连的帐户必须具有所需的角色或权限。 例如，如果在分别名为 VNet1 和 VNet2 的两个虚拟网络之间创建了对等互连，则必须为帐户分配针对每个虚拟网络的最小角色或权限：

|虚拟网络|部署模型|角色|权限|
|---|---|---|---|
|VNet1|Resource Manager|[网络参与者](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |经典|[经典网络参与者](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#classic-network-contributor)|不适用|
|VNet2|Resource Manager|[网络参与者](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||经典|[经典网络参与者](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

详细了解[内置角色](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)以及将特定的权限分配到[自定义角色](../active-directory/role-based-access-control-custom-roles.md?toc=%2fvirtual-network%2ftoc.json)（仅限 Resource Manager）。

## <a name="delete"></a>删除资源
完成本教程后，可能需要删除本教程中创建的资源，以免产生使用费。 删除资源组会删除其中包含的所有资源。

### <a name="delete-portal"></a>Azure 门户

1. 在门户的搜索框中，输入 myResourceGroup。 在搜索结果中，单击“myResourceGroup”。
2. 在“myResourceGroup”边栏选项卡中，单击“删除”图标。
3. 若要确认删除，请在“键入资源组名称”框中输入 myResourceGroup，然后单击“删除”。

### <a name="delete-cli"></a>Azure CLI

在 Linux、macOS 或 Windows 命令 shell 中，输入以下命令。

```azurecli
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

在 PowerShell 命令提示符下输入以下命令：

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

- 在针对生产用途创建虚拟网络对等互连之前，请充分熟悉重要的[虚拟网络对等互连约束和行为](virtual-network-manage-peering.md#about-peering)。
- 了解所有的[虚拟网络对等互连设置](virtual-network-manage-peering.md#create-peering)。
- 了解如何使用虚拟网络对等互连[创建中心辐射型网络拓扑](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering)。