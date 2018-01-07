---
title: "查看在 Azure 区域之间执行 Azure VM 复制的体系结构 | Azure"
description: "本文概述使用 Azure Site Recovery 服务在 Azure 区域之间复制 Azure VM 时所用的组件和体系结构。"
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
origin.date: 09/10/2017
ms.date: 01/01/2018
ms.author: v-yeche
ms.openlocfilehash: 6015a01a87d198cffede405aa99597b9b5e40b1e
ms.sourcegitcommit: 90e4b45b6c650affdf9d62aeefdd72c5a8a56793
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2017
---
# <a name="azure-to-azure-replication-architecture"></a>Azure 到 Azure 复制体系结构

本文介绍使用 [Azure Site Recovery](../site-recovery-overview.md) 服务在 Azure 区域之间对 Azure 虚拟机 (VM) 进行复制、故障转移和恢复时使用的体系结构和过程。

>[!NOTE]
>使用 Site Recovery 服务进行 Azure VM 复制当前处于预览阶段。

## <a name="architectural-components"></a>体系结构组件

下面的概要视图展示了某个特定区域（在此示例中为“中国东部”位置）中的 Azure VM 环境。 在 Azure VM 环境中：
- 应用可在包含的磁盘分布于多个存储帐户中 VM 上运行。
- VM 可以包含在虚拟网络中的一个或多个子网中。

**Azure 到 Azure 复制**

![客户环境](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>复制过程

### <a name="step-1"></a>步骤 1

启用 Azure VM 复制时，将根据源区域设置自动在目标区域中创建下面所示的资源。 可根据需要自定义目标资源设置。 

![启用复制过程，步骤 1](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**资源** | **详细信息**
--- | ---
目标资源组 | 故障转移后复制的 VM 所属的资源组。
目标虚拟网络 | 故障转移后复制的 VM 所在的虚拟网络。 创建源虚拟网络与目标虚拟网络之间的网络映射，反之亦然。
缓存存储帐户 | 在源 VM 更改复制到目标存储帐户前，系统会跟踪这些更改并将更改发送到目标位置的缓存存储帐户。 这可最大限度降低对在 VM 上运行的生产应用的影响。
目标存储帐户  | 目标位置中的存储帐户，将向其中复制数据。
目标可用性集  | 故障转移后复制的 VM 所在的可用性集。

### <a name="step-2"></a>步骤 2

由于启用了复制，Site Recovery 扩展移动服务会自动安装在 VM 上：

1. 向 Site Recovery 注册 VM。

2. 为 VM 配置连续复制。 在 VM 磁盘上写入的数据会连续传输到源位置的缓存存储帐户。

   ![启用复制过程，步骤 2](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)

 Site Recovery 不必与 VM 建立入站连接。 仅需要与 Site Recovery 服务 URL/IP 地址、Office 365 身份验证 URL/IP 地址和缓存存储帐户 IP 地址进行出站连接。

### <a name="step-3"></a>步骤 3

正在连续复制时，磁盘写入内容会立即传输到缓存存储帐户。 Site Recovery 处理数据，并将其发送到目标存储帐户。 处理数据后，每隔几分钟就会在目标存储帐户中生成恢复点。

## <a name="failover-process"></a>故障转移过程

如果启动故障转移，系统会在目标资源组、目标虚拟网络、目标子网和目标可用性集中创建 VM。 可在故障转移过程中使用任意恢复点。

![故障转移过程](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>后续步骤

遵循[快速入门](azure-to-azure-quickstart.md)将 Azure VM 复制到次要区域。
<!-- Update_Description: new articles on concepts azure to azure architecture -->