---
title: "安装移动服务以便将 VMware 复制到 Azure | Azure"
description: "本文介绍如何安装移动服务代理，以便使用 Azure Site Recovery 服务将 VMware 复制到 Azure。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 3189fbcd-6b5b-4ffb-b5a9-e2080c37f9d9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 06/27/2017
ms.date: 08/28/2017
ms.author: v-yeche
ms.openlocfilehash: 99f857c4fa03d0432593f348b1c60329a11004f2
ms.sourcegitcommit: 1ca439ddc22cb4d67e900e3f1757471b3878ca43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="step-10-install-the-mobility-service"></a>步骤 10：安装移动服务

本文介绍在 Azure 门户中使用 [Azure Site Recovery](site-recovery-overview.md) 服务将本地 VMware 虚拟机复制到 Azure 时，如何配置源和目标设置。

移动服务可以捕获计算机上的数据写入，并将其转发给进程服务器。 应在要复制到 Azure 的每台计算机上安装移动服务。

可以手动安装移动服务，也可以在启用复制后，从 Site Recovery 进程服务器推送安装，亦可以使用 System Center Configuration Manager 工具进行安装。 如果使用推送安装，在启用复制时会在 VM 上安装该服务。

请将评论和问题发布到这篇文章的底部，或者发布到 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=hypervrecovmgr)。

## <a name="install-manually"></a>手动安装

1. 查看手动安装的[先决条件](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites)。
2. 按照[这些说明](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)操作，使用门户进行手动安装。
3. 若要通过命令行进行安装，请按照[这些说明](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)操作。

## <a name="install-from-the-process-server"></a>从进程服务器安装

若要在为 VM 启用复制后从进程服务器推送移动服务安装，需要有一个可供进程服务器访问 VM 的帐户。 该帐户仅用于推送安装。

1. 应[创建了一个帐户](vmware-walkthrough-prepare-vmware.md)用于推送安装。 然后指定在 Site Recovery 部署过程中配置源设置时要使用的帐户。
2. 若要在运行 Windows 或 Linux 的 VM 上推送移动服务，请按照[这些说明](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)进行操作。

## <a name="other-methods"></a>其他方法

详细了解[如何使用配置管理器安装移动服务](site-recovery-install-mobility-service-using-sccm.md)或使用 [Azure Automation DSC](site-recovery-automate-mobility-service-install.md) 安装移动服务。

## <a name="next-steps"></a>后续步骤

转到[步骤 11：启用复制](vmware-walkthrough-enable-replication.md)

<!--Update_Description: new articles on site recovery mobility from vmware to azure-->