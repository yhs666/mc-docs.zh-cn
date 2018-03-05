1. 在新窗口中，登录到 [Azure 门户](https://portal.azure.cn/)。
2. 在左菜单中，依次单击“创建资源”、“数据库”，然后在“Azure Cosmos DB”下单击“创建”。

   ![Azure 门户的屏幕截图，其中突出显示了“更多服务”和“Azure Cosmos DB”](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. 在“新建帐户”窗格中，为 Azure Cosmos DB 帐户指定所需的配置。 

    使用 Azure Cosmos DB，可以选择以下三个编程模型之一：MongoDB、SQL 和表（键/值）。 
    <!-- Not Available on Gremlin (graph database) and Cassandra -->

    在本快速入门教程中，我们会针对 MongoDB API 编程，因此，在填写表单时，请选择“MongoDB”。 但如果有来自目录应用的文档数据或键/值（表）数据，请意识到 Azure Cosmos DB 可以为所有任务关键型应用程序提供高度可用的多区域分布式数据库服务平台。
    <!-- NOTICE: global-distributed TO multiple region-distributed -->
    <!-- Not Available on Graph data for a social media app-->

    以表中的信息作为指南，填写“新建帐户”窗格。

    ![“新 Azure Cosmos DB”窗格的屏幕截图](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-2.png)

    设置|建议的值|说明
    ---|---|---
    ID|*唯一值*|选择用于标识 Azure Cosmos DB 帐户的唯一名称。 *documents.azure.cn* 会追加到所提供的 ID 以创建 URI，因此，请使用唯一但可识别的 ID。 ID 只能包含小写字母、数字及“-”字符，且长度必须为 3 到 50 个字符。
    API|MongoDB|API 确定要创建的帐户的类型。 Azure Cosmos DB 提供了三种 API，用于满足应用程序的需求：SQL（文档数据库）、MongoDB（文档数据库）和 Azure 表，每个目前都需要单独的帐户。 <br><br>之所以选择 **MongoDB** 是因为，在本快速入门中将创建可使用 MongoDB 查询的文档数据库。<br><br>[详细了解 MongoDB API](../articles/cosmos-db/mongodb-introduction.md)|
<!-- Not Available on  Gremlin (graph database) and Cassandra -->
    Subscription|*Your subscription*|The Azure subscription that you want to use for the Azure Cosmos DB account. 
    Resource Group|*The same value as ID*|The new resource group name for your account. For simplicity, you can use the same name as your ID. 
    Location|*The region closest to your users*|The geographic location in which to host your Azure Cosmos DB account. Choose the location closest to your users to give them the fastest access to the data.

4. 单击“创建”  以创建帐户。
5. 在工具栏上，单击“通知”可监视部署过程。

    ![“部署已启动”通知](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-nosql-notification.png)

6.  部署完成后，请从“所有资源”磁贴打开新帐户。 

    ![“所有资源”磁贴中的 Azure Cosmos DB 帐户](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-all-resources.png)
<!--Update_Description: wording update -->
<!--ms.date: 03/05/2018-->