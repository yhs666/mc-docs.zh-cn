---
title: include 文件
description: include 文件
services: cosmos-db
author: rockboyfor
ms.service: cosmos-db
ms.topic: include
origin.date: 04/13/2018
ms.date: 03/18/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 28f60f67093bf0b2d320f02f37f0643f7e98ca07
ms.sourcegitcommit: 66e360fe2577c9b7ddd96ff78e0ede36c3593b99
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2019
ms.locfileid: "57988653"
---
<!--Verify sucessfully-->
现在可以在 Azure 门户中使用数据资源管理器工具来创建数据库和表。 

1. 依次单击“数据资源管理器” > “新建表”。 

    此时，最右侧将显示“添加表”区域，可能需要向右滚动才能看到。

    ![Azure 门户中的数据资源管理器](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

2. 在“添加表”页上，输入新表的设置。

    设置|建议的值|说明
    ---|---|---
    表 ID|sample-table|新表的 ID。 表名称与数据库 ID 的字符要求相同。 `/ \ # ?` 或尾随空格。
    存储容量| 固定 (10 GB)|使用默认值“固定 (10 GB)”。 此值是数据库的存储容量。
    吞吐量|400 RU|将吞吐量更改为每秒 400 个请求单位 (RU/s)。 如果想要减少延迟，以后可以增加吞吐量。

    单击 **“确定”**。

    数据资源管理器将显示新的数据库和表。

    ![显示新的数据库和集合的 Azure 门户数据资源管理器](./media/cosmos-db-create-table/azure-cosmos-db-new-table.png)

<!--Verify sucessfully-->
<!--Update_Description: new articles on  -->
<!--ms.date: 03/18/2019-->