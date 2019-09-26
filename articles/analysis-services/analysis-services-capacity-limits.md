---
title: Azure Analysis Services 资源和对象限制 | Azure
description: 介绍 Azure Analysis Services 资源和对象限制。
author: rockboyfor
manager: digimobile
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 08/23/2019
ms.date: 09/23/2019
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: b36e18d1ce53da7117d1fa18d1dbaed073d94cb1
ms.sourcegitcommit: 6a62dd239c60596006a74ab2333c50c4db5b62be
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155840"
---
<!--Verify sucessfull-->
# <a name="analysis-services-resource-and-object-limits"></a>Analysis Services 资源和对象限制

本文介绍资源和模型对象限制。

## <a name="tier-limits"></a>层限制

有关基本层和标准层的 QPU 和内存限制，请参阅 [Azure Analysis Services 定价页](https://www.azure.cn/pricing/details/analysis-services/)。

<!--Not Available on developer,-->
<!--Not Available on ### Developer tier-->
<!--Notice: Standared tier from S0,S1,S2,S4 in Mooncake-->

## <a name="object-limits"></a>对象限制

以下限制是理论限制。 数值减小，性能会下降。

|Object|最大大小/数量|  
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
