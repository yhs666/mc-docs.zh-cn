预配 MongoLab 数据库时，MongoLab 会采用 MongoDB 标准连接字符串格式向 Azure 发送一个连接 URI。 该值用于通过所选 MongoDB 驱动程序建立 MongoDB 连接。 有关连接字符串的详细信息，请参阅 mongodb.org 上的 [Connections](http://www.mongodb.org/display/DOCS/Connections)（连接）。

**此 URI 包含数据库用户名和密码。将其视为敏感信息，不要共享。**

可通过下列步骤在 Azure 门户中检索该 URI：

1. 选择“外接程序”。  
![AddonsButton][button-addons]
1. 在外接程序列表中找到 MongoLab 服务。  
![MongolabEntry][entry-mongolabaddon]
1. 单击外接程序的名称，转至外接程序页面。
1. 单击“连接信息”。  
![ConnectionInfoButton][button-connectioninfo]  
MongoLab URI 显示：  
![ConnectionInfoScreen][screen-connectioninfo]  
1.  单击 MONGOLAB_URI 值右侧的剪贴板按钮，将完整值复制到剪贴板。

[entry-mongolabaddon]: ./media/howto-get-connectioninfo-mongolab/entry-mongolabaddon.png
[button-connectioninfo]: ./media/howto-get-connectioninfo-mongolab/button-connectioninfo.png
[screen-connectioninfo]: ./media/howto-get-connectioninfo-mongolab/dialog-mongolab_connectioninfo.png
[button-addons]: ./media/howto-get-connectioninfo-mongolab/button-addons.png