---
title: Azure 和 Linux | Azure
description: 介绍 Linux 虚拟机上的 Azure 计算、存储和网络服务。
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: 7965a80f-ea24-4cc2-bc43-60b574101902
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: overview
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 11/29/2017
ms.date: 08/12/2019
ms.author: v-yeche
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 816e701036e10d4edc17ee6db109df3007c90296
ms.sourcegitcommit: 8ac3d22ed9be821c51ee26e786894bf5a8736bfc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68912956"
---
# <a name="azure-and-linux"></a>Azure 和 Linux
Azure 是一个不断增长的集成公有云服务集合，包括分析、虚拟机、数据库、移动、网络、存储和 Web&mdash;是托管解决方案的理想选择。  Azure 提供可缩放的计算平台，允许即用即付，而无需投资购买本地硬件。  Azure 允许根据客户端所需的任何规模，随时扩展和缩减解决方案。

<!-- Not Available on  Azure vs AWS [definition mapping document](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/)-->

<!-- redirect https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/ to https://docs.microsoft.com/zh-cn/azure/architecture/aws-professional/services-->

## <a name="regions"></a>Regions
Azure 资源分布在中国各地的多个地理区域。  一个“区域”代表位于单个地理区域的多个数据中心。 Azure 目前（截至 2019 年 8 月）在中国正式推出了 4 个区域。 可在以下页面上找到现有区域和新宣布推出区域的更新列表：

<!--MOONCAKE: CUSTOMIZE ON CHINA-->
<!--MOONCAKE: CUSTOMIZE 4 regions on August 2019->

