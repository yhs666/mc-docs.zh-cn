---
title: "什么是 Azure SQL 数据仓库？ | Azure"
description: "企业级分布式数据库，可处理 PB 量级的关系数据和非关系数据。 它是行业首个云数据仓库，可以在数秒内增长、收缩和暂停。"
services: sql-data-warehouse
documentationcenter: NA
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
origin.date: 10/23/2017
ms.date: 01/15/2018
ms.author: v-yeche
ms.openlocfilehash: 07e19ab2a25b5faddd103ba6e08b58d0ce87ff4e
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="what-is-azure-sql-data-warehouse"></a>什么是 Azure SQL 数据仓库？

SQL 数据仓库是基于云的企业数据仓库 (EDW)，可利用大规模并行处理 (MPP) 对数 PB 的数据快速运行复杂的查询。 将 SQL 数据仓库用作大数据解决方案的关键组件。 使用简单的 PolyBase T-SQL 查询将大数据导入 SQL 数据仓库，然后利用 MPP 运行高性能分析。 进行集成和分析时，数据仓库是企业获取见解能够依赖的唯一事实来源。  

## <a name="key-component-of-big-data-solution"></a>大数据解决方案的关键组件
SQL 数据仓库是云中端到端大数据解决方案的关键组件。

![数据仓库解决方案](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

在云数据解决方案中，可从各种源将数据引入大数据存储中。 将数据置于大数据存储中以后，Hadoop、Spark 和机器学习算法就可以准备和训练数据。 当数据准备就绪，可以进行复杂的分析时，SQL 数据仓库就会使用 PolyBase 来查询大数据存储。 PolyBase 使用标准 T-SQL 查询将数据引入 SQL 数据仓库中。

SQL 数据仓库通过列存储将数据存储到关系表中。 此格式可显著降低数据存储费用，改进查询性能。 将数据存储到 SQL 数据仓库以后，即可大规模地运行分析。 与传统数据库系统相比，数分钟的分析查询只需数秒即可完成，数天的查询只需数小时。 

分析结果可以传输到世界各地的报告数据库或应用程序。 然后即可通过业务分析获得进行明智的业务决策所需的见解。

## <a name="optimization-choices"></a>优化选项

SQL 数据仓库提供[性能层](performance-tiers.md)，方便用户根据数据需求（不管大小）进行灵活的设置。 可以选择经过弹性优化或计算优化的数据仓库。 

- 弹性优化性能层在体系结构中将计算层和存储层分开。 此选项非常适合可以根据短期的高峰活动频繁进行缩放，从而充分利用独立的计算层和存储层的工作负荷。 此计算层的价格门槛最低，可以根据大多数客户工作负荷的需求进行缩放。

- **“计算优化”性能层**使用最新的 Azure 硬件，引入了新的 NVMe 固态磁盘缓存，让访问频率最高的数据靠近 CPU，这正是你所需要的。 此性能层可以对存储自动分层，尤其适合复杂的查询，因为所有 IO 对计算层来说都是本地的。 另外，列存储进行了增强，以便在 SQL 数据仓库中存储无限数量的数据。 计算优化性能层提供最高级别的可伸缩性，允许扩展到 30,000 个计算数据仓库单位 (cDWU)。 对于需要进行持续且速度极快的操作的工作负荷，请选择此层。

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