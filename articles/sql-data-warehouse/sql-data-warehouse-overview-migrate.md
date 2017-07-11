---
title: "将解决方案迁移到 SQL 数据仓库 | Azure"
description: "有关将解决方案转移到 Azure SQL 数据仓库平台的迁移指南。"
services: sql-data-warehouse
documentationcenter: NA
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 198365eb-7451-4222-b99c-d1d9ef687f1b
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
origin.date: 10/31/2016
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: 4c8195323a60609cd4258bf268158a1f650ff0c0
ms.sourcegitcommit: 3727b139aef04c55efcccfa6a724978491b225a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2017
---
<a id="migrate-your-solution-to-sql-data-warehouse" class="xliff"></a>

# 将解决方案迁移到 SQL 数据仓库
SQL 数据仓库是一种分布式数据库系统，可根据你的需要弹性缩放。 为了保持性能与缩放性，SQL 数据仓库中未实现 SQL Server 的所有功能。 以下迁移主题大致介绍了将解决方案迁移到 SQL 数据仓库时考虑的一些重要因素。 由于针对缩放性设计数据仓库引入了不同的设计模式，因此传统的方法不一定最合适。 因此，用户可能会发现，调整现有解决方案可确保充分利用 SQL 数据仓库所提供的分布式平台。

此外必须记住，SQL 数据仓库是基于 Azure 的平台。 因此，迁移过程很有可能需将数据传输到云中。 数据传输本身是一个话题，应予以谨慎考虑，尤其是随着卷增大时。 数据传输和数据加载是不同的主题。

<a id="migration-guidance" class="xliff"></a>

## 迁移指南
在开始迁移之前，请务必通读这些文章，确保了解一些产品差异和基本概念。

* [迁移架构][Migrate your schema]
* [迁移数据][Migrate your data]
* [迁移代码][Migrate your code]

<a id="next-steps" class="xliff"></a>

## 后续步骤
CAT（客户顾问团队）也有一些通过博客发布的很棒的 SQL 数据仓库指南。  有关迁移的其他指南，请参阅他们的 [在实践中将数据迁移到 Azure SQL 数据仓库][Migrating data to Azure SQL Data Warehouse in practice] 一文。

<!--Image references-->

<!--Article references-->
[Migrate your schema]: sql-data-warehouse-migrate-schema.md
[Migrate your data]: sql-data-warehouse-migrate-data.md
[Migrate your code]: sql-data-warehouse-migrate-code.md

<!--MSDN references-->

<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/