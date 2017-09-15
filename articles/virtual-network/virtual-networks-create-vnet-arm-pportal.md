---
title: "创建包含多个子网的 Azure 虚拟网络 | Azure"
description: "了解如何在 Azure 中创建包含多个子网的虚拟网络。"
services: virtual-network
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 07/26/2017
ms.date: 09/04/2017
ms.author: v-yeche
ms.custom: 
ms.openlocfilehash: 4a8c3a3af511c36f46e2c104697594497b3c07cd
ms.sourcegitcommit: 095c229b538d9d2fc51e007abe5fde8e46296b4f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2017
---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>创建包含多个子网的虚拟网络

本教程介绍如何创建包含独立公共子网和专用子网的基本 Azure 虚拟网络。 可在子网中创建 Azure 资源，例如虚拟机、应用服务环境、虚拟机规模集、Azure HDInsight 和云服务。 虚拟网络中的资源可以彼此通信，并可以与连接到虚拟网络的其他网络中的资源通信。

以下部分提供了使用 [Azure 门户](#portal)、Azure 命令行接口 ([Azure CLI](#azure-cli))、[Azure PowerShell](#powershell) 和 [Azure 资源管理器模板](#resource-manager-template)创建虚拟网络的步骤。 不管使用哪种工具来创建虚拟网络，结果都是一样的。 单击工具链接会直接转到本教程的相关部分。 详细了解所有[虚拟网络](virtual-network-manage-network.md)和[子网](virtual-network-manage-subnet.md)设置。

本文提供通过资源管理器部署模型（创建新虚拟网络时建议使用的部署模型）创建虚拟网络的步骤。 如果需要创建虚拟网络（经典），请参阅[创建虚拟网络（经典）](create-virtual-network-classic.md)。 如果不熟悉 Azure 的部署模型，请参阅[了解 Azure 部署模型](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fvirtual-network%2ftoc.json)。

## <a name="portal"></a>Azure 门户

1. 在 Internet 浏览器中，转到 [Azure 门户](https://portal.azure.cn)。 使用 [Azure 帐户](../azure-glossary-cloud-terminology.md?toc=%2fvirtual-network%2ftoc.json#account)登录。 如果没有 Azure 帐户，可以注册 [试用版](https://www.azure.cn/pricing/1rmb-trial-full/)。
2. 在门户中，单击“+新建” > “网络” > “虚拟网络”。
3. 在“创建虚拟网络”边栏选项卡中输入以下值，然后单击“创建”：

    |设置|值|
    |---|---|
    |名称|myVnet|
    |地址空间|10.0.0.0/16|
    |子网名称|公共|
    |子网地址范围|10.0.0.0/24|
    |资源组|保留选中“新建”，输入 **myResourceGroup**。|
    |订阅和位置|选择订阅和位置。

    如果你不熟悉 Azure，请详细了解[资源组](../azure-glossary-cloud-terminology.md?toc=%2fvirtual-network%2ftoc.json#resource-group)、[订阅](../azure-glossary-cloud-terminology.md?toc=%2fvirtual-network%2ftoc.json#subscription)和[位置](https://azure.microsoft.com/regions)（也称为“区域”）。
4. 在门户中创建虚拟网络时，只能创建一个子网。 本教程将在创建虚拟网络后创建第二个子网。 随后可在“公共”子网中创建可通过 Internet 访问的资源。 还可以在“专用”子网中创建无法通过 Internet 访问的资源。 若要创建第二个子网，请在页面顶部的“搜索资源”框中输入 **myVnet**。 在搜索结果中，单击“myVnet”。 如果订阅中有多个同名的虚拟网络，请检查每个虚拟网络下面列出的资源组。 确保单击的是包含“myResourceGroup”资源组的“myVnet”搜索结果。
5. 在“myVnet”边栏选项卡中，单击“设置”下面的“子网”。
6. 在“myVnet - 子网”边栏选项卡中单击“+子网”。
7. 在“添加子网”边栏选项卡中为“名称”输入“专用”。 为“地址范围”输入 **10.0.1.0/24**。  单击 **“确定”**。
8. 在“myVnet - 子网”边栏选项卡中查看子网。 可以看到所创建的“公共”和“专用”子网。
9. **可选**：若要删除在本教程中创建的资源，请完成本文的[删除资源](#delete-portal)中所述的步骤。

## <a name="azure-cli"></a>Azure CLI

无论是在 Windows、Linux 还是 macOS 上，执行 Azure CLI 命令的结果都是相同的。 但是，脚本在不同操作系统 shell 中的结果会有差异。 以下步骤中的脚本在 Bash shell 中执行。 

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]
1. [安装并配置 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)。 确保已安装最新版本的 Azure CLI。 若要获取 CLI 命令的帮助，请键入 `az <command> --help`。
2. 如果在本地运行 CLI，请使用 `az cloud set -n AzureChinaCloud
az login` 命令登录到 Azure。 如果使用的是 Cloud Shell，则已经登录。
3. 查看以下脚本及其注释。 在浏览器中，复制该脚本并将其粘贴到 CLI 会话中：

    ```azurecli
    #!/bin/bash

    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location chinaeast

    # Create a virtual network with one subnet named Public.
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name Public

    # Create an additional subnet named Private in the virtual network.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name myVnet \
      --resource-group myResourceGroup
    ```

4. 运行完脚本后，请查看虚拟网络的子网。 复制以下命令，并将其粘贴到 CLI 会话中：

    ```azurecli
    az network vnet subnet list --resource-group myResourceGroup --vnet-name myVnet --output table
    ```

5. **可选**：若要删除在本教程中创建的资源，请完成本文的[删除资源](#delete-cli)中所述的步骤。

## <a name="powershell"></a>PowerShell

1. 安装最新版本的 PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) 模块。 如果不熟悉 Azure PowerShell，请参阅 [Azure PowerShell 概述](https://docs.microsoft.com/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json)。
2. 在 PowerShell 会话中，使用 `login-azurermaccount` 命令以 [Azure 帐户](../azure-glossary-cloud-terminology.md?toc=%2fvirtual-network%2ftoc.json#account)登录到 Azure。

3. 查看以下脚本及其注释。 在浏览器中，复制该脚本并将其粘贴到 PowerShell 会话中：

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location chinaeast

    # Create the public and private subnets.
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24

    # Create a virtual network.
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Location chinaeast `
      -Name myVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    ```

4. 若要查看虚拟网络的子网，请复制以下命令，并将其粘贴到 PowerShell 会话中：

    ```powershell
    $Vnet.subnets | Format-Table Name, AddressPrefix
    ```

5. **可选**：若要删除在本教程中创建的资源，请完成本文的[删除资源](#delete-powershell)中所述的步骤。

## <a name="resource-manager-template"></a>Resource Manager 模板

可以使用 Azure Resource Manager 模板部署虚拟网络。 若要详细了解模板，请参阅[什么是 Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fvirtual-network%2ftoc.json#template-deployment)。 若要访问模板并了解其参数，请参阅[创建包含两个子网的虚拟网络](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets/)模板。 可以使用[门户](#template-portal)、[Azure CLI](#template-cli) 或 [PowerShell](#template-powershell) 部署模板。

**可选**：若要删除在本教程中创建的资源，请完成本文的所有[删除资源](#delete)小节中所述的步骤。

### <a name="template-portal"></a>Azure 门户

1. 在浏览器中打开[模板页](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)。
2. 单击“部署到 Azure”按钮。 如果尚未登录到 Azure，请在显示的 Azure 门户登录屏幕中登录。
3. 使用 [Azure 帐户](../azure-glossary-cloud-terminology.md?toc=%2fvirtual-network%2ftoc.json#account)登录到门户。 如果没有 Azure 帐户，可以注册 [试用版](https://www.azure.cn/pricing/1rmb-trial-full/)。
4. 输入以下参数值：

    |参数|值|
    |---|---|
    |订阅|选择订阅|
    |资源组|MyResourceGroup|
    |位置|选择位置|
    |VNet 名称|myVnet|
    |VNet 地址前缀|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|公共|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|专用|

5. 同意条款和条件，然后单击“购买”部署虚拟网络。

### <a name="template-cli"></a>Azure CLI

1. [安装并配置 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)。 确保已安装最新版本的 Azure CLI。 若要获取 CLI 命令的帮助，请键入 `az <command> --help`。
2. 如果在本地运行 CLI，请使用 `az cloud set -n AzureChinaCloud
az login` 命令登录到 Azure。 如果使用的是 Cloud Shell，则已经登录。
3. 若要为虚拟网络创建资源组，请复制以下命令，并将其粘贴到 CLI 会话中：

    ```azurecli
    az group create --name myResourceGroup --location chinaeast
    ```

4. 可以使用以下参数选项之一来部署模板：
    - **默认的参数值**。 输入以下命令：

        ```azurecli
        az group deployment create --resource-group myResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
        ```
    - **自定义参数值**。 部署模板之前下载并修改模板。 还可以在命令行中使用参数来部署模板，或使用单独的参数文件来部署模板。 若要下载模板和参数文件，请在[创建包含两个子网的虚拟网络](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets/)模板页上单击“在 GitHub 上浏览”按钮。 在 GitHub 中单击 **azuredeploy.parameters.json** 或 **azuredeploy.json** 文件。 然后单击该文件对应的“Raw”按钮。 在浏览器中，复制该文件的内容。 将内容保存到计算机上的某个文件中。 可以修改模板中的参数值，或使用单独的参数文件部署模板。  

    若要详细了解如何使用这些方法部署模板，请键入 `az group deployment create --help`。

### <a name="template-powershell"></a>PowerShell

1. 安装最新版本的 PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) 模块。 如果不熟悉 Azure PowerShell，请参阅 [Azure PowerShell 概述](https://docs.microsoft.com/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json)。
2. 在 PowerShell 会话中，若要使用 [Azure 帐户](../azure-glossary-cloud-terminology.md?toc=%2fvirtual-network%2ftoc.json#account)登录，请输入 `login-azurermaccount`。
3. 若要为虚拟网络创建资源组，请输入以下命令：

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location chinaeast
    ```

4. 可以使用以下参数选项之一来部署模板：
    - **默认的参数值**。 输入以下命令：

        ```powershell
        New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName myResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json
        ```

    - **自定义参数值**。 部署模板之前下载并修改模板。 还可以在命令行中使用参数来部署模板，或使用单独的参数文件来部署模板。 若要下载模板和参数文件，请在[创建包含两个子网的虚拟网络](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets/)模板页上单击“在 GitHub 上浏览”按钮。 在 GitHub 中单击 **azuredeploy.parameters.json** 或 **azuredeploy.json** 文件。 然后单击该文件对应的“Raw”按钮。 在浏览器中，复制该文件的内容。 将内容保存到计算机上的某个文件中。 可以修改模板中的参数值，或使用单独的参数文件部署模板。  

    若要详细了解如何使用这些方法部署模板，请键入 `Get-Help New-AzureRmResourceGroupDeployment`。 

## <a name="delete"></a>删除资源

完成本教程后，可以删除创建的资源，以免产生使用费。 删除资源组会删除其中包含的所有资源。

### <a name="delete-portal"></a>Azure 门户

1. 在门户的搜索框中，输入 myResourceGroup。 在搜索结果中，单击“myResourceGroup”。
2. 在“myResourceGroup”边栏选项卡中，单击“删除”图标。
3. 若要确认删除，请在“键入资源组名称”框中输入 myResourceGroup，然后单击“删除”。

### <a name="delete-cli"></a>Azure CLI

在 CLI 会话中输入以下命令：

```azurecli
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

在 PowerShell 会话中输入以下命令：

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>后续步骤

- 若要了解有关所有虚拟网络和子网设置的信息，请参阅[管理虚拟网络](virtual-network-manage-network.md#view-vnet)和[管理虚拟网络子网](virtual-network-manage-subnet.md#create-subnet)。 可以根据不同的要求，通过多种选项在生产环境中使用虚拟网络和子网。
- 若要筛选入站和出站子网流量，请创建[网络安全组](virtual-networks-nsg.md)并将其应用到子网。
- 创建 [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fvirtual-network%2ftoc.json) 或 [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fvirtual-network%2ftoc.json) 虚拟机，并将其连接到现有的虚拟网络。
- 若要在同一 Azure 位置连接两个虚拟网络，请在虚拟网络之间创建[虚拟网络对等互连](virtual-network-peering-overview.md)。
- 使用 [VPN 网关](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fvirtual-network%2ftoc.json)或 [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fvirtual-network%2ftoc.json) 线路将虚拟网络连接到本地网络。

<!--Update_Description: wording update, update reference link-->