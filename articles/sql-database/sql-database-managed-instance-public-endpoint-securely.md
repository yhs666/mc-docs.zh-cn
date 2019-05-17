---
title: 保护托管实例公共终结点 - Azure SQL 数据库托管实例 | Microsoft Docs
description: 在 Azure 中安全使用包含托管实例的公共终结点
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto, carlrab
manager: digimobile
origin.date: 04/16/2019
ms.date: 04/29/2019
ms.openlocfilehash: 2321471b762215710f127ea1450152d8f8498adb
ms.sourcegitcommit: c776f4ee0064d3a22da18182bb2f33078741b6fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64449247"
---
# <a name="using-azure-sql-database-managed-instance-securely-with-public-endpoint"></a>在公共终结点中安全使用 Azure SQL 数据库托管实例

可以启用 Azure SQL 数据库托管实例，以通过[公共终结点](../virtual-network/virtual-network-service-endpoints-overview.md)提供用户连接。 本文提供有关如何提高此配置的安全性的指导。

## <a name="scenarios"></a>方案

托管实例提供专用终结点用于从其虚拟网络内部启用连接。 默认选项是提供最大的隔离性。 但是，在某些情况下，需要使用公共终结点连接：

- 与仅限多租户的 PaaS 产品/服务集成。
- 数据交换吞吐量比使用 VPN 时更高。
- 公司政策禁止在企业网络中使用 PaaS。

## <a name="deploying-managed-instance-for-public-endpoint-access"></a>部署托管实例以访问公共终结点

可以访问公共终结点的托管实例的常用部署模型是在专用的隔离虚拟网络中创建实例，但不一定非要这样做。 在此配置中，虚拟网络只是用于实现虚拟群集隔离。 如果托管实例 IP 地址空间与企业网络 IP 地址空间重叠，则此做法没有意义。

## <a name="securing-data-in-motion"></a>保护动态数据

如果客户端驱动程序支持加密，则始终加密托管实例数据流量。 托管实例与其他 Azure 虚拟机或 Azure 服务之间的数据永远不会离开 Azure 主干网络。 如果存在托管实例到本地网络的连接，我们建议配合使用 Express Route 和 Microsoft 对等互连。 Express Route 有助于避免通过公共 Internet 移动数据（对于托管实例专用连接，只能使用专用对等互连）。

## <a name="locking-down-inbound-and-outbound-connectivity"></a>锁定入站和出站连接

下图显示了建议的安全配置。

![managed-instance-vnet.png](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

托管实例具有[专用公共终结点地址](sql-database-managed-instance-find-management-endpoint-ip-address.md)。 应在客户端出站防火墙和网络安全组规则中设置此 IP 地址，以限制出站连接。

为了确保发往托管实例的流量来自受信任的源，我们建议使用已知的 IP 地址从源建立连接。 使用网络安全组限制对端口 3342 上的托管实例公共终结点的访问。

如果客户端需要从本地网络发起连接，请确保发起地址可转换为一组已知的 IP。 如果无法做到这一点（例如，移动工作者就是一个典型的示例），我们建议使用[点到站点 VPN 连接和专用终结点](sql-database-managed-instance-configure-p2s.md)。

如果连接是从 Azure 发起的，我们建议从分配的已知 [VIP](../virtual-network/virtual-networks-reserved-public-ip.md)（例如虚拟机）发出流量。 为便于管理 VIP 地址，客户可以考虑使用[公共 IP 地址前缀](../virtual-network/public-ip-address-prefix.md)。