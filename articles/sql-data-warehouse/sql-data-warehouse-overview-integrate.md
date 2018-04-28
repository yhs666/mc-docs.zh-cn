---
title: 使用 SQL 数据仓库构建集成解决方案 | Azure
description: 用于集成 SQL 数据仓库的工具以及提供相应解决方案的合作伙伴。
services: sql-data-warehouse
author: rockboyfor
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
origin.date: 04/13/2018
ms.date: 04/25/2018
ms.author: v-yeche
ms.openlocfilehash: 6103648ffe54852e3afe77902bdb65a757e818b4
ms.sourcegitcommit: 0fedd16f5bb03a02811d6bbe58caa203155fd90e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
---
# <a name="integrate-other-services-with-sql-data-warehouse"></a>使用 SQL 数据仓库集成其他服务
除了本身的核心功能以外，SQL 数据仓库还允许用户集成 Azure 中的其他许多服务。 这些服务包括：

* Azure 流分析
<!-- Not Available Power BI, Azure Data Factory, Azure Machine Learning -->

我们正努力连接多个跨 Azure 生态系统的服务。

<!-- Not Available ## Power BI -->
<!-- Not Available ## Azure Data Factory -->
<!-- Not Available ## Azure Machine Learning -->
##<a name="azure-stream-analytics"></a>Azure 流分析
Azure 流分析是复杂、完全托管的基础结构，用于处理和使用从 Azure 事件中心生成的事件数据。  通过与 SQL 数据仓库集成，可以有效地处理流数据，并将其与关系数据一起存储以实现更深入、更高级的分析。  

* **作业输出**：将流分析作业的输出直接发送到 SQL 数据仓库。

有关详细信息，请参阅[与 Azure 流分析集成](sql-data-warehouse-integrate-azure-stream-analytics.md)。

## <a name="next-steps"></a>后续步骤
要与 Azure SQL 数据库集成，请参阅[配置 SQL 数据库弹性查询](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)

<!--MSDN references-->

<!--Other Web references-->