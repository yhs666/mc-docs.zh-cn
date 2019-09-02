---
title: Azure Key Vault 的安全特性
description: 用于评估 Azure Key Vault 的安全属性的清单
services: key-vault
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
origin.date: 01/31/2019
ms.date: 08/12/2019
ms.author: v-biyu
ms.openlocfilehash: d66b0f6e02148e55468bb2a81488edcb45117131
ms.sourcegitcommit: 84f6eb9f6eb8d5382a05e5850f2c222ef394943b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68633032"
---
# <a name="security-attributes-for-azure-key-vault"></a>Azure Key Vault 的安全特性

本文记录了内置到 Azure Key Vault 中的安全特性。 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 静态加密（例如服务器端加密、带客户托管密钥的服务器端加密，以及其他加密功能）| 是 | 加密所有对象。 |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | 所有通信都通过加密的 API 调用进行 |
| 列级加密（Azure 数据服务）| 不适用 |  |
| 加密的 API 调用| 是 | 使用 HTTPS。 |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 是 | 使用虚拟网络 (VNet) 服务终结点。 |
| VNet 注入支持| 否 |  |
| 网络隔离和防火墙支持| 是 | 使用 VNet 防火墙规则。 |
| 强制隧道支持| 否 |  |

## <a name="detection"></a>检测

| 安全属性 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | 使用 Log Analytics。 |

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 | 身份验证通过 Azure Active Directory 来进行。 |
| 授权| 是 | 使用密钥保管库访问策略。 |


## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制/管理平面日志记录和审核| 是 | 使用 Log Analytics。 |
| 数据平面日志记录和审核| 是 | 使用 Log Analytics。 |

## <a name="access-controls"></a>访问控制

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制/管理平面访问控制 | 是 | Azure Resource Manager 基于角色的访问控制 (RBAC) |
| 数据平面访问控制（在每个服务级别） | 是 | 密钥保管库访问策略 |