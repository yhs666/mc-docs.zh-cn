---
title: "Azure 订阅限制和配额 | Microsoft Docs"
description: "提供常见的 Azure 订阅和服务限制、配额和约束的列表。 这包括有关如何增加限制以及最大值的信息。"
services: 
documentationcenter: 
author: alexchen2016
manager: digimobile
editor: 
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/01/2017
ms.date: 09/25/2017
ms.author: v-junlch
ms.openlocfilehash: 551f0295962c344eb578e992e0085fe8cae7a26e
ms.sourcegitcommit: c13aee6f5e18d15bcc29fae1eefd2b72f2558dfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2017
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure 订阅和服务限制、配额和约束
本文列出了一些最常见的 Azure 限制，有时也称为配额。 本文当前并不涵盖所有 Azure 服务。 随着时间的推移，此列表将进行扩展和更新，以涵盖更多平台。

若要了解有关 Azure 定价的详细信息，请访问 [Azure 定价概述](https://www.azure.cn/pricing/)。 在那里可以使用[定价计算器](https://www.azure.cn/pricing/calculator/)或访问某服务（例如，[Windows VM](https://www.azure.cn/pricing/details/virtual-machines#Windows)）的定价详细信息页估计成本。

> [!NOTE]
> 如果想要提高限制或配额，使其超出**默认限制**，可以免费建立联机客户支持请求。 无法将限制提高到超过下表中显示的**最大限制**值。 如果没有 **最大限制** 列，则资源没有可调整的限制。 
> 
> 1 元试用订阅不符合增加限制或配额的条件。 如果有 1 元试用订阅，可将其升级到即用即付订阅。 
> 

## <a name="limits-and-the-azure-resource-manager"></a>限制和 Azure Resource Manager
现在可以将多个 Azure 资源合并到单个 Azure 资源组中。 在使用资源组时，以前针对全局的限制会通过 Azure Resource Manager 在区域级别进行管理。 有关 Azure 资源组的详细信息，请参阅 [Azure Resource Manager 概述](azure-resource-manager/resource-group-overview.md)。

在下面的限制中，添加了一个新表以反映在使用 Azure Resource Manager 时限制中的任何差异。 例如，会存在一个**订阅限制**表和一个**订阅数限制 - Azure Resource Manager** 表。 如果某个限制同时适用于这两种方案，它会仅显示在第一个表中。 除非另有说明，否则限制是跨所有区域的全局限制。

> [!NOTE]
> 请务必强调 Azure 资源组中的资源配额是用户的订阅可以访问的每个区域，而不像服务管理配额那样是可以访问的每个订阅。 我们来使用核心配额作为示例。 如果需要根据对核心的支持请求增加配额，则需要决定想要在哪个区域中使用多少核心，并针对自己希望的 Azure 资源组核心配额的数量和区域进行特定请求。 因此，如果需要在西欧使用 30 个核心以在那里运行应用程序，则应专门在西欧请求 30 个核心。 但这不会增加在任何其他区域的核心配额 -- 仅西欧会有 30 个核心配额。
> <!-- -->
> 因此，可能会发现考虑决定你在任何一个区域中的工作负荷所需的 Azure 资源组配额数量，以及请求你考虑在其中进行部署的每个区域的数量很有用。 有关发现特定区域的当前配额的更多帮助，请参阅[排查部署问题](./azure-resource-manager/resource-manager-common-deployment-errors.md)。
> 
> 

## <a name="service-specific-limits"></a>服务特定的限制
- [Active Directory](#active-directory-limits)
- [应用服务](#app-service-limits)
- [应用程序网关](#application-gateway-limits)
- [自动化](#automation-limits) 
- [Azure Cosmos DB](#azure-cosmos-db-limits)
- [Azure Redis 缓存](#azure-redis-cache-limits)
- [备份](#backup-limits)
- [批处理](#batch-limits)
- [CDN](#cdn-limits)
- [云服务](#cloud-services-limits)
- [DNS](#dns-limits)
- [事件中心](#event-hubs-limits)
- [IoT 中心](#iot-hub-limits)
- [密钥保管库](#key-vault-limits)
- [媒体服务](#media-services-limits)
- [移动服务](#mobile-services-limits)
- [监视限制](#monitor-limits)
- [多重身份验证](#multi-factor-authentication)
- [联网](#networking-limits)
- [通知中心服务](#notification-hub-service-limits)
- [资源组](#resource-group-limits)
- [计划程序](#scheduler-limits)
- [服务总线](#service-bus-limits)
- [站点恢复](#site-recovery-limits)
- [SQL 数据库](#sql-database-limits)
- [存储](#storage-limits)
- [流分析](#stream-analytics-limits)
- [订阅](#subscription-limits)
- [流量管理器](#traffic-manager-limits)
- [虚拟机](#virtual-machines-limits)
- [虚拟机规模集](#virtual-machine-scale-sets-limits)

### 订阅限制 <a name="subscription-limits"></a>
#### <a name="subscription-limits"></a>订阅限制
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>订阅数限制 - Azure Resource Manager
使用 Azure Resource Manager 和 Azure 资源组时，以下限制适用。 未使用 Azure Resource Manager 更改的限制不会在下面列出。 请参阅上表了解这些限制。

有关处理 Resource Manager 请求限制的信息，请参阅[限制 Resource Manager 请求](./azure-resource-manager/resource-manager-request-limits.md)。

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### 资源组限制 <a name="resource-group-limits"></a>
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### 虚拟机限制 <a name="virtual-machines-limits"></a>
#### <a name="virtual-machine-limits"></a>虚拟机限制
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>虚拟机限制 - Azure Resource Manager
使用 Azure Resource Manager 和 Azure 资源组时，以下限制适用。 未使用 Azure Resource Manager 更改的限制不会在下面列出。 请参阅上表了解这些限制。

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>虚拟机规模集限制
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>网络限制
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>网络限制
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>应用程序网关限制
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>流量管理器限制
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS 限制
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

### 存储限制 <a name="storage-limits"></a>
有关存储帐户限制的详细信息，请参阅 [Azure 存储可伸缩性和性能目标](storage/common/storage-scalability-targets.md)。
<!--like # storage accts --> 
#### <a name="storage-service-limits"></a>存储服务限制
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>虚拟机磁盘限制 
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

有关其他详细信息，请参阅[虚拟机大小](virtual-machines/linux/sizes.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)。

#### <a name="managed-virtual-machine-disks"></a>托管虚拟机磁盘

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>非托管虚拟机磁盘

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>存储资源提供程序限制
[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="cloud-services-limits"></a>云服务限制
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>应用服务限制
以下应用服务限制包括 Web 应用、移动应用、API 应用和逻辑应用的限制。

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>计划程序限制
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>批处理限制
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Azure Cosmos DB 限制
Azure Cosmos DB 是全局缩放数据库，可对吞吐量和存储进行缩放，以处理应用程序的任何需求。 如果对 Azure Cosmos DB 提供的规模有任何问题，请发送电子邮件到 askcosmosdb@microsoft.com。

### <a name="media-services-limits"></a>媒体服务限制
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN 限制
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>移动服务限制
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>监视限制
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>通知中心服务限制
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>事件中心限制
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>服务总线限制
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT 中心限制
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="stream-analytics-limits"></a>流分析限制
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### Active Directory 限制 <a name="active-directory-limits"></a>
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="backup-limits"></a>备份限制
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>站点恢复限制
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis 缓存限制
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>密钥保管库限制
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>多重身份验证
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>自动化限制
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### SQL 数据库限制 <a name="sql-database-limits"></a>
有关 Azure SQL 数据库限制，请参阅 [SQL 数据库资源限制](sql-database/sql-database-resource-limits.md)。

## <a name="see-also"></a>另请参阅
[了解 Azure 限制和增加](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Azure 的虚拟机和云服务大小](virtual-machines/linux/sizes.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)

[云服务的大小](cloud-services/cloud-services-sizes-specs.md)

<!-- Update_Description: wording update -->
