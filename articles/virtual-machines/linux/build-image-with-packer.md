---
title: "如何使用 Packer 创建 Linux Azure VM 映像 | Azure"
description: "了解如何使用 Packer 在 Azure 中创建 Linux 虚拟机映像"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 06/12/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: f33c64a73a5b92e7685503a74f613de7e70b7c29
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 如何使用 Packer 在 Azure 中创建 Linux 虚拟机映像
<a id="how-to-use-packer-to-create-linux-virtual-machine-images-in-azure" class="xliff"></a>
从定义 Linux 分发版和操作系统版本的映像创建 Azure 中的每个虚拟机 (VM)。 映像可以包括预安装的应用程序和配置。 Azure 应用商店为最常见的分发和应用程序环境提供了许多第一和第三方映像，或者用户可根据需求创建自己的自定义映像。 本文详细介绍如何使用开源工具 [Packer](https://www.packer.io/) 在 Azure 中定义和生成自定义映像。

## 创建支持性 Azure 资源
<a id="create-supporting-azure-resources" class="xliff"></a>
生成过程中，Packer 将在生成源 VM 时创建临时 Azure 资源。 若要捕获该源 VM 用作映像，必须定义资源组和存储帐户。 Packer 生成过程的输出存储于此资源组和存储帐户中。

首先，使用 [az group create](https://docs.microsoft.com/cli/azure/group#create) 创建资源组。 以下示例在“chinaeast”位置创建名为“myResourceGroup”的资源组：

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

```azurecli
az group create -n myResourceGroup -l chinaeast
```

接下来，使用 [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#create) 创建存储帐户。 存储帐户名称必须唯一，长度为 3 到 24 个字符，并且只能包含数字和小写字母。 以下示例创建一个名为 mystorageaccount 的存储帐户：

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --name mystorageaccount \
    --sku Standard_LRS
```

## 创建 Azure 凭据
<a id="create-azure-credentials" class="xliff"></a>
使用服务主体通过 Azure 对 Packer 进行身份验证。 Azure 服务主体是可与应用、服务和自动化工具（如 Packer）结合使用的安全性标识。 用户控制和定义服务主体可在 Azure 中执行的操作的权限。

通过 [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) 创建服务主体并输出 Packer 需要的凭据：

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

前面命令的输出示例如下所示：

```azurecli
"f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
"0e760437-bf34-4aad-9f8d-870be799c55d",
"72f988bf-86f1-41af-91ab-2d7cd011db47"
```

若要进行 Azure 身份验证，还需要通过 [az account show](https://docs.microsoft.com/cli/azure/account#show) 获取 Azure 订阅 ID：

```azurecli
az account show --query [id] --output tsv
```

在下一步中使用这两个命令的输出。

## 定义 Packer 模板
<a id="define-packer-template" class="xliff"></a>
若要生成映像，请创建一个模板作为 JSON 文件。 在模板中，定义执行实际生成过程的生成器和设置程序。 Packer 具有 [Azure 设置程序](https://www.packer.io/docs/builders/azure.html)，允许定义 Azure 资源，如在前一步骤中创建的服务主体凭据。

创建名为 ubuntu.json 的文件并粘贴以下内容。 为以下内容输入自己的值：

| 参数       | 获取位置 |
|-----------------|----------------------------------------------------|
| client_id      | `az ad sp` 创建命令的第一行输出 - appId |
| client_secret  | `az ad sp` 创建命令的第二行输出 - password |
| tenant_id      | `az ad sp` 创建命令的第三行输出 - tenant |
| subscription_id | `az account show` 命令的输出 |
| storage_account | 在 `az storage account create` 中指定的名称 |
| cloud_environment_name | 它是 `AzureChinaCloud` |

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",

    "resource_group_name": "myResourceGroup",
    "storage_account": "mystorageaccount",

    "capture_container_name": "images",
    "capture_name_prefix": "packer",

    "os_type": "Linux",
    "image_publisher": "Canonical",
    "image_offer": "UbuntuServer",
    "image_sku": "16.04.0-LTS",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "cloud_environment_name": "AzureChinaCloud",
    "location": "China East",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "execute_command": "chmod +x {{ .Path }}; {{ .Vars }} sudo -E sh '{{ .Path }}'",
    "inline": [
      "apt-get update",
      "apt-get upgrade -y",
      "apt-get -y install nginx",

      "/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync"
    ],
    "inline_shebang": "/bin/sh -x",
    "type": "shell"
  }]
}
```

此模板生成 Ubuntu 16.04 LTS 映像，请安装 NGINX，然后取消设置 VM。

> [!NOTE]
> 如果在此模板上进行扩展以便设置用户凭据，请将取消设置 Azure 代理的设置程序命令调整为读取 `-deprovision` 而非 `deprovision+user`。
> `+user` 标志从源 VM 中删除所有用户帐户。

## 生成 Packer 映像
<a id="build-packer-image" class="xliff"></a>
如果本地计算机上尚未安装 Packer，请[按照 Packer 安装说明](https://www.packer.io/docs/install/index.html)进行安装。

按如下所述指定 Packer 模板文件，生成映像：

```bash
./packer build ubuntu.json
```

前面命令的输出示例如下所示：

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-hlz1xtcy8n'
==> azure-arm:  -> Location          : 'China East'
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> dept : engineering
==> azure-arm:  ->> task : image deployment
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-hlz1xtcy8n'
==> azure-arm:  -> DeploymentName    : 'pkrdphlz1xtcy8n'
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-hlz1xtcy8n'
==> azure-arm:  -> DeploymentName    : 'pkrdphlz1xtcy8n'
==> azure-arm: Getting the VM's IP address ...
==> azure-arm:  -> ResourceGroupName   : 'packer-Resource-Group-hlz1xtcy8n'
==> azure-arm:  -> PublicIPAddressName : 'packerPublicIP'
==> azure-arm:  -> NicName             : 'packerNic'
==> azure-arm:  -> Network Connection  : 'PublicEndpoint'
==> azure-arm:  -> IP Address          : '13.90.250.248'
==> azure-arm: Waiting for SSH to become available...
==> azure-arm: Connected to SSH!
==> azure-arm: Provisioning with shell script: /tmp/packer-shell529418469
    azure-arm: Get:1 http://security.ubuntu.com/ubuntu xenial-security InRelease [102 kB]
    azure-arm: Hit:2 http://azure.archive.ubuntu.com/ubuntu xenial InRelease
    azure-arm: Get:3 http://azure.archive.ubuntu.com/ubuntu xenial-updates InRelease [102 kB]
    azure-arm: Get:4 http://azure.archive.ubuntu.com/ubuntu xenial-backports InRelease [102 kB]
    [snip]
    azure-arm: WARNING! The waagent service will be stopped.
    azure-arm: WARNING! Cached DHCP leases will be deleted.
    azure-arm: WARNING! root password will be disabled. You will not be able to login as root.
    azure-arm: WARNING! /etc/resolvconf/resolv.conf.d/tail and /etc/resolvconf/resolv.conf.d/original will be deleted.
    azure-arm: WARNING! packer account and entire home directory will be deleted.
==> azure-arm: Querying the machine's properties ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-hlz1xtcy8n'
==> azure-arm:  -> ComputeName       : 'pkrvmhlz1xtcy8n'
==> azure-arm:  -> OS Disk           : 'https://mystorageaccount.blob.core.chinacloudapi.cn/images/pkroshlz1xtcy8n.vhd'
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-hlz1xtcy8n'
==> azure-arm:  -> ComputeName       : 'pkrvmhlz1xtcy8n'
==> azure-arm: Capturing image ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-hlz1xtcy8n'
==> azure-arm:  -> ComputeName       : 'pkrvmhlz1xtcy8n'
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-hlz1xtcy8n'
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : 'https://mystorageaccount.blob.core.chinacloudapi.cn/images/pkroshlz1xtcy8n.vhd'
Build 'azure-arm' finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

StorageAccountLocation: chinaeast
OSDiskUri: https://mystorageaccount.blob.core.chinacloudapi.cn/system/Microsoft.Compute/Images/images/packer-osDisk.643f37d7-5a5d-43bf-96ed-2d598ada6e65.vhd
OSDiskUriReadOnlySas: https://mystorageaccount.blob.core.chinacloudapi.cn/system/Microsoft.Compute/Images/images/packer-osDisk.643f37d7-5a5d-43bf-96ed-2d598ada6e65.vhd?se=2017-07-08T20%3A57%3A53Z&sig=yl1yl3I2gKnO0I%2B7paw%2FQzKT5dawf5i%2B
LPmATMt5ot4%3D&sp=r&sr=b&sv=2015-02-21
TemplateUri: https://mystorageaccount.blob.core.chinacloudapi.cn/system/Microsoft.Compute/Images/images/packer-vmTemplate.643f37d7-5a5d-43bf-96ed-2d598ada6e65.json
TemplateUriReadOnlySas: https://mystorageaccount.blob.core.chinacloudapi.cn/system/Microsoft.Compute/Images/images/packer-vmTemplate.643f37d7-5a5d-43bf-96ed-2d598ada6e65.json?se=2017-07-08T20%3A57%3A53Z&sig=GB1iSl0hhw1ZYG4nl%2BCfR9WEaquCF
OEhNtKlvp%2B5TdE%3D&sp=r&sr=b&sv=2015-02-21
```

## 创建 Azure 映像
<a id="create-azure-image" class="xliff"></a>
Packer 生成过程的输出是指定的存储帐户中的虚拟硬盘 (VHD)。 通过 [az image create](https://docs.microsoft.com/cli/azure/image#create) 从此 VHD 中创建 Azure 映像，并指定在 Packer 生成输出的末尾处注明的 `OSDiskUri` 路径。 下例创建一个名为 `myImage` 的映像：

```azurecli
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --os-type linux \
    --source https://mystorageaccount.blob.core.chinacloudapi.cn/system/Microsoft.Compute/Images/images/packer-osDisk.643f37d7-5a5d-43bf-96ed-2d598ada6e65.vhd
```

此映像可用于在整个 Azure 订阅中创建 VM。 不限于在与源映像相同的资源组中创建 VM。

## 从 Azure 映像创建 VM
<a id="create-vm-from-azure-image" class="xliff"></a>
现在可以通过 [az vm create](https://docs.microsoft.com/cli/azure/vm#create) 从映像创建 VM。 指定通过 `--image` 参数创建的映像。 以下示例从 myImage 创建名为 myVM 的 VM，并生成 SSH 密钥（如果它们尚不存在）：

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

创建 VM 需要几分钟时间。 创建 VM 后，请记下 Azure CLI 显示的 `publicIpAddress`。 此地址用于通过 Web 浏览器访问 NGINX 站点。

若要使 VM 能使用 Web 流量，请通过 [az vm open-port](https://docs.microsoft.com/cli/azure/vm#open-port) 从 Internet 打开端口 80：

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## 测试 VM 和 NGINX
<a id="test-vm-and-nginx" class="xliff"></a>
现可打开 Web 浏览器，并在地址栏中输入 `http://publicIpAddress`。 在 VM 创建过程中提供自己的公共 IP 地址。 默认 NGINX 页如下例所示：

![NGINX 默认站点](./media/build-image-with-packer/nginx.png) 

## 后续步骤
<a id="next-steps" class="xliff"></a>
在此示例中，是在已安装 NGINX 的情况下使用 Packer 创建 VM 映像。 可将此 VM 映像与现有部署工作流一起使用，如将应用部署到通过 Chef 或 puppet 从映像创建的 VM。

有关其他 Linux 发行版的更多示例 Packer 模板，请参阅[此 GitHub 存储库](https://github.com/hashicorp/packer/tree/master/examples/azure)。