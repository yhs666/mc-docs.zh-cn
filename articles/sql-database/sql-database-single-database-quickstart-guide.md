---
title: 快速入门 - Azure SQL 数据库中的单一数据库 | Microsoft Docs
description: 了解如何快速开始使用 Azure SQL 数据库中的单一数据库
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: WenJason
ms.author: v-jay
ms.reviewer: carlr
manager: digimobile
origin.date: 02/04/2019
ms.date: 02/25/2019
ms.openlocfilehash: 60b46a20478ff3e13b71f96ee000b2cc5c559a43
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663829"
---
# <a name="getting-started-with-single-databases-in-azure-sql-database"></a>开始使用 Azure SQL 数据库中的单一数据库

[单一数据库](sql-database-single-index.yml)是完全托管的 PaaS 数据库即服务 (DbaaS)，也是新式云原生应用程序的理想存储引擎。 本部分介绍如何在 SQL 数据库中快速配置和创建单一数据库。

## <a name="quickstart-overview"></a>快速入门概述

本部分提供了可帮助你快速开始使用单一数据库的可用文章的概述。 以下快速入门可帮助你快速创建单一数据库、配置数据库服务器防火墙规则，然后使用 `.bacpac` 文件将数据库导入新的单一数据库：

- [使用 Azure 门户创建单一数据库](sql-database-single-database-get-started.md)。
- 创建数据库后，需要[通过配置防火墙规则来保护该数据库](sql-database-server-level-firewall-rule.md)。
- 若要将 SQL Server 上的现有数据库迁移到 Azure，应该安装[数据迁移助手 (DMA)](https://www.microsoft.com/download/details.aspx?id=53595)，用于分析 SQL Server 上的数据库，并找出可能会阻止迁移到单一数据库部署选项的任何问题。 如果找不到任何问题，可将数据库导出为 `.bacpac` 文件，然后[使用 Azure 门户或 SqlPackage 导入](sql-database-import.md)该文件。

## <a name="automating-management-operations"></a>自动化管理操作

可以使用 PowerShell 或 Azure CLI 创建、配置和缩放数据库。

- [使用 PowerShell 创建和配置单一数据库](scripts/sql-database-create-and-configure-database-powershell.md)
- [使用 Azure CLI 创建和配置单一数据库](scripts/sql-database-create-and-configure-database-cli.md)
- [使用 PowerShell 更新单一数据库和缩放资源](scripts/sql-database-monitor-and-scale-database-powershell.md)
- [使用 Azure CLI 更新单一数据库和缩放资源](scripts/sql-database-monitor-and-scale-database-cli.md)

## <a name="next-steps"></a>后续步骤

- 查看 [Azure SQL 数据库支持的功能的概要列表](sql-database-features.md)。
- 了解如何[使数据库变得更安全](sql-database-security-tutorial.md)。
- 在[如何在 Azure SQL 数据库中使用单一数据库](sql-database-howto-single-database.md)中查看更深入的操作指南。
- 查找在 [PowerShell](sql-database-powershell-samples.md) 和 [Azure CLI](sql-database-cli-samples.md) 中编写的其他示例脚本。
- 详细了解可用于配置数据库的[管理 API](sql-database-single-databases-manage.md)。
