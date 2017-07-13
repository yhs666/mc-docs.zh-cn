---
title: "使用 Azure Site Recovery 复制到辅助站点时的支持矩阵 | Azure"
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
origin.date: 05/24/2017
ms.date: 07/10/2017
ms.author: v-yeche
ms.openlocfilehash: e529ec7df1a61ff391f7582ccf42a256cb956136
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 使用 Azure Site Recovery 复制到辅助站点时的支持矩阵
<a id="support-matrix-for-replication-to-a-secondary-site-with-azure-site-recovery" class="xliff"></a>

本文总结了使用 Azure Site Recovery 复制到辅助本地站点时受到支持的功能。

## 部署选项
<a id="deployment-options" class="xliff"></a>

**部署** | **VMware/物理服务器** | Hyper-V（有/无 SCVMM）
--- | --- | ---
**Azure 门户** | 本地 VMware VM 到辅助 VMware 站点。<br/><br/> 下载 [InMage Scout 用户指南](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf)（Azure 门户中未提供）。 | 将 VMM 云中的本地 Hyper-V VM 复制到辅助 VMM 云。<br></br> 如果不具有 VMM，则不支持  <br/><br/> 仅限标准 Hyper-V 副本。 不支持 SAN。
**经典管理门户** | 仅限维护模式。 无法创建新的保管库。 | 仅限维护模式<br></br> 支持 SCVMM
**PowerShell** | 不支持 | 支持<br></br> 支持 SCVMM

## 本地服务器
<a id="on-premises-servers" class="xliff"></a>

### 虚拟化服务器
<a id="virtualization-servers" class="xliff"></a>

**部署** | **支持**
--- | ---
**VMware VM/物理服务器** | vSphere 6.0、5.5 或具有最新更新的 5.1
Hyper-V（有 VMM） | VMM 2016 和 VMM 2012 R2

  > [!NOTE]
  > 目前不支持混合使用 Windows Server 2016 和 2012 R2 主机的 VMM 2016 云。

### 主机服务器
<a id="host-servers" class="xliff"></a>

**部署** | **支持**
--- | ---
**VMware VM/物理服务器** | vCenter 5.5 或 6.0（仅支持 5.5 功能）
Hyper-V（无 VMM） | 不是复制到辅助站点时的受支持配置
**Hyper-V（有 VMM）** | Windows Server 2016 和带最新更新的 Windows Server 2012 R2。<br/><br/> Windows Server 2016 主机应由 VMM 2016 托管。

## 复制计算机 OS 版本支持
<a id="support-for-replicated-machine-os-versions" class="xliff"></a>
下表总结了使用 Azure Site Recovery 时遇到的各种部署方案中的操作系统支持。 此支持适用于在所述的 OS 上运行的任何工作负荷。

**VMware/物理服务器** | Hyper-V（有 VMM）
--- | --- | ---
64 位 Windows Server 2012 R2、Windows Server 2012、至少具有 SP1 的 Windows Server 2008 R2<br/><br/> Red Hat Enterprise Linux 6.7、7.1、7.2 <br/><br/> Centos 6.5、6.6、6.7、7.0、7.1、7.2 <br/><br/> Oracle Enterprise Linux 6.4/6.5（运行 Red Hat 兼容内核），或 Unbreakable Enterprise Kernel Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 | [Hyper-V 支持](https://technet.microsoft.com/library/mt126277.aspx)的所有来宾操作系统

>[!NOTE]
>只能复制具有以下存储的 Linux 计算机：文件系统（EXT3、ETX4、ReiserFS、XFS）；多路径软件设备映射器；卷管理器 (LVM2)。
>不支持使用 HP CCISS 控制器存储的物理服务器。
>ReiserFS 文件系统仅在 SUSE Linux Enterprise Server 11 SP3 上受支持。

## 网络配置
<a id="network-configuration" class="xliff"></a>

### 主机
<a id="hosts" class="xliff"></a>

**配置** | **VMware/物理服务器** | Hyper-V（有 VMM）
--- | --- | ---
NIC 组合 | 是 | 是
VLAN | 是 | 是
IPv4 | 是 | 是
IPv6 | 否 | 否

### 来宾 VM
<a id="guest-vms" class="xliff"></a>

**配置** | **VMware/物理服务器** | Hyper-V（有 VMM）
--- | --- | ---
NIC 组合 | 否 | 否
IPv4 | 是 | 是
IPv6 | 否 | 否
静态 IP (Windows) | 是 | 是
静态 IP (Linux) | 是 | 是
多 NIC | 是 | 是

## 存储
<a id="storage" class="xliff"></a>

### 主机存储
<a id="host-storage" class="xliff"></a>

存储（主机） | **VMware/物理服务器** | Hyper-V（有 VMM）
--- | --- | ---
NFS | 是 | 不适用
SMB 3.0 | 不适用 | 是
SAN (ISCSI) | 是 | 是
多路径 (MPIO) | 是 | 是

### 来宾或物理服务器存储
<a id="guest-or-physical-server-storage" class="xliff"></a>

**配置** | **VMware/物理服务器** | Hyper-V（有 VMM）
--- | --- | ---
VMDK | 是 | 不适用
VHD/VHDX | 不适用 | 是（最多 16 个磁盘）
第 2 代 VM | 不适用 | 是
共享群集磁盘 | 是  | 否
加密磁盘 | 否 | 否
UEFI| 否 | 不适用
NFS | 否 | 否
SMB 3.0 | 否 | 否
RDM | 是 | 不适用
磁盘 > 1 TB | 否 | 是
包含条带化磁盘的卷 > 1 TB<br/><br/> LVM | 是 | 是
存储空间 | 否 | 是
热添加/移除磁盘 | 否 | 否
排除磁盘 | 否 | 是
多路径 (MPIO) | 不适用 | 是

## 保管库
<a id="vaults" class="xliff"></a>

**操作** | **VMware/物理服务器** | Hyper-V（有 VMM）
--- | --- | ---
跨资源组移动保管库（订阅内或跨订阅移动） | 否 | 否
跨资源组移动存储、网络和 Azure VM（订阅内或跨订阅移动） | 否 | 否

## 提供程序和代理
<a id="provider-and-agent" class="xliff"></a>

**Name** | **说明** | **最新版本** | **详细信息**
--- | --- | --- | --- | ---
**Azure Site Recovery 提供程序** | 协调本地服务器与 Azure 之间的通信 <br/><br/> 安装在本地 VMM 服务器或 Hyper-V 服务器（如果没有 VMM 服务器）上 | 5.1.19（[可从门户获取](http://aka.ms/downloaddra)） | [最新功能和修复](https://support.microsoft.com/zh-cn/kb/3155002)
**移动服务** | 协调本地 VMware 服务器/物理服务器与辅助站点之间的复制<br/><br/> 在想要复制的 VMware VM 或物理服务器上安装  | N/A（可从门户获取） | 不适用

## 后续步骤
<a id="next-steps" class="xliff"></a>

- [将 VMM 云中的 Hyper-V VM 复制到辅助站点](site-recovery-vmm-to-vmm.md)
