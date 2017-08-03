---
title: "设计第一个 Azure SQL 数据库 - C# | Azure"
description: "学习设计第一个 Azure SQL 数据库，并使用 ADO.NET 通过 C# 程序连接到它。"
services: sql-database
documentationcenter: 
author: Hayley244
manager: digimobile
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
origin.date: 06/20/2017
ms.date: 07/31/2017
ms.author: v-haiqya
ms.openlocfilehash: 92da60694e9cdd71b4fb0e5fbf353c505a498246
ms.sourcegitcommit: 2e85ecef03893abe8d3536dc390b187ddf40421f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>设计 Azure SQL 数据库，并使用 C# 和 ADO.NET 进行连接

Azure SQL 数据库是 Microsoft 云（“Azure”）中的关系数据库即服务 (DBaaS)。 本教程介绍如何使用 Azure 门户和 ADO.NET 完成以下操作： 

> [!div class="checklist"]
> * 在 Azure 门户中创建数据库
> * 在 Azure 门户中设置服务器级防火墙规则
> * 使用 ADO.NET 连接到数据库
> * 使用 ADO.NET 创建表
> * 使用 ADO.NET 插入数据 
> * 使用 ADO.NET 查询这些数据

如果没有 Azure 订阅，可以在开始前[创建一个免费帐户](https://www.azure.cn/pricing/1rmb-trial/)。

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]

<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]

## <a name="next-steps"></a>后续步骤

可以使用以下后续步骤：

- [在 C# 程序中使用 LINQ 进行 SQL 查询](https://msdn.microsoft.com/library/bb425822.aspx)
- [将 SQL Server 数据库迁移至 Azure SQL 数据库](sql-database-migrate-your-sql-server-database.md)