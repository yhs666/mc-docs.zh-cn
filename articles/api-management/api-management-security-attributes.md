---
title: Azure API 管理的常见安全属性
description: 用于评估 API 管理的常见安全属性的清单
services: api-management
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: api-management
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 7304bf739fffbd232c9f019c8f138372a77383e3
ms.sourcegitcommit: 99ef971eb118e3c86a6c5299c7b4020e215409b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2019
ms.locfileid: "65830032"
---
# <a name="common-security-attributes-for-api-management"></a>API 管理的常见安全属性

安全性已集成到 Azure 服务的各个方面。 本文记录了内置到 API 管理中的常见安全特性。

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 静态加密：<ul><li>服务器端加密</li><li>使用客户托管密钥的服务器端加密</li><li>其他加密功能（例如客户端、始终加密等）</ul>| 是（仅服务端加密） | 敏感数据（例如证书、密钥以及使用机密命名的值）使用服务托管的、基于服务实例的密钥进行加密。 |
| 传输中加密：<ul><li>快速路由加密</li><li>VNet 中加密</li><li>VNet-VNet 加密</ul>| 是 | [快速路由](../expressroute/index.yml)和 VNet 加密由 [Azure 网络](../virtual-network/index.yml)提供。 |
| 加密密钥处理（CMK、BYOK 等）| 否 | 所有加密密钥都是基于服务实例的，也是通过服务托管的。 |
| 列级加密（Azure 数据服务）| 不适用 | |
| 加密的 API 调用| 是 | 通过 [Azure 资源管理器](../azure-resource-manager/index.yml)经 TLS 进行管理平面调用。 需要有效 JSON web 令牌 (JWT)。  可以通过 TLS 以及某个受支持的身份验证机制（例如，客户端证书或 JWT）对数据平面调用进行保护。
 |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 否 | |
| VNet 注入支持| 是 | |
| 网络隔离和防火墙支持| 是 | 分别使用网络安全组 (NSG) 和 Azure 应用程序网关（或其他软件设备）。 |
| 强制隧道支持| 是 | Azure 网络支持强制隧道。 |

## <a name="detection"></a>检测

| 安全属性 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | |

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 | |
| 授权| 是 | |


## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制和管理平面日志记录和审核| 是 | [Azure Monitor 活动日志](../azure-monitor/platform/activity-logs-overview.md) |
| 数据平面日志记录和审核| 是 | [Azure Monitor 诊断日志](../azure-monitor/platform/diagnostic-logs-overview.md)。  |

## <a name="configuration-management"></a>配置管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 | 使用 [Azure API 管理 DevOps 资源工具包](https://aka.ms/apimdevops) |

## <a name="vulnerability-scans-false-positives"></a>漏洞扫描误报

此部分记录不影响 Azure API 管理的常见漏洞。

| 漏洞               | 说明                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed 是在实现 TLS SessionTicket 扩展过程中出现的漏洞，在某些 F5 产品中发现。 该漏洞会导致从取消初始化的内存中泄露（“溢出”）最多 31 字节的数据。 这是由 TLS 堆栈导致的，该堆栈填充了会话 ID，从客户端传递，在带有数据的情况下有 32 位长。 |