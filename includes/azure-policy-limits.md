---
title: include 文件
description: include 文件
services: azure-policy
author: WenJason
ms.service: azure-policy
ms.topic: include
origin.date: 08/16/2018
ms.date: 09/10/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 384ecdeb3239e8f5885cf80e732e69e6b73d8dce
ms.sourcegitcommit: 1b60848d25bbd897498958738644a4eb9cf3a302
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2018
ms.locfileid: "43731227"
---
Azure Policy 的每个对象类型都有一个最大计数。 “作用域”条目是指订阅或管理组。

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
