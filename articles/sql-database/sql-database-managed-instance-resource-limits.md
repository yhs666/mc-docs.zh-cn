---
title: Azure SQL 数据库资源限制 - 托管实例 | Microsoft Docs
description: 本文概述托管实例的 Azure SQL 数据库资源限制。
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab, jovanpop, sachinp, sstein
origin.date: 06/26/2019
ms.date: 09/09/2019
ms.openlocfilehash: 7dbfa45cdabf60eddae44314b56b454b552dd3ac
ms.sourcegitcommit: 2610641d9fccebfa3ebfffa913027ac3afa7742b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2019
ms.locfileid: "70372970"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Azure SQL 数据库托管实例资源限制概述

本文概述 Azure SQL 数据库托管实例的资源限制，并介绍如何请求提高这些限制。

> [!NOTE]
> 有关支持的功能和 T-SQL 语句的差异，请参阅[功能差异](sql-database-features.md)和 [T-SQL 语句支持](sql-database-managed-instance-transact-sql-information.md)。

## <a name="instance-level-resource-limits"></a>实例级资源限制

托管实例的某些特征和资源限制取决于底层基础结构和体系结构。 这些限制取决于硬件代系和服务层级。

### <a name="hardware-generation-characteristics"></a>硬件代次特征

Azure SQL 数据库托管实例可部署在两个硬件代次上：Gen4 和 Gen5。 硬件代次具有不同的特征，如下表所述：

|   | **Gen4** | **Gen5** |
| --- | --- | --- |
| 硬件 | Intel E5-2673 v3 (Haswell) 2.4-GHz 处理器、附加的 SSD vCore = 1 PP（物理核心） | Intel E5-2673 v4 (Broadwell) 2.3-GHz 处理器、快速 NVMe SSD、vCore=1 LP（超线程） |
| vCore 数目 | 8、16、24 个 vCore | 4、8、16、24、32、40、64、80 个 vCore |
| 最大内存（内存/核心比） | 每个 vCore 7 GB<br/>添加更多 vCore 以获得更多内存。 | 每个 vCore 5.1 GB<br/>添加更多 vCore 以获得更多内存。 |
| 最大内存中 OLTP 内存 | 实例限制：每个 vCore 3 GB<br/>数据库限制：<br/> - 8 核：每个数据库 8 GB<br/> - 16 核：每个数据库 20 GB<br/> - 24 核：每个数据库 36 GB | 实例限制：每个 vCore 2.5 GB<br/>数据库限制：<br/> - 8 核：每个数据库 13 GB<br/> - 16 核：每个数据库 32 GB |
| 最大实例预留存储 |  常规用途：8 TB<br/>业务关键：1TB | 常规用途：8 TB<br/> 业务关键型 1 TB、2 TB 或 4 TB，具体取决于核心数 |

### <a name="service-tier-characteristics"></a>服务层特征

托管实例有两个服务层级：“常规用途”和“业务关键”。 这些层提供不同的功能，如下表中所述：

| **功能** | **常规用途** | **业务关键** |
| --- | --- | --- |
| vCore 数目\* | 第 4 代：8、16、24<br/>Gen5：4、8、16、24、32、40、64、80 | Gen4：8、16、24、32 <br/> Gen5：4、8、16、24、32、40、64、80 |
| 最大内存 | Gen4：56 GB - 168 GB (7GB/vCore)<br/>Gen5：40.8 GB - 408 GB (5.1GB/vCore)<br/>添加更多 vCore 以获得更多内存。 | Gen4：56 GB - 168 GB (7GB/vCore)<br/>Gen5：40.8 GB - 408 GB (5.1GB/vCore)<br/>添加更多 vCore 以获得更多内存。 |
| 最大实例预留存储大小 | - 2 TB，适用于 4 个 vCore（仅限 Gen5）<br/>- 8 TB，适用于其他大小 | Gen4：1 TB <br/> Gen5： <br/>- 1 TB，适用于 4、8、16 个 vCore<br/>- 2 TB（适用于 24 个 vCore）<br/>- 4 TB（适用于 32、40、64、80 个 vCore） |
| 最大数据库大小 | 由每个实例的最大存储大小决定 | 由每个实例的最大存储大小决定 |
| 每个实例的数据库数目上限 | 100 | 100 |
| 每个实例的数据库文件数目上限 | 最多 280 个 | 每个数据库 32,767 个文件 |
| 数据/日志 IOPS（近似值） | 500 - 7,500（每个文件）<br/>\*[增大文件大小以获取更多 IOPS](/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)| 11 K - 110 K (1375/vCore)<br/>添加更多 vCore 以获得更好的 IO 性能。 |
| 日志写入吞吐量限制 | 3 MB/s（每个 vCore）<br/>最大为 22 MB/秒（每个实例） | 4 MB/秒（每个 vCore）<br/>最大为 48 MB/秒（每个实例）|
| 数据吞吐量（近似值） | 100 - 250 MB/s（每个文件）<br/>\*[增大文件大小以获得更好的 IO 性能](/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes) | 不适用 |
| 存储 IO 延迟（近似） | 5-10 毫秒 | 1-2 毫秒 |
| 最大 tempDB 大小 | 192 - 1,920 GB（每个 vCore 为 24 GB）<br/>添加更多 vCore 以获得更多 TempDB 空间。 | 受最大实例存储大小限制。 TempDB 日志文件大小目前的限制为 24GB/vCore。 |
| 最大会话数 | 30000 | 30000 |

