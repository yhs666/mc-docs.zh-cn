---
title: "Azure 快速入门 - 创建 Windows VM CLI | Azure"
description: "快速了解如何使用 Azure CLI 创建 Windows 虚拟机。"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 05/11/2017
ms.date: 10/16/2017
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 6aa91b9ca3be582dba41f4509e625952c18cccfc
ms.sourcegitcommit: f69d54334a845e6084e7cd88f07714017b5ef822
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="create-a-windows-virtual-machine-with-the-azure-cli"></a>使用 Azure CLI 创建 Windows 虚拟机

Azure CLI 用于从命令行或脚本创建和管理 Azure 资源。 本指南详细介绍如何使用 Azure CLI 部署运行 Windows Server 2016 的虚拟机。 部署完成后，我们连接到服务器并安装 IIS。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，此快速入门教程要求运行 Azure CLI 2.0.4 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。 

## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#create) 创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源的逻辑容器。 

以下示例在“chinaeast”位置创建名为“myResourceGroup”的资源组。

```azurecli 
az group create --name myResourceGroup --location chinaeast
```

## <a name="create-virtual-machine"></a>创建虚拟机

使用 [az vm create](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#az_vm_create) 创建 VM。 

以下示例创建一个名为 myVM 的 VM。 此示例使用 azureuser 作为管理用户名，使用 myPassword12 作为密码。 更新这些值，使其适用于环境。 创建与虚拟机的连接时，需要这些值。

```azurecli 
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
```

创建 VM 后，Azure CLI 显示类似于以下示例的信息。 记下 `publicIpAaddress`。 此地址用于访问 VM。

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "chinaeast",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>为 Web 流量打开端口 80 

默认情况下，仅允许通过 RDP 连接登录到 Azure 中部署的 Windows 虚拟机。 如果此 VM 会用作 Web 服务器，则需要从 Internet 打开端口 80。 使用 [az vm open-port](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#open-port) 命令打开所需端口。  

 ```azurecli  
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-virtual-machine"></a>连接到虚拟机

使用以下命令创建与虚拟机的远程桌面会话。 将 IP 地址替换为虚拟机的公共 IP 地址。 出现提示时，输入创建虚拟机时使用的凭据。

```bash 
mstsc /v:<Public IP Address>
```

## <a name="install-iis-using-powershell"></a>使用 PowerShell 安装 IIS

登录到 Azure VM 后，可以使用单行 PowerShell 安装 IIS，并启用本地防火墙规则以允许 Web 流量。 打开 PowerShell 提示符并运行以下命令：

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>查看 IIS 欢迎页

IIS 已安装，并且现在已从 Internet 打开 VM 上的端口 80 - 可以使用所选的 Web 浏览器查看默认的 IIS 欢迎页。 请务必使用前面记录的公共 IP 地址访问默认页面。 

![IIS 默认站点](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组、VM 和所有相关的资源，可以使用 [az group delete](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#delete) 命令将其删除。

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

在本快速入门中，部署了一个简单的虚拟机、一条网络安全组规则，并安装了一个 Web 服务器。 若要深入了解 Azure 虚拟机，请继续学习 Windows VM 教程。

> [!div class="nextstepaction"]
> [Azure Windows 虚拟机教程](./tutorial-manage-vm.md)

<!--Update_Description: wording update-->