* [Azure China Regions](https://www.azure.cn/support/service-dashboard/)

## Availability
Azure announced an industry leading single instance virtual machine Service Level Agreement of 99.9% provided you deploy the VM with premium storage for all disks.  In order for your deployment to qualify for the standard 99.95% VM Service Level Agreement, you still need to deploy two or more VMs running your workload inside of an availability set. An availability set ensures that your VMs are distributed across multiple fault domains in the Azure data centers as well as deployed onto hosts with different maintenance windows. The full [Azure SLA](https://www.azure.cn/support/sla/virtual-machines/) explains the guaranteed availability of Azure as a whole.

## Managed Disks

Managed Disks handles Azure Storage account creation and management in the background for you, and ensures that you do not have to worry about the scalability limits of the storage account. You specify the disk size and the performance tier (Standard or Premium), and Azure creates and manages the disk. As you add disks or scale the VM up and down, you don't have to worry about the storage being used. If you're creating new VMs, [use the Azure CLI](quick-create-cli.md) or the Azure portal to create VMs with Managed OS and data disks. If you have VMs with unmanaged disks, you can [convert your VMs to be backed with Managed Disks](convert-unmanaged-to-managed-disks.md).

You can also manage your custom images in one storage account per Azure region, and use them to create hundreds of VMs in the same subscription. For more information about Managed Disks, see the [Managed Disks Overview](../linux/managed-disks-overview.md).

## Azure Virtual Machines & Instances
Azure supports running a number of popular Linux distributions provided and maintained by a number of partners.  You can find distributions such as CentOS, SUSE Linux Enterprise, Debian, Ubuntu, CoreOS, FreeBSD, and more in the Azure Marketplace. Azure actively works with various Linux communities to add even more flavors to the [Azure endorsed Linux Distros](endorsed-distros.md) list.

<!-- Not Available on Red Hat Enterprise and RancherOS -->

如果首选的 Linux 分发版目前不在库中，可以通过[在 Azure 中创建和上传 Linux VHD](create-upload-generic.md) 来“自带 Linux”VM。

借助 Azure 虚拟机，用户可以采用灵活的方式部署各种计算解决方案。 几乎可以在任何操作系统（Windows、Linux 或从我们不断增长的合作伙伴列表中的任一合作伙伴自定义创建的操作系统）上部署几乎任何工作负荷和任何语言。 没有找到所需的映像？  别担心，也可以使用本地的自有映像。

## <a name="vm-sizes"></a>VM 大小
VM 的[大小](sizes.md)由所要运行的工作负荷决定。 然后，选择的大小决定了处理能力、内存和存储容量等因素。 Azure 提供各种大小来支持多种类型的用途。

Azure 根据 VM 的大小和操作系统[按小时进行收费](https://www.azure.cn/pricing/details/virtual-machines/)。 对于不足一小时的部分，Azure 仅根据使用的分钟数计费。 存储将另行定价和收费。

## <a name="automation"></a>自动化
若要实现适当的 DevOps 区域性，所有基础结构都必须是代码。  如果所有基础结构都是代码，便可以轻松实现重建（Phoenix 服务器）。  Azure 可与所有主要自动化工具（如 Ansible、Chef、SaltStack 和 Puppet）配合使用。  Azure 也有自己的自动化工具：

* [Azure 模板](create-ssh-secured-vm-from-template.md)
* [Azure VMAccess](using-vmaccess-extension.md)

Azure 正在支持它的大多数 Linux 发行版中推出 [cloud-init](https://cloud-init.io/) 支持。  目前，默认情况下 Canonical Ubuntu VM 在启用 cloud-init 的情况下进行部署。 CentOS 和 Fedora 支持 cloud-init。

<!-- Not Available on Red Hat Familiy-->

* [在 Azure Linux VM 上使用 cloud-init](using-cloud-init.md)

## <a name="quotas"></a>配额
每个 Azure 订阅都有默认的配额限制，此限制会在为项目部署大量 VM 时造成影响。 每个订阅的当前限制是每区域 20 个 VM。  若要快速轻松地提高配额限制，可以开具支持票证来请求提高限制。  有关配额限制的更多详细信息，请参阅：

* [Azure 订阅服务限制](../../azure-subscription-service-limits.md)

## <a name="partners"></a>合作伙伴
Azure 与合作伙伴紧密合作，以确保及时更新可用映像并针对 Azure 运行时进行优化。  有关 Azure 合作伙伴的详细信息，请参阅以下链接：

* Azure 上的 Linux - [认可的分发](endorsed-distros.md)
* SUSE - [Azure 市场 - SUSE Linux Enterprise Server](https://market.azure.cn/marketplace/apps/SUSE.SLES?tab=Overview)
* Red Hat - [Azure 市场 - Red Hat Enterprise Linux](https://market.azure.cn/marketplace/apps?search=redhat)
* Canonical - [Azure 市场 - Ubuntu Server 16.04 LTS](https://market.azure.cn/marketplace/partners/canonical/ubuntuserver1604lts/)
* Debian - [Azure 市场 - Debian 8 "Jessie"](https://market.azure.cn/marketplace/partners/credativ/debian8/)
* FreeBSD - [Azure 市场 - FreeBSD](https://market.azure.cn/marketplace/apps?search=FreeBSD)
* CoreOS - [Azure 市场 - CoreOS (Stable)](https://market.azure.cn/marketplace/partners/coreos/coreosstable/)
* Bitnami - [Bitnami Library for Azure](https://azure.bitnami.com/)
* Jenkins - [Azure 市场 - Jenkins Platform](https://market.azure.cn/marketplace/apps?search=jenkins)


<!--MOONCAKE CUSTOMIZE * Red Hat - [Azure Marketplace - Red Hat Enterprise Linux](https://market.azure.cn/marketplace/apps?search=redhat)-->
<!--MOONCAKE CUSTOMIZE * FreeBSD - [Azure Marketplace - FreeBSD](https://market.azure.cn/marketplace/apps?search=FreeBSD)-->

<!-- Not Availalbe on 79-80 * RancherOS - [Azure Marketplace - RancherOS](https://market.azure.cn/marketplace/partners/rancher/rancheros/)-->
<!-- Not Availalbe on 80-81 * Mesosphere - [Azure Marketplace - Mesosphere DC/OS on Azure](https://market.azure.cn/marketplace/partners/mesosphere/dcosdcos/)-->
<!-- Not Availalbe on 80-81 * Docker - [Azure Marketplace - Azure Container Service with Docker Swarm](https://market.azure.cn/marketplace/partners/microsoft/acsswarms/)-->
<!-- Notice: URL is correct on [Azure Marketplace - Jenkins Platform](https://market.azure.cn/marketplace/apps?search=jenkins)-->

## <a name="getting-started-with-linux-on-azure"></a>开始在 Azure 中使用 Linux
若要开始使用 Azure，需要 Azure 帐户、已安装 Azure CLI 和一对 SSH 公钥和私钥。

### <a name="sign-up-for-an-account"></a>注册帐户
使用 Azure 云的第一步是注册 Azure 帐户。  若要开始，请转到 [Azure 帐户注册](https://www.azure.cn/pricing/1rmb-trial/)页。

### <a name="install-the-cli"></a>安装 CLI
使用新的 Azure 帐户，可以立即开始使用 Azure 门户（一个基于 Web 的管理面板）。  若要通过命令行管理 Azure 云，请安装 `azure-cli`。  在 Mac 或 Linux 工作站上安装 [Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

### <a name="create-an-ssh-key-pair"></a>创建 SSH 密钥对
现在已有 Azure 帐户、Azure Web 门户和 Azure CLI。  下一步是创建 SSH 密钥对，使用它可以通过 SSH 连接到 Linux 而无需使用密码。  [在 Linux 和 Mac 上创建 SSH 密钥](mac-create-ssh-keys.md)可启用无密码登录和更高的安全性。

### <a name="create-a-vm-using-the-cli"></a>使用 CLI 创建 VM
使用 CLI 创建 Linux VM 是部署 VM 的一种快速方法，无需离开正在使用的终端。  通过命令行标志或开关提供可以在 Web 门户上指定的所有内容。  

* [使用 CLI 创建 Linux VM](quick-create-cli.md)

### <a name="create-a-vm-in-the-portal"></a>在门户中创建 VM
通过在 Azure Web 门户上创建 Linux VM，可以轻松地指向和单击用于访问部署的各个选项。  因此，不需要使用命令行标记或开关，而可以在布局良好的 Web 界面上查看各种选项和设置。  通过命令行接口提供的所有功能也都在门户中提供。

* [使用门户创建 Linux VM](quick-create-portal.md)

### <a name="log-in-using-ssh-without-a-password"></a>不使用密码通过 SSH 登录
VM 现在正在 Azure 上运行，用户可以登录。  使用密码通过 SSH 登录既不安全耗时也长。  使用 SSH 密钥是最安全且最快捷的登录方式。  通过门户或 CLI 创建 Linux VM 时，有两种身份验证选择。  如果为 SSH 选择密码，则 Azure 将 VM 配置为允许通过密码登录。  如果选择使用 SSH 公钥，则 Azure 将 VM 配置为只允许通过 SSH 密钥登录，并禁止密码登录。 若要通过只允许 SSH 密钥登录来保护 Linux VM，请在门户或 CLI 中创建 VM 的过程中使用 SSH 公钥选项。

## <a name="related-azure-components"></a>相关 Azure 组件
## <a name="storage"></a>存储
* [Azure 存储简介](../../storage/common/storage-introduction.md)
* [使用 azure-cli 将磁盘添加到 Linux VM](add-disk.md)
* [如何在 Azure 门户中将数据磁盘附加到 Linux VM](attach-disk-portal.md)

## <a name="networking"></a>网络
* [虚拟网络概述](../../virtual-network/virtual-networks-overview.md)
* [Azure 中的 IP 地址](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [在 Azure 中打开 Linux VM 的端口](nsg-quickstart.md)
* [在 Azure 门户中创建完全限定的域名](portal-create-fqdn.md)

<!-- Not Avaialble ## Containers-->

## <a name="next-steps"></a>后续步骤
现在已概要了解 Azure 上的 Linux。  下一步是进一步的研究，并创建一些 VM 组件！

* [通过 Azure CLI 浏览不断增多的常见任务的示例脚本列表](cli-samples.md)

<!--Update_Description: update meta properties, wording update, update link -->
