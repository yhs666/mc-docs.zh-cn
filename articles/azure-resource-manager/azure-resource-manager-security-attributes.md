---
title: Azure 资源管理器的常见安全属性
description: 用于评估 Azure 资源管理器的常见安全属性的清单
services: azure-resource-manager
author: rockboyfor
manager: digimobile
ms.service: azure-resource-manager
ms.topic: conceptual
origin.date: 04/25/2019
ms.date: 06/03/2019
ms.author: v-yeche
ms.openlocfilehash: e5e855ab655c63824aa3c6b088784d7979171ee0
ms.sourcegitcommit: d75eeed435fda6e7a2ec956d7c7a41aae079b37c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66195493"
---
# <a name="common-security-attributes-for-azure-resource-manager"></a>Azure 资源管理器的常见安全属性

安全性已集成到 Azure 服务的各个方面。 本文记录了内置到 Azure 资源管理器中的常见安全属性。

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>预防

| 安全属性 | 是/否 | 注释 |
|---|---|--|
| 静态加密：<ul><li>服务器端加密</li><li>使用客户托管密钥的服务器端加密</li><li>其他加密功能（例如客户端、始终加密等）</ul>| 是 |  |
| 传输中加密：<ul><li>快速路由加密</li><li>Vnet 中加密</li><li>VNet-VNet 加密</ul>| 是 | HTTPS/TLS。 |
| 加密密钥处理（CMK、BYOK 等）| 不适用 | Azure 资源管理器不存储客户内容，仅存储控制数据。 |
| 列级加密（Azure 数据服务）| 是 | |
| 加密的 API 调用| 是 | |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | 是/否 | 注释 |
|---|---|--|
| 服务终结点支持| 否 | |
| VNet 注入支持| 是 | |
| 网络隔离和防火墙支持| 否 |  |
| 强制隧道支持| 否 |  |

## <a name="detection"></a>检测

| 安全属性 | 是/否 | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 否 | |

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | 是/否 | 注释|
|---|---|--|
| 身份验证| 是 | 基于 [Azure Active Directory](/active-directory)。|
| 授权| 是 | |

## <a name="audit-trail"></a>审核线索

| 安全属性 | 是/否 | 注释|
|---|---|--|
| 控制和管理平面日志记录和审核| 是 | 活动日志公开对资源执行的所有写入操作（PUT、POST、DELETE）；请参阅[查看活动日志来审核对资源的操作](resource-group-audit.md)。 |
| 数据平面日志记录和审核| 不适用 | |

## <a name="configuration-management"></a>配置管理

| 安全属性 | 是/否 | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 |  |

<!--Update_Description: new articles on azure resource manager security attributes -->
<!--ms.date: 05/27/2019-->