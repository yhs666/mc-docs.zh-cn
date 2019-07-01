---
title: 什么是 Azure SQL 数据仓库？ | Microsoft Docs
description: 企业级分布式数据库，可处理 PB 量级的关系数据和非关系数据。 它是行业首个云数据仓库，可以在数秒内增长、收缩和暂停。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: design
origin.date: 05/30/2018
ms.date: 06/24/2019
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 0c11e02e7a3a3f16c64db5b7b21612d4400a433f
ms.sourcegitcommit: 4d78c9881b553cd8feecb5555efe0de708545a63
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "67151767"
---
# <a name="what-is-azure-sql-data-warehouse"></a>什么是 Azure SQL 数据仓库？

SQL 数据仓库是基于云的企业数据仓库 (EDW)，可利用大规模并行处理 (MPP) 对数 PB 的数据快速运行复杂的查询。 将 SQL 数据仓库用作大数据解决方案的关键组件。 使用简单的 [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017&viewFallbackFrom=azure-sqldw-latest) T-SQL 查询将大数据导入 SQL 数据仓库，然后利用 MPP 运行高性能分析。 进行集成和分析时，数据仓库是企业获取见解能够依赖的唯一事实来源。  

## <a name="key-component-of-big-data-solution"></a>大数据解决方案的关键组件

SQL 数据仓库是云中端到端大数据解决方案的关键组件。

![数据仓库解决方案](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

在云数据解决方案中，可从各种源将数据引入大数据存储中。 将数据置于大数据存储中以后，Hadoop、Spark 和机器学习算法就可以准备和训练数据。 当数据准备就绪，可以进行复杂的分析时，SQL 数据仓库就会使用 PolyBase 来查询大数据存储。 PolyBase 使用标准 T-SQL 查询将数据引入 SQL 数据仓库中。
 
SQL 数据仓库通过列存储将数据存储到关系表中。 此格式可显著降低数据存储费用，改进查询性能。 将数据存储到 SQL 数据仓库以后，即可大规模地运行分析。 与传统数据库系统相比，数分钟的分析查询只需数秒即可完成，数天的查询只需数小时。 

分析结果可以传输到世界各地的报告数据库或应用程序。 然后即可通过业务分析获得进行明智的业务决策所需的见解。

## <a name="next-steps"></a>后续步骤

- 探索 [Azure SQL 数据仓库体系结构](/sql-data-warehouse/massively-parallel-processing-mpp-architecture)
- 快速[创建 SQL 数据仓库][create a SQL Data Warehouse]
- [加载示例数据][load sample data]。


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[SLA for SQL Data Warehouse]: https://www.azure.cn/support/sla/sql-data-warehouse/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://www.azure.cn/support/legal/sla/

<!--Update_Description: update meta properties, wording update, update link -->