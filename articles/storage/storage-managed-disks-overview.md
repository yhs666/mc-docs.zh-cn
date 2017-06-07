---
title: Azure 高级和标准托管磁盘概述 | Azure
description: 可在使用 Azure VM 时处理存储帐户的 Azure 托管磁盘的概述
services: storage
documentationcenter: na
author: ramankumarlive
manager: tadb
editor: tysonn

ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: ramankum
---

# Azure 托管磁盘概述

Azure 托管磁盘通过管理与 VM 磁盘关联的[存储帐户](./storage-introduction.md)来简化 Azure IaaS VM 的磁盘管理。只需指定所需的类型（[高级](./storage-premium-storage.md)或[标准](./storage-standard-storage.md)）和磁盘大小，Azure 便会自动创建和管理磁盘。

>[!NOTE]
托管磁盘要求打开端口 8443；如果想要阻止该端口，则必须使用非托管磁盘。
>

## 托管磁盘的优势

让我们了解一下使用托管磁盘可以获得的一些优势。

### 简单且可缩放的 VM 部署

托管磁盘在幕后处理存储。以前，必须创建存储帐户来存储 Azure VM 的磁盘（VHD 文件）。在扩展时，必须创建额外的存储帐户，使任何磁盘不会超出存储的 IOPS 限制。使用托管磁盘处理存储时，不再受到存储帐户限制（例如每个帐户 20,000 IOPS）的约束。另外，不再需要将自定义映像（VHD 文件）复制到多个存储帐户。可在一个中心位置管理自定义映像（每个 Azure 区域保存一个存储帐户），并使用它们在一个订阅中创建数百个 VM。

使用托管磁盘可在一个订阅中创建多达 10,000 个 VM **磁盘**，这样，便可以在单个订阅中创建数百个 **VM**。此功能还允许使用应用商店映像在 VMSS 中创建多达一千个 VM，因此进一步提高了[虚拟机规模集 (VMSS)](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) 的可伸缩性。

### 可用性集的可靠性更高

托管磁盘确保可用性集中的 VM 磁盘彼此之间完全隔离以避免单一故障点，因此提高了可用性集的可靠性。它通过自动将磁盘放在不同的存储缩放单位（戳记）中来提供这一优势。如果某个戳记因硬件或软件故障而失败，只有在该戳记中包含磁盘的 VM 实例才会失败。例如，假设某个应用程序在五个 VM 上运行并且这些 VM 位于某个可用性集中。这些 VM 的磁盘不会存储在同一个戳记中，因此，如果一个戳记发生故障，该应用程序的其他实例仍可继续运行。

### 更安全

可以使用 [Azure 基于角色的访问控制 (RBAC)](../active-directory/role-based-access-control-what-is.md) 向一个或多个用户分配对托管磁盘的特定权限。托管磁盘公开各种操作，包括读取、写入（创建/更新）、删除、导出磁盘，以及检索磁盘的[共享访问签名 (SAS) URI](./storage-dotnet-shared-access-signature-part-1.md)。可以仅向某人授予执行作业时所需的操作的访问权限。例如，如果不希望某人将托管磁盘复制到存储帐户，可以选择不授予对该托管磁盘的导出操作的访问权限。类似地，如果不希望某人使用 SAS URI 复制托管磁盘，可以选择不授予对该托管磁盘的该权限。

## <a name="pricing-and-billing"></a> 定价和计费 

使用托管磁盘时，将考虑以下计费因素：

* 存储类型

* 磁盘大小

* 事务数

* 出站数据传输

* 托管磁盘快照（全磁盘复制）

下面将更详细地介绍每个因素。

**存储类型：**托管磁盘提供 2 个性能层：[高级](./storage-premium-storage.md)（基于 SSD）和[标准](./storage-standard-storage.md)（基于 HDD）。托管磁盘的计费取决于为磁盘选择的存储类型。

**磁盘大小**：托管磁盘的计费取决于磁盘的预配大小。Azure 会将预配大小映射（向上舍入）到以下表格中指定的最接近的托管磁盘选项。每个托管磁盘将映射到其中一种受支持的预配大小并相应地计费。例如，如果创建了一个标准托管磁盘并将预配大小指定为 200 GB，则会根据 S20 磁盘类型的定价计费。

下面是高级托管磁盘可用的磁盘大小：

| **高级托管<br>磁盘类型** | **P10** | **P20** | **P30** |
|------------------|---------|---------|----------------|
| 磁盘大小 | 128 GB | 512 GB | 1024 GB (1 TB) |

下面是标准托管磁盘可用的磁盘大小：

| **标准托管<br>磁盘类型** | **S4** | **S6** | **S10** | **S20** | **S30** |
|------------------|---------|---------|----------------|---------|----------------|
| 磁盘大小 | 32 GB | 64 GB | 128 GB | 512 GB | 1024 GB (1 TB) |

**事务数**：根据在标准托管磁盘上执行的事务数计费。高级托管磁盘不产生事务费用。

