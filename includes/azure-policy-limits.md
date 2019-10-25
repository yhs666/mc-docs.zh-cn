---
title: include 文件
description: include 文件
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 06/05/2019
ms.date: 10/15/2019
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 99b3a1f5d23dcc1797f4305ace7496a087ff232e
ms.sourcegitcommit: 0bfa3c800b03216b89c0461e0fdaad0630200b2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72526688"
---
Azure Policy 的每个对象类型都有一个最大计数。 _作用域_条目是指订阅或[管理组](../articles/governance/management-groups/index.md)。

| Where | 对象 | 最大计数 |
|---|---|---|
| 作用域 | 策略定义 | 500 |
| 作用域 | 计划定义 | 100 |
| 租户 | 计划定义 | 1,000 |
| 作用域 | 策略或计划分配 | 100 |
| 策略定义 | parameters | 20 个 |
| 计划定义 | 策略 | 100 |
| 计划定义 | parameters | 100 |
| 策略或计划分配 | 排除项（不在范围内的项） | 400 |
| 策略规则 | 嵌套式条件语句 | 512 |
