1. 在新浏览器窗口中，登录到 [Azure 门户](https://portal.azure.cn/)。
2. 单击“新建” > “数据库” > “Azure Cosmos DB”。

   ![Azure 门户“数据库”窗格](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. 在“新建帐户”页上，输入新 Azure Cosmos DB 帐户的设置。 

    设置|建议的值|说明
    ---|---|---
    ID|*输入唯一名称*|输入标识此 Azure Cosmos DB 帐户的唯一名称。 由于 documents.azure.cn 将追加到所提供的 ID 后面以创建 URI，因此，请使用唯一但可识别的 ID。<br><br>ID 只能包含小写字母、数字和连字符 (-) 字符，并且必须包含 3 到 50 个字符。
    API|SQL|API 确定要创建的帐户的类型。 Azure Cosmos DB 提供了三种 API，用于满足应用程序的需求：SQL（文档数据库）、MongoDB（文档数据库）和 Azure 表，每个目前都需要单独的帐户。 <br><br>之所以选择 **SQL** 是因为，在本快速入门中将创建可使用 SQL 语法查询并可通过 SQL API 访问的文档数据库。<br><br>[详细了解 SQL API](../articles/cosmos-db/documentdb-introduction.md)|
<!-- Not Available on Gremlin (graph database) and Cassandra -->
    Subscription|*Your subscription*|Select Azure subscription that you want to use for this Azure Cosmos DB account. 
    Resource Group|Create new<br><br>*Then enter the same unique name as provided above in ID*|Select **Create New**, then enter a new resource-group name for your account. For simplicity, you can use the same name as your ID. 
    Location|*Select the region closest to your users*|Select geographic location in which to host your Azure Cosmos DB account. Use the location that's closest to your users to give them the fastest access to the data.
    Enable geo-redundancy| Leave blank | This creates a replicated version of your database in a second (paired) region. Leave this blank.  
    Pin to dashboard | Select | Select this box so that your new database account is added to your portal dashboard for easy access.

    Then click **Create**.

    ![The new account blade for Azure Cosmos DB](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

4. 创建帐户需要几分钟时间。 在创建帐户过程中，门户会在右侧显示“部署 Azure Cosmos DB”磁贴，可能需要向右滚动仪表板才能看到此磁贴。 另外，还会在屏幕顶部附近显示进度条。 你可以查看任一区域来了解进度。 

    ![Azure 门户“通知”窗格](./media/cosmos-db-create-dbaccount/deploying-cosmos-db.png)

    创建帐户后，会显示“祝贺你!已创建 Azure Cosmos DB 帐户”页。
<!--Update_Description: wording update-->
<!--ms.date: 12/25/2017-->
