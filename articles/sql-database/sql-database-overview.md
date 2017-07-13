---
title: "什么是 Azure SQL 数据库？ | Azure"
description: "本文概述 Azure SQL 数据库。"
services: sql-database
documentationcenter: na
author: Hayley244
manager: digimobile
editor: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
origin.date: 11/28/2016
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: 46d6c727d74014c129302611d9c411b87be041c8
ms.sourcegitcommit: bb82041119027be7a62fc96945d92a8a452e7849
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
# Azure SQL 数据库概述
<a id="azure-sql-database-overview" class="xliff"></a>
本主题概述 Azure SQL 数据库。 有关 Azure SQL 逻辑服务器的信息，请参阅[逻辑服务器](sql-database-server-overview.md)。

## 什么是 Azure SQL 数据库？
<a id="what-is-azure-sql-database" class="xliff"></a>
Azure SQL 数据库中的每个数据库都与逻辑服务器相关联。 数据库可以是：

- 具有其[自己的一组资源](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTU) 的单一数据库
- [共享一组资源](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (eDTU) 的[弹性池](sql-database-elastic-pool.md)的一部分
- [向外扩展的一组共享数据库](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling)的一部分，可以是单一数据库，也可以是入池数据库
- 参与[多租户 SaaS 设计模式](sql-database-design-patterns-multi-tenancy-saas-applications.md)的一组数据库的一部分，该模式中可以包含单一数据库或池中的数据库（或两者） 

## 如何连接 Azure SQL 数据库并向其进行身份验证？
<a id="how-do-i-connect-and-authenticate-to-an-azure-sql-database" class="xliff"></a>

- **身份验证和授权**：Azure SQL 数据库支持 SQL 身份验证和 Azure Active Directory 身份验证（但存在某些限制 - 请参阅[使用 Azure Active Directory 身份验证连接到 SQL 数据库](sql-database-aad-authentication.md)了解身份验证。 可以通过服务器的 master 数据库或直接连接到用户数据库，连接到 Azure SQL 数据库并向其进行身份验证。 有关详细信息，请参阅[在 Azure SQL 数据库中管理数据库和登录名](sql-database-manage-logins.md)。 不支持 Windows 身份验证。 
- **TDS**：Azure SQL 数据库支持表格格式数据流 (TDS) 协议客户端 7.3 版或更高版本。
- **TCP/IP**：只允许 TCP/IP 连接。
- **SQL 数据库防火墙**：为了保护用户数据，在用户指定哪些计算机具有访问权限之前，SQL 数据库防火墙将禁止所有对数据库服务器或其数据库的访问。 请参阅[防火墙](sql-database-firewall-configure.md)

## 支持哪些排序规则？
<a id="what-collations-are-supported" class="xliff"></a>
Azure SQL 数据库使用的默认数据库排序规则是 SQL_LATIN1_GENERAL_CP1_CI_AS。其中，LATIN1_GENERAL 是英语（美国），CP1 是代码页 1252，CI 是不区分大小写，AS 是区分重音。 无法改变 V12 数据库的排序规则。 有关如何设置排序规则的详细信息，请参阅 [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx)。

## 数据库对象有哪些命名要求？
<a id="what-are-the-naming-requirements-for-database-objects" class="xliff"></a>

所有新对象的名称都必须符合 SQL Server 标识符规则。 有关详细信息，请参阅[标识符](https://msdn.microsoft.com/library/ms175874.aspx)。

## Azure SQL 数据库支持哪些功能？
<a id="what-features-are-supported-by-azure-sql-databases" class="xliff"></a>

有关受支持功能的信息，请参阅[功能](sql-database-features.md)。 有关不支持某些类型的功能的原因，另请参阅 [Azure SQL 数据库 Transact-SQL 差异](sql-database-transact-sql-information.md)来了解详细背景。

## 如何管理 Azure SQL 数据库？
<a id="how-do-i-manage-an-azure-sql-database" class="xliff"></a>

可以使用几种方法管理 Azure SQL 数据库逻辑服务器：
- [Azure 门户](sql-database-manage-portal.md)
- [PowerShell](sql-database-manage-powershell.md)
- [Transact-SQL](sql-database-manage-azure-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [REST](https://docs.microsoft.com/rest/api/sql/)

## 后续步骤
<a id="next-steps" class="xliff"></a>

- 有关 Azure SQL 数据库服务的信息，请参阅[什么是 SQL 数据库？](sql-database-technical-overview.md)
- 有关受支持功能的信息，请参阅[功能](sql-database-features.md)
- 有关 Azure SQL 逻辑服务器的概述，请参阅 [SQL 数据库逻辑服务器概述](sql-database-server-overview.md)
- 有关 Transact-SQL 支持和差异的信息，请参阅 [Azure SQL 数据库 Transact-SQL 差异](sql-database-transact-sql-information.md)。
- 按 **服务层**了解具体的资源配额和限制。 有关服务层的概述，请参阅 [SQL 数据库服务层](sql-database-service-tiers.md)。
- 有关与安全性相关的指导原则，请参阅 [Azure SQL 数据库安全指导原则和限制](sql-database-security-guidelines.md)。
- 有关驱动程序可用性和 SQL 数据库支持的信息，请参阅[用于 SQL 数据库和 SQL Server 的连接库](sql-database-libraries.md)。