---
title: Azure 备份的常见安全属性
description: 用于评估 Azure 备份的常见安全属性的清单
services: backup
author: lingliw
manager: digimobile
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: v-lingwu
ms.openlocfilehash: 104af22c7763f8077da09a7250da3231ef2d661f
ms.sourcegitcommit: 461c7b2e798d0c6f1fe9c43043464080fb8e8246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68818467"
---
# <a name="security-attributes-for-azure-backup"></a>Azure 备份的安全属性

本文记录了内置到 Azure 备份中的安全属性。 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 静态加密：<ul><li>服务器端加密</li><li>使用客户托管密钥的服务器端加密</li><li>其他加密功能（例如客户端、始终加密等）</ul>| 是 | 对存储帐户使用存储服务加密。 |
| 传输中加密：<ul><li>快速路由加密</li><li>VNet 中加密</li><li>VNet-VNet 加密</ul>| 否 | 使用 HTTPS。 |
| 加密密钥处理（CMK、BYOK 等）| 否 |  |
| 列级加密（Azure 数据服务）| 否 |  |
| 加密的 API 调用| 是 |  |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 否 |  |
| VNet 注入支持| 否 |  |
| 网络隔离和防火墙支持| 是 | 对 VM 备份支持强制隧道。 对 VM 内运行的工作负荷不支持强制隧道。 |
| 强制隧道支持| 否 |  |

## <a name="detection"></a>检测

| 安全属性 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | 通过诊断日志支持 Log Analytics。 有关详细信息，请参阅[使用 Log Analytics 监视 Azure 备份保护的工作负荷](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/)。 |

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 | 身份验证通过 Azure Active Directory 来进行。 |
| 授权| 是 | 使用客户创建和内置的 RBAC 角色。 有关详细信息，请参阅[使用基于角色的访问控制管理 Azure 备份恢复点](/backup/backup-rbac-rs-vault)。 |


## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制和管理平面日志记录和审核| 是 | 来自 Azure 门户的所有客户触发操作都会记录到活动日志中。 |
| 数据平面日志记录和审核| 否 | 无法直接访问 Azure 备份数据平面。  |

## <a name="configuration-management"></a>配置管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是|  |