---
title: 在 Azure Stack 上运行 Windows 虚拟机 | Microsoft Docs
description: 了解如何在 Azure Stack 上运行 Windows 虚拟机。
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: how-to
origin.date: 11/01/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.reviewer: kivenkat
ms.lastreviewed: 11/01/2019
ms.openlocfilehash: be11af9a1e01731a9b89c4103c861b15996329ab
ms.sourcegitcommit: 7dfb76297ac195e57bd8d444df89c0877888fdb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020539"
---
# <a name="run-a-windows-virtual-machine-on-azure-stack"></a>在 Azure Stack 上运行 Windows 虚拟机

除 VM 本身以外，在 Azure Stack 中预配虚拟机 (VM) 还需要其他一些组件，包括网络和存储资源。 本文介绍在 Azure 上运行 Windows VM 的最佳做法。

![Azure Stack 上的 Windows VM 体系结构](./media/iaas-architecture-vm-windows/image1.png)

## <a name="resource-group"></a>资源组

[资源组](/azure-resource-manager/resource-group-overview)是保存相关 Azure Stack 资源的逻辑容器。 一般情况下，可根据资源的生存期及其管理者将资源分组。

将共享相同生命周期、密切相关的资源放入同一[资源组](/azure-resource-manager/resource-group-overview)。 资源组可让你以组的形式部署和监视资源，并按资源组跟踪计费成本。 还可以删除作为集的资源，这适用于测试部署。 指定有意义的资源名称，以便简化特定资源的查找并了解其角色。 有关详细信息，请参阅 [Azure 资源的建议命名约定](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)。

## <a name="virtual-machine"></a>虚拟机

可以通过发布的映像列表或上传到 Azure Stack Blob 存储的自定义托管映像或虚拟硬盘 (VHD) 文件来预配 VM。

Azure Stack 提供了与 Azure 不同的虚拟机大小。 有关详细信息，请参阅 [Azure Stack 中的虚拟机大小](/azure-stack/user/azure-stack-vm-sizes)。 如果要将现有工作负荷转移到 Azure Stack，开始时请先使用与本地服务器/Azure 最匹配的 VM 大小。 然后从 CPU、内存和每秒磁盘输入/输出操作次数 (IOPS) 等方面测量实际工作负荷的性能，并根据需要调整大小。

## <a name="disks"></a>磁盘

成本取决于预配磁盘的容量。 IOPS 和吞吐量（即数据传输速率）取决于 VM 大小，因此在预配磁盘时，请全面考虑三个因素（容量、IOPS 和吞吐量）。

Azure Stack 上的磁盘 IOPS（每秒输入/输出操作次数）是与 [VM 大小](/azure-stack/user/azure-stack-vm-sizes)相关的函数，而与磁盘类型无关。 这意味着，对于 Standard_Fs 系列 VM，不管你选择 SSD 还是 HDD 作为磁盘类型，单个额外的数据磁盘的 IOPS 限制都是 2300。 施加的 IOPS 限制是一种上限（最大可能值），目的是防止邻域干扰。 它不是你会在特定 VM 大小上获得的 IOPS 的保证。

我们还建议使用[托管磁盘](/azure-stack/user/azure-stack-managed-disk-considerations)。 托管磁盘可代你处理存储，简化磁盘管理。 托管磁盘不需要存储帐户。 只需指定磁盘的大小和类型，就可以将它部署为高度可用的资源。

OS 磁盘是存储在 Azure Stack blob 存储中的 VHD，因此即使主机关闭，OS 磁盘也仍然存在。 我们还建议创建一个或多个[数据磁盘](/azure-stack/user/azure-stack-manage-vm-disks)（用于保存应用程序数据的持久性 VHD）。 如果可能，请将应用程序安装在数据磁盘上，而不是 OS 磁盘上。 某些旧版应用程序可能需要将组件安装在 C: 驱动器上；在这种情况下，可使用 PowerShell [重设 OS 磁盘的大小](/virtual-machines/virtual-machines-windows-expand-os-disk)。

还使用临时磁盘（Windows 上的 D: 驱动器）创建 VM。 此磁盘存储在 Azure Stack 存储基础结构中的临时卷上。 它在重新启动期间以及发生其他 VM 生命周期事件期间可能会被删除。 只使用此磁盘存储临时数据，如页面文件或交换文件。

## <a name="network"></a>网络

网络组件包括以下资源：

-   虚拟网络  。 每个 VM 都会部署到可细分为多个子网的虚拟网络中。

-   **网络接口 (NIC)** 。 NIC 使 VM 能够与虚拟网络进行通信。 如果 VM 需要多个 NIC，请注意每种 [VM 大小](/azure-stack/user/azure-stack-vm-sizes)都定义了最大 NIC 数量。

-   **公共 IP 地址/VIP**。 需要使用公共 IP 地址才能与 VM 通信 - 例如，通过远程桌面 (RDP)。 公共 IP 地址可以是动态的或静态的。 默认是动态的。

-   如果需要不会更改的固定 IP 地址 — 例如，如果需要创建 DNS 'A' 记录或将 IP 地址添加到安全列表，请保留[静态 IP 地址](/virtual-network/virtual-networks-reserved-public-ip)。

