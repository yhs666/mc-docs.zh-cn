---
title: 使用 Azure Site Recovery 将 Azure IaaS VM 在 Azure 区域之间进行灾难恢复时的 Azure Site Recovery 支持矩阵 | Azure
description: 总结出于灾难恢复 (DR) 需求将 Azure 虚拟机 (VM) 从一个区域复制到另一个区域时 Azure Site Recovery 支持的操作系统和配置。
services: site-recovery
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.devlang: na
ms.topic: article
origin.date: 10/28/2018
ms.date: 12/10/2018
ms.author: v-yeche
ms.openlocfilehash: 023e0ba49ac79f628f32639357d63225aa3ed373
ms.sourcegitcommit: 5f2849d5751cb634f1cdc04d581c32296e33ef1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53028628"
---
# <a name="support-matrix-for-replicating-from-one-azure-region-to-another"></a>用于在 Azure 区域之间进行复制的支持矩阵

本文总结了使用 [Azure Site Recovery](site-recovery-overview.md) 服务在不同 Azure 区域之间通过 Azure VM 复制、故障转移和恢复来部署灾难恢复时所支持的配置和组件。

## <a name="deployment-method-support"></a>部署方法支持

**部署方法** |  支持/不支持
--- | ---
**Azure 门户** | 支持
**PowerShell** | [使用 PowerShell 进行 Azure 到 Azure 的复制](azure-to-azure-powershell.md)
**REST API** | 目前不支持
**CLI** | 目前不支持

## <a name="resource-support"></a>资源支持

**资源操作** | **详细信息**
--- | --- | ---
跨资源组移动保管库 | 不支持
**跨资源组移动计算/存储/网络资源** | 不支持。<br/><br/> 如果在 VM 复制后移动 VM 或相关组件（如存储/网络），则需要禁用并重新启用 VM 的复制。
**将 Azure VM 从一个订阅复制到另一个订阅以进行灾难恢复** | 在同一 Azure Active Directory 租户中受支持。
**在受支持的地理群集内跨区域迁移虚拟机（订阅内和跨订阅）** | 在同一 Azure Active Directory 租户中受支持。
**在同一区域内迁移 VM** | 不支持。

# <a name="region-support"></a>区域支持

可在同一地理群集中的任意两个区域之间复制和恢复 VM。

地理群集 | **Azure 区域**
-- | --
中国 | 中国东部、中国北部

## <a name="cache-storage"></a>缓存存储

此表汇总了对复制期间 Site Recovery 使用的缓存存储帐户的支持。

**设置** | **详细信息**
--- | ---
常规用途 V2 存储帐户（热存储层和冷存储层） | 不支持。 | 因为 V2 的事务成本远高于 V1 存储帐户，因此缓存存储存在限制。
虚拟网络的 Azure 存储防火墙  | 否 | 不支持访问用于存储复制数据的缓存存储帐户上特定的 Azure 虚拟网络。

## <a name="replicated-machine-operating-systems"></a>复制的计算机操作系统

Site Recovery 支持复制那些运行本节中所列操作系统的 Azure VM。

### <a name="windows"></a>Windows

**操作系统** | **详细信息**
--- | ---
Windows Server 2016  | 服务器核心、带桌面体验的服务器
Windows Server 2012 R2 |
Windows Server 2012 |
Windows Server 2008 R2 | 运行 SP1 或更高版本

#### <a name="linux"></a>Linux

