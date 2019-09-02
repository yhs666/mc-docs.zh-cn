---
title: include 文件
description: include 文件
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 05/17/2018
ms.date: 01/14/2019
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: 0546e3c2d75f15676c417e7c189e1128942d0356
ms.sourcegitcommit: 4f91d9bc4c607cf254479a6e5c726849caa95ad8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53996462"
---
### <a name="sql-databases"></a>SQL 数据库

|  |  |
|---------|---------|
| [允许的 SQL DB SKU](../articles/governance/policy/samples/allowed-sql-db-skus.md) | 要求 SQL 数据库使用已批准的 SKU。 指定一个允许的 SKU ID 的数组或允许的 SKU 名称的数组。 |
| [审核 DB 级别的威胁检测设置](../articles/governance/policy/samples/audit-db-threat-detection-setting.md) | 如果 SQL 数据库安全警报策略未设置成指定状态，则审核这些策略。 指定一个值，该值指示威胁检测是启用还是禁用状态。  |
| [审核 SQL 数据库加密](../articles/governance/policy/samples/sql-database-encryption-audit.md) | 如果 SQL 数据库未启用透明数据加密，则会进行审核。 |
| [审核 SQL DB 级别审核设置](../articles/governance/policy/samples/audit-sql-db-audit-setting.md) | 如果 SQL 数据库审核设置不匹配指定设置，则审核这些设置。 指定用于指示应启用或禁用审核设置的值。  |
