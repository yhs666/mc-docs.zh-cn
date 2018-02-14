---
title: "使用 SQL 数据仓库构建集成解决方案 | Azure"
description: "用于集成 SQL 数据仓库的工具以及提供相应解决方案的合作伙伴。"
services: sql-data-warehouse
documentationcenter: NA
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
origin.date: 10/31/2016
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: 41856bdcf6e623cd3e3643b87284ae30fbf489c6
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a>在 SQL 数据仓库中利用其他服务
除了本身的核心功能以外，SQL 数据仓库还允许用户利用 Azure 中的其他许多服务。  具体而言，我们目前已采取多种措施深度集成了以下服务：

* Azure 流分析
<!-- Not Available Power BI, Azure Data Factory, Azure Machine Learning -->

我们正努力连接多个跨 Azure 生态系统的服务。

<!-- Not Available ## Power BI -->
<!-- Not Available ## Azure Data Factory -->
<!-- Not Available ## Azure Machine Learning -->
##<a name="azure-stream-analytics"></a>Azure 流分析
Azure 流分析是复杂、完全托管的基础结构，用于处理和使用从 Azure 事件中心生成的事件数据。  通过与 SQL 数据仓库集成，可以有效地处理流数据，并将其与关系数据一起存储以实现更深入、更高级的分析。  

* **作业输出**：将流分析作业的输出直接发送到 SQL 数据仓库。

有关详细信息，请参阅[与 Azure 流分析集成](sql-data-warehouse-integrate-azure-stream-analytics.md)或 [Azure 流分析文档](http://docs.azure.cn/zh-cn/stream-analytics/)。

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

<!-- Not Available on [Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md -->
<!-- Not Available on [Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md -->
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
<!-- Not Available on [Power BI]: sql-data-warehouse-integrate-power-bi.md -->
<!-- Not Available on [Partners]: sql-data-warehouse-partner-business-intelligence.md -->

<!--MSDN references-->

<!--Other Web references-->