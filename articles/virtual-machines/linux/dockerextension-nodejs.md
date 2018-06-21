---
title: 将 Azure Docker VM 扩展与 Azure CLI 1.0 配合使用 | Azure
description: 了解如何使用 Docker VM 扩展快速安全地在 Azure 中通过 Resource Manager 模板部署 Docker 环境。
services: virtual-machines-linux
documentationcenter: ''
author: hayley244
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 05/11/2017
ms.date: 09/04/2017
ms.author: v-haiqya
ms.openlocfilehash: 4eb922ad1fa9555375b33a857ecb2f6fb12d0fa2
ms.sourcegitcommit: da549f499f6898b74ac1aeaf95be0810cdbbb3ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2017
ms.locfileid: "21925806"
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension-with-the-azure-cli-10"></a>在 Azure 中通过 Azure CLI 1.0 使用 Docker VM 扩展创建 Docker 环境
Docker 是流行的容器管理和映像处理平台，可让你在 Linux（和 Windows）上快速操作容器。 在 Azure 中，可以根据需要使用各种方式部署 Docker。 本文重点介绍如何使用 Docker VM 扩展和 Azure Resource Manager 模板。 

有关不同部署方法的详细信息，包括如何使用 Docker Machine，请参阅以下文章：

* 若要快速建立应用原型，可以使用 [Docker Machine](docker-machine.md) 创建一个 Docker 主机。
* 对于更大、更稳定的环境，可以使用 Azure Docker VM 扩展，它还支持 [Docker Compose](https://docs.docker.com/compose/overview/) ，可生成一致的容器部署。 本文详细介绍如何使用 Azure Docker VM 扩展。

## <a name="cli-versions-to-complete-the-task"></a>用于完成任务的 CLI 版本
可使用以下 CLI 版本之一完成任务：

- [Azure CLI 1.0](#azure-docker-vm-extension-overview) - 适用于经典部署模型和资源管理部署模型（本文）的 CLI
- [Azure CLI 2.0](dockerextension.md) - 适用于资源管理部署模型的下一代 CLI 

## <a name="azure-docker-vm-extension-overview"></a>Azure Docker VM 扩展概述
Azure Docker VM 扩展在 Linux 虚拟机 (VM) 中安装并配置 Docker 守护程序、Docker 客户端和 Docker Compose。 使用 Azure Docker VM 扩展，可以拥有更多的控制和功能，而不仅仅是使用 Docker Machine 或自行创建 Docker 主机。 借助这些附加功能（例如 [Docker Compose](https://docs.docker.com/compose/overview/)），Azure Docker VM 扩展可用于更可靠的开发人员或生产环境。

Azure Resource Manager 模板定义环境的整个结构。 模板可用于创建和配置资源，例如 Docker 主机 VM、存储、基于角色的访问控制 (RBAC) 和诊断功能。 可重复使用这些模板，以一致的方式创建其他部署。 有关 Azure Resource Manager 和模板的详细信息，请参阅 [Resource Manager 概述](../../azure-resource-manager/resource-group-overview.md)。 

## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>使用 Azure Docker VM 扩展部署模板
让我们使用现有的快速入门模板，创建使用 Azure Docker VM 扩展来安装和配置 Docker 主机的 Ubuntu VM。 可以在此处查看模板： [使用 Docker 轻松部署 Ubuntu VM](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)。 

>[!NOTE]
> 必须修改从 GitHub 存储库“azure-quickstart-templates”部署的模板，以适应 Azure 中国云环境。 例如，替换某些终结点 -- 将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.azure.com”替换为“chinacloudapp.cn”。

需要安装[最新的 Azure CLI](../../cli-install-nodejs.md)，然后按如下所示，使用 Resource Manager 模式登录：

```azurecli
azure config mode arm
```

使用 Azure CLI 部署模板，并指定模板 URI。 以下示例在“chinanorth”位置创建名为“myResourceGroup”的资源组。 使用自己的资源组名称和位置，如下所示：

```azurecli
azure group create \
    --name myResourceGroup \
    --location chinanorth \
    --template-file /path/to/azuredeploy.json
```

根据提示命名存储帐户，提供用户名和密码以及 DNS 名称。 输出类似于以下示例：

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: azureuser
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicidns
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            chinanorth
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Azure CLI 仅在几秒后就会返回提示符，但 Azure Docker VM 扩展仍在创建和配置 Docker 主机。 需要几分钟才能完成部署。 可以使用 `azure vm show` 命令查看有关 Docker 主机状态的详细信息。

以下示例在名为 myResourceGroup 的资源组中检查名为 myDockerVM（模板中的默认名称 - 请不要更改该名称）的 VM 的状态。 输入在上一步中创建的资源组的名称：

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

`azure vm show` 命令的输出类似于以下示例：

```azurecli
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :chinanorth
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :chinanorth
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicdns.chinanorth.chinacloudapp.cn
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

在输出的顶部附近，可看到 VM 的 **ProvisioningState**。 当此项显示“已成功”时，表示部署完毕，此时可通过 SSH 连接到 VM。

在输出快到末尾的位置，*FQDN* 显示 Docker 主机的完全限定域名。 在剩余步骤中，会使用此 FQDN 通过 SSH 连接到 Docker 主机。

## <a name="deploy-your-first-nginx-container"></a>部署第一个 nginx 容器
完成部署后，通过 SSH 从本地计算机连接到新的 Docker 主机。 输入自己的用户名和 FQDN，如下所示：

```bash
ssh ops@mypublicdns.chinanorth.chinacloudapp.cn
```

登录 Docker 主机后，请运行 nginx 容器：

```bash
sudo docker run -d -p 80:80 nginx
```

下载 nginx 映像并启动容器时，输出类似于以下示例：

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

检查在 Docker 主机上运行的容器的状态，如下所示：

```bash
sudo docker ps
```

输出类似于以下示例，显示 nginx 容器正在运行，以及正在转发 TCP 端口 80 和 443：

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

若要查看容器的运行情况，请打开 Web 浏览器并输入 Docker 主机的 FQDN 名称：

![运行 ngnix 容器](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker VM 扩展模板参考
之前的示例使用现有的快速入门模板。 还可使用自己的 Resource Manager 模板部署 Azure Docker VM 扩展。 为此，请将以下内容添加到 Resource Manager 模板，并适当地定义 VM 的 *vmName*：

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

有关更详细的 Resource Manager 模板用法演练，请阅读 [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md)（Azure Resource Manager 概述）。

## <a name="next-steps"></a>后续步骤
可能需要使用 [Docker Compose](https://docs.docker.com/compose/overview/) [配置 Docker 守护程序 TCP 端口](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket)，了解 [Docker 安全性](https://docs.docker.com/engine/security/security/)或部署容器。 有关 Azure Docker VM 扩展本身的详细信息，请参阅 [GitHub 项目](https://github.com/Azure/azure-docker-extension/)。

阅读有关 Azure 中其他 Docker 部署选项的详细信息：

* [通过 Azure 驱动程序使用 Docker Machine](docker-machine.md)  
* [开始使用 Docker 和 Compose，在 Azure 虚拟机上定义和运行多容器应用程序](docker-compose-quickstart.md)。