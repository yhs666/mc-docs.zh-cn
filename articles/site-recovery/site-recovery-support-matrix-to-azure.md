---
title: "复制到 Azure 时的 Azure Site Recovery 支持矩阵 | Azure"
description: "汇总了 Azure Site Recovery 支持的操作系统和组件"
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
origin.date: 06/05/2017
ms.date: 07/10/2017
ms.author: v-yeche
ms.openlocfilehash: c9606924e02cae4c028c7bb7feee0c673bd21d0b
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 从本地复制到 Azure 时的 Azure Site Recovery 支持矩阵
<a id="azure-site-recovery-support-matrix-for-replicating-from-on-premises-to-azure" class="xliff"></a>

本文汇总了复制和恢复到 Azure 时 Azure Site Recovery 支持的配置和组件。 有关 Azure Site Recovery 要求的详细信息，请参阅[先决条件](site-recovery-prereq.md)。

## 部署选项支持
<a id="support-for-deployment-options" class="xliff"></a>

**部署** | **VMware/物理服务器** | **Hyper-V（具有/不具有 Virtual Machine Manager）** |
--- | --- | ---
**Azure 门户** | 本地 VMware VM 到 Azure 存储，使用 Azure Resource Manager 或经典存储和网络。<br/><br/> 故障转移到基于 Resource Manager 的 VM 或经典 VM。 | 本地 Hyper-V VM 到 Azure 存储，使用 Resource Manager 或经典存储和网络。<br/><br/> 故障转移到基于 Resource Manager 的 VM 或经典 VM。
**经典管理门户** | 仅限维护模式。 无法创建新的保管库。 | 仅限维护模式。
**PowerShell** | 目前不支持。 | 支持

## 数据中心管理服务器支持
<a id="support-for-datacenter-management-servers" class="xliff"></a>

### 虚拟化管理实体
<a id="virtualization-management-entities" class="xliff"></a>

**部署** | **支持**
--- | ---
**VMware VM/物理服务器** | vSphere 6.0、5.5 或具有最新更新的 5.1
**Hyper-V（具有 Virtual Machine Manager）** | System Center Virtual Machine Manager 2016 和 System Center Virtual Machine Manager 2012 R2

  >[!Note]
  > 目前不支持混合使用 Windows Server 2016 和 2012 R2 主机的 System Center Virtual Machine Manager 2016 云。

### 主机服务器
<a id="host-servers" class="xliff"></a>

**部署** | **支持**
--- | ---
**VMware VM/物理服务器** | vCenter 5.5 或 6.0（仅支持 5.5 功能）
**Hyper-V（具有/不具有 Virtual Machine Manager）** | Windows Server 2016、带有最新更新的 Windows Server 2012 R2。<br></br>如果使用了 SCVMM，Windows Server 2016 主机应由 SCVMM 2016 托管。

  >[!Note]
  >目前不支持混合使用主机（运行 Windows Server 2016 和 2012 R2）的 Hyper-V 站点。 当前不支持恢复到 Windows Server 2016 主机上 VM 的备用位置。

## 复制计算机 OS 版本支持
<a id="support-for-replicated-machine-os-versions" class="xliff"></a>

