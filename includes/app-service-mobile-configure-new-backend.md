
1. 单击**应用程序服务**按钮，选择你的移动应用后端，选择**快速入门**，然后选择你的客户端平台 (iOS、 Android、 Xamarin、 Cordova)。

    ![包含突出显示的移动应用程序快速入门的 azure 门户][quickstart]

2. 如果未配置数据库连接，创建一个，方法是执行以下操作：

    ![使用移动应用连接到数据库的 azure 门户][connect]

    a. 创建新的 SQL 数据库和服务器。

    ![与移动应用程序的 azure 门户创建新的数据库和服务器][server]

    b. 等到成功创建数据连接。

    ![成功创建数据连接的 azure 门户通知][notification]

    c. 数据连接必须成功。

    ![Azure 门户的通知，"你已有的数据连接"][already-connection]

3. 在“2.创建表 API”下，为“后端语言”选择“Node.js”。 
 
4. 接受该确认，然后选择**创建 TodoItem 表**。  
    此操作数据库中创建新的待办事项 item 表。 

    >[!IMPORTANT]
    > 切换到 Node.js 的一个现有的后端将覆盖所有内容。 若要改为创建的.NET 后端，请参阅[对于移动应用程序使用.NET 后端服务器 SDK][instructions]。

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