**出站数据传输**：[出站数据传输](https://www.azure.cn/pricing/details/data-transfer/)（Azure 数据中心送出的数据）会产生带宽使用费。

**托管磁盘快照（全磁盘复制）**：托管快照是托管磁盘的只读副本，它作为标准托管磁盘进行存储。使用快照可以在任意时间点备份托管磁盘。这些快照独立于源磁盘存在，可用来创建新的托管磁盘。托管快照的费用与标准托管磁盘相同。例如，如果创建 128 GB 高级托管磁盘的快照，则托管快照的费用等于 128 GB 标准托管磁盘。

托管磁盘目前不支持[增量快照](./storage-incremental-snapshots.md)，但将来会支持。

若要了解有关如何使用托管磁盘创建快照的详细信息，请查看下列资源：

* [在 Windows 中使用快照创建存储为托管磁盘的 VHD 副本](../virtual-machines/windows/snapshot-copy-managed-disk.md)
* [在 Linux 中使用快照创建存储为托管磁盘的 VHD 副本](../virtual-machines/linux/snapshot-copy-managed-disk.md)

有关托管磁盘的详细定价信息，请参阅[托管磁盘定价](https://www.azure.cn/pricing/details/managed-disks/)。

## 映像

托管磁盘还支持创建托管自定义映像。可以从存储帐户中的自定义 VHD 创建映像，也可以直接从正在运行的 VM 创建映像。这会将与正在运行的 VM 关联的所有托管磁盘捕获到单个映像中，包括 OS 和数据磁盘。这样，便可以使用自定义映像创建数百个 VM，且无需复制或管理任何存储帐户。

有关创建映像的信息，请查看以下文章：
* [如何捕获 Azure 中通用 VM 的托管映像](../virtual-machines/windows/capture-image-resource/)
* [如何使用 Azure CLI 2.0（预览版）通用化和捕获 Linux 虚拟机](../virtual-machines/linux/virtual-machines-linux-capture-image.md)

## 映像与快照

我们经常看到词语“映像”与 VM 一起使用，现在又看到了“快照”。了解映像与快照之间的区别很重要。使用托管磁盘，可以创建已解除分配的通用 VM 的映像。此映像将包含附加到该 VM 的所有磁盘。可以使用此映像创建新的 VM，并在其中包含所有磁盘。

快照是磁盘在创建快照那一刻的副本。它仅适用于一个磁盘。如果某个 VM 仅包含一个磁盘 (OS)，可为它创建快照或映像，并且可以通过该快照或映像创建 VM。

如果 VM 包含五个磁盘并且这些磁盘已条带化，情况又是怎样呢？ 可以创建每个磁盘的快照，但是系统对于 VM 中的磁盘状况没有意识 – 快照只知道一个磁盘的状况。在这种情况下，快照彼此之间需要相互协调，而目前不支持此功能。因此，对于这种情况，如果希望创建 VM 的副本，则需要创建映像。默认情况下，映像将包含所有五个磁盘的协调副本。

## Azure 备份服务支持 

可以使用 Azure 备份来备份具有非托管磁盘的虚拟机。

还可将 Azure 备份服务与托管磁盘配合使用，以创建具有基于时间备份的备份作业、轻松 VM 还原和备份保留策略。可以在[对具有托管磁盘的 VM 使用 Azure 备份服务](../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup)中阅读详细内容。

## 托管磁盘和存储服务加密 (SSE)

Azure 存储支持自动对写入到存储帐户中的数据进行加密。有关更多详细信息，请参阅[静态数据的 Azure 存储服务加密](./storage-service-encryption.md)。可对托管磁盘上的数据执行哪些操作？ 目前，无法为托管磁盘启用存储服务加密，但将来会发布此功能。在此期间，需要知道如何使用位于已加密存储帐户中且自身已加密的 VHD 文件。

将数据写入存储帐户时，SSE 会将数据加密。如果某个 VHD 文件已使用 SSE 加密，则无法使用该 VHD 文件来创建使用托管磁盘的 VM。此外，无法将已加密的非托管磁盘转换为托管磁盘。最后，如果在该存储帐户中禁用加密，它不会反过来对 VHD 文件进行解密。

若要使用已加密的磁盘，必须先将 VHD 文件复制到一个从未加密过的存储帐户。然后，可以创建包含托管磁盘的 VM 并在创建期间指定该 VHD 文件，或者将复制的 VHD 文件附加到某个包含托管磁盘且正在运行的 VM。

## 后续步骤

有关托管磁盘的详细信息，请参阅以下文章。

### 托管磁盘入门 

* [使用 Resource Manager 和 PowerShell 创建 VM](../virtual-machines/virtual-machines-windows-ps-create.md)

* [使用 Azure CLI 2.0（预览版）创建 Linux VM](../virtual-machines/linux/quick-create-cli.md)

* [使用 PowerShell 将托管数据磁盘附加到 Windows VM](../virtual-machines/windows/attach-disk-ps.md)

* [将托管磁盘添加到 Linux VM](../virtual-machines/linux/quick-create-cli.md)

### 托管磁盘存储选项的比较 

* [高级存储和磁盘](./storage-premium-storage.md)

* [标准存储和磁盘](./storage-standard-storage.md)

### 操作指南

* [从 AWS 和其他平台迁移到 Azure 中的托管磁盘](../virtual-machines/windows/on-prem-to-azure.md)

* [将 Azure VM 转换为 Azure 中的托管磁盘](../virtual-machines/virtual-machines-windows-migrate-to-managed-disks.md)

<!---HONumber=Mooncake_0313_2017-->