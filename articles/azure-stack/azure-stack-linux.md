---
title: 将 Linux 映像添加到 Azure Stack
description: 了解如何将 Linux 映像添加到 Azure Stack。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/08/2018
ms.date: 05/24/2018
ms.author: v-junlch
ms.reviewer: jeffgo
ms.openlocfilehash: ecd19b38bf49f687f46a5a95d42edfa080f31b49
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647759"
---
# <a name="add-linux-images-to-azure-stack"></a>将 Linux 映像添加到 Azure Stack

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可以通过将基于 Linux 的映像添加到 Azure Stack 市场，在 Azure Stack 上部署 Linux 虚拟机 (VM)。 将 Linux 映像添加到 Azure Stack 的最简单方法是通过市场管理。 这些映像已准备好，并已针对与 Azure Stack 的兼容性进行测试。

## <a name="marketplace-management"></a>市场管理

若要从 Azure 市场下载 Linux 映像，请使用以下文章中的过程。 选择要在 Azure Stack 上提供给用户的 Linux 映像。 

[将市场项从 Azure 下载到 Azure Stack](azure-stack-download-azure-marketplace-item.md)。

请注意，这些映像频繁更新，因此请经常查看市场管理以保持最新。

## <a name="prepare-your-own-image"></a>准备自己的映像

 只要有可能，请下载通过市场管理提供的映像，这些映像已针对 Azure Stack 进行了准备和测试。 
 
 Azure Linux 代理（通常称为 `WALinuxAgent` 或 `walinuxagent`）是必需的，并非所有代理版本都可以在 Azure Stack 上正常工作。 如果创建自己的映像，则应该使用版本 2.2.18 或更高版本。 请注意，目前 Azure Stack 不支持 [cloud-init](https://cloud-init.io/)。

 可以按照以下说明准备自己的 Linux 映像：

   - [基于 CentOS 的分发版](../virtual-machines/linux/create-upload-centos.md)
   - [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md)
   - [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
   - [SLES 和 openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md)
   - [Ubuntu Server](../virtual-machines/linux/create-upload-ubuntu.md)

    
## <a name="add-your-image-to-the-marketplace"></a>将映像添加到市场
 
按照[将映像添加到市场](azure-stack-add-vm-image.md)进行操作。 请确保 `OSType` 参数已设置为 `Linux`。

将映像添加到市场 后，便会创建市场项，用户就可以部署 Linux 虚拟机了。

<!-- Update_Description: wording update -->