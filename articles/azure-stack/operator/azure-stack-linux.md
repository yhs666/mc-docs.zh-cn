---
title: 将 Linux 映像添加到 Azure Stack
description: 了解如何将 Linux 映像添加到 Azure Stack。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/21/2019
ms.date: 07/29/2019
ms.author: v-jay
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: c73b98f9b903a86ea3b54bdef7a616a3c08a97e2
ms.sourcegitcommit: 4d34571d65d908124039b734ddc51091122fa2bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68513442"
---
# <a name="add-linux-images-to-azure-stack"></a>将 Linux 映像添加到 Azure Stack

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可以通过将基于 Linux 的映像添加到 Azure Stack 市场，在 Azure Stack 上部署 Linux 虚拟机 (VM)。 将 Linux 映像添加到 Azure Stack 的最简单方法是通过市场管理。 这些映像已准备好，并已针对与 Azure Stack 的兼容性进行测试。

## <a name="marketplace-management"></a>市场管理

若要从 Azure 市场下载 Linux 映像，请使用[将市场项从 Azure 下载到 Azure Stack](azure-stack-download-azure-marketplace-item.md) 一文中的过程。 选择要在 Azure Stack 上提供给用户的 Linux 映像。

这些映像频繁更新，因此请经常查看“市场管理”以保持最新。

## <a name="prepare-your-own-image"></a>准备自己的映像

请尽可能通过“市场管理”下载可用的映像。 这些映像已针对 Azure Stack 进行了准备和测试。

### <a name="azure-linux-agent"></a>Azure Linux 代理

Azure Linux 代理（通常称为 `WALinuxAgent` 或 `walinuxagent`）是必需的，并非所有代理版本都可以在 Azure Stack 上正常工作。 Azure Stack 不支持 2.2.20 和 2.2.35 之间的版本。 若要使用 2.2.35 以上的最新代理版本，请应用 1901 修补程序/1902 修补程序，或者将 Azure Stack 更新到 1903 版（或更高版本）。 请注意，目前 Azure Stack 不支持 [cloud-init](https://cloud-init.io/)。

| Azure Stack 内部版本 | Azure Linux 代理内部版本 |
| ------------- | ------------- |
| 1.1901.0.99 或更低版本 | 2.2.20 |
| 1.1902.0.69  | 2.2.20  |
|  1.1901.3.105   | 2.2.35 或更高版本 |
| 1.1902.2.73  | 2.2.35 或更高版本 |
| 1.1903.0.35  | 2.2.35 或更高版本 |
| 1903 之后的版本 | 2.2.35 或更高版本 |
| 不支持 | 2.2.21-2.2.34 |

可以按照以下说明准备自己的 Linux 映像：

* [基于 CentOS 的分发版](/virtual-machines/linux/create-upload-centos?toc=%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](/virtual-machines/linux/debian-create-upload-vhd?toc=%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
* [SLES 和 openSUSE](/virtual-machines/linux/suse-create-upload-vhd?toc=%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu Server](/virtual-machines/linux/create-upload-ubuntu?toc=%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="add-your-image-to-the-marketplace"></a>将映像添加到市场

按照[将映像添加到市场](azure-stack-add-vm-image.md)进行操作。 请确保 `OSType` 参数已设置为 `Linux`。

将映像添加到市场 后，便会创建市场项，用户就可以部署 Linux 虚拟机了。

## <a name="next-steps"></a>后续步骤

* [将市场项从 Azure 下载到 Azure Stack](azure-stack-download-azure-marketplace-item.md)
* [Azure Stack 市场概述](azure-stack-marketplace.md)
