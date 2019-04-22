---
title: Azure SQL 数据库中托管实例的连接体系结构 | Microsoft Docs
description: 了解 Azure SQL 数据库托管实例的通信和连接体系结构，以及如何组件如何将流量定向到托管实例。
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: bonova, carlrab
manager: digimobile
origin.date: 02/26/2019
ms.date: 04/15/2019
ms.openlocfilehash: f98f60db729ca0079fe960399e039fe7c9b2fe15
ms.sourcegitcommit: 9f7a4bec190376815fa21167d90820b423da87e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "59529185"
---
# <a name="connectivity-architecture-for-a-managed-instance-in-azure-sql-database"></a>Azure SQL 数据库中托管实例的连接体系结构 

本文介绍 Azure SQL 数据库托管实例中的通信。 此外介绍连接体系结构，以及组件如何将流量定向到托管实例。  

SQL 数据库托管实例放置在专用于托管实例的 Azure 虚拟网络和子网中。 此部署提供：

- 安全的专用 IP 地址。
- 将本地网络连接到托管实例的功能。
- 将托管实例连接到链接服务器或其他本地数据存储的功能。
- 将托管实例连接到 Azure 资源的功能。

## <a name="communication-overview"></a>通信概述

下图显示了连接到托管实例的实体。 此外，显示了需要与托管实例通信的资源。 插图底部的通信流程表示作为数据源连接到托管实例的客户应用程序和工具。  

