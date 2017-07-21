---
title: "在迁移到 SQL 数据库之前使用 SQL Server Management Studio 解决 SQL Server 数据库兼容性问题 | Microsoft Docs"
description: "Azure SQL 数据库, 数据库迁移, 兼容性, SQL Azure 迁移向导"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5f7d3544-b07e-415a-a2ae-96e49bf5d756
ms.service: sql-database
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 11/08/2016
ms.author: v-johch
ms.openlocfilehash: 38129347e2b58e5b1d63e4f030570b97a5eaa1e2
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>在迁移到 SQL 数据库之前，使用 SQL Server Management Studio 解决 SQL Server 数据库的兼容性问题

> [!div class="op_single_selector"]
>- 使用 [SQL Azure 迁移向导](./sql-database-cloud-migrate-fix-compatibility-issues.md)
>- 使用 [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
>- 使用 [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

在迁移到 Azure SQL 数据库之前，高级用户可使用 SQL Server Management Studio 解决 SQL Server 数据库的兼容性问题。

> [!IMPORTANT]
> 建议始终使用最新版本的 Management Studio 以与 Microsoft Azure 和 SQL 数据库的更新保持同步。 [更新 SQL Server Management Studio](https://msdn.microsoft.com/zh-cn/library/mt238290.aspx)。

## <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio
使用 SQL Server Management Studio 通过各种 Transact-SQL 命令（如 **ALTER DATABASE**）来修复兼容性问题。 该方法主要面向能够在实时数据库上轻松使用 Transact-SQL 的高级用户。 如果不是，建议使用 SSDT。 

## <a name="next-steps"></a>后续步骤

- [最新版本的 SSDT](https://msdn.microsoft.com/zh-cn/library/mt204009.aspx)
- [最新版本的 SQL Server Management Studio](https://msdn.microsoft.com/zh-cn/library/mt238290.aspx)
- [将兼容的 SQL Server 数据库迁移到 SQL 数据库](./sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>其他资源

- [SQL 数据库 V12](./sql-database-v12-whats-new.md)
- [Transact-SQL 部分支持或不支持的函数](./sql-database-transact-sql-information.md)
- [使用 SQL Server 迁移助手迁移非 SQL Server 数据库](http://blogs.msdn.com/b/ssma/)