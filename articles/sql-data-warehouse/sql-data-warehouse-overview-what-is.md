---
title: 什么是 Azure SQL 数据仓库？ | Azure
description: 企业级分布式数据库，可处理 PB 量级的关系数据和非关系数据。 它是行业首个云数据仓库，可以在数秒内增长、收缩和暂停。
services: sql-data-warehouse
documentationcenter: NA
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
origin.date: 10/23/2017
ms.date: 04/25/2018
ms.author: v-yeche
ms.openlocfilehash: 8ef7190212f195572facc3f8a996edfe18d83371
ms.sourcegitcommit: 0fedd16f5bb03a02811d6bbe58caa203155fd90e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
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
对 SQL 数据仓库有了初步的认识后，请学习如何快速创建 SQL 数据仓库和[加载示例数据][load sample data]。 如果用户不熟悉 Azure，可在遇到新术语时查看 [Azure 术语表][Azure glossary] 。 或者，查看一下以下一些其他 SQL 数据仓库资源。  
<!-- Not Available on [create a SQL Data Warehouse][create a SQL Data Warehouse] -->

<!-- Not Available * [Customer success stories]-->
* [博客]
* [功能请求]
<!-- Not Available * [Videos] -->
<!-- Not Available * [Customer Advisory Team blogs]-->
<!-- Not Available * [Create support ticket]-->
* [MSDN 论坛]
<!-- Not Available * [Stack Overflow forum]-->
<!-- Not Available * [Twitter] -->

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
<!-- Not Available [Create support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md-->
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
<!-- Not Available on [create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md -->
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
<!-- Not Available [SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md -->
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
<!--Not Available [Customer success stories]: https://azure.microsoft.com/en-us/case-studies/?service=sql-data-warehouse -->
[博客]: https://www.azure.cn/blog/tags/SQL%20数据库
<!--Not Available [Customer Advisory Team blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/ -->
[功能请求]: https://www.azure.cn/support/support-ticket-form/?l=zh-cn
[MSDN 论坛]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSQLDataWarehouse
<!--Not Available [Stack Overflow forum]: http://stackoverflow.com/questions/tagged/azure-sqldw-->
<!--Not Available on [Twitter]: https://twitter.com/hashtag/SQLDW -->
<!--Not Available on [Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse -->
[SLA for SQL Data Warehouse]: https://www.azure.cn/support/sla/sql-data-warehouse/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://www.azure.cn/support/legal/sla/

<!--Update_Description: update meta properties, wording update, update link -->