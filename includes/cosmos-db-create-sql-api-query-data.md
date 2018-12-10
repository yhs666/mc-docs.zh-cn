---
title: include 文件
description: include 文件
services: cosmos-db
author: rockboyfor
ms.service: cosmos-db
ms.topic: include
origin.date: 04/13/2018
ms.date: 06/04/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 0ab6b4c3dce7f6ce2d54e7d210f8096f2f0e85b0
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52657735"
---
现在可以在数据资源管理器中使用查询来检索和筛选数据。

1. 请注意，查询默认设置为 `SELECT * FROM c`。 此默认查询检索并显示集合中的所有文档。 

    ![数据资源管理器中的默认查询是“SELECT * FROM c”](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)

2. 在“文档”选项卡上，单击“编辑筛选器”按钮，将 `ORDER BY c._ts DESC` 添加到查询谓词框中，再单击“应用筛选器”，从而更改查询。

    ![添加“ORDER BY c._ts DESC”并单击“应用筛选器”，更改默认查询](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

此修改后的查询根据文档的时间戳按降序列出文档，所以现在最先列出的是第二个文档。 如果熟悉 SQL 语法，可以在此框中输入任何受支持的 [SQL 查询](../articles/cosmos-db/sql-api-sql-query.md)。 

数据资源管理器中的工作到此结束。 继续处理代码前，请注意，还可以使用数据资源管理器创建存储过程、UDF 和触发器，实现服务器端业务逻辑和缩放吞吐量。 数据资源管理器公开 API 中提供的所有内置编程数据访问，但你可以使用它轻松访问 Azure 门户中的数据。
<!-- Update_Description: update meta properties -->