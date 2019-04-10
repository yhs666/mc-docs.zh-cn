---
title: SQL 数据库的 Azure 资源管理器模板 | Microsoft Docs
description: 使用 Azure 资源管理器模板创建和配置 Azure SQL 数据库。
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: overview-samples
ms.devlang: ''
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
origin.date: 02/04/2019
ms.date: 04/08/2019
ms.openlocfilehash: 02a68da4138183261ef4256c51e4bfe064add13b
ms.sourcegitcommit: 0777b062c70f5b4b613044804706af5a8f00ee5d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2019
ms.locfileid: "59003444"
---
# <a name="azure-resource-manager-templates-for-azure-sql-database"></a>Azure SQL 数据库的 Azure 资源管理器模板

使用 Azure 资源管理器模板可将基础结构定义为代码，并将解决方案部署到 Azure China Cloud。

## <a name="single-database--elastic-pool"></a>单一数据库和弹性池

下表包含 Azure SQL 数据库的 Azure 资源管理器模板链接。

| |  |
|---|---|
| [单一数据库](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-database-transparent-encryption-create) | 此 Azure 资源管理器模板创建包含逻辑服务器的单一 Azure SQL 数据库并配置防火墙规则。 |
| [逻辑服务器](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server) | 此 Azure 资源管理器模板创建 Azure SQL 数据库的逻辑服务器。 |
| [弹性池](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-elastic-pool-create) | 使用此模板可以部署新的弹性池，并在其中分配新的关联 SQL Server 和新的 SQL 数据库。 |
| [故障转移组](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-with-failover-group) | 此模板创建两个 Azure SQL 逻辑服务器、一个 SQL 数据库和一个故障转移组。|
| [高级威胁防护](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-advanced-threat-protection-server-policy) | 使用此模板可以部署一个启用了高级威胁防护的 Azure SQL 逻辑服务器和可选的 Azure SQL 数据库。 SQL 高级威胁防护是高级 SQL 安全功能的统一程序包。|
| [威胁检测](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-threat-detection-db-policy-multiple-databases) | 使用此模板可以部署启用了威胁检测的 Azure SQL 逻辑服务器和一组 Azure SQL 数据库。对于每个数据库，需要提供用于接收警报的电子邮件地址。 威胁检测是 SQL 高级威胁防护 (ATP) 产品/服务的一部分，它提供一个安全层来应对 SQL 服务器和数据库受到的潜在威胁。|
| [在 Azure Blob 存储中审核](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-blob-storage) | 使用此模板可以部署启用了审核的 Azure SQL 逻辑服务器，以将审核日志写入 Blob 存储。 Azure SQL 数据库审核会跟踪数据库事件，并将这些事件写入到可放入 Azure 存储帐户的审核日志。|
| [使用 SQL 数据库的 Azure Web 应用](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database) | 此示例在“基本”服务级别创建免费的 Azure Web 应用和 SQL 数据库。|
| [使用 SQL 数据库的 Azure Web 应用和 Redis 缓存](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) | 此模板在同一资源组中创建 Web 应用、Redis 缓存和 SQL 数据库，并在适用于 SQL 数据库和 Redis 缓存的 Web 应用中创建两个连接字符串。|
| [使用 SQL 数据库的 HDInsight 群集](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-with-sql-database) | 使用此模板可以创建 HDInsight 群集、SQL 数据库服务器、SQL 数据库和两个表。 [将 Sqoop 与 HDInsight 中的 Hadoop 配合使用](/hdinsight/hadoop/hdinsight-use-sqoop)一文中使用了此模板 |
| [按计划运行 SQL 存储过程的 Azure 逻辑应用](https://github.com/Azure/azure-quickstart-templates/tree/master/101-logic-app-sql-proc) | 使用此模板可以创建一个按计划运行 SQL 存储过程的逻辑应用。 可将该过程的所有参数放入该模板的正文部分。|

