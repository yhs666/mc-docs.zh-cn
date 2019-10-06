---
title: Azure 服务总线中继的安全控制
description: 用于评估 Azure 服务总线中继的安全控制的清单
services: service-bus-relay
ms.service: service-bus-relay
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
origin.date: 09/04/2019
ms.date: 09/26/2019
ms.author: mbaldwin
ms.openlocfilehash: 28fa95b9ce5272067d5dee5528541f11748efc5b
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71330461"
---
# <a name="security-controls-for-azure-service-bus-relay"></a>Azure 服务总线中继的安全控制

本文介绍 Azure 服务总线中继中内置的安全控制。

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>网络

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 否 |  |
| 网络隔离和防火墙支持| 否 |  |
| 强制隧道支持| 不适用 | 中继是 TLS 隧道  |

## <a name="monitoring--logging"></a>监视和日志记录

| 安全控制 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | |
| 控制和管理平面日志记录和审核| 是 | 通过 [Azure 资源管理器](../azure-resource-manager/index.yml)。 |
| 数据平面日志记录和审核| 是 | 连接成功/失败、错误和记录的内容。  |

## <a name="identity"></a>标识

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 | 通过 SAS。 |
| 授权|  是 | 通过 SAS。 |

## <a name="data-protection"></a>数据保护

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务器端静态加密：Microsoft 管理的密钥 |  不适用 | 中继是一个 Web 套接字，不保存数据。 |
| 服务器端静态加密：客户管理的密钥 (BYOK) | 否 | 仅使用 Microsoft TLS 证书。  |
| 列级加密（Azure 数据服务）| 不适用 | |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | 服务需要 TLS。 |
| 加密的 API 调用| 是 | HTTPS。 |


## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 | 通过 [Azure 资源管理器](../azure-resource-manager/index.yml)。|
