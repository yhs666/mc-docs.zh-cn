---
title: include 文件
description: include 文件
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 08/16/2018
ms.date: 07/01/2019
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: 343fe5d3c786231dc2ef55cd8d8904c256659325
ms.sourcegitcommit: 153236e4ad63e57ab2ae6ff1d4ca8b83221e3a1c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2019
ms.locfileid: "67173646"
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
