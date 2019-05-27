---
title: Azure Analysis Services 资源和对象限制 | Azure
description: 介绍 Azure Analysis Services 资源和对象限制。
author: rockboyfor
manager: digimobile
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 04/11/2019
ms.date: 06/03/2019
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: 79a7b0867eb1b45babf37153b52883d7180f1d9c
ms.sourcegitcommit: d75eeed435fda6e7a2ec956d7c7a41aae079b37c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66195422"
---
# <a name="analysis-services-resource-and-object-limits"></a>Analysis Services 资源和对象限制
<!--Verify sucessfull-->

本文介绍资源和模型对象限制。

## <a name="tier-limits"></a>层限制

<!--Not Available on ### Developer tier-->
### <a name="basic-tier"></a>基本层

建议在具有小型表格模型的生产解决方案、限制用户并发性和要求简单数据刷新的场合下使用该层。 查询副本横向扩展*不适用于*此层。 此层不支持透视、多个分区和 DirectQuery 表格模型功能。  

|计划  |QPU  |内存 (GB)  |
|---------|---------|---------|
|B1    |    40     |    10 个     |
|B2    |    80     |    20 个     |

### <a name="standard-tier"></a>标准层

此层适用于需要弹性用户并发性，且数据模型不断扩大的任务关键型生产应用程序。 它支持使用高级数据刷新实现接近实时的数据模型更新，并支持所有表格建模功能。

|计划  |QPU  |内存 (GB)  |
|---------|---------|---------|
|S0    |    40     |    10 个     |
|S1    |    100     |    25     |
|S2   |    200     |    50     |
|S4    |    400     |    100     |

<!--Notice: Standared tier from S0,S1,S2,S4 in Mooncake-->

## <a name="object-limits"></a>对象限制

以下限制是理论限制。 数值减小，性能会下降。

|对象|最大大小/数量|  
|------------|----------------------------|  
|一个实例中的数据库数|16,000|  
|数据库中表和列的组合数|16,000|  
|表中的行数|无限制<br /><br /> **警告：** 具有限制：表中单列的非重复值不能超过 1,999,999,997 个。|  
|表中的层次结构数|15,999|  
|层次结构中的级别数|15,999|  
|关系|8,000|  
|所有表中的键列数|15,999|  
|表中的度量值|2^31-1 = 2,147,483,647|  
|查询返回的单元数|2^31-1 = 2,147,483,647|  
|源查询的记录大小|64 K|  
|对象名称的长度|512 个字符|

<!-- Update_Description: wording update -->
