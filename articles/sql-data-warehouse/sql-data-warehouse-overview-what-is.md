---
title: 什么是 Azure SQL 数据仓库？ | Microsoft Docs
description: 企业级分布式数据库，可处理 PB 量级的关系数据和非关系数据。 它是行业首个云数据仓库，可以在数秒内增长、收缩和暂停。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: overview
ms.component: design
origin.date: 04/17/2018
ms.date: 11/12/2018
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: bc49e374b4f86ec77055b3797dc917787186cd7c
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52672737"
---
# <a name="what-is-azure-sql-data-warehouse"></a>什么是 Azure SQL 数据仓库？

SQL 数据仓库是基于云的企业数据仓库 (EDW)，可利用大规模并行处理 (MPP) 对数 PB 的数据快速运行复杂的查询。 将 SQL 数据仓库用作大数据解决方案的关键组件。 使用简单的 PolyBase T-SQL 查询将大数据导入 SQL 数据仓库，然后利用 MPP 运行高性能分析。 进行集成和分析时，数据仓库是企业获取见解能够依赖的唯一事实来源。  


## <a name="key-component-of-big-data-solution"></a>大数据解决方案的关键组件
SQL 数据仓库是云中端到端大数据解决方案的关键组件。

![数据仓库解决方案](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

在云数据解决方案中，可从各种源将数据引入大数据存储中。 将数据置于大数据存储中以后，Hadoop、Spark 和机器学习算法就可以准备和训练数据。 当数据准备就绪，可以进行复杂的分析时，SQL 数据仓库就会使用 PolyBase 来查询大数据存储。 PolyBase 使用标准 T-SQL 查询将数据引入 SQL 数据仓库中。
 
SQL 数据仓库通过列存储将数据存储到关系表中。 此格式可显著降低数据存储费用，改进查询性能。 将数据存储到 SQL 数据仓库以后，即可大规模地运行分析。 与传统数据库系统相比，数分钟的分析查询只需数秒即可完成，数天的查询只需数小时。 

分析结果可以传输到世界各地的报告数据库或应用程序。 然后即可通过业务分析获得进行明智的业务决策所需的见解。


## <a name="next-steps"></a>后续步骤
对 SQL 数据仓库有了初步的认识后，请继续了解如何快速[创建 SQL 数据仓库][创建 SQL 数据仓库]和[加载示例数据][加载示例数据]。 如果不熟悉 Azure，遇到新术语时，[Azure 词汇表][Azure 词汇表]可以提供帮助。 或者，查看一下以下一些其他 SQL 数据仓库资源。  

<!-- Not Available * [Customer success stories]-->
* [博客]
* [功能请求] <!-- Not Available * [Videos] -->
<!-- Not Available * [Customer Advisory Team blogs]-->
<!-- Not Available * [Create support ticket]-->
* [MSDN 论坛] <!-- Not Available * [Stack Overflow forum]-->
<!-- Not Available * [Twitter] -->

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
<!-- Not Available [Create support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md--> [加载示例数据]: ./sql-data-warehouse-load-sample-databases.md [创建 SQL 数据仓库]: ./sql-data-warehouse-get-started-provision.md [迁移文档]: ./sql-data-warehouse-overview-migrate.md <!-- Not Available [SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md -->
[集成工具概述]: ./sql-data-warehouse-overview-integrate.md [备份和还原概述]: ./sql-data-warehouse-restore-database-overview.md [Azure 词汇表]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
<!--Not Available [Customer success stories]: https://azure.microsoft.com/en-us/case-studies/?service=sql-data-warehouse --> [博客]: https://www.azure.cn/blog/tags/SQL%20数据库 <!--Not Available [Customer Advisory Team blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/ -->
[MSDN 论坛]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSQLDataWarehouse <!--Not Available [Stack Overflow forum]: http://stackoverflow.com/questions/tagged/azure-sqldw-->
<!--Not Available on [Twitter]: https://twitter.com/hashtag/SQLDW -->
<!--Not Available on [Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse -->
[SQL 数据仓库的 SLA]: https://www.azure.cn/support/sla/sql-data-warehouse/ [批量许可]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37 [服务级别协议]: https://www.azure.cn/support/legal/sla/

<!--Update_Description: update meta properties, wording update, update link -->