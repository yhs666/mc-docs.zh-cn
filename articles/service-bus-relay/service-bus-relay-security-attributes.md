---
title: Azure 服务总线中继的常见安全属性
description: 用于评估 Azure 服务总线中继的常见安全属性的清单
services: service-bus-relay
ms.service: service-bus-relay
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: v-lingwu
ms.openlocfilehash: 660d12377af83b5df0a25500159f79620d80b00d
ms.sourcegitcommit: 68f7c41974143a8f7bd9b7a54acf41c09893e587
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68332177"
---
# <a name="security-attributes-for-azure-service-bus-relay"></a>Azure 服务总线中继的安全属性

本文记录了内置到 Azure 服务总线中继中的安全属性。

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 静态加密：<ul><li>服务器端加密</li><li>使用客户托管密钥的服务器端加密</li><li>其他加密功能（例如客户端、始终加密等）</ul>|  不适用 | 中继是一个 Web 套接字，不保存数据。 |
| 传输中加密：<ul><li>快速路由加密</li><li>VNet 中加密</li><li>VNet-VNet 加密</ul>| 是 | 服务需要 TLS。 |
| 加密密钥处理（CMK、BYOK 等）| 否 | 仅使用 Microsoft TLS 证书。  |
| 列级加密（Azure 数据服务）| 不适用 | |
| 加密的 API 调用| 是 | HTTPS。 |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 否 |  |
| 网络隔离和防火墙支持| 否 |  |
| 强制隧道支持| 不适用 | 中继是 TLS 隧道  |

## <a name="detection"></a>检测

| 安全属性 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | |

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 | 通过 SAS。 |
| 授权|  是 | 通过 SAS。 |


## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制和管理平面日志记录和审核| 是 | 通过 [Azure 资源管理器](../azure-resource-manager/index.yml)。 |
| 数据平面日志记录和审核| 是 | 连接成功/失败、错误和记录的内容。  |

## <a name="configuration-management"></a>配置管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 | 通过 [Azure 资源管理器](../azure-resource-manager/index.yml)。|
