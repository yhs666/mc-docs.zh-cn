---
title: "查看使用 Azure Site Recovery 执行 Hyper-V 到 Azure 的复制（不使用 System Center VMM）的先决条件 | Azure"
description: "介绍使用 Azure Site Recovery 设置从本地 Hyper-V VM 到 Azure 的复制、故障转移和恢复的先决条件。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 7ef3fb46-52f5-4c8a-b1a1-658c2305762a
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 06/21/2017
ms.date: 08/28/2017
ms.author: v-yeche
ms.openlocfilehash: 9dbd6a2f7039a5587f684feeff10eebfd39821fd
ms.sourcegitcommit: 1ca439ddc22cb4d67e900e3f1757471b3878ca43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="step-2-review-the-prerequisites-for-hyper-v-without-vmm-to-azure-replication"></a>步骤 2：查看执行 Hyper-V（不使用 VMM）到 Azure 的复制的先决条件

下表对先决条件进行了汇总。

**先决条件** | **详细信息** 
--- | --- 
**Azure** | 了解 [Azure 要求](site-recovery-prereq.md#azure-requirements)。
**本地服务器** | [详细了解](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm)本地 Hyper-V 主机的要求。
**本地 Hyper-V VM** | 想要复制的虚拟机应正在运行[受支持的操作系统](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)，并且符合 [Azure 先决条件](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements)。
**Azure URL** | Hyper-V 主机需访问以下 URL：<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> 如果设置了基于 IP 地址的防火墙规则，请确保这些规则允许与 Azure 通信。<br/></br> 允许 [Azure 数据中心 IP 范围](https://www.microsoft.com/download/confirmation.aspx?id=41653)和 HTTPS (443) 端口。<br/></br> 允许订阅的 Azure 区域的 IP 地址范围以及中国北部的 IP 地址范围（用于访问控制和标识管理）。

## <a name="next-steps"></a>后续步骤

- 如果执行的是完整部署，请转到[步骤 3：规划容量](hyper-v-site-walkthrough-capacity.md)
- 如果执行的是简单的测试部署，请转到[步骤 4：规划网络](hyper-v-site-walkthrough-network.md)。

<!--Update_Description: update reference link -->