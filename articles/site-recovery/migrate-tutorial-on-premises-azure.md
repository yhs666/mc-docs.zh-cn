---
title: 使用 Azure Site Recovery 将本地计算机迁移到 Azure | Azure
description: 本文将介绍如何使用 Azure Site Recovery 将本地计算机迁移到 Azure。
services: site-recovery
author: rockboyfor
ms.service: site-recovery
ms.topic: tutorial
origin.date: 09/12/2018
ms.date: 09/24/2018
ms.author: v-yeche
ms.custom: MVC
ms.openlocfilehash: 83a9cb87b45869f23e11942e80af73d14b75592f
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52666627"
---
# <a name="migrate-on-premises-machines-to-azure"></a>将本地计算机迁移到 Azure

除了使用 [Azure Site Recovery](site-recovery-overview.md) 服务管理和协调本地计算机和 Azure VM 的灾难恢复以实现业务连续性和灾难恢复 (BCDR) 外，还可以使用 Site Recovery 管理本地计算机到 Azure 的迁移。

本教程介绍如何将本地 VM 和物理服务器迁移到 Azure。 本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 选择复制目标
> * 设置源和目标环境
> * 设置复制策略
> * 启用复制
> * 运行测试迁移，确保一切按预期正常工作
> * 面向 Azure 运行一次性故障转移

此教程为系列教程中的第三个教程。 本教程假设你已完成前面教程中的以下任务：

1. [准备 Azure](tutorial-prepare-azure.md)
2. 在本地准备 [VMware](vmware-azure-tutorial-prepare-on-premises.md) 或 [Hyper-V] (hyper-v-prepare-on-premises-tutorial.md) 服务器。

在开始之前，查看用于灾难恢复的 [VMware](vmware-azure-architecture.md) 或 [Hyper-V](hyper-v-azure-architecture.md) 体系结构会有所帮助。

## <a name="prerequisites"></a>先决条件

不支持半虚拟化驱动程序导出的设备。

## <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

