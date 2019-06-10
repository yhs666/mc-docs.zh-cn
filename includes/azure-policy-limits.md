---
title: include 文件
description: include 文件
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 08/16/2018
ms.date: 06/17/2019
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: 4755536b2a0079c36d42e1822c45e8057acbfc6f
ms.sourcegitcommit: d7db02d1b62c7b4deebd5989be97326b4425d1d3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66689114"
---
Azure Policy 的每个对象类型都有一个最大计数。 _作用域_条目是指订阅或[管理组](../articles/governance/management-groups/index.md)。

| Where | 对象 | 最大计数 |
|---|---|---|
| 作用域 | 策略定义 | 250 |
| 作用域 | 计划定义 | 100 |
| 租户 | 计划定义 | 1,000 |
| 作用域 | 策略或计划分配 | 100 |
| 策略定义 | parameters | 20 个 |
| 计划定义 | 策略 | 100 |
| 计划定义 | parameters | 100 |
| 策略或计划分配 | 排除项（不在范围内的项） | 400 |
| 策略规则 | 嵌套式条件语句 | 512 |
