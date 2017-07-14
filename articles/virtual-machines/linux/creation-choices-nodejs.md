---
title: "使用 Azure CLI 1.0 创建 Linux VM 的不同方式 | Azure"
description: "介绍在 Azure 上创建 Linux 虚拟机的不同方法，并提供每种方法的工具和教程的链接。"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
origin.date: 05/11/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.openlocfilehash: 32173eb5b7cfddf6e7a8d921042255bf089d3584
ms.sourcegitcommit: 7d2235bfc3dc1e2f64ed8beff77e87d85d353c4f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# 在 Azure 中创建 Linux 虚拟机的不同方式
<a id="different-ways-to-create-a-linux-virtual-machine-in-azure" class="xliff"></a>
在 Azure 中，可以使用习惯的工具和工作流灵活创建 Linux 虚拟机 (VM)。 本文汇总了这些方法的差异，并举例说明如何创建 Linux VM。

## Azure CLI
<a id="azure-cli" class="xliff"></a>
可使用以下 CLI 版本之一在 Azure 中创建 VM：

- Azure CLI 1.0 - 用于经典部署模型和资源管理部署模型（本文）的 CLI
- [Azure CLI 2.0](../windows/creation-choices.md) - 适用于资源管理部署模型的下一代 CLI

Azure CLI 1.0 可通过 npm 包、发行版提供的程序包或 Docker 容器跨平台使用。 有关详细信息，请参阅 [如何安装和配置 Azure CLI](../../cli-install-nodejs.md)。 以下教程提供了有关使用 Azure CLI 1.0 的示例。 阅读下面每篇文章，了解更多有关所示的 CLI 快速启动命令的更多详细信息：

* [Create a Linux VM from the Azure CLI for dev and test（从 Azure CLI 创建用于开发和测试的 Linux VM）](quick-create-cli-nodejs.md)

  * 以下示例使用名为 *azure_id_rsa.pub* 的公钥创建 CoreOS VM：

    ```azurecli
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
      --image-urn CoreOS
    ```
* [使用 Azure 模板创建受保护的 Linux VM](create-ssh-secured-vm-from-template.md)

  * 以下示例使用 GitHub 上存储的模板创建 VM：

    ```azurecli
    azure group create --name myResourceGroup --location chinaeast 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```
* [使用 Azure CLI 创建完整的 Linux 环境](create-cli-complete-nodejs.md)

  * 包括在可用性集中创建负载均衡器和多个 VM。
* [将磁盘添加到 Linux VM](add-disk.md)

  * 以下示例将 *5* Gb 磁盘添加到名为 *myVM* 的现有 VM：

    ```azurecli
    azure vm disk attach-new \
        --resource-group myResourceGroup \
        --vm-name myVM \
        --size-in-GB 5
    ```

## Azure 门户
<a id="azure-portal" class="xliff"></a>
[Azure 门户](https://portal.azure.cn)允许用户快速创建 VM，因为不需在系统上安装任何内容。 使用 Azure 门户创建 VM：

* [使用 Azure 门户创建 Linux VM](quick-create-portal.md) 

## 操作系统和映像选项
<a id="operating-system-and-image-choices" class="xliff"></a>
创建 VM 时，可根据要运行的操作系统选择映像。 Azure 及其合作伙伴提供了许多映像，其中一些映像包括预安装的应用程序和工具。 也可上传自己的某个映像（请参阅[以下部分](#use-your-own-image)）。

### Azure 映像
<a id="azure-images" class="xliff"></a>
使用 `azure vm image` CLI 命令可按发布者、发行版本和内部版本查看可用内容。

列出可用的发布者，如下所示：

```azurecli
azure vm image list-publishers --location chinaeast
```

列出特定发布者的可用产品，如下所示：

```azurecli
azure vm image list-offers --location chinaeast --publisher Canonical
```

列出给定产品的可用 SKU （分发版），如下所示：

```azurecli
azure vm image list-skus --location chinaeast --publisher Canonical --offer UbuntuServer
```

列出给定发行版的所有可用映像，如下所示：

```azurecli
azure vm image list --location chinaeast --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

请参阅[使用 Azure CLI 导航并选择 Azure 虚拟机映像](cli-ps-findimage.md#use-azure-cli-10)，获取有关浏览和使用可用映像的更多示例。

`azure vm quick-create` 和 `azure vm create` 命令提供的别名可用于快速访问较常见的发行版及其最新版本。 使用别名通常比每次创建 VM 时指定发布者、产品、SKU 和版本更加快捷：

| 别名 | 发布者 | 产品 | SKU | 版本 |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |最新 |
| CoreOS |CoreOS |CoreOS |Stable |最新 |
| Debian |credativ |Debian |8 |最新 |
| openSUSE |SUSE |openSUSE |13.2 |latest |
| SLES |SLES |SLES |12-SP1 |最新 |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |最新 |

### 使用你自己的映像
<a id="use-your-own-image" class="xliff"></a>
若要进行具体的自定义，可以通过 *捕获* 现有 Azure VM 来使用基于该 VM 的映像。 也可以上传本地创建的映像。 有关受支持的发行版以及如何使用你自己的映像的详细信息，请参阅以下文章：

* [Azure endorsed distributions（Azure 认可的分发版）](endorsed-distros.md)
* [Information for non-endorsed distributions（有关未认可分发版的信息）](create-upload-generic.md)
* [上传自定义磁盘映像并从其创建 Linux VM](upload-vhd.md)
* [如何捕获用作 Resource Manager 模板的 Linux 虚拟机](capture-image.md)。

  * 用于捕获现有 VM 的快速入门示例命令：

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --vm-name myVM
    azure vm generalize --resource-group myResourceGroup --vm-name myVM
    azure vm capture --resource-group myResourceGroup --vm-name myVM --vhd-name-prefix myCapturedVM
    ```

## 后续步骤
<a id="next-steps" class="xliff"></a>
* 通过[门户](quick-create-portal.md)、[CLI](quick-create-cli.md) 或 [Azure Resource Manager 模板](../windows/cli-deploy-templates.md)创建 Linux VM。
* 创建 Linux VM 后，可[添加数据磁盘](add-disk.md)。
* [重置密码或 SSH 密钥和管理用户](using-vmaccess-extension.md)