> [!NOTE]
> - 与最大存储大小限制进行比较的实例存储大小同时包括用户数据库和系统数据库中的数据和日志文件大小。 可以使用 <a href="https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql">sys.master_files</a> 系统视图来确定数据库使用的空间总量。 错误日志不会持久保存，不包括在大小中。 备份不包括在存储大小中。
> - 吞吐量和 IOPS 还取决于不受托管实例显式限制的页面大小。

## <a name="supported-regions"></a>支持的区域

托管实例只能在[支持的区域](https://azure.microsoft.com/global-infrastructure/services/?regions=china-non-regional,china-east,china-east-2,china-north,china-north-2&products=all)中创建。 若要在当前不支持的区域中创建托管实例，可以[通过 Azure 门户发送支持请求](#obtaining-a-larger-quota-for-sql-managed-instance)。

## <a name="supported-subscription-types"></a>支持的订阅类型

目前，托管实例仅支持以下订阅类型中的部署：

- [提前支付](https://docs.azure.cn/billing/billing-sign-up-azure-account-and-get-a-pia-subscription)

## <a name="regional-resource-limitations"></a>区域资源限制

支持的订阅类型可以包含每个区域的有限数量的资源。 托管实例根据订阅类型对每个 Azure 区域实施两种默认限制：

- **子网限制**：在单一区域中部署托管实例的子网数上限。
- **vCore 限制**：可跨单一区域的所有实例部署的 vCore 数上限。

> [!Note]
> 这些限制是默认设置，不是技术限制。 如果在当前区域中需要更多托管实例，可以[在 Azure 门户中创建特殊支持请求](#obtaining-a-larger-quota-for-sql-managed-instance)，以根据需要提高限制。 或者，可以在另一个 Azure 区域中创建新的托管实例，而不需要发送支持请求。

下表显示了受支持订阅的默认区域限制：

|订阅类型| 托管实例子网数目上限 | vCore 单元数目上限* |
| :---| :--- | :--- |
|提前支付|3|320|

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>获取更大的 SQL 托管实例配额

如果在当前区域中需要更多托管实例，请使用 Azure 门户发送扩展配额的支持请求。
若要启动获取更大配额的过程，请执行以下操作：

1. 打开“帮助 + 支持”，单击“新建支持请求”   。

   ![帮助和支持](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. 在新支持请求的“基本信息”选项卡上：
   - 对于“问题类型”，选择“服务和订阅限制(配额)”   。
   - 对于“订阅”，请选择自己的订阅。 
   - 对于“配额类型”，选择“SQL 数据库托管实例”   。
   - 对于“支持计划”，选择自己的支持计划  。

     ![问题类型配额](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. 单击“下一步”  。
4. 在新支持请求的“问题”选项卡上： 
   - 对于“严重性”，选择问题的严重性级别  。
   - 对于“详细信息”，提供有关问题的其他信息，包括错误消息  。
   - 对于“文件上传”，附加包含详细信息的文件（最多 4 MB）  。

     ![问题详细信息](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > 有效的请求应包括：
     > - 需要提高订阅限制的区域。
     > - 每个服务层级在配额增加后在现有的子网中所需的 vCore 数目（如果需要扩展任何现有的子网）。
     > - 所需的新子网数目，以及每个服务层级在新子网内的 vCore 总数（如果需要在新子网中部署托管实例）。

5. 单击“下一步”  。
6. 在新支持请求的“联系人信息”选项卡上，输入首选联系方式（电子邮件或电话）和联系人详细信息。
7. 单击**创建**。

## <a name="next-steps"></a>后续步骤

- 有关托管实例的详细信息，请参阅[什么是托管实例？](sql-database-managed-instance.md)。
- 有关定价信息，请参阅 [SQL 数据库托管实例定价](https://azure.cn/pricing/details/sql-database)。
- 若要了解如何创建第一个托管实例，请参阅[快速入门指南](sql-database-managed-instance-get-started.md)。