![连接体系结构中的实体](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

托管实例属于平台即服务 (PaaS)。 Microsoft 使用自动化代理（管理、部署和维护）基于遥测数据流管理此服务。 由于管理工作由 Microsoft 负责，客户无法通过远程桌面协议 (RDP) 访问托管实例的虚拟群集计算机。

由最终用户或应用程序启动的某些 SQL Server 操作可能需要使用托管实例来与平台交互。 一种情况是创建托管实例数据库。 此资源是通过 Azure 门户、PowerShell、Azure CLI 和 REST API 公开的。

托管实例依赖于使用 Azure 存储等 Azure 服务进行备份、使用 Azure 服务总线传送遥测数据、使用 Azure Active Directory 进行身份验证，以及使用 Azure Key Vault 进行透明数据加密 (TDE)。 托管实例与这些服务建立连接。

所有通信使用证书进行加密和签名。 为了检查通信方的可信度，托管实例会不断地通过联系证书颁发机构来验证这些证书。 如果证书被撤销或不可验证，则托管实例会关闭连接以保护数据。

## <a name="high-level-connectivity-architecture"></a>高级连接体系结构

从较高层面讲，托管实例是一组服务组件。 这些组件托管在客户虚拟网络子网中运行的一组专用隔离虚拟机上。 这些计算机构成了虚拟群集。

一个虚拟群集可以承载多个托管实例。 当客户更改子网中预配的实例数时，群集可根据需要自动扩展或收缩。

仅当客户应用程序在虚拟网络、对等互连的虚拟网络或者通过 VPN 或 Azure ExpressRoute 连接的网络中运行时，才能连接到托管实例以及查询和更新数据库。 此网络必须使用一个终结点和一个专用 IP 地址。  

![连接体系结构示意图](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Azure 管理和部署服务在虚拟网络外部运行。 托管实例和 Azure 服务通过采用公共 IP 地址的终结点进行连接。 当托管实例建立出站连接时，在接收端，网络地址转换 (NAT) 会使该连接看上去来自此公共 IP 地址。

管理流量通过客户的虚拟网络传送。 这意味着，虚拟网络基础结构的要素可能会使实例发生故障并变得不可用，从而对管理流量造成不利影响。

> [!IMPORTANT]
> 为改善客户体验和服务可用性，Azure 会针对 Azure 虚拟网络基础结构要素应用网络意向策略。 该策略可影响托管实例的工作方式。 此平台机制以透明方式向用户传达网络要求。 该策略的主要目的是防止网络配置不当，并确保托管实例正常运行。 删除某个托管实例时，会一并删除网络意向策略。

## <a name="virtual-cluster-connectivity-architecture"></a>虚拟群集连接体系结构

让我们更深入地了解托管实例的连接体系结构。 以下关系图演示了虚拟群集的概念布局。

![虚拟群集的连接体系结构](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

客户端使用 `<mi_name>.<dns_zone>.database.chinacloudapi.cn` 格式的主机名连接到托管实例。 此主机名将解析为专用 IP 地址，不过，它将在公共域名系统 (DNS) 区域中注册，且可公开解析。 `zone-id` 是创建群集时自动生成的。 如果新建的群集托管辅助托管实例，它会将其区域 ID 与主群集共享。

此专用 IP 地址属于将流量定向到托管实例网关 (GW) 的托管实例内部负载均衡器 (ILB)。 由于多个托管实例可能在同一群集中运行，因此 GW 使用托管实例主机名来将流量重新定向到正确的 SQL 引擎服务。

管理和部署服务使用映射到外部负载均衡器的[管理终结点](#management-endpoint)连接到托管实例。 仅当流量是在一组专用于托管实例管理组件的预定义端口上收到的时，才将流量路由到节点。 节点上的内置防火墙设置为只允许来自 Microsoft IP 范围的流量。 证书将对管理组件与管理平面之间的所有通信进行相互身份验证。

## <a name="management-endpoint"></a>管理终结点

Azure 使用一个管理终结点来管理托管实例。 此终结点位于该实例的虚拟群集内部。 管理终结点在网络级别受到内置防火墙的保护。 在应用程序级别，管理终结点受到证书相互验证的保护。 若要查找终结点的 IP 地址，请参阅[确定管理终结点的 IP 地址](sql-database-managed-instance-find-management-endpoint-ip-address.md)。

在托管实例中启动连接时（提供备份和审核日志），流量似乎是从管理终结点的公共 IP 地址启动的。 可以通过将防火墙规则设置为只允许托管实例的 IP 地址，来限制从托管实例访问公共服务。 有关详细信息，请参阅[验证托管实例的内置防火墙](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md)。

> [!NOTE]
> 与在托管实例中启动的连接的防火墙不同，位于托管实例所在区域中的 Azure 服务具有一个针对在这些服务之间传送的流量进行优化的防火墙。

## <a name="network-requirements"></a>网络要求

在虚拟网络中的专用子网内部署托管实例。 该子网必须具有以下特征：

- **专用子网：** 托管实例的子网不能包含其他任何关联的云服务，且不能是网关子网。 该子网不能包含除该托管实例以外的其他任何资源，以后无法在该子网中添加资源。
- **网络安全组 (NSG)**：与虚拟网络关联的 NSG 必须在其他任何规则的前面定义[入站安全规则](#mandatory-inbound-security-rules)和[出站安全规则](#mandatory-outbound-security-rules)。 可以使用某个 NSG 通过筛选端口 1433 上的流量，来控制对托管实例数据终结点的访问。
- **用户定义的路由 (UDR) 表：** 与虚拟网络关联的 UDR 表必须包含特定的[条目](#user-defined-routes)。
- **没有服务终结点：** 不应将任何服务终结点与托管实例的子网相关联。 创建虚拟网络时，请务必禁用“服务终结点”选项。
- **足够的 IP 地址：** 托管实例子网必须至少有 16 个 IP 地址。 建议的最少数目为 32 个 IP 地址。 有关详细信息，请参阅[确定托管实例的子网大小](sql-database-managed-instance-determine-size-vnet-subnet.md)。 根据[托管实例的网络要求](#network-requirements)配置托管实例后，可将其部署在[现有网络](sql-database-managed-instance-configure-vnet-subnet.md)中。 否则，请创建[新的网络和子网](sql-database-managed-instance-create-vnet-subnet.md)。

> [!IMPORTANT]
> 如果目标子网缺少这些特征，则无法部署新的托管实例。 创建托管实例时，将会针对子网应用网络意向策略，以防止对网络设置进行不合规的更改。 从子网中删除最后一个实例后，网络意向策略也会一并删除。

### <a name="mandatory-inbound-security-rules"></a>强制性入站安全规则

| Name       |端口                        |协议|源           |目标|操作|
|------------|----------------------------|--------|-----------------|-----------|------|
|管理  |9000、9003、1438、1440、1452|TCP     |任意              |任意        |允许 |
|mi_subnet   |任意                         |任意     |MI SUBNET        |任意        |允许 |
|health_probe|任意                         |任意     |AzureLoadBalancer|任意        |允许 |

### <a name="mandatory-outbound-security-rules"></a>强制性出站安全规则

| Name       |端口          |协议|源           |目标|操作|
|------------|--------------|--------|-----------------|-----------|------|
|管理  |80、443、12000|TCP     |任意              |AzureChinaCloud  |允许 |
|mi_subnet   |任意           |任意     |任意              |MI SUBNET*  |允许 |

> [!IMPORTANT]
> 确保端口 9000、9003、1438、1440、1452 只有一个入站规则，端口 80、443、12000 只有一个出站规则。 如果单独为每个端口配置入站和出站规则，则无法通过 ARM 部署预配托管实例。 如果这些端口在单独的规则中，则部署将会失败并出现错误代码 `VnetSubnetConflictWithIntendedPolicy`

\* MI SUBNET 是指子网的 IP 地址范围，采用 10.x.x.x/y 格式。 可以在 Azure 门户上的子网属性中找到此信息。

> [!IMPORTANT]
> 尽管所需的入站安全规则允许来自端口 9000、9003、1438、1440 和 1452 上的任意资源的流量，但这些端口受内置防火墙的保护。 有关详细信息，请参阅[确定管理终结点地址](sql-database-managed-instance-find-management-endpoint-ip-address.md)。

> [!NOTE]
> 如果在托管实例中使用事务复制，并使用任何实例数据库作为发布方或分发方，请在子网的安全规则中打开端口 445（TCP 出站）。 此端口允许访问 Azure 文件共享。

### <a name="user-defined-routes"></a>用户定义的路由

|Name|地址前缀|下一跃点|
|----|--------------|-------|
|subnet_to_vnetlocal|[mi_subnet]|虚拟网络|
|mi-0-5-next-hop-internet|0.0.0.0/5|Internet|
|mi-11-8-nexthop-internet|11.0.0.0/8|Internet|
|mi-12-6-nexthop-internet|12.0.0.0/6|Internet|
|mi-128-3-nexthop-internet|128.0.0.0/3|Internet|
|mi-16-4-nexthop-internet|16.0.0.0/4|Internet|
|mi-160-5-nexthop-internet|160.0.0.0/5|Internet|
|mi-168-6-nexthop-internet|168.0.0.0/6|Internet|
|mi-172-12-nexthop-internet|172.0.0.0/12|Internet|
|mi-172-128-9-nexthop-internet|172.128.0.0/9|Internet|
|mi-172-32-11-nexthop-internet|172.32.0.0/11|Internet|
|mi-172-64-10-nexthop-internet|172.64.0.0/10|Internet|
|mi-173-8-nexthop-internet|173.0.0.0/8|Internet|
|mi-174-7-nexthop-internet|174.0.0.0/7|Internet|
|mi-176-4-nexthop-internet|176.0.0.0/4|Internet|
|mi-192-128-11-nexthop-internet|192.128.0.0/11|Internet|
|mi-192-160-13-nexthop-internet|192.160.0.0/13|Internet|
|mi-192-169-16-nexthop-internet|192.169.0.0/16|Internet|
|mi-192-170-15-nexthop-internet|192.170.0.0/15|Internet|
|mi-192-172-14-nexthop-internet|192.172.0.0/14|Internet|
|mi-192-176-12-nexthop-internet|192.176.0.0/12|Internet|
|mi-192-192-10-nexthop-internet|192.192.0.0/10|Internet|
|mi-192-9-nexthop-internet|192.0.0.0/9|Internet|
|mi-193-8-nexthop-internet|193.0.0.0/8|Internet|
|mi-194-7-nexthop-internet|194.0.0.0/7|Internet|
|mi-196-6-nexthop-internet|196.0.0.0/6|Internet|
|mi-200-5-nexthop-internet|200.0.0.0/5|Internet|
|mi-208-4-nexthop-internet|208.0.0.0/4|Internet|
|mi-224-3-nexthop-internet|224.0.0.0/3|Internet|
|mi-32-3-nexthop-internet|32.0.0.0/3|Internet|
|mi-64-2-nexthop-internet|64.0.0.0/2|Internet|
|mi-8-7-nexthop-internet|8.0.0.0/7|Internet|
||||

此外，还可以将条目添加到路由表，以通过虚拟网络网关或虚拟网络设备 (NVA) 路由发往本地专用 IP 范围的流量。

如果虚拟网络包含自定义 DNS，请添加 Azure 递归解析程序 IP 地址（例如 168.63.129.16）的条目。 有关详细信息，请参阅[设置自定义 DNS](sql-database-managed-instance-custom-dns.md)。 自定义 DNS 服务器必须能够解析以下域及其子域中的主机名：*microsoft.com*、*windows.net*、*windows.com*、*msocsp.com*、*digicert.com*、*live.com*、*microsoftonline.com* 和 *microsoftonline-p.com*。

## <a name="next-steps"></a>后续步骤

- 有关概述，请参阅  [SQL 数据库高级数据安全性](sql-database-managed-instance.md)。
- 了解如何设置可用于部署托管实例的[新 Azure 虚拟网络](sql-database-managed-instance-create-vnet-subnet.md)或[现有 Azure 虚拟网络](sql-database-managed-instance-configure-vnet-subnet.md)。
- [计算用于部署托管实例的子网的大小](sql-database-managed-instance-determine-size-vnet-subnet.md)。
- 了解如何通过以下方式创建托管实例：
  - 通过 [Azure 门户](sql-database-managed-instance-get-started.md)。
  - 通过使用 PowerShell 设置。
  - 使用 [Azure 资源管理器模板](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)。
  - 使用 [Azure 资源管理器模板（使用包含 SSMS 的 JumpBox）](https://portal.azure.cn/)。