-   还可以为 IP 地址创建完全限定的域名 (FQDN)。 然后，可以在 DNS 中注册指向 FQDN 的 [CNAME 记录](https://en.wikipedia.org/wiki/CNAME_record)。 有关详细信息，请参阅[在 Azure 门户中创建完全限定的域名](/virtual-machines/virtual-machines-windows-portal-create-fqdn)。

-   **网络安全组 (NSG)** 。 NSG 用于允许或拒绝流向 VM 的网络流量。 NSG 可与子网或单个 VM 实例相关联。

所有 NSG 都包含一组[默认规则](/virtual-network/security-overview#default-security-rules)，其中包括阻止所有入站 Internet 流量的规则。 无法删除默认规则，但其他规则可以覆盖它们。 若要启用 Internet 流量，请创建允许特定端口入站流量的规则 — 例如，将端口 80 用于 HTTP。 若要启用 RDP，请添加允许 TCP 端口 3389 的入站流量的 NSG 规则。

## <a name="operations"></a>操作

**诊断**。 启用监视和诊断，包括基本运行状况指标、诊断基础结构日志和[启动诊断](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/)。 如果 VM 陷入不可启动状态，启动诊断有助于诊断启动故障。 创建用于存储日志的 Azure 存储帐户。 标准的本地冗余存储 (LRS) 帐户足以存储诊断日志。 有关详细信息，请参阅[启用监视和诊断](/azure-stack/user/azure-stack-metrics-azure-data)。

**可用性**。 由于 Azure Stack 操作员计划的计划内维护，你的 VM 可能需要重新启动。 为了提高可用性，请在[可用性集](/azure-stack/operator/azure-stack-overview#providing-high-availability)中部署多个 VM。

**备份** 有关保护 Azure Stack IaaS VM 的建议，请参阅此文。

**停止 VM**。 Azure 对“已停止”和“已解除分配”状态进行了区分。 VM 状态为“已停止”时，将计费，但 VM 为“已解除分配”状态时，则不计费。 在 Azure Stack 门户中，“停止”  按钮可解除分配 VM。 如果在已登录时通过 OS 关闭，VM 会停止，但  不会解除分配，因此仍会产生费用。

**删除 VM**。 如果删除 VM，不会删除 VM 磁盘。 这意味着可以安全地删除 VM，而不会丢失数据。 但是，仍将收取存储费用。 若要删除 VM 磁盘，请删除托管磁盘对象。 要防止意外删除，请使用*资源锁*锁定整个资源组或锁定单个资源（如 VM）。

## <a name="security-considerations"></a>安全注意事项

将 VM 载入到 [Azure 安全中心](/security-center/quick-onboard-azure-stack)以获取 Azure 资源的安全状态的中心视图。 安全中心监视潜在的安全问题，并全面描述了部署的安全运行状况。 安全中心针对每个 Azure 订阅进行配置。 根据[将 Azure 订阅载入安全中心标准版](/security-center/security-center-get-started)中所述启用安全数据收集。 启用数据收集后，安全中心会自动扫描该订阅下创建的所有 VM。

**修补程序管理**。 在 VM 上配置修补程序管理。 如果启用，安全中心会检查是否缺少任何安全更新和关键更新。 使用 VM 上的[组策略设置](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates)可启用自动系统更新。

**反恶意软件**。 如果启用，安全中心将检查是否已安装反恶意软件。 还可以使用安全中心从 Azure 门户中安装反恶意软件。

**访问控制**。 使用[基于角色的访问控制 (RBAC)](/active-directory/role-based-access-control-what-is) 来控制对 Azure 资源的访问。 RBAC 允许你将授权角色分配给开发运营团队的成员。 例如，“读者”角色可以查看 Azure 资源，但不能创建、管理或删除这些资源。 某些权限特定于 Azure 资源类型。 例如，“虚拟机参与者”角色可以执行重启或解除分配 VM、重置管理员密码、创建新的 VM 等操作。 可能对此体系结构有用的其他[内置 RBAC 角色](/active-directory/role-based-access-built-in-roles)包括[开发测试实验室用户](/active-directory/role-based-access-built-in-roles#devtest-labs-user)和[网络参与者](/active-directory/role-based-access-built-in-roles#network-contributor)。

> [!Note]  
> RBAC 不限制已登录到 VM 的用户可以执行的操作。 这些权限由来宾 OS 上的帐户类型决定。

**审核日志**。 使用[活动日志](/azure-stack/user/azure-stack-metrics-azure-data?#activity-log)可查看预配操作和其他 VM 事件。

**数据加密**。 Azure Stack 使用静态加密来保护存储子系统级别的用户数据和基础结构数据。 Azure Stack 的存储子系统是配合 128 位 AES 加密法使用 BitLocker 加密的。 有关更多详细信息，请参阅[此文](/azure-stack/operator/azure-stack-security-bitlocker)。


## <a name="next-steps"></a>后续步骤

- 若要详细了解 Azure Stack VM，请参阅 [Azure Stack VM 功能](azure-stack-vm-considerations.md)。  
- 若要详细了解 Azure 云模式，请参阅[云设计模式](https://docs.microsoft.com/azure/architecture/patterns)。
