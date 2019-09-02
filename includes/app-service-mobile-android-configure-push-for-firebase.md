1. 在 [Azure 门户](https://portal.azure.cn/)中，单击“浏览全部” > “应用服务”，然后单击移动应用后端。 在“设置”下，单击“应用服务推送”，然后单击通知中心名称。
2. 转到“Google (GCM)”，输入上一步骤中从 Firebase 获取的“服务器密钥”值，然后单击“保存”。

    ![设置门户中的 GCM API 密钥](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

现在，移动应用后端已配置为使用 Firebase Cloud Messaging。 这样便可以使用通知中心向 Android 设备上运行的应用发送推送通知。

<!-- URLs. -->

<!-- images -->