---
title: " 管理在 Azure 中运行的进程服务器（经典）| Azure"
description: "本文介绍如何在 Azure 中设置故障回复进程服务器（经典）。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 06/29/2017
ms.date: 09/11/2017
ms.author: v-yeche
ms.openlocfilehash: 3d06ba91b3587097b3adbb3ab22da1cb16a0560a
ms.sourcegitcommit: 76a57f29b1d48d22bb4df7346722a96c5e2c9458
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2017
---
# <a name="manage-a-process-server-running-in-azure-classic"></a>管理在 Azure（经典）中运行的进程服务器
> [!div class="op_single_selector"]
> * [Azure 经典](./site-recovery-vmware-setup-azure-ps-classic.md)
> * [Resource Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

故障回复期间，如果 Azure 虚拟网络与本地网络之间存在高延迟，建议在 Azure 中部署进程服务器。 本文介绍如何设置、配置和管理在 Azure 中运行的进程服务器。

> [!NOTE]
> 如果在故障转移期间使用了经典部署模型作为虚拟机的部署模型，则要使用本文。 如果使用了资源管理器部署模型作为部署模型，请按照[如何设置和配置故障回复进程服务器（资源管理器）](./site-recovery-vmware-setup-azure-ps-resource-manager.md)中的步骤进行操作

## <a name="prerequisites"></a>先决条件

[!INCLUDE [site-recovery-vmware-process-server-prereq](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>在 Azure 上部署进程服务器

1. 在 Azure Marketplace 中，使用 **Azure Site Recovery 进程服务器 V2** 创建虚拟机 </br>
    ![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)
2. 确保选择 **经典** </br>
  ![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)
3. 在“创建虚拟机向导 > 基本设置”中，确保选择已将虚拟机故障转移到的订阅和位置。</br>
  ![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)
4. 确保虚拟机已连接到故障转移后的虚拟机要连接到的 Azure 虚拟网络。</br>
  ![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)
5. 预配进程服务器虚拟机后，需要登录并将其注册到配置服务器。

> [!NOTE]
> 要能够使用此进程服务器进行故障回复，需要将其注册到本地配置服务器。

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>将进程服务器（在 Azure 中运行）注册到配置服务器（在本地运行）

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-the-process-server-to-latest-version"></a>将进程服务器升级到最新版本。

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-the-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>将进程服务器（在 Azure 中运行）从配置服务器（在本地运行）中注销

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

<!--Update_Description: new articles on vmware setup azure ps classis in site recovery -->