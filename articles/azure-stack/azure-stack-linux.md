---
title: "将 Linux 映像添加到 Azure Stack"
description: "了解如何将 Linux 映像添加到 Azure Stack。"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/22/2018
ms.date: 03/02/2018
ms.author: v-junlch
ms.reviewer: jeffgo
ms.openlocfilehash: 5e2203968329d86b20d958c7b11fd643f761fb89
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="add-linux-images-to-azure-stack"></a>将 Linux 映像添加到 Azure Stack

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可以通过将基于 Linux 的映像添加到 Azure Stack Marketplace，在 Azure Stack 上部署 Linux 虚拟机 (VM)。 将 Linux 映像添加到 Azure Stack 的最简单方法是通过 Marketplace 管理。 这些映像已准备好，并已针对与 Azure Stack 的兼容性进行测试。

## <a name="marketplace-management"></a>Marketplace 管理

若要从 Azure Marketplace 下载 Linux 映像，请使用以下文章中的过程。 选择要在 Azure Stack 上提供给用户的 Linux 映像。

[将 Marketplace 项从 Azure 下载到 Azure Stack](azure-stack-download-azure-marketplace-item.md)。

## <a name="prepare-your-own-image"></a>准备自己的映像

可以按照下列其中一个说明准备自己的 Linux 映像：

   - [基于 CentOS 的分发版](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   - [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   - [SLES 和 openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   - [Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

1. 下载并安装 [Azure Linux 代理](https://github.com/Azure/WALinuxAgent/)。

    需要 Azure Linux 代理版本 2.2.2 或更高版本，才能在 Azure Stack 上预配 Linux VM，某些版本无法正常工作（例如，2.2.12 和 2.2.13）。 先前列出的许多发行版已包含代理的版本（通常称为 `WALinuxAgent` 或 `walinuxagent`）。 如果创建自定义映像，必须手动安装代理。 可从包名称或通过在 VM 上运行 `/usr/sbin/waagent -version` 来确定已安装的版本。

    按照下面的说明手动安装 Azure Linux 代理：

   a. 首先，从 [GitHub](https://github.com/Azure/WALinuxAgent/releases) 下载最新的 Azure Linux 代理，例如：

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.21.tar.gz
   b. 解压缩 Azure 代理：

            # tar -vzxf v2.2.21.tar.gz
   c. 安装 python-setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools
   d.单击“验证存储凭据”以验证存储帐户。 安装 Azure 代理：

            # cd WALinuxAgent-2.2.21
            # sudo python3 setup.py install --register-service

     已并行安装 Python 2.x 和 Python 3.x 的系统可能需要运行以下命令：

         sudo python3 setup.py install --register-service
     有关详细信息，请参阅 Azure Linux 代理[自述文件](https://github.com/Azure/WALinuxAgent/blob/master/README.md)。
2. [将映像添加到 Marketplace](azure-stack-add-vm-image.md)。 请确保 `OSType` 参数已设置为 `Linux`。
3. 将映像添加到 Marketplace 后，便会创建 Marketplace 项，用户就可以部署 Linux 虚拟机了。

## <a name="next-steps"></a>后续步骤
[在 Azure Stack 中提供服务概述](azure-stack-offer-services-overview.md)

