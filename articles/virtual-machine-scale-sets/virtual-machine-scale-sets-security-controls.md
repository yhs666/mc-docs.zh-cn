---
title: Azure 虚拟机规模集的安全控制
description: 用于评估 Azure 虚拟机规模集的安全控制的清单
services: virtual-machine-scale-sets
ms.service: virtual-machine-scale-sets
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.topic: conceptual
origin.date: 09/05/2019
ms.date: 09/23/2019
ms.author: v-junlch
ms.openlocfilehash: 92f1ca1c266784bcca38b67ac12e0180e1b4e131
ms.sourcegitcommit: 73a8bff422741faeb19093467e0a2a608cb896e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2019
ms.locfileid: "71673609"
---
# <a name="security-controls-for-azure-virtual-machine-scale-sets"></a>Azure 虚拟机规模集的安全控制

本文介绍 Azure 虚拟机规模集中内置的安全控制。

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>网络

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 是 | |
| VNet 注入支持| 是 | |
| 网络隔离和防火墙支持| 是 |  |
| 强制隧道支持| 是 | 请参阅[使用 Azure 资源管理器部署模型配置强制隧道](/vpn-gateway/vpn-gateway-forced-tunneling-rm)。 |

## <a name="monitoring--logging"></a>监视和日志记录

| 安全控制 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics 等）| 是 | 请参阅[在 Azure 中监视和更新 Linux 虚拟机](/virtual-machines/linux/tutorial-monitoring)和[在 Azure 中监视和更新 Windows 虚拟机](/virtual-machines/windows/tutorial-monitoring)。 |
| 控制和管理平面日志记录和审核| 是 |  |
| 数据平面日志记录和审核 | 否 |  |

## <a name="identity"></a>标识

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 |  |
| 授权| 是 |  |

## <a name="data-protection"></a>数据保护

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务器端静态加密：Microsoft 管理的密钥 | 是 | 请参阅[如何在 Azure 中加密 Linux 虚拟机](/virtual-machines/linux/encrypt-disks)和[加密 Windows VM 上的虚拟磁盘](/virtual-machines/windows/encrypt-disks)。 |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | Azure 虚拟机支持 [ExpressRoute](/expressroute) 和 VNet 加密。 |
| 服务器端静态加密：客户管理的密钥 (BYOK) | 是 | 客户管理的密钥是受支持的 Azure 加密方案； |
| 列级加密（Azure 数据服务）| 不适用 | |
| 加密的 API 调用| 是 | 通过 HTTPS 和 SSL。 |

## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 |  | 

