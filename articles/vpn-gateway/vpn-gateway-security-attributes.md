---
title: Azure VPN 网关的安全属性
description: 用于评估 Azure VPN 网关的安全属性的清单
services: sql-database
author: WenJason
manager: digimobile
ms.service: load-balancer
ms.topic: conceptual
origin.date: 05/06/2019
ms.date: 08/12/2019
ms.author: v-jay
ms.openlocfilehash: 9c6dcc34adcaaf7cf88cf81ae35935984106ef50
ms.sourcegitcommit: 193f49f19c361ac6f49c59045c34da5797ed60ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68732450"
---
# <a name="security-attributes-for-azure-vpn-gateway"></a>Azure VPN 网关的安全属性

本文阐述了 Azure VPN 网关中内置的常用安全属性。

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 静态加密（例如服务器端加密、带客户托管密钥的服务器端加密，以及其他加密功能） | 不适用 | VPN 网关传输客户数据，不存储客户数据 |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | VPN 网关加密 Azure VPN 网关和客户本地 VPN 设备之间 (S2S) 或 VPN 客户端之间 (P2S) 的客户数据包。 VPN 网关还支持 VNet 到 VNet 加密。 |
| 加密密钥处理（CMK、BYOK 等）| 否 | 客户指定的预共享密钥进行静态加密，但尚未与 CMK 集成。 |
| 列级加密（Azure 数据服务）| 不适用 | |
| 加密的 API 调用| 是 | 通过 [Azure 资源管理器](../azure-resource-manager/index.yml)和 HTTPS  |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 不适用 | |
| VNet 注入支持| 不适用 | 上获取。 |
| 网络隔离和防火墙支持| 是 | VPN 网关是每个客户虚拟网络的专用 VM 实例  |
| 强制隧道支持| 是 |  |

## <a name="detection"></a>检测

| 安全属性 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | 请参阅 [Azure Monitor 诊断日志/警报](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Azure Monitor 指标/警报](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md)。  |

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 | [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)，用于管理服务和配置 Azure VPN 网关。 |
| 授权| 是 | 支持通过 [RBAC](../role-based-access-control/overview.md) 进行授权。 |


## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制和管理平面日志记录和审核| 是 | Azure 资源管理器活动日志。 |
| 数据平面日志记录和审核 | 是 | [Azure Monitor 诊断日志](../azure-resource-manager/resource-group-audit.md)，用于 VPN 连接日志记录和审核。 |

## <a name="configuration-management"></a>配置管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 | 进行管理操作时，可以将 Azure VPN 网关配置的状态导出为 Azure 资源管理器模板，并在一段时间内进行版本控制。 | 