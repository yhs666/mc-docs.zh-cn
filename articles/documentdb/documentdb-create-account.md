---
title: "如何创建 DocumentDB 帐户 | Microsoft Docs"
description: "使用 DocumentDB 构建 NoSQL 数据库。 根据这些说明来创建 DocumentDB 帐户，并开始构建运行速度飞快且可全局缩放的 NoSQL 数据库。"
keywords: "构建数据库"
services: documentdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: monicar
ms.assetid: 0e7f8488-7bb7-463e-b6fd-3ae91a02c03a
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/17/2017
wacn.date: 
ms.author: v-junlch
redirect_url: https://aka.ms/acdbnetqa
ROBOTS: NOINDEX, NOFOLLOW
ms.translationtype: Human Translation
ms.sourcegitcommit: 4a18b6116e37e365e2d4c4e2d144d7588310292e
ms.openlocfilehash: 8b7cbe372f34f259dd1c9c91970a0d0211466426
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017


---
# <a name="how-to-create-an-azure-documentdb-nosql-account-using-the-azure-portal"></a>如何使用 Azure 门户创建 DocumentDB NoSQL 帐户
> [!div class="op_single_selector"]
> * [Azure 门户](documentdb-create-account.md)
> * [Azure CLI 1.0](documentdb-automation-resource-manager-cli-nodejs.md)
> * [Azure CLI 2.0](documentdb-automation-resource-manager-cli.md)
> * [Azure PowerShell](documentdb-manage-account-with-powershell.md)

若要使用 DocumentDB 构建数据库，必须：

- 有一个 Azure 帐户。 如果没有 Azure 帐户，可以获取 [Azure 帐户](https://www.azure.cn/pricing/1rmb-trial/)。
- 创建 DocumentDB 帐户。  

可以使用 Azure 门户、Azure Resource Manager 模板或 Azure 命令行界面 (CLI) 来创建 DocumentDB 帐户。 本文说明了如何使用 Azure 门户创建 DocumentDB 帐户。 若要使用 Azure Resource Manager 或 Azure CLI 创建帐户，请参阅[自动创建 DocumentDB 数据库帐户](documentdb-automation-resource-manager-cli.md)。

1. 登录到 [Azure 门户](https://portal.azure.cn/)。
2. 在左侧导航栏中，依次单击“新建”、“数据库”、“DocumentDB”。

   ![Azure 门户的屏幕截图，其中突出显示了“更多服务”和 NoSQL (DocumentDB)](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-1.png)  
3. 在“新建帐户”边栏选项卡中，为 DocumentDB 帐户指定所需的配置。

    ![“新建 DocumentDB”边栏选项卡的屏幕截图](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-2.png)

   - 在“ID”框中输入一个名称，用于标识 DocumentDB 帐户。  对“ID”进行验证后，“ID”框中会出现一个绿色的复选标记。  该“ID”值将成为 URI 中的主机名。  “ID”只能包含小写字母、数字及“-”字符，且长度必须为 3 到 50 个字符。 请注意，*documents.azure.cn* 会追加到所选终结点名称后面，其结果将成为 DocumentDB 帐户终结点。
   - 在 **NoSQL API** 框中，选择要使用的编程模型：

     - **DocumentDB**：DocumentDB API 可通过 .NET、Java、Node.js、Python 和 JavaScript [SDK](documentdb-sdk-dotnet.md)，以及 HTTP [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) 使用，对所有 DocumentDB 功能提供编程访问权限。
     - **MongoDB**：DocumentDB 还为 **MongoDB** API 提供[协议级别支持](documentdb-protocol-mongodb.md)。 选择 MongoDB API 选项时，可以使用现有的 MongoDB SDK 和 [工具](documentdb-mongodb-mongochef.md) 来和 DocumentDB 对话。 [无需更改代码](documentdb-import-data.md)即可将现有的 MongoDB 应用[转为](documentdb-connect-mongodb-account.md)使用 DocumentDB，并利用完全托管的数据库作为一种服务，且具有无限缩放、全局复制等功能。
   - 对于“订阅”，请选择要用于 DocumentDB 帐户的 Azure 订阅。 如果帐户只有一个订阅，则默认为选择该帐户。
   - 在“资源组”中，为 DocumentDB 帐户选择或创建资源组。  默认创建新的资源组。 有关详细信息，请参阅 [使用 Azure 门户管理 Azure 资源](../azure-resource-manager/resource-group-portal.md)。
   - 使用“位置”指定在其中托管 DocumentDB 帐户的地理位置。
4. 在配置新的 DocumentDB 帐户后，单击“创建”。 若要检查部署状态，请查看“通知中心”。  

   ![快速创建数据库 - 通知中心的屏幕截图，其中显示正在创建 DocumentDB 帐户](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-4.png)  

   ![通知中心的屏幕截图，其中显示 DocumentDB 帐户已成功创建并且部署到资源组 - 在线数据库创建者通知](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-5.png)
5. 在创建 DocumentDB 帐户后，可随时将其与默认设置配合使用。 DocumentDB 帐户的默认一致性设置为“会话”。  可单击资源菜单中的“默认一致性”  调整默认一致性。 若要了解有关 DocumentDB 提供的一致性级别的详细信息，请参阅 [DocumentDB 中的一致性级别](documentdb-consistency-levels.md)。

   ![“资源组”边栏选项卡的屏幕截图 — 开始应用程序开发](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-6.png)  

   ![“一致性级别”边栏选项卡的屏幕截图 — 会话一致性](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-7.png)  

[How to: Create a DocumentDB account]: #Howto
[Next steps]: #NextSteps


## <a name="next-steps"></a>后续步骤
有了 DocumentDB 帐户后，接下来的步骤是创建 DocumentDB 集合和数据库。

可以使用以下其中一项来创建新的集合与数据库：

- Azure 门户，如[使用 Azure 门户创建 DocumentDB 集合](documentdb-create-collection.md)中所述。
- 包含示例数据的综合教程：[.NET](documentdb-get-started.md)、[.NET MVC](documentdb-dotnet-application.md)、[Java](documentdb-java-application.md)、[Node.js](documentdb-nodejs-application.md) 或 [Python](documentdb-python-application.md)。
- 可在 GitHub 中找到 [.NET](documentdb-dotnet-samples.md#database-examples)、[Node.js](documentdb-nodejs-samples.md#database-examples) 或 [Python](documentdb-python-samples.md#database-examples) 示例代码。
- [.NET](documentdb-sdk-dotnet.md)、[.NET Core](documentdb-sdk-dotnet-core.md)、[Node.js](documentdb-sdk-node.md)、[Java](documentdb-sdk-java.md)、[Python](documentdb-sdk-python.md) 和 [REST](https://msdn.microsoft.com/library/azure/mt489072.aspx) SDK。

创建数据库和集合后，需要向集合[添加文档](documentdb-view-json-document-explorer.md)。

在集合中添加文档后，可以使用 [DocumentDB SQL](documentdb-sql-query.md) 对这些文档[执行查询](documentdb-sql-query.md#ExecutingSqlQueries)。 可以在门户、[REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) 或某个 [SDK](documentdb-sdk-dotnet.md) 中，使用[查询资源管理器](documentdb-query-collections-query-explorer.md)执行查询。

### <a name="learn-more"></a>了解详细信息
若要了解有关 DocumentDB 的详细信息，请参阅以下资源：

- [DocumentDB 简介](./documentdb-resources.md)