复制到 Azure 时，受保护的虚拟机必须符合 [Azure 要求](#failed-over-azure-vm-requirements)。
下表总结了使用 Azure Site Recovery 时的各种部署方案中的复制操作系统支持。 此支持适用于在所述的 OS 上运行的任何工作负荷。

 **VMware/物理服务器** | Hyper-V（有/无 VMM） |
--- | --- |
64 位 Windows Server 2012 R2、Windows Server 2012、至少具有 SP1 的 Windows Server 2008 R2<br/><br/> Red Hat Enterprise Linux 6.7、6.8、7.1、7.2 <br/><br/>CentOS 6.5、6.6、6.7、 6.8、7.0、7.1、7.2 <br/><br/>Ubuntu 14.04 LTS 服务器[（支持的内核版本）](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Oracle Enterprise Linux 6.4、6.5（运行 Red Hat 兼容内核或 Unbreakable Enterprise Kernel Release 3 (UEK3)） <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> SUSE Linux Enterprise Server 11 SP4 <br/>（不支持复制计算机从 SLES 11 SP3 升级到 SLES 11 SP4。 如果已将复制计算机从 SLES 11SP3 升级到 SLES 11 SP4，则需要禁用复制，并在升级后再次对计算机启用保护。） | [Azure 支持的](https://technet.microsoft.com/library/cc794868.aspx)

>[!Note]
> 在 Linux 服务器中，下列目录（如果设置为单独的分区/文件系统）必须位于源服务器的同一磁盘（OS 磁盘）：   / (root)、/boot、/usr、/usr/local、/var 和 /etc<br/><br/>
> XFS 文件系统上的 ASR 目前不支持 XFS v5 功能（例如元数据校验和）。 请确保 XFS 文件系统不使用任何 v5 功能。 可以使用 xfs_info 实用工具来检查 XFS 超级块中是否存在分区。 如果 ftype 设置为 1，则会使用 XFSv5 功能。
>

## 网络配置支持
<a id="support-for-network-configuration" class="xliff"></a>
下表总结了使用 Azure Site Recovery 复制到 Azure 时的各种部署方案中的网络配置支持。

### 主机网络配置
<a id="host-network-configuration" class="xliff"></a>

**配置** | **VMware/物理服务器** | **Hyper-V（具有/不具有 Virtual Machine Manager）**
--- | --- | ---
NIC 组合 | 是<br/><br/>在物理机中不受支持| 是
VLAN | 是 | 是
IPv4 | 是 | 是
IPv6 | 否 | 否

### 来宾 VM 网络配置
<a id="guest-vm-network-configuration" class="xliff"></a>

**配置** | **VMware/物理服务器** | **Hyper-V（具有/不具有 Virtual Machine Manager）**
--- | --- | ---
NIC 组合 | 否 | 否
IPv4 | 是 | 是
IPv6 | 否 | 否
静态 IP (Windows) | 是 | 是
静态 IP (Linux) | 否 | 否
多 NIC | 是 | 是

### 故障转移 Azure VM 网络配置
<a id="failed-over-azure-vm-network-configuration" class="xliff"></a>

**Azure 网络** | **VMware/物理服务器** | **Hyper-V（具有/不具有 Virtual Machine Manager）**
--- | --- | ---
Express Route | 是 | 是
ILB | 是 | 是
ELB | 是 | 是
流量管理器 | 是 | 是
多 NIC | 是 | 是
保留 IP | 是 | 是
IPv4 | 是 | 是
保留源 IP | 是 | 是

## 存储支持
<a id="support-for-storage" class="xliff"></a>
下表总结了使用 Azure Site Recovery 复制到 Azure 时的各种部署方案中的存储配置支持。

### 主机存储配置
<a id="host-storage-configuration" class="xliff"></a>

**配置** | **VMware/物理服务器** | **Hyper-V（具有/不具有 Virtual Machine Manager）**
--- | --- | ---
NFS | 在 VMware 上支持<br/><br/> 在物理服务器上不支持 | 不适用
SMB 3.0 | 不适用 | 是
SAN (ISCSI) | 是 | 是
多路径 (MPIO)<br></br>已使用 Microsoft DSM、EMC PowerPath 5.7 SP4、EMC PowerPath DSM for CLARiiON 进行测试 | 是 | 是

### 来宾或物理服务器存储配置
<a id="guest-or-physical-server-storage-configuration" class="xliff"></a>

**配置** | **VMware/物理服务器** | **Hyper-V（具有/不具有 Virtual Machine Manager）**
--- | --- | ---
VMDK | 是 | 不适用
VHD/VHDX | 不适用 | 是
第 2 代 VM | 不适用 | 是
EFI/UEFI| 否 | 是
共享群集磁盘 | 在 VMware 上支持<br/><br/> 在物理服务器上不适用 | 否
加密磁盘 | 否 | 否
NFS | 否 | 不适用
SMB 3.0 | 否 | 否
RDM | 是<br/><br/> 在物理服务器上不适用 | 不适用
磁盘 > 1 TB | 否 | 否
包含条带化磁盘的卷 > 1 TB<br/><br/> LVM 逻辑卷管理 | 是 | 是
存储空间 | 否 | 是
热添加/移除磁盘 | 否 | 否
排除磁盘 | 是 | 是
多路径 (MPIO) | 不适用 | 是

**Azure 存储** | **VMware/物理服务器** | **Hyper-V（具有/不具有 Virtual Machine Manager）**
--- | --- | ---
LRS | 是 | 是
GRS | 是 | 是
RA-GRS | 是 | 是
冷存储 | 否 | 否
热存储| 否 | 否
静态加密 (SSE)| 是 | 是
高级存储 | 是 | 是
导入/导出服务 | 否 | 否

## Azure 计算配置支持
<a id="support-for-azure-compute-configuration" class="xliff"></a>

**计算功能** | **VMware/物理服务器** | **Hyper-V（具有/不具有 Virtual Machine Manager）**
--- | --- | ---
可用性集 | 是 | 是
HUB | 是 | 是  

## 故障转移 Azure VM 要求
<a id="failed-over-azure-vm-requirements" class="xliff"></a>

可以部署 Site Recovery 以复制运行受 Azure 支持的任何操作系统的虚拟机和物理服务器。 这包括大多数的 Windows 和 Linux 版本。 复制到 Azure 时，想要复制的本地 VM 必须符合以下 Azure 要求。

**实体** | **要求** | **详细信息**
--- | --- | ---
**来宾操作系统** | 对于从 Hyper-V 到 Azure 的复制，Site Recovery 支持 [Azure 支持](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx)的所有操作系统。| 如果不支持，先决条件检查将会失败。
**来宾操作系统体系结构** | 64 位 | 如果不支持，先决条件检查将会失败
**操作系统磁盘大小** | 最大 1023 GB | 如果不支持，先决条件检查将会失败
**操作系统磁盘计数** | 1 | 如果不支持，先决条件检查将会失败。
**数据磁盘计数** | 如果将 **VMware VM 复制到 Azure**，则为 64 个或更少；如果将 **Hyper-V VM 复制到 Azure**，则为 16 个或更少 | 如果不支持，先决条件检查将会失败
**数据磁盘 VHD 大小** | 最大 1023 GB | 如果不支持，先决条件检查将会失败
**网络适配器** | 支持多个适配器 |
**共享 VHD** | 不支持 | 如果不支持，先决条件检查将会失败
**FC 磁盘** | 不支持 | 如果不支持，先决条件检查将会失败
**硬盘格式** | VHD <br/><br/> VHDX | 尽管 Azure 目前不支持 VHDX，但当你故障转移到 Azure 时，Site Recovery 会自动将 VHDX 转换为 VHD。 当你故障回复到本地时，虚拟机将继续使用 VHDX 格式。
**Bitlocker** | 不支持 | 保护虚拟机之前，必须先禁用 Bitlocker。
**VM 名称** | 介于 1 和 63 个字符之间。 限制为字母、数字和连字符。 VM 名称必须以字母或数字开头和结尾。 | 在 Site Recovery 中更新虚拟机属性中的值。
**VM 类型** | 第 1 代<br/><br/> 第 2 代 - Windows | OS 磁盘类型为“基本”的第 2 代 VM（其中包括一个或两个格式化为 VHDX 的数据卷），并且支持的磁盘空间大小小于 300 GB。<br></br>不支持 Linux 第 2 代 VM。 [了解详细信息](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## 恢复服务保管库操作支持
<a id="support-for-recovery-services-vault-actions" class="xliff"></a>

**操作** | **VMware/物理服务器** | **Hyper-V（无 Virtual Machine Manager）** | **Hyper-V（具有 Virtual Machine Manager）**
--- | --- | --- | ---
跨资源组移动保管库<br/><br/> 订阅内和跨订阅移动 | 否 | 否 | 否
跨资源组移动存储、网络和 Azure VM<br/><br/> 订阅内和跨订阅移动 | 否 | 否 | 否

## 提供程序和代理支持
<a id="support-for-provider-and-agent" class="xliff"></a>

**Name** | **说明** | **最新版本** | **详细信息**
--- | --- | --- | --- | ---
**Azure Site Recovery 提供程序** | 协调本地服务器与 Azure 之间的通信 <br/><br/> 安装在本地 Virtual Machine Manager 服务器或 Hyper-V 服务器（如果没有 Virtual Machine Manager 服务器）上 | 5.1.19（[可从门户获取](http://aka.ms/downloaddra)） | [最新功能和修复](https://support.microsoft.com/kb/3155002)
**Azure Site Recovery 统一安装（VMware 到 Azure）** | 协调本地 VMware 服务器和 Azure 之间的通信 <br/><br/> 安装在本地 VMware 服务器上 | 9.3.4246.1（可从门户获取） | [最新功能和修复](https://support.microsoft.com/kb/3155002)
**移动服务** | 协调本地 VMware 服务器/物理服务器和 Azure/辅助站点之间的复制<br/><br/> 在想要复制的 VMware VM 或物理服务器上安装  | N/A（可从门户获取） | 不适用
**Azure 恢复服务 (MARS) 代理** | 协调 Hyper-V VM 与 Azure 之间的复制<br/><br/> 在本地 Hyper-V 服务器（包含或不包含 Virtual Machine Manager 服务器）上安装 | 最新代理（[可从门户获取](http://aka.ms/latestmarsagent)） |

## 后续步骤
<a id="next-steps" class="xliff"></a>
[检查先决条件](site-recovery-prereq.md)