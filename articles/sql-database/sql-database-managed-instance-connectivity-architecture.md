---
title: Azure SQL 数据库托管实例连接体系结构 | Microsoft Docs
description: 本文概述 Azure SQL 数据库托管实例通信，介绍连接体系结构，以及不同的组件如何将流量定向到托管实例。
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
origin.date: 02/18/2019
ms.date: 03/11/2019
ms.openlocfilehash: 6f6399c0c872353fd0ca2d6a766f6ae61e0068fd
ms.sourcegitcommit: 0ccbf718e90bc4e374df83b1460585d3b17239ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2019
ms.locfileid: "57347200"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Azure SQL 数据库托管实例连接体系结构

本文概述 Azure SQL 数据库托管实例通信，介绍连接体系结构，以及不同的组件如何将流量定向到托管实例。  

Azure SQL 数据库托管实例置于专用于托管实例的 Azure VNet 和子网中。 此部署支持以下方案：

- 保护专用 IP 地址。
- 直接从本地网络连接到托管实例。
- 将托管实例连接到链接服务器或其他本地数据存储。
- 将托管实例连接到 Azure 资源。

## <a name="communication-overview"></a>通信概述

下图演示了连接到托管实例的实体，以及托管实例为保持正常运行而必须访问的资源。

![连接体系结构实体](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

插图底部显示的通信表示作为数据源连接到托管实例的客户应用程序和工具。  

由于托管实例是一种服务即平台 (PaaS) 产品/服务，因此 Azure 基于遥测数据流使用自动化代理来管理该服务（管理、部署和维护）。 由于托管实例管理完全由 Azure 负责，客户将无法通过 RDP 访问托管实例虚拟群集计算机。 

某些由最终用户或应用程序启动的 SQL Server 操作可能需要托管实例来与平台进行交互。 一种情况是创建托管实例数据库，这是一种通过 Azure 门户、PowerShell、Azure CLI 和 REST API 公开的资源。

托管实例依赖其他 Azure 服务才能正常运行（例如使用 Azure 存储进行备份、使用 Azure 服务总线进行遥测、使用 Azure AD 进行身份验证、使用 Azure Key Vault 进行 TDE），并相应地启动到这些服务的连接。

上述的所有通信都使用证书进行加密和签名。 若要确保通信方受信任，托管实例需不断地通过联系证书颁发机构来验证这些证书。 如果证书被撤销或托管实例无法验证它们，则会关闭连接以保护数据。

## <a name="high-level-connectivity-architecture"></a>高级连接体系结构

从较高层面讲，托管实例是一组服务组件，托管在一组专用的、在客户虚拟网络子网中运行并形成虚拟群集的独立虚拟机上。

多个托管示例可托管在单个虚拟群集中。 当客户更改子网中已预配的实例数时，群集可根据需要自动增加或减少。

客户应用程序只有使用具有专用 IP 地址的终结点在虚拟网络、对等虚拟网络或连接了 VPN/快速路由的网络中运行时，才能连接到托管实例、执行查询和更新数据库。  

![连接体系结构关系图](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Azure 管理和部署服务在虚拟网络外部运行，因此托管实例和 Azure 服务之间的连接将要通过使用公共 IP 地址的终结点。 当托管实例创建出站连接时，由于网络地址转换 (NAT)，该实例在接收端看起来似乎来自这个公共 IP。 

管理流量流经客户虚拟网络。 这意味着虚拟网络基础结构的元素会影响并可能损害管理流量，从而导致实例进入错误状态并变得不可用。

> [!IMPORTANT]
> 为改善客户体验和服务可用性，Azure 会向可能会影响托管实例正常运行的 Azure 虚拟网络基础结构元素应用网络意向策略。 这是一种向最终用户透明地传递网络需求的平台机制，主要目的是防止网络配置错误并确保托管实例正常操作。 托管实例删除时一并删除网络意向策略。

## <a name="virtual-cluster-connectivity-architecture"></a>虚拟群集连接体系结构

让我们更深入地了解托管实例连接体系结构。 以下关系图演示了虚拟群集的概念布局。

![连接体系结构关系图虚拟群集](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

客户端通过采用 `<mi_name>.<dns_zone>.database.chinacloudapi.cn` 形式的主机名连接到托管实例。 尽管此主机名在公用 DNS 区域中注册且可公开解析，但它仍解析为专用 IP 地址。 `zone-id` 于群集创建时自动生成。 如果新创建的群集托管辅助托管实例，它会将其区域 ID 与主群集共享。

此专用 IP 地址属于将流量定向到托管实例网关 (GW) 的托管实例内部负载均衡器 (ILB)。 由于多个托管实例可能在同一群集中运行，因此 GW 使用托管实例主机名来将流量重新定向到正确的 SQL 引擎服务。

管理和部署服务使用映射到外部负载均衡器的[管理终结点](#management-endpoint)连接到托管实例。 只有在一组预定义的专用于托管实例管理组件的端口上进行接收时，才将流量路由到节点。 将会在节点上配置内置的防火墙，只允许来自 Microsoft 的特定 IP 范围的流量。 管理组件与管理平面之间的所有通信都是相互认证的。

## <a name="management-endpoint"></a>管理终结点

托管实例虚拟群集包含一个管理终结点，可供 Azure 用于管理托管实例。 管理终结点在网络级别受内置防火墙保护，在应用程序级别受相互证书验证保护。 可以[查找管理终结点 IP 地址](sql-database-managed-instance-find-management-endpoint-ip-address.md)。

从托管实例（备份、审核日志）内部发起连接时，看起来流量来自管理终结点公共 IP 地址。 可以限制从托管实例到公共服务的访问，方法是设置防火墙规则，只允许托管实例 IP 地址。 更详细地了解可以[验证托管实例内置防火墙](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md)的方法。

> [!NOTE]
> 这不适合为托管实例所在区域的 Azure 服务设置防火墙规则，因为 Azure 平台会针对已并置服务之间的流量进行优化。

## <a name="network-requirements"></a>网络要求

可以在符合以下要求的虚拟网络的专用子网（托管实例子网）中部署托管实例：

- **专用子网**：托管实例子网不能包含与它关联的任何其他云服务，并且不能是网关子网。 无法在包含托管实例以外资源的子网中创建托管实例，并且以后无法在子网中添加其他资源。
- **网络安全组 (NSG)**：与虚拟网络关联的 NSG 必须将定义的这些强制性[入站安全规则](#mandatory-inbound-security-rules)和[出站安全规则](#mandatory-outbound-security-rules)包含在其他任何规则的前面。 可以使用某个 NSG 通过筛选端口 1433 上的流量来完全控制对托管实例数据终结点的访问。
- **用户定义的路由表 (UDR)**：与虚拟网络关联的用户定义路由表必须包含用户定义的路由表中的这些[条目](#user-defined-routes)。
- **没有服务终结点**：托管实例子网不能有关联的服务终结点。 创建虚拟网络时，请务必禁用“服务终结点”选项。
- **足够的 IP 地址**：托管实例子网必须最少具有 16 个 IP 地址（建议的最小值是 32 个 IP 地址）。 有关详细信息，请参阅[确定托管实例的子网大小](sql-database-managed-instance-determine-size-vnet-subnet.md)。 可以在[现有网络](sql-database-managed-instance-configure-vnet-subnet.md)中部署托管实例，前提是已将其配置为符合[托管实例网络要求](#network-requirements)；也可以创建[新的网络和子网](sql-database-managed-instance-create-vnet-subnet.md)。

> [!IMPORTANT]
> 如果目标子网不符合上述所有要求，则无法部署新的托管实例。 创建托管实例时，会在子网上应用一个网络意向策略，以防止对网络配置进行不符合标准的更改。 从子网中删除最后一个实例后，“网络意向策略”也将被删除

### <a name="mandatory-inbound-security-rules"></a>强制性入站安全规则

| Name       |端口                        |协议|源           |目标|操作|
|------------|----------------------------|--------|-----------------|-----------|------|
|管理  |9000、9003、1438、1440、1452|TCP     |任意              |任意        |允许 |
|mi_subnet   |任意                         |任意     |MI SUBNET        |任意        |允许 |
|health_probe|任意                         |任意     |AzureLoadBalancer|任意        |允许 |

### <a name="mandatory-outbound-security-rules"></a>强制性出站安全规则

| Name       |端口          |协议|源           |目标|操作|
|------------|--------------|--------|-----------------|-----------|------|
|管理  |80、443、12000|TCP     |任意              |Internet   |允许 |
|mi_subnet   |任意           |任意     |任意              |MI SUBNET*  |允许 |

\* MI SUBNET 是指子网的 IP 地址范围，采用 10.x.x.x/y 格式。 此信息可在 Azure 门户中找到（通过子网属性）。

> [!IMPORTANT]
> 虽然强制入站安全规则允许来自端口 9000、9003、1438、1440、1452 上的任意资源的流量，但这些端口受内置防火墙的保护。 此[文章](sql-database-managed-instance-find-management-endpoint-ip-address.md)介绍如何发现管理终结点 IP 地址并验证防火墙规则。
> [!NOTE]
> 如果在托管实例中使用事务性复制并将任何实例数据库用作发布服务器或分发服务器，则还需要在子网的安全规则中打开端口 445（TCP 出站）才能访问 Azure 文件共享。

### <a name="user-defined-routes"></a>用户定义路由

|Name|地址前缀|网络跃点|
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

此外，还可以将条目添加到路由表，以便通过虚拟网络网关或虚拟网络设备 (NVA) 路由发往本地专用 IP 范围的流量。

- **可选的自定义 DNS**：如果在虚拟网络中指定了自定义 DNS，则必须将 Azure 的递归解析程序 IP 地址（例如 168.63.129.16）添加到列表。 有关详细信息，请参阅[配置自定义 DNS](sql-database-managed-instance-custom-dns.md)。 自定义 DNS 服务器必须能够解析下列域及其子域中的主机名：*microsoft.com*、*windows.net*、*windows.com*、*msocsp.com*、*digicert.com*、*live.com*、*microsoftonline.com* 和 *microsoftonline-p.com*。

## <a name="next-steps"></a>后续步骤

- 有关概述，请参阅 [什么是托管实例](sql-database-managed-instance.md)
- 了解如何[配置新的 VNet](sql-database-managed-instance-create-vnet-subnet.md) 或[配置现有的 VNet](sql-database-managed-instance-configure-vnet-subnet.md)，以便在其中部署托管实例。
- [计算要在其中部署托管实例的子网的大小](sql-database-managed-instance-determine-size-vnet-subnet.md)。
- 参阅有关如何通过以下方式创建托管实例的快速入门：
  - 通过 [Azure 门户](sql-database-managed-instance-get-started.md)
  - 使用 [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
  - 使用 [Azure 资源管理器模板](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)
  - 使用 [Azure 资源管理器模板（包含 SSMS 的 jumpbox）](https://portal.azure.cn/) 
