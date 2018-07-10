---
title: include 文件
description: include 文件
services: azure-policy
author: WenJason
ms.service: azure-policy
ms.topic: include
origin.date: 05/17/2018
ms.date: 07/09/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: f0d9ed95e4887c14a3826d8a5eb22b27ed96164c
ms.sourcegitcommit: 18810626635f601f20550a0e3e494aa44a547f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37405420"
---
### <a name="sql-databases"></a>SQL 数据库

|  |  |
|---------|---------|
| [允许的 SQL DB SKU](../articles/azure-policy/scripts/allowed-sql-db-skus.md) | 要求 SQL 数据库使用已批准的 SKU。 指定一个允许的 SKU ID 的数组或允许的 SKU 名称的数组。 |
| [审核 DB 级别的威胁检测设置](../articles/azure-policy/scripts/audit-db-threat-det-setting.md) | 如果 SQL 数据库安全警报策略未设置成指定状态，则审核这些策略。 指定一个值，该值指示威胁检测是启用还是禁用状态。  |
| [审核 SQL 数据库加密](../articles/azure-policy/scripts/sql-database-encryption-audit.md) | 如果 SQL 数据库未启用透明数据加密，则会进行审核。 |
| [审核 SQL DB 级别审核设置](../articles/azure-policy/scripts/audit-sql-db-audit-setting.md) | 如果 SQL 数据库审核设置不匹配指定设置，则审核这些设置。 指定用于指示应启用或禁用审核设置的值。  |
| [审核透明数据加密状态](../articles/azure-policy/scripts/audit-trans-data-enc-status.md) | 如果 SQL 数据库透明数据加密未启用，则审核该加密。  |