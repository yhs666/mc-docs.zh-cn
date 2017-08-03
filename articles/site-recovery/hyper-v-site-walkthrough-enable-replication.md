---
title: "使用 Azure Site Recovery 为复制到 Azure 的 Hyper-V VM（不使用 System Center VMM）启用复制 | Azure"
description: "总结了使用 Azure Site Recovery 服务为 Hyper-V VM 启用到 Azure 的复制所需的步骤"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: bd31ef01-69f1-4591-a519-e42510a6e2f4
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 06/21/2017
ms.date: 07/31/2017
ms.author: v-yeche
ms.openlocfilehash: 52fa3c9dd878f51de400a9f6c8cfd122a3e0120e
ms.sourcegitcommit: 66db84041f1e6e77ef9534c2f99f1f5331a63316
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="step-10-enable-replication-for-hyper-v-vms-replicating-to-azure"></a>步骤 10：为复制到 Azure 的 Hyper-V VM 启用复制

本文介绍如何使用 Azure 门户中的 [Azure Site Recovery](site-recovery-overview.md) 服务为本地 Hyper-V 虚拟机（不由 System Center VMM 托管）启用到 Azure 的复制。

## <a name="before-you-start"></a>开始之前

在开始之前，请确保 Azure 用户帐户具有启用新的虚拟机到 Azure 的复制所需的[权限](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)。

## <a name="exclude-disks-from-replication"></a>从复制中排除磁盘

默认情况下将复制计算机上的所有磁盘。 你可以从复制中排除磁盘。 例如，你可能不想复制包含临时数据，或者每次重启计算机或应用程序时会刷新的数据（例如 pagefile.sys 或 SQL Server tempdb）的磁盘。
<!-- Not Available [Learn more](site-recovery-exclude-disk.md) -->

## <a name="replicate-vms"></a>复制 VM

为 VM 启用复制，如下所示：          

1. 单击“复制应用程序” > “源”。 首次设置复制后，单击“+复制”即可对其他计算机启用复制。

    ![启用复制](./media/hyper-v-site-walkthrough-enable-replication/enable-replication.png)
2. 在“源”中选择 Hyper-V 站点。 。
3. 在“目标”中，选择保管库订阅，以及故障转移后要在 Azure 中使用的故障转移模型（经典或资源管理）。
4. 选择要使用的存储帐户。 如果没有要使用的帐户，可以[创建一个](#set-up-an-azure-storage-account)。 。
5. 选择 Azure VM 在故障转移后创建时所要连接的 Azure 网络和子网。

    - 若要将网络设置应用到所有启用复制功能的计算机，请选择“立即为选定的计算机配置”。
    -  选择“稍后配置”以选择每个计算机的 Azure 网络。
    - 如果没有要使用的网络，可以[创建一个](#set-up-an-azure-network)。 选择适用的子网。 然后，单击“确定” 。

   ![启用复制](./media/hyper-v-site-walkthrough-enable-replication/enable-replication11.png)

6. 在“虚拟机” > “选择虚拟机”中，单击并选择要复制的每个计算机。 只能选择可以启用复制的计算机。 。

    ![启用复制](./media/hyper-v-site-walkthrough-enable-replication/enable-replication5-for-exclude-disk.png)

7. 在“属性” > “配置属性”中，选择所选 VM 的操作系统和 OS 磁盘。
8. 验证 Azure VM 名称（目标名称）是否符合 [Azure 虚拟机要求](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements)。
9. 默认选择 VM 的所有磁盘进行复制。 请清除磁盘以将其排除。
10. 单击“确定”保存更改。 可以稍后再设置其他属性。

     ![启用复制](./media/hyper-v-site-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

11. 在“复制设置” > “配置复制设置”中，选择要应用于受保护 VM 的复制策略。 。 可以在“复制策略”> 策略名称 >“编辑设置”中修改复制策略。 应用的更改将用于已在复制的计算机和新计算机。

   ![启用复制](./media/hyper-v-site-walkthrough-enable-replication/enable-replication7.png)

可以在“作业” > “Site Recovery 作业”中，跟踪“启用保护”作业的进度。 在“完成保护”作业运行之后，计算机就可以进行故障转移了。

## <a name="next-steps"></a>后续步骤

转到[步骤 11：运行测试故障转移](hyper-v-site-walkthrough-test-failover.md)

<!--Update_Description: new article about walkthrought enable replication from hyper-v to azure  -->