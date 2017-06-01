---
redirect_url: https://azure.cn/documentation/articles/documentdb-create-account
ROBOTS: NOINDEX, NOFOLLOW
ms.translationtype: Human Translation
ms.sourcegitcommit: 4a18b6116e37e365e2d4c4e2d144d7588310292e
ms.openlocfilehash: 82d197a922f7b73f2eff7f88ce2f351f8e31eadf
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017



---

# <a name="create-an-azure-documentdb-account-with-mongodb-api"></a>使用 MongoDB API 创建 DocumentDB 帐户
现在可以将 DocumentDB 数据库用作为 MongoDB 编写的应用的数据存储。 若要使用此功能，需要一个 Azure 帐户和一个 DocumentDB 帐户。 本教程指导完成创建与 MongoDB 应用配合使用的 DocumentDB 帐户的过程。 

可使用 Azure 门户或者 Azure CLI（结合 Azure Resource Manager 模板）创建支持 MongoDB 帐户的 DocumentDB。 本文说明如何使用 Azure 门户创建支持 MongoDB 帐户的 DocumentDB。 若要将 Azure CLI 和 Azure Resource Manager 配合使用来创建帐户，请参阅[使用 Azure CLI 2.0 自动执行 DocumentDB 帐户管理](documentdb-automation-resource-manager-cli.md)。

## <a name="prerequisite"></a>先决条件
一个 Azure 帐户。 如果没有 Azure 帐户，请立即创建 [Azure 帐户](/pricing/1rmb-trial/)。
## <a name="create-an-azure-documentdb-account"></a>创建 DocumentDB 帐户

1. 在 Internet 浏览器中，登录 [Azure 门户](https://portal.azure.cn)。
2. 在左侧导航栏中，单击“DocumentDB”。

    ![突出显示 DocumentDB NoSQL 条目的门户左侧导航栏的屏幕截图](./media/documentdb-create-mongodb-account/portalleftnav.png)

3. 或者，单击“更多服务 >”，在顶部搜索栏中键入 **DocumentDB**，然后单击“DocumentDB”。

    ![正在搜索 DocumentDB NoSQL 条目的“更多服务”边栏选项卡的屏幕截图](./media/documentdb-create-mongodb-account/more-services-search.PNG)

4. 在“DocumentDB”边栏选项卡顶部，单击顶部操作栏上的“+ 添加”。

    ![“DocumentDB”资源边栏选项卡上的“添加”按钮的屏幕截图](./media/documentdb-create-mongodb-account/add-documentdb-account.png)

5. 在“DocumentDB 帐户”边栏选项卡中，为帐户指定所需的配置。

   ![具有 MongoDB 协议支持的“新 DocumentDB”边栏选项卡屏幕截图](./media/documentdb-create-mongodb-account/create-documentdb-mongodb-account.PNG)

    - 在“ID”框中，输入一个名称用于标识帐户。  对“ID”进行验证后，“ID”框中会出现一个绿色的复选标记。  该“ID”值将成为 URI 中的主机名。  “ID”只能包含小写字母、数字及“-”字符，且长度必须为 3 到 50 个字符。 请注意，*documents.azure.cn* 会追加到所选择的终结点名称后面，其结果将成为帐户终结点。

    - 对于“API”，请选择“MongoDB”。 此选项可指定想要用于与 DocumentDB 数据库进行交互的通信 API。

    - 对于“订阅”，请选择要用于帐户的 Azure 订阅。 如果帐户只有一个订阅，则默认为选择该帐户。

    - 在“资源组”中，为帐户选择或创建资源组。  默认情况下，会选择 Azure 订阅下的现有资源组。  但是，可以选择创建要将帐户添加到其中的新资源组。 有关详细信息，请参阅 [使用 Azure 门户管理 Azure 资源](../azure-resource-manager/resource-group-portal.md)。

    - 使用“位置”指定要在其中托管帐户的地理位置。

6. 在配置了新的帐户选项后，单击“创建”。  可能需要几分钟来创建帐户。

   可通过通知中心监视进度。  

   ![通知中心的屏幕截图，其中显示正在创建 DocumentDB 帐户](./media/documentdb-create-mongodb-account/create-documentdb-mongodb-deployment-status.png)  

7. 若要访问新帐户，请单击左侧菜单上的“DocumentDB”。 在常规的 DocumentDB 帐户和具有 Mongo 协议支持的 DocumentDB 帐户列表中，单击新帐户的名称。
8. DocumentDB 帐户现在可用于 MongoDB 应用。

   ![默认帐户边栏选项卡的屏幕截图](./media/documentdb-create-mongodb-account/defaultaccountblade.png)

## <a name="next-steps"></a>后续步骤
- 了解如何[连接](documentdb-connect-mongodb-account.md)到具有 MongoDB 协议支持的 DocumentDB 帐户。


