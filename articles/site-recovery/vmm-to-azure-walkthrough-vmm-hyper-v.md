---
title: "准备 System Center VMM 以将 Hyper-V 复制到 Azure | Azure"
description: "介绍如何准备 System Center VMM 以使用 Azure Site Recovery 将 Hyper-V 复制到 Azure"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: afcd81ae-d192-4013-a0af-3dac45b3c7e9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 07/23/2017
ms.date: 08/28/2017
ms.author: v-yeche
ms.openlocfilehash: 31c8310206c7ece5f2b90e14df4e23efab7ac3d5
ms.sourcegitcommit: 1ca439ddc22cb4d67e900e3f1757471b3878ca43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-to-azure"></a>步骤 6：准备 VMM 服务器和 Hyper-V 主机以将 Hyper-V 复制到 Azure

为部署设置 [Azure 组件](vmm-to-azure-walkthrough-prepare-azure.md)后，请使用本文中的说明准备本地 VMM 服务器和 Hyper-V 主机，以便与 Azure Site Recovery 进行交互。

阅读本文后，欢迎在页面底部发表任何看法，或者在 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=hypervrecovmgr)上咨询技术问题。

## <a name="prepare-vmm-servers"></a>准备 VMM 服务器

- 至少需要一个满足 Site Recovery 复制支持要求的 VMM 服务器 (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)。
- 请确保已为[网络映射](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure)准备好 VMM 服务器。
- 请确保 VMM 服务器可访问这些 URL

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

- 如果设置了基于 IP 地址的防火墙规则，请确保这些规则允许与 Azure 通信。
- 允许 [Azure 数据中心 IP 范围](https://www.microsoft.com/download/confirmation.aspx?id=41653)和 HTTPS (443) 端口。
- 允许订阅的 Azure 区域的 IP 地址范围以及中国北部的 IP 地址范围（用于访问控制和标识管理）。

在 Site Recovery 部署期间，需下载 Site Recovery 提供程序并将其安装在每个 VMM 服务器上。 在恢复服务保管库中注册 VMM 服务器。

## <a name="next-steps"></a>后续步骤

转到[步骤 7：创建保管库](vmm-to-azure-walkthrough-create-vault.md)

<!--Update_Description: new articles on site recovery vmm server and hyper-V host from vmm to azure-->