---
title: include 文件
description: include 文件
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 08/16/2018
ms.date: 11/12/2018
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: 92af7a9f9656037a42b85eb2a2de3448a41fd659
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676321"
---
Azure Policy 的每个对象类型都有一个最大计数。 _作用域_条目是指订阅或[管理组](../articles/governance/management-groups/index.md)。

| Where | 对象 | 最大计数 |
|---|---|---|
| 作用域 | 策略定义 | 250 |
| 作用域 | 计划定义 | 100 |
| 租户 | 计划定义 | 1000 |
| 作用域 | 策略/计划分配数 | 100 |
| 策略定义 | parameters | 20 个 |
| 计划定义 | 策略 | 100 |
| 计划定义 | parameters | 100 |
| 策略/计划分配数 | 排除项（不在范围内的项） | 100 |
| 策略规则 | 嵌套条件语句 | 512 |
