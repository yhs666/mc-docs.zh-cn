可以在 Azure 应用商店订阅由 Azure 托管且全权管理的 MongoDB 数据库。 为此，请执行以下步骤：

1. 登录到 Azure 管理门户。
1. 单击“新建” 。  
![新建][button-new]
1. 选择“应用商店”。  
![应用商店][button-store]
1. 选择“MongoLab”。 可以在“应用服务”类别以及“全部”下找到该选项。  
![MongoLab][entry-mongolab]
1. 单击“下一步”。  
![下一步][button-next]  
  随即显示 MongoLab 应用商店条目。  
![NewMongoLab][screen-newmongolab]
1. 选择所需的“订阅”选项。
1. 输入数据库的“名称”。 名称只能包含字母数字字符、短划线、句点和下划线。 MongoLab 还要求该名称必须唯一，因此可能要求用户在名称已被使用时重新提交请求。
1. 选择所需的“区域”。
1. 单击“下一步”。  
![下一步][button-next]
1. 查看应用商店购买信息，然后单击“购买”确认。  
![下一步][button-purchase]  
1. 工具栏进度按钮提供预配状态。  
![ProgressButton][button-progress]  
预配完成时会显示一条成功消息。  
![SuccessMessage][message-success]

祝贺你！ MongoLab 已在所选 Azure 区域预配 MongoDB 数据库。 现在，用户可以访问我们的管理 UI，享受全天候支持服务。

[button-new]: ./media/howto-provision-mongolab/button-new.png
[button-store]: ./media/howto-provision-mongolab/button-store.png
[button-next]: ./media/howto-provision-mongolab/button-next.png
[button-purchase]: ./media/howto-provision-mongolab/button-purchase.png
[button-progress]: ./media/howto-provision-mongolab/button-progress.png
[entry-mongolab]: ./media/howto-provision-mongolab/entry-mongolab.png 
[screen-newmongolab]: ./media/howto-provision-mongolab/screen-newmongolab.png 
[message-success]: ./media/howto-provision-mongolab/message-provisionsuccess.png