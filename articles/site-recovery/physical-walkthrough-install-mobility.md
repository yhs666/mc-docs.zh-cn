---
title: "安装移动服务以便将物理服务器复制到 Azure | Azure"
description: "本文介绍如何在物理服务器上安装移动服务代理，以便使用 Azure Site Recovery 服务将物理服务器复制到 Azure。"
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
ms.date: 07/31/2017
ms.author: v-yeche
ms.openlocfilehash: 542380b8015013e3b4da32a20eb5b93f810226c8
ms.sourcegitcommit: 66db84041f1e6e77ef9534c2f99f1f5331a63316
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="step-9-install-the-mobility-service"></a>步骤 9：安装移动服务

本文介绍如何安装移动服务组件，以便在 Azure 门户中使用 [Azure Site Recovery](site-recovery-overview.md) 服务将本地 Windows/Linux 物理服务器复制到 Azure。

移动服务可以捕获计算机上的数据写入，并将其转发给进程服务器。 应在要复制到 Azure 的每个服务器上安装移动服务。

可以在启用复制后，从 Site Recovery 进程服务器使用推送安装安装移动服务，也可以使用 System Center Configuration Manager 等工具进行安装。 若要使用推送安装，请在启用复制时在服务器上安装该服务。

<!-- Not Available ## Install manually -->
<!-- Not Available site-recovery-vmware-to-azure-install-mob-svc.md -->

## <a name="install-from-the-process-server"></a>从进程服务器安装

若要在为计算机启用复制时从进程服务器推送移动服务安装，需要有一个可供进程服务器访问计算机的帐户。 该帐户仅用于推送安装。

1. 如果尚未创建帐户，请遵循以下指南创建一个：

    - 可以使用域帐户，也可以使用本地帐户
    - 对于 Windows，如果使用的不是域帐户，则需在本地计算机上禁用远程用户访问控制。 为此，请在注册表中的 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System 下添加值为 1 的 DWORD 项 LocalAccountTokenFilterPolicy。
    - 若要通过 CLI 添加 Windows 注册表项，请键入：

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - 对于 Linux，该帐户应是源 Linux 服务器上的root 用户。

<!-- Not Available [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) -->
## <a name="other-installation-methods"></a>其他安装方法

- [了解](site-recovery-install-mobility-service-using-sccm.md)如何使用配置管理器安装移动服务
- [了解](site-recovery-automate-mobility-service-install.md)如何使用 Azure Automation DSC 安装。

## <a name="next-steps"></a>后续步骤

转到[步骤 10：启用复制](physical-walkthrough-enable-replication.md)

<!--Update_Description: new article about walkthrought install mobility from physical to azure -->