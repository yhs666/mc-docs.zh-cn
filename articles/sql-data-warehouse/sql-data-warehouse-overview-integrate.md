---
title: 使用 SQL 数据仓库构建集成解决方案 | Microsoft 文档
description: 用于集成 SQL 数据仓库的工具以及提供相应解决方案的合作伙伴。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
origin.date: 04/17/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 0933135e94510856d8db763a171cec1c7ead16f4
ms.sourcegitcommit: c596d3a0f0c0ee2112f2077901533a3f7557f737
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "49089168"
---
# <a name="integrate-other-services-with-sql-data-warehouse"></a>使用 SQL 数据仓库集成其他服务
除了本身的核心功能以外，SQL 数据仓库还允许用户集成 Azure 中的其他许多服务。 这些服务包括：

* Azure 流分析 <!-- Not Available Power BI, Azure Data Factory, Azure Machine Learning -->

我们正努力连接多个跨 Azure 生态系统的服务。

<!-- Not Available ## Power BI -->
<!-- Not Available ## Azure Data Factory -->
<!-- Not Available ## Azure Machine Learning -->
## <a name="azure-stream-analytics"></a>Azure 流分析
Azure 流分析是复杂、完全托管的基础结构，用于处理和使用从 Azure 事件中心生成的事件数据。  通过与 SQL 数据仓库集成，可以有效地处理流数据，并将其与关系数据一起存储以实现更深入、更高级的分析。  

* **作业输出** ：将流分析作业的输出直接发送到 SQL 数据仓库。

有关详细信息，请参阅[与 Azure 流分析集成](sql-data-warehouse-integrate-azure-stream-analytics.md)。

## <a name="next-steps"></a>后续步骤
要与 Azure SQL 数据库集成，请参阅[配置 SQL 数据库弹性查询](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)

<!--MSDN references-->

<!--Other Web references-->