**操作系统** | **详细信息**
--- | ---
CentOS | 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3,7.4, 7.5
Ubuntu 14.04 LTS Server | [受支持的内核版本](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Ubuntu 16.04 LTS Server | [受支持的内核版本](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)<br/><br/> 使用基于密码的身份验证和登录的 Ubuntu 服务器以及用于配置云 VM 的 cloud-init 包可能会在故障转移时禁用基于密码的登录（具体取决于 cloudinit 配置）。 通过从“支持”>“故障排除”>“设置”菜单（Azure 门户中的故障转移 VM）重置密码，可以在虚拟机上重新启用基于密码的登录。
Debian 7 | [受支持的内核版本](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | [受支持的内核版本](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1、SP2、SP3。 [受支持的内核版本](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> 不支持将复制计算机从 SP3 升级到 SP4。 如果已升级复制的计算机，则需要禁用复制并在升级后重新启用复制。
SUSE Linux Enterprise Server 11 | SP4

<!-- Not Available - Red Hat Enterprise Linux 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5 -->
<!-- Not Available - Oracle Enterprise Linux 6.4, 6.5 -->

#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Azure 虚拟机支持的 Ubuntu 内核版本

**版本** | **移动服务版本** | **内核版本** |
--- | --- | --- |
14.04 LTS | 9.19 | 3.13.0-24-generic 到 3.13.0-153-generic、<br/>3.16.0-25-generic 到 3.16.0-77-generic、<br/>3.19.0-18-generic 到 3.19.0-80-generic、<br/>4.2.0-18-generic 到 4.2.0-42-generic、<br/>4.4.0-21-generic 到 4.4.0-131-generic |
14.04 LTS | 9.18 | 3.13.0-24-generic 到 3.13.0-151-generic、<br/>3.16.0-25-generic 到 3.16.0-77-generic、<br/>3.19.0-18-generic 到 3.19.0-80-generic、<br/>4.2.0-18-generic 到 4.2.0-42-generic、<br/>4.4.0-21-generic 到 4.4.0-128-generic |
14.04 LTS | 9.17 | 3.13.0-24-generic 到 3.13.0-147-generic、<br/>3.16.0-25-generic 到 3.16.0-77-generic、<br/>3.19.0-18-generic 到 3.19.0-80-generic、<br/>4.2.0-18-generic 到 4.2.0-42-generic、<br/>4.4.0-21-generic 到 4.4.0-124-generic |
14.04 LTS | 9.16 | 3.13.0-24-generic 到 3.13.0-144-generic、<br/>3.16.0-25-generic 到 3.16.0-77-generic、<br/>3.19.0-18-generic 到 3.19.0-80-generic、<br/>4.2.0-18-generic 到 4.2.0-42-generic、<br/>4.4.0-21-generic 到 4.4.0-119-generic |
|||
16.04 LTS | 9.19 | 4.4.0-21-generic 到 4.4.0-131-generic、<br/>4.8.0-34-generic 到 4.8.0-58-generic、<br/>4.10.0-14-generic 到 4.10.0-42-generic、<br/>4.11.0-13-generic 到 4.11.0-14-generic、<br/>4.13.0-16-generic 到 4.13.0-45-generic、<br/>4.15.0-13-generic 到 4.15.0-30-generic<br/>4.11.0-1009-azure 到 4.11.0-1016-azure、<br/>4.13.0-1005-azure 到 4.13.0-1018-azure <br/>4.15.0-1012-azure 到 4.15.0-1019-azure|
16.04 LTS | 9.18 | 4.4.0-21-generic 到 4.4.0-128-generic、<br/>4.8.0-34-generic 到 4.8.0-58-generic、<br/>4.10.0-14-generic 到 4.10.0-42-generic、<br/>4.11.0-13-generic 到 4.11.0-14-generic、<br/>4.13.0-16-generic 到 4.13.0-45-generic、<br/>4.11.0-1009-azure 到 4.11.0-1016-azure、<br/>4.13.0-1005-azure 到 4.13.0-1018-azure |
16.04 LTS | 9.17 | 4.4.0-21-generic 到 4.4.0-124-generic、<br/>4.8.0-34-generic 到 4.8.0-58-generic、<br/>4.10.0-14-generic 到 4.10.0-42-generic、<br/>4.11.0-13-generic 到 4.11.0-14-generic、<br/>4.13.0-16-generic 到 4.13.0-41-generic、<br/>4.11.0-1009-azure 到 4.11.0-1016-azure、<br/>4.13.0-1005-azure 到 4.13.0-1016-azure |
16.04 LTS | 9.16 | 4.4.0-21-generic 到 4.4.0-119-generic、<br/>4.8.0-34-generic 到 4.8.0-58-generic、<br/>4.10.0-14-generic 到 4.10.0-42-generic、<br/>4.11.0-13-generic 到 4.11.0-14-generic、<br/>4.13.0-16-generic 到 4.13.0-38-generic、<br/>4.11.0-1009-azure 到 4.11.0-1016-azure、<br/>4.13.0-1005-azure 到 4.13.0-1012-azure |

#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Azure 虚拟机支持的 Debian 内核版本

**版本** | **移动服务版本** | **内核版本** |
--- | --- | --- |
Debian 7 | 9.17,9.18,9.19 | 3.2.0-4-amd64 到 3.2.0-6-amd64、3.16.0-0.bpo.4-amd64 |
Debian 7 | 9.16 | 3.2.0-4-amd64 到 3.2.0-5-amd64、3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.19 | 3.16.0-4-amd64 到 3.16.0-6-amd64、4.9.0-0.bpo.4-amd64 到 4.9.0-0.bpo.7-amd64 |
Debian 8 | 9.17、9.18 | 3.16.0-4-amd64 到 3.16.0-6-amd64、4.9.0-0.bpo.4-amd64 到 4.9.0-0.bpo.6-amd64 |
Debian 8 | 9.16 | 3.16.0-4-amd64 到 3.16.0-5-amd64、4.9.0-0.bpo.4-amd64 到 4.9.0-0.bpo.5-amd64 |

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Azure 虚拟机支持的 SUSE Linux Enterprise Server 12 内核版本

**版本** | **移动服务版本** | **内核版本** |
--- | --- | --- |
SUSE Linux Enterprise Server 12（SP1、SP2、SP3） | 9.19 | SP1 3.12.49-11-default 到 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default 到 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default 到 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default 到 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default 到 4.4.140-94.42-default |
SUSE Linux Enterprise Server 12（SP1、SP2、SP3） | 9.18 | SP1 3.12.49-11-default 到 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default 到 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default 到 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default 到 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default 到 4.4.138-94.39-default |
SUSE Linux Enterprise Server 12（SP1、SP2、SP3） | 9.17 | SP1 3.12.49-11-default 到 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default 到 3.12.74-60.64.88-default</br></br> SP2 4.4.21-69-default 到 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default</br></br>SP3 4.4.73-5-default 到 4.4.126-94.22-default |

## <a name="replicated-machines---linux-file-systemguest-storage"></a>复制的计算机 - Linux 文件系统/来宾存储

* 文件系统：ext3、ext4、ReiserFS（仅限 Suse Linux Enterprise Server）和 XFS
* 卷管理器：LVM2
* 多路径软件：设备映射器

## <a name="replicated-machines---compute-settings"></a>复制的计算机 - 计算设置

**设置** | **支持** | **详细信息**
--- | --- | ---
大小 | 具有至少 2 个 CPU 内核和 1 GB RAM 的任意 Azure VM 大小 | 验证 [Azure 虚拟机大小](../virtual-machines/windows/sizes.md)。
可用性集 | 支持 | 如果启用使用默认选项复制 Azure VM，则会根据源区域设置自动创建可用性集。 可以修改这些设置。
可用性区域 | 不支持 | 当前无法复制在可用性区域中部署的 VM。
混合使用权益 (HUB) | 支持 | 如果源 VM 启用了 HUB 许可证，则测试故障转移或故障转移 VM 也使用 HUB 许可证。
VM 规模集 | 不支持 |
Azure 库映像 - 由 Azure 发布 | 支持 | 如果 VM 在受支持的操作系统上运行，则支持该配置。
Azure 库映像 - 第三方发布 | 支持 | 如果 VM 在受支持的操作系统上运行，则支持该配置。
自定义映像 - 第三方发布 | 支持 | 如果 VM 在受支持的操作系统上运行，则支持该配置。
使用 Site Recovery 迁移 VM | 支持 | 如果使用 Site Recovery 将 VMware VM 或物理计算机迁移到 Azure，则需要卸载计算机上运行的旧版移动服务，并在重启计算机后将该计算机复制到另一个 Azure 区域。

## <a name="replicated-machines---disk-actions"></a>复制的计算机 - 磁盘操作

**操作** | **详细信息**
-- | ---
调整复制的 VM 上的磁盘大小 | 支持
将磁盘添加到复制的 VM | 不支持。<br/><br/> 需要为 VM 禁用复制，添加磁盘，然后再次启用复制。

## <a name="replicated-machines---storage"></a>复制的计算机 - 存储

此表汇总了对 Azure VM OS 磁盘、数据磁盘和临时磁盘的支持。

- 请务必遵循适用于 [Linux](../virtual-machines/linux/disk-scalability-targets.md) 和 [Windows](../virtual-machines/windows/disk-scalability-targets.md) VM 的 VM 磁盘限制以及目标，以避免任何性能问题。
- 如果使用默认设置进行部署，Site Recovery 会根据源设置自动创建磁盘和存储帐户。
- 如果自定义，请确保遵循指南。

**组件** | **支持** | **详细信息**
--- | --- | ---
OS 磁盘的最大大小 | 2048 GB | [深入了解 ](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)VM 磁盘相关信息。
临时磁盘 | 不支持 | 始终从复制中排除临时磁盘。<br/><br/> 请勿在临时磁盘上存放任何持久性数据。 [了解详细信息](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk)。
数据磁盘的最大大小 | 4095 GB |
数据磁盘的最大数量 | 最多为 64，根据对特定的 Azure VM 大小的支持而定 | [深入了解 ](../virtual-machines/windows/sizes.md)VM 大小相关信息。
数据磁盘更改率 | 每个高级存储的磁盘最大为 10 MBps。 每个标准存储的磁盘最大为 2 MBps。 | 如果磁盘上的平均数据更改率持续高于最大值，复制将跟不上。<br/><br/>  但是，如果偶尔超出最大值，则复制可跟上，但可能会看到稍有延迟的恢复点。
数据磁盘 - 标准存储帐户 | 支持 |
数据磁盘 - 高级存储帐户 | 支持 | 如果 VM 将磁盘分散在高级和标准存储帐户上，则可以为每个磁盘选择不同的目标存储帐户，以确保在目标区域中具有相同的存储配置。
托管磁盘 - 标准 | 在支持 Azure Site Recovery 的 Azure 区域中受支持。 |  
托管磁盘 - 高级 | 在支持 Azure Site Recovery 的 Azure 区域中受支持。 |
冗余 | LRS 和 GRS 受支持。<br/><br/> ZRS 不受支持。
冷存储和热存储 | 不支持 | 冷存储和热存储不支持 VM 磁盘
存储空间 | 支持 |         
静态加密 (SSE) | 支持 | SSE 是存储帐户的默认设置。   
适用于 Windows OS 的 Azure 磁盘加密 (ADE) | 支持为[使用 Azure AD 应用的加密](/security/azure-security-disk-encryption-windows-aad)启用的 VM |
适用于 Linux OS 的 Azure 磁盘加密 (ADE) | 不支持 |
热添加/移除磁盘 | 不支持 | 如果在 VM 上添加或删除数据磁盘，需要先禁用复制然后重新为 VM 启用复制。
排除磁盘 | 不支持|   默认情况下，排除临时磁盘。
存储空间直通  | 不支持|
横向扩展文件服务器  | 不支持|
LRS | 支持 |
GRS | 支持 |
RA-GRS | 支持 |
ZRS | 不支持 |  
冷存储和热存储 | 不支持 | 冷存储和热存储不支持虚拟机磁盘
虚拟网络的 Azure 存储防火墙  | 是 | 若要限制对存储帐户的虚拟网络访问，请确保允许受信任的 Azure 服务访问存储帐户。
常规用途 V2 存储帐户（包括热存储层和冷存储层） | 否 | 与常规用途 V1 存储帐户相比，事务成本大幅增加

>[!IMPORTANT]
> 确保观察 [Linux](../virtual-machines/linux/disk-scalability-targets.md) 或 [Windows](../virtual-machines/windows/disk-scalability-targets.md) 虚拟机的 VM 磁盘可伸缩性和性能目标，以避免任何性能问题。 如果遵从默认设置，Site Recovery 将基于源配置创建所需的磁盘和存储帐户。 如果自定义和选择自己的设置，请确保遵循源 VM 的磁盘可伸缩性和性能目标。

## <a name="replicated-machines---networking"></a>复制的计算机 - 网络
**配置** | **支持** | **详细信息**
--- | --- | ---
NIC | 特定 Azure VM 大小支持的最大数量 | 在故障转移期间创建 VM 时会创建 NIC。<br/><br/> 故障转移 VM 上的 NIC 数目取决于启用复制时源 VM 上的 NIC 数目。 如果在启用复制后添加或删除 NIC，不会影响故障转移后复制的 VM 上的 NIC 数目。
Internet 负载均衡器 | 支持 | 在恢复计划中使用 Azure 自动化脚本关联预配置的负载均衡器。
内部负载均衡器 | 支持 | 在恢复计划中使用 Azure 自动化脚本关联预配置的负载均衡器。
公共 IP 地址 | 支持 | 将现有的公共 IP 地址与 NIC 关联。 或者，在恢复计划中使用 Azure 自动化脚本创建公共 IP 地址并将其与 NIC 关联。
NIC 上的 NSG | 支持 | 在恢复计划中使用 Azure 自动化脚本将 NSG 与 NIC 关联。  
子网上的 NSG | 支持 | 在恢复计划中使用 Azure 自动化脚本将 NSG 与子网关联。
保留（静态）IP 地址 | 支持 | 如果源 VM 上的 NIC 具有静态 IP 地址，并且目标子网具有相同的可用 IP 地址，则会将它分配给故障转移 VM。<br/><br/> 如果目标子网没有相同的可用 IP 地址，则为 VM 保留子网中的某个可用 IP 地址。<br/><br/> 此外可以在“复制的项” > “设置” > “计算和网络” > “网络接口”中指定固定的 IP 地址和子网。
动态 IP 地址 | 支持 | 如果源上的 NIC 具有动态 IP 地址，故障转移 VM 上的 NIC 也默认为动态。<br/><br/> 如果有需要，可以将其修改为固定的 IP 地址。
流量管理器     | 支持 | 可以预配置流量管理器，这样在正常情况下，流量路由到源区域中的终结点；发生故障转移时，流量路由到目标区域中的终结点。
Azure DNS | 支持 |
自定义 DNS  | 支持 |    
未经身份验证的代理 | 支持 | 请参阅[网络指南文档。](site-recovery-azure-to-azure-networking-guidance.md)    
经过身份验证的代理 | 不支持 | 如果 VM 正在使用经过身份验证的代理进行出站连接，则不可使用 Azure Site Recovery 进行复制。    
本地站点到站点 VPN（使用或不使用 ExpressRoute）| 支持 | 确保将 UDR 和 NSG 配置为站点恢复流量不会路由到本地。 请参阅[网络指南文档。](site-recovery-azure-to-azure-networking-guidance.md)  
VNET 到 VNET 连接 | 支持 | 请参阅[网络指南文档。](site-recovery-azure-to-azure-networking-guidance.md)  
虚拟网络服务终结点 | 支持 | 若要限制对存储帐户的虚拟网络访问，请确保允许受信任的 Azure 服务访问存储帐户。 
加速网络 | 支持 | 必须在源 VM 上启用加速网络。 [了解详细信息](azure-vm-disaster-recovery-with-accelerated-networking.md)。

## <a name="next-steps"></a>后续步骤
- 请参阅 [Azure VM 复制网络指南](site-recovery-azure-to-azure-networking-guidance.md)。
- 通过[复制 Azure VM](site-recovery-azure-to-azure.md) 来部署灾难恢复。

<!--Update_Description: update meta properties, wording update, update link -->