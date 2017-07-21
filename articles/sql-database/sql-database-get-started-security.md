---
title: "SQL 数据库教程：安全性入门"
description: "了解如何创建用户帐户来访问和管理数据库。"
keywords: 
services: sql-database
documentationCenter: 
authors: CarlRabeler
manager: jhubbard
editor: 
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/17/2016
ms.author: v-johch
ms.openlocfilehash: 399c3901c78bd802ac6b273337dbfbaeebf8cf89
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>SQL 数据库教程：创建 SQL 数据库用户帐户用于访问和管理数据库

> [!div class="op_single_selector"]
>- [入门教程](./sql-database-get-started-security.md)
>- [授予访问权限](./sql-database-manage-logins.md)

在本教程中，了解如何使用 SQL Server Management Studio (SSMS) 执行以下操作：

- 使用服务器级主体登录名登录到 SQL 数据库。
- 创建 SQL 数据库用户帐户。
- 向 SQL 数据库用户授予 [db_owner 权限](https://msdn.microsoft.com/zh-cn/library/ms189121.aspx#Anchor_0)。
- 使用非服务器级主体的用户帐户连接到 SQL 数据库。

[!INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

[!INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]

## <a name="next-steps"></a>后续步骤
完成本 SQL 数据库教程、创建用户帐户并为该帐户授予 dbo 权限后，便可详细了解 [SQL 数据库安全](./sql-database-manage-logins.md)。