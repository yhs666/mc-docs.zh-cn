---
title: include 文件
description: include 文件
services: cosmos-db
author: rockboyfor
ms.service: cosmos-db
ms.topic: include
origin.date: 04/13/2018
ms.date: 12/31/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: b60e6ea4ffcd64d06dbf292e9d74845861db7bfd
ms.sourcegitcommit: 54ddd3dc2452d7af3a6fa66dae908ad0c4ef99dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2018
ms.locfileid: "53814781"
---
1. 在新窗口中，登录到 [Azure 门户](https://portal.azure.cn/)。
2. 在左菜单中，依次单击“创建资源”、“数据库”，然后在“Azure Cosmos DB”下单击“创建”。

   ![Azure 门户的屏幕截图，其中突出显示了“更多服务”和“Azure Cosmos DB”](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. 在“创建 Azure Cosmos DB 帐户”页中，输入新 Azure Cosmos DB 帐户的设置。 

    设置|值|Description
    ---|---|---
    订阅|订阅|选择要用于此 Azure Cosmos DB 帐户的 Azure 订阅。 
    资源组|新建<br><br>然后输入在 ID 中提供的同一唯一名称|选择“新建”。 然后输入帐户的新资源组名称。 为简单起见，可以使用与 ID 相同的名称。 
    帐户名|输入唯一的名称|输入标识此 Azure Cosmos DB 帐户的唯一名称。 由于 documents.azure.cn 字符串将追加到所提供的 ID 后面以创建 URI，因此，请使用唯一的 ID。<br><br>该 ID 只能使用小写字母、数字和连字符 (-) 字符。 它的长度必须介于 3 到 31 个字符之间。
    API|MongoDB|API 确定要创建的帐户的类型。 Azure Cosmos DB 提供两种 API：适用于文档数据库的 Core(SQL) 和适用于文档数据库的 MongoDB。 目前，你必须为每种 API 创建单独的帐户。 <br><br>选择“MongoDB”，因为本快速入门将创建使用 MongoDB API 的表。|
    位置|选择离用户最近的区域|选择用于托管 Azure Cosmos DB 帐户的地理位置。 使用离用户最近的位置，使他们能够以最快的速度访问数据。

    选择“查看 + 创建”。 可以跳过“网络”和“标记”部分。 

    ![Azure Cosmos DB 的“新建帐户”页](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png)

4. 创建帐户需要几分钟时间。 等待门户中显示“祝贺你!**使用 MongoDB API 的 Azure Cosmos DB 帐户已准备就绪”** 页。

    ![Azure 门户“通知”窗格](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png)

<!--Update_Description: wording update -->
