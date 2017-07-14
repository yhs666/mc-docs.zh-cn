---
title: "Azure 快速入门 - 创建 VM CLI | Azure"
description: "快速了解如何使用 Azure CLI 创建虚拟机。"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 05/11/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.custom: mvc
ms.openlocfilehash: c605c5030402808cde858a6efefec2c887a4cc21
ms.sourcegitcommit: 51a25dbbf5f32fe524860b1bb107108122b47bf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# 使用 Azure CLI 创建 Linux 虚拟机
<a id="create-a-linux-virtual-machine-with-the-azure-cli" class="xliff"></a>

Azure CLI 用于从命令行或脚本创建和管理 Azure 资源。 本指南详细介绍了如何使用 Azure CLI 部署运行 Ubuntu 服务器的虚拟机。 服务器部署以后，将创建 SSH 连接，并且安装 NGINX webserver。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

本快速入门需要 Azure CLI 2.0.4 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)。 

## 登录 Azure
<a id="log-in-to-azure" class="xliff"></a> 

使用 [az login](https://docs.microsoft.com/cli/azure/#login) 命令登录到 Azure 订阅，并按照屏幕上的说明进行操作。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

```azurecli 
az login
```

## 创建资源组
<a id="create-a-resource-group" class="xliff"></a>

使用 [az group create](https://docs.microsoft.com/cli/azure/group#create) 命令创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源的逻辑容器。 

以下示例在“chinaeast”位置创建名为“myResourceGroup”的资源组。

```azurecli 
az group create --name myResourceGroup --location chinaeast
```

## 创建虚拟机
<a id="create-virtual-machine" class="xliff"></a>

使用 [az vm create](https://docs.microsoft.com/cli/azure/vm#create) 命令创建 VM。 

下面的示例创建一个名为 *myVM* 的 VM，并且在默认密钥位置中不存在 SSH 密钥时创建这些密钥。 若要使用特定的一组密钥，请使用 `--ssh-key-value` 选项。  

```azurecli 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

创建 VM 后，Azure CLI 将显示类似于以下示例的信息。 记下 `publicIpAddress`。 此地址用于访问 VM。

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "chinaeast",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## 为 Web 流量打开端口 80
<a id="open-port-80-for-web-traffic" class="xliff"></a> 

默认情况下，仅允许通过 SSH 连接登录到 Azure 中部署的 Linux 虚拟机。 如果此 VM 将用作 Web 服务器，则需要从 Internet 打开端口 80。 使用 [az vm open-port](https://docs.microsoft.com/cli/azure/vm#open-port) 命令打开所需端口。  

 ```azurecli 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## 通过 SSH 连接到你的 VM
<a id="ssh-into-your-vm" class="xliff"></a>

使用以下命令创建与虚拟机的 SSH 会话。 确保将 *<publicIpAddress>* 替换为虚拟机的相应公共 IP 地址。  在上例中，我们的 IP 地址为 *40.68.254.142*。

```bash 
ssh <publicIpAddress>
```

## 安装 NGINX
<a id="install-nginx" class="xliff"></a>

使用以下 bash 脚本更新包源并安装最新的 NGINX 包。 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## 查看 NGINX 欢迎页
<a id="view-the-nginx-welcome-page" class="xliff"></a>

NGINX 已安装，并且现在已从 Internet 打开 VM 上的端口 80 - 可以使用所选的 Web 浏览器查看默认的 NGINX 欢迎页。 请务必使用前面记录的 *publicIpAddress* 访问默认页面。 

![NGINX 默认站点](./media/quick-create-cli/nginx.png) 

## 清理资源
<a id="clean-up-resources" class="xliff"></a>

如果不再需要资源组、VM 和所有相关的资源，可以使用 [az group delete](https://docs.microsoft.com/cli/azure/group#delete) 命令将其删除。

```azurecli 
az group delete --name myResourceGroup
```

## 后续步骤
<a id="next-steps" class="xliff"></a>

在本快速入门中，部署了一个简单的虚拟机、一条网络安全组规则，并安装了一个 Web 服务器。 若要详细了解 Azure 虚拟机，请继续学习 Linux VM 的教程。

> [!div class="nextstepaction"]
> [Azure Linux 虚拟机教程](./tutorial-manage-vm.md)
