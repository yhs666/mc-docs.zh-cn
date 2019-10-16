---
title: Azure 服务总线消息传送的常见安全属性
description: 用于评估 Azure 服务总线消息传送的常见安全属性的清单
services: service-bus-messaging
ms.service: service-bus-messaging
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: 283c5f72347e6f1f4c7826ce0dfb3d9154d216af
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71329874"
---
# <a name="security-attributes-for-azure-service-bus-messaging"></a>Azure 服务总线消息传送的安全属性

本文记录了内置到 Azure 服务总线消息传送中的安全属性。

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 静态加密：<ul><li>服务器端加密</li><li>使用客户托管密钥的服务器端加密</li><li>其他加密功能（例如客户端、始终加密等）</ul>|  默认情况下，服务器端静态加密为“是”。 | 目前尚不支持客户托管密钥和 BYOK。 客户端加密是客户端的责任 |
| 传输中加密：<ul><li>快速路由加密</li><li>VNet 中加密</li><li>VNet-VNet 加密</ul>| 是 | 支持标准的 HTTPS/TLS 机制。 |
| 加密密钥处理（CMK、BYOK 等）| 否 |   |
| 列级加密（Azure 数据服务）| 不适用 | |
| 加密的 API 调用| 是 | API 调用通过 [Azure 资源管理器](../azure-resource-manager/index.yml)和 HTTPS 进行。 |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 是（仅限高级层） | 仅[服务总线高级层](service-bus-premium-messaging.md)支持 VNet 服务终结点。 |
| VNet 注入支持| 否 | |
| 网络隔离和防火墙支持| 是（仅限高级层） |  |
| 强制隧道支持| 否 |  |

## <a name="detection"></a>检测

| 安全属性 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | 通过 [Azure Monitor 和警报](service-bus-metrics-azure-monitor.md)支持。 |

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 | 请参阅[服务总线身份验证和授权](service-bus-authentication-and-authorization.md)。|
| 授权| 是 | 支持通过 [RBAC](service-bus-managed-service-identity.md)（预览版）和 SAS 令牌进行授权；请参阅[服务总线身份验证和授权](service-bus-authentication-and-authorization.md)。 |



## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制和管理平面日志记录和审核| 是 | 操作日志可用；请参阅[服务总线诊断日志](service-bus-diagnostic-logs.md)。  |
| 数据平面日志记录和审核| 否 |  |

## <a name="configuration-management"></a>配置管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 | 支持通过 [Azure 资源管理器 API](https://docs.microsoft.com/rest/api/resources/) 进行资源提供程序版本控制。|