1. 登录到 [Azure 门户](https://portal.azure.cn) > **恢复服务**。
2. 单击“创建资源” > “监视和管理” > “备份和站点恢复”。
3. 在“名称”中，指定友好名称 **ContosoVMVault**。 如果有多个订阅，请选择合适的一个。
4. 创建资源组 **ContosoRG**。
5. 指定 Azure 区域。 若要查看受支持的区域，请参阅 [Azure Site Recovery 定价详细信息](https://www.azure.cn/pricing/details/site-recovery/)中的“地域可用性”。
6. 若要从仪表板快速访问保管库，请单击“固定到仪表板”，然后单击“创建”。

   ![新保管库](./media/migrate-tutorial-on-premises-azure/onprem-to-azure-vault.png)

新保管库将添加到“仪表板”中的“所有资源”下，以及“恢复服务保管库”主页面上。

## <a name="select-a-replication-goal"></a>选择复制目标

选择要复制的内容以及要复制到的位置。
1. 单击“恢复服务保管库”> 保管库。
2. 在“资源”菜单中，单击“Site Recovery” > “准备基础结构” > “保护目标”。
3. 在“保护目标”中，选择要迁移的内容。
    - VMware：选择“到 Azure” > “是，使用 VMWare vSphere 虚拟机监控程序”。
    - “物理计算机”：选择“到 Azure” > “未虚拟化/其他”。
    - Hyper-V：选择“到 Azure” > “是，使用 Hyper-V”。 如果 Hyper-V VM 由 VMM 管理，则选择“是”。

## <a name="set-up-the-source-environment"></a>设置源环境

- [设置](vmware-azure-tutorial.md#set-up-the-source-environment) VMware VM 的源环境。
- [设置](physical-azure-disaster-recovery.md#set-up-the-source-environment)物理服务器的源环境。
- [设置](hyper-v-azure-tutorial.md#set-up-the-source-environment) Hyper-V VM 的源环境。

## <a name="set-up-the-target-environment"></a>设置目标环境

选择并验证目标资源。

1. 单击“准备基础结构” > “目标”，然后选择要使用的 Azure 订阅。
2. 指定资源管理器部署模型。
3. Site Recovery 检查是否有一个或多个兼容的 Azure 存储帐户和网络。

## <a name="set-up-a-replication-policy"></a>设置复制策略

- 为 VMware VM [设置复制策略](vmware-azure-tutorial.md#create-a-replication-policy)。
- 为物理服务器[设置复制策略](physical-azure-disaster-recovery.md#create-a-replication-policy)。
- 为 Hyper-V VM [设置复制策略](hyper-v-azure-tutorial.md#set-up-a-replication-policy)。

## <a name="enable-replication"></a>启用复制

- 为 VMware VM [启用复制](vmware-azure-tutorial.md#enable-replication)。
- 为物理服务器[启用复制](physical-azure-disaster-recovery.md#enable-replication)。
- 在[使用](hyper-v-vmm-azure-tutorial.md#enable-replication)或[不使用 VMM](hyper-v-azure-tutorial.md#enable-replication) 的情况下，为 Hyper-V VM 启用复制。

## <a name="run-a-test-migration"></a>运行测试迁移

运行 [测试故障转移](tutorial-dr-drill-azure.md) ，确保一切如预期正常运行。

## <a name="migrate-to-azure"></a>迁移到 Azure

为想要迁移的计算机运行故障转移。

1. 在“设置” > “复制的项”中，单击计算机 >“故障转移”。
2. 在“故障转移”中，选择要故障转移到的“恢复点”。 选择最新的恢复点。
3. 加密密钥设置与此方案无关。
4. 选择“在开始故障转移前关闭计算机”。 在触发故障转移之前，Site Recovery 会尝试关闭虚拟机。 即使关机失败，故障转移也仍会继续。 可以在“作业”页上跟踪故障转移进度。
5. 检查 Azure VM 是否在 Azure 中按预期显示。
6. 在“复制的项”中，右键单击 VM >“完成迁移”。 该操作将完成迁移过程、停止 VM 的复制，并停止对 VM 的 Site Recovery 计费。

    ![完成迁移](./media/migrate-tutorial-on-premises-azure/complete-migration.png)

> [!WARNING]
> **请勿取消正在进行的故障转移**：在故障转移开始前，VM 复制已停止。 如果取消正在进行的故障转移，故障转移会停止，但 VM 将不再进行复制。

在某些情况下，故障转移需要大约八到十分钟的时间完成其他进程。 你可能注意到物理服务器、VMware Linux 计算机、未启用 DHCP 服务的 VMware VM 和未安装以下启动驱动程序：storvsc、vmbus、storflt、intelide、atapi 的 VMware VM 需要更长的测试故障转移时间。

## <a name="after-migration"></a>迁移之后

在计算机迁移到 Azure 后，应当完成许多步骤。

<!-- Not Available on [recovery plans]( https://docs.azure.cn/site-recovery/site-recovery-runbook-automation)-->

### <a name="post-migration-steps-in-azure"></a>Azure 中的迁移后步骤

- 执行任何迁移后应用微调，例如，更新数据库连接字符串和 Web 服务器配置。 
- 在当前在 Azure 中运行的迁移后应用程序上执行最终的应用程序和迁移验收测试。
- [Azure VM 代理](/virtual-machines/extensions/agent-windows)管理 VM 与 Azure 结构控制器之间的交互。 它是某些 Azure 服务所必需的，例如 Azure 备份、Site Recovery 和 Azure 安全。
    - 如果是迁移 VMware 计算机和物理服务器，则移动服务安装程序会在 Windows 计算机上安装可用的 Azure VM 代理。 在 Linux VM 上，建议你在故障转移后安装代理。 a
    - 如果是将 Azure VM 迁移到次要区域，则必须在迁移之前在 VM 上预配 Azure VM 代理。
    - 如果是将 Hyper-V VM 迁移到 Azure，请在迁移之后在 Azure VM 上安装 Azure VM 代理。
- 手动从 VM 中删除任何 Site Recovery 提供程序/代理。 如果迁移 VMware VM 或物理服务器，请从 Azure VM 中[卸载移动服务][vmware-azure-install-mobility-service.md#uninstall-mobility-service-on-a-windows-server-computer]。
- 为增强恢复能力，请采取以下措施：
    - 通过使用 Azure 备份服务备份 Azure VM 来确保数据安全。 [了解详细信息]( https://docs.azure.cn/backup/quick-backup-vm-portal)。
    - 通过使用 Site Recovery 将 Azure VM 复制到次要区域，使工作负荷保持运行并持续可用。 [了解详细信息](azure-to-azure-quickstart.md)。
- 为提高安全性，请采取以下措施：

    <!-- Not Available on  [Just in time administration]( https://docs.azure.cn/security-center/security-center-just-in-time)-->
    - 使用[网络安全组](/virtual-network/security-overview)限制到管理终结点的网络流量。
    
    <!-- Not Available on  [Azure Disk Encryption](/security/azure-security-disk-encryption-overview)--> <!-- Not Available on  [securing IaaS resources]( https://www.azure.cn/services/virtual-machines/secure-well-managed-iaas/ )-->
    <!-- Not Available on  [Azure Security Center](https://www.azure.cn/home/features/security-center/ )-->
    <!-- Not Available on  - For monitoring and management:-->
    <!-- Not Available on  [Azure Cost Management](/cost-management/overview)-->

### <a name="post-migration-steps-on-premises"></a>本地的迁移后步骤

- 将应用流量转移到在迁移后的 Azure VM 实例上运行的应用。
- 从本地 VM 清单中删除本地 VM。
- 从本地备份中删除本地 VM。
- 更新任何内部文档，以显示 Azure VM 的新位置和 IP 地址。

## <a name="next-steps"></a>后续步骤

在本教程中，我们已将本地 VM 迁移到 Azure VM。 现在，你可以为 Azure VM [设置到次要 Azure 区域的灾难恢复](azure-to-azure-replicate-after-migration.md)。

<!-- Update_Description: update meta properties, update link, wording update -->
