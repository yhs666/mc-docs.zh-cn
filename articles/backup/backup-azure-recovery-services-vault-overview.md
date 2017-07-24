---
title: "恢复服务保管库概述 | Microsoft Docs"
description: "恢复服务保管库和 Azure 备份保管库的概述和比较。"
services: backup
documentationcenter: " "
author: alexchen2016
manager: digimobile
ms.assetid: 38d4078b-ebc8-41ff-9bc8-47acf256dc80
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 05/15/2017
ms.date: 06/29/2017
ms.author: v-junlch
ms.openlocfilehash: 2d03b35e54c5e124c19cffa839dbc247772a9648
ms.sourcegitcommit: d5d647d33dba99fabd3a6232d9de0dacb0b57e8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="recovery-services-vaults-overview"></a>恢复服务保管库概述

本文介绍恢复服务保管库的功能。 恢复服务保管库是 Azure 中用于存储数据的存储实体。 数据通常是虚拟机 (VM)、工作负荷、服务器或工作站的数据或配置信息的副本。 恢复服务保管库是 Resource Manager 版本的备份保管库。 Microsoft 建议你使用恢复服务保管库并将任何备份保管库转换为恢复服务保管库。

## <a name="what-is-a-recovery-services-vault"></a>什么是恢复服务保管库？

恢复服务保管库是 Azure 中的联机存储实体，用于保存备份副本、恢复点、备份策略之类的数据。 可以使用恢复服务保管库为各种 Azure 服务（例如 IaaS VM（Linux 或 Windows））和 Azure SQL 数据库存储备份数据。 恢复服务保管库支持 System Center DPM、Windows Server、Azure 备份服务器等。 使用恢复服务保管库可以方便地组织备份数据，并将管理开销降至最低。

在 Azure 订阅中，可以根据自己的偏好创建任意数目的恢复服务保管库。

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>比较恢复服务保管库和备份保管库

恢复服务保管库基于 Azure 的 Azure Resource Manager 模型，而备份保管库则基于 Azure Service Manager 模型。 将备份保管库升级到恢复服务保管库时，备份数据在升级过程中和升级后均会保持不变。 恢复服务保管库提供不适用于备份保管库的功能，例如：

- **有助于确保备份数据安全的增强功能**：使用恢复服务保管库时，Azure 备份提供用于保护云备份的安全功能。 这些安全功能确保可以保护备份并从云备份安全地恢复数据，即使生产服务器和备份服务器受到安全威胁。 

- **针对混合 IT 环境进行集中监视**：使用恢复服务保管库时，可以通过中心门户监视 [Azure IaaS VM](backup-azure-manage-vms-classic.md) 和[本地资产](backup-azure-manage-windows-server-classic.md)。 [了解详细信息](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **基于角色的访问控制 (RBAC)**：RBAC 在 Azure 中提供精细的访问管理控制。 [Azure 提供各种内置角色](../active-directory/role-based-access-built-in-roles.md)，而 Azure 备份具有三个[用于管理恢复点的内置角色](backup-rbac-rs-vault.md)。 恢复服务保管库与 RBAC 兼容，后者会限制对已定义用户角色集的备份和还原访问权限。 [了解详细信息](backup-rbac-rs-vault.md)

- **保护 Azure 虚拟机的所有配置**：恢复服务保管库保护基于 Resource Manager 的 VM，其中包括高级磁盘、托管磁盘和加密 VM。 将备份保管库升级到恢复服务保管库以后，即可将基于 Service Manager 的 VM 升级到基于 Resource Manager 的 VM。 在升级保管库的同时，可以保留基于 Service Manager 的 VM 恢复点，并为已升级（已启用 Resource Manager）的 VM 配置保护。 [了解详细信息](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **适用于 IaaS VM 的即时还原**：使用恢复服务保管库时，可以从 IaaS VM 还原文件和文件夹，不需还原整个 VM，从而缩短还原时间。 适用于 IaaS VM 的即时还原可用于 Windows 和 Linux VM。 [了解详细信息](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)


## <a name="next-steps"></a>后续步骤
请参阅以下文章：</br>
[备份 IaaS VM](backup-azure-arm-vms-prepare.md)</br>
[备份 Azure 备份服务器](backup-azure-microsoft-azure-backup-classic.md)</br>
[备份 Windows Server](backup-configure-vault.md)

