---
title: Azure SQL 数据库的安全属性
description: 用于评估 Azure SQL 数据库的安全属性的清单
services: sql-database
author: WenJason
manager: digimobile
ms.service: load-balancer
ms.topic: conceptual
origin.date: 05/06/2019
ms.date: 05/20/2019
ms.author: v-jay
ms.openlocfilehash: 3eeb6db2d897f162782fe18f5a726488aa1277ca
ms.sourcegitcommit: f0f5cd71f92aa85411cdd7426aaeb7a4264b3382
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65629477"
---
# <a name="security-attributes-for-azure-sql-database"></a>Azure SQL 数据库的安全属性

本文记录了内置到 Azure SQL 数据库中的常见安全属性。

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 静态加密：<ul><li>服务器端加密</li><li>使用客户托管密钥的服务器端加密</li><li>其他加密功能（例如客户端、始终加密等）</ul>| 是 | 称为“使用中加密”，详见 [Always Encrypted](sql-database-always-encrypted.md) 一文中的说明。 服务端加密使用[透明数据加密](transparent-data-encryption-azure-sql.md) (TDE)。|
| 传输中加密：<ul><li>ExpressRoute 加密</li><li>Vnet 中加密</li><li>VNet-VNet 加密</ul>| 是 | 使用 HTTPS。 |
| 加密密钥处理（CMK、BYOK 等）| 是 | 提供服务托管和客户托管密钥处理（后者通过 [Azure Key Vault](../key-vault/index.yml) 进行）。 |
| 列级加密（Azure 数据服务）| 是 | 通过 [Always Encrypted](sql-database-always-encrypted.md) 进行。 |
| 加密的 API 调用| 是 | 使用 HTTPS/SSL。 |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 是 | 仅适用于[单一数据库](sql-database-single-index.yml)。 |
## <a name="detection"></a>检测

| 安全属性 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持 | 是 ||

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 访问管理 - 身份验证| 是 | Azure Active Directory。 |
| 访问管理 - 授权| 是 |  |


## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制/管理计划日志记录和审核| 是 | 仅对某些事件为是。 |
| 数据平面日志记录和审核 | 是 | 通过 [SQL 审核](sql-database-auditing.md)。 |

## <a name="configuration-management"></a>配置管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 否  | | 

## <a name="additional-security-attributes-for-sql-database"></a>SQL 数据库的其他安全属性

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 预防：漏洞评估 | 是 | 请参阅 [SQL 漏洞评估服务可帮助你识别数据库漏洞](sql-vulnerability-assessment.md)。 |
| 预防：数据发现和分类  | 是 | 请参阅 [Azure SQL 数据库和 SQL 数据仓库数据发现和分类](sql-database-data-discovery-and-classification.md)。 |
| 检测：威胁检测 | 是 | 请参阅 [Azure SQL 数据库的高级威胁防护](sql-database-threat-detection-overview.md)。 |
