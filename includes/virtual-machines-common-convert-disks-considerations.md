---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 07/01/2019
ms.author: v-yeche
ms.openlocfilehash: eb989640bca73ec28ff67912fe80f8f1dcf7842c
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569670"
---
* 该转换需要重启 VM，因此请在预先存在的维护时段内计划 VM 迁移。 

* 该转换是不可逆的。 

* 请注意，任何具有[虚拟机参与者](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor)角色的用户将不能更改 VM 大小（因为它们可以预转换）。 这是因为包含托管磁盘的 VM 要求用户对 OS 磁盘具有 Microsoft.Compute/disks/write 权限。

* 请务必测试转换。 在生产环境中执行迁移之前迁移测试性虚拟机。

* 在转换期间，会解除分配 VM。 转换完成后，VM 在启动时会接收新的 IP 地址。 如果需要，可向 VM [分配静态 IP 地址](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md)。

* 查看支持转换过程所需的 Azure VM 代理最低版本。 有关如何检查和更新目标版本的信息，请参阅 [Minimum version support for VM agents in Azure](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)（对 Azure 中的 VM 代理的最低版本支持）

<!-- Update_Description: update meta properties， wordin update -->