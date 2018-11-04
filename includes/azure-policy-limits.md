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
ms.openlocfilehash: 8e917acf47f6b7f5822b8b9f3aeeb9e5a286d9c9
ms.sourcegitcommit: b8e99939a5493a15b78c32e87bfbf76a8c96a84a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50409008"
---
Azure Policy 的每个对象类型都有一个最大计数。 _作用域_条目是指订阅或[管理组](../articles/governance/management-groups/overview.md)。

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
