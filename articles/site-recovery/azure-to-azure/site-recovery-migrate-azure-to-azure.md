---
title: "在 Azure 区域之间迁移 Azure IaaS VM | Azure"
description: "使用 Azure Site Recovery 将 Azure IaaS 虚拟机从一个 Azure 区域迁移到另一个 Azure 区域。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/31/2017
ms.date: 01/01/2018
ms.author: v-yeche
ms.openlocfilehash: 6491ec2b66fe66d1c99f1eab93a181a27a3504a6
ms.sourcegitcommit: 90e4b45b6c650affdf9d62aeefdd72c5a8a56793
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>使用 Azure Site Recovery 在 Azure 区域之间迁移 Azure IaaS 虚拟机

如果想要使用 [Site Recovery](../site-recovery-overview.md) 服务在 Azure 区域之间迁移 Azure 虚拟机 (VM)，请参阅本文。

## <a name="prerequisites"></a>先决条件

准备好想要迁移的 IaaS VM。

## <a name="deployment-steps"></a>部署步骤

1. [创建保管库](azure-to-azure-tutorial-enable-replication.md#create-a-vault)。
2. 为需要迁移的 VM [启用复制](azure-to-azure-tutorial-enable-replication.md#enable-replication)，并选择 Azure 作为源。 目前不支持使用托管磁盘来实现 Azure VM 的本机复制。
3. [运行故障转移](../site-recovery-failover.md)。 初始复制完成后，可以运行从一个 Azure 区域到另一个 Azure 区域的故障转移。 （可选）可以创建一个恢复计划并运行故障转移，在区域之间迁移多个虚拟机。 [详细了解](../site-recovery-create-recovery-plans.md)恢复计划。

## <a name="next-steps"></a>后续步骤
若要了解其他复制方案，请参阅[什么是 Azure Site Recovery？](../site-recovery-overview.md)
<!--Update_Description: new articles on site recovery migrate azure to azure -->