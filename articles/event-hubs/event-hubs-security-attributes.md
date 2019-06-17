---
title: Azure 事件中心的常见安全属性
description: 用于评估 Azure 事件中心的常见安全属性的清单
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
origin.date: 05/22/2019
ms.date: 06/17/2019
ms.author: v-biyu
ms.openlocfilehash: cfa0648705134263be43bae04544797468284fdc
ms.sourcegitcommit: d7db02d1b62c7b4deebd5989be97326b4425d1d3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687495"
---
# <a name="common-security-attributes-for-azure-event-hubs"></a>Azure 事件中心的常见安全属性

本文记录了内置到 Azure 事件中心的常见安全属性。

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 静态加密：<ul><li>服务器端加密</li><li>使用客户托管密钥的服务器端加密</li><li>其他加密功能（例如客户端、始终加密等）</ul>|  是 | |
| 传输中加密：<ul><li>快速路由加密</li><li>Vnet 中加密</li><li>VNet-VNet 加密</ul>| 是 | |
| 加密密钥处理（CMK、BYOK 等）| 否 |  |
| 列级加密（Azure 数据服务）| 不适用 | |
| 加密的 API 调用| 是 |  |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 是 |  |
| vNET 注入支持| 否 | |
| 网络隔离和防火墙支持| 是 |  |
| 强制隧道支持| 否 |  |

## <a name="detection"></a>检测

| 安全属性 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics 等）| 是 | |

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 | |
| 授权|  是 | |


## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制和管理平面日志记录和审核| 是 |  |
| 数据平面日志记录和审核| 是 |   |

## <a name="configuration-management"></a>配置管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 | |
