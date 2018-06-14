---
title: 使用 Azure IoT 中心路由消息 (Java)
description: 如何使用路由规则和自定义终结点将消息发送到其他后端服务，从而处理 Azure IoT 中心的设备到云消息。
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 06/29/2017
ms.author: v-yiso
ms.date: 06/11/2018
ms.openlocfilehash: c651be794478e3968e91b5c6c78d409f8fc1e7ae
ms.sourcegitcommit: 6f42cd6478fde788b795b851033981a586a6db24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2018
ms.locfileid: "34695067"
---
# <a name="routing-messages-with-iot-hub-java"></a>使用 IoT 中心路由消息 (Java)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Azure IoT 中心是一项完全托管的服务，可在数百万个设备和一个解决方案后端之间实现安全可靠的双向通信。 其他教程（[IoT 中心入门]和[使用 IoT 中心发送“云到设备”消息][lnk-c2d]）介绍了如何使用 IoT 中心的“设备到云”和“云到设备”的基本消息传递功能。

本教程以 [IoT 中心入门]教程中所示的代码为基础，说明如何按可缩放的方式通过消息路由处理设备到云的消息。 本教程描述了如何处理需要解决方案后端立即执行操作的消息。 例如，设备可能会发送一条警报消息，触发在 CRM 系统中插入票证。 与此相反，数据点消息仅送入分析引擎。 例如，设备中存储便于日后分析的温度遥测是数据点消息。

在本教程最后，会运行 3 个 Java 控制台应用：

* **simulated-device**（ [IoT 中心入门] 教程中创建的应用的修改版本）每秒发送一次设备到云的数据点消息，每 10 秒发送一次设备到云的交互式消息。 此应用使用 AMQP 协议实现与 IoT 中心的通信。
* **read-d2c-messages** 显示设备应用发送的遥测数据。
* **read-critical-queue** 从附加到 IoT 中心的服务总线队列中取消关键消息的排队。

> [!NOTE]
> IoT 中心对许多设备平台和语言（包括 C、Java 和 JavaScript）提供 SDK 支持。 若要了解如何将本教程中的设备替换为物理设备，以及如何将设备连接到 IoT 中心，请参阅 [Azure IoT 开发人员中心]。
> 
> 

要完成本教程，需要以下各项：

* [IoT 中心入门] 教程的完整工作版本。
* 最新的 [Java SE 开发工具包 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
+ 有效的 Azure 帐户。 <br/>如果没有帐户，可以创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)，只需几分钟即可完成。

我们还建议阅读 [Azure 存储]和 [Azure 服务总线]。

## <a name="send-interactive-messages-from-a-device-app"></a>从设备应用发送交互式消息
在本部分中，会修改在 [IoT 中心入门]教程中创建的设备应用，不定期发送需要立即处理的消息。

1. 使用文本编辑器打开 simulated-device\src\main\java\com\mycompany\app\App.java 文件。 本文件包含用于 [IoT 中心入门] 教程中创建的 **simulated-device** 应用的代码。
2. 使用以下代码替换 **MessageSender** 类：

    ```java
    private static class MessageSender implements Runnable {

        public void run()  {
            try {
                double minTemperature = 20;
                double minHumidity = 60;
                Random rand = new Random();

                while (true) {
                    String msgStr;
                    Message msg;
                    if (new Random().nextDouble() > 0.7) {
                        if (new Random().nextDouble() > 0.5) {
                            msgStr = "This is a critical message.";
                            msg = new Message(msgStr);
                            msg.setProperty("level", "critical");
                        } else {
                            msgStr = "This is a storage message.";
                            msg = new Message(msgStr);
                            msg.setProperty("level", "storage");
                        }
                    } else {
                        double currentTemperature = minTemperature + rand.nextDouble() * 15;
                        double currentHumidity = minHumidity + rand.nextDouble() * 20; 
                        TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
                        telemetryDataPoint.deviceId = deviceId;
                        telemetryDataPoint.temperature = currentTemperature;
                        telemetryDataPoint.humidity = currentHumidity;

                        msgStr = telemetryDataPoint.serialize();
                        msg = new Message(msgStr);
                    }

                    System.out.println("Sending: " + msgStr);

                    Object lockobj = new Object();
                    EventCallback callback = new EventCallback();
                    client.sendEventAsync(msg, callback, lockobj);

                    synchronized (lockobj) {
                        lockobj.wait();
                    }
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                System.out.println("Finished.");
            }
        }
    }
    ```
   
    此方法会将 `"level": "critical"` 和 `"level": "storage"` 属性随机添加到设备发送的消息，以模拟需要应用程序后端立即执行操作的消息或需要永久存储的消息。 应用程序支持基于消息正文的路由消息。
   
   > [!NOTE]
   > 可使用消息属性根据各种方案路由消息，包括冷路径处理和此处所示的热路径示例。
   > 
   > 

2. 保存并关闭 simulated-device\src\main\java\com\mycompany\app\App.java 文件。

    > [!NOTE]
    > 强烈建议按 MSDN 文章 [Transient Fault Handling]（暂时性故障处理）中所述实施指数退让等重试策略。

3. 若要使用 Maven 构建 **simulated-device** 应用，请在 simulated-device 文件夹的命令提示符处执行以下命令：

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-to-your-iot-hub-and-route-messages-to-it"></a>向 IoT 中心添加一个队列并向其路由消息

本部分创建一个服务总线队列并将其连接到 IoT 中心，还会配置 IoT 中心，根据消息上的现有属性发送消息到队列。 若要深入了解如何处理来自服务总线队列的消息，请参阅 [队列入门][lnk-sb-queues-java]教程。

1. 按[队列入门][lnk-sb-queues-java]中所述，创建服务总线队列。 记下命名空间和队列名称。

    > [!NOTE]
    > 用作 IoT 中心终结点的服务总线队列和主题不能启用“会话”或“重复项检测”。 如果启用了其中任一选项，该终结点将在 Azure 门户中显示为“无法访问”。

2. 在 Azure 门户中，打开 IoT 中心并单击“终结点” 。

    ![IoT 中心的终结点][30]

3. 在“终结点”边栏选项卡中，单击顶部的“添加”，将队列添加到 IoT 中心。 将终结点命名为“CriticalQueue”，并使用下拉列表选择“服务总线队列”、队列所在的服务总线命名空间和队列名称。 完成后，单击底部的“**保存**”。

    ![添加终结点][31]

4. 现在单击 IoT 中心的“路由”  。 单击边栏选项卡顶部的“添加” ，创建将消息路由到刚添加的队列的路由规则。 选择“DeviceTelemetry”  作为数据源。 输入 `level="critical"` 作为条件，并选择刚添加为自定义终结点的队列作为路由规则终结点。  。

    ![添加路由][32]

    请确保回退路由设为“开”。 此设置是 IoT 中心的默认配置。

    ![回退路由][33]

## <a name="optional-read-from-the-queue-endpoint"></a>（可选）从队列终结点读取
可按照[队列入门][lnk-sb-queues-java]中的说明，选择性地从队列终结点读取消息。 将应用命名为 **read-critical-queue**。

## <a name="run-the-applications"></a>运行应用程序
现在即可运行 3 个应用程序。

1. 若要运行 **read-d2c-messages** 应用程序，请在命令提示符或外壳处导航到 read-d2c 文件夹并执行以下命令：

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![运行 read-d2c-messages][readd2c]

2. 若要运行 **read-critical-queue** 应用程序，请在命令提示符或外壳处导航到 read-critical-queue 文件夹并执行以下命令：

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![运行 read-critical-messages][readqueue]

3. 若要运行 **simulated-device** 应用，请在命令提示符或 shell 处导航到 simulated-device 文件夹并执行以下命令：

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![运行 simulated-device][simulateddevice]

## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>（可选）向 IoT 中心添加存储容器并向其路由消息

本部分创建一个存储帐户并将其连接到 IoT 中心，还会配置 IoT 中心，根据消息上的现有属性发送消息到帐户。 有关如何管理存储的详细信息，请参阅 [Azure 存储入门][Azure 存储]。

 > [!NOTE]
   > 如果并不局限于一个**终结点**，则除了 **CriticalQueue** 以外，还可以设置 **StorageContainer**，并同时运行两者。

1. 根据 [Azure 存储文档][lnk-storage] 中所述创建存储帐户。 记下帐户名称。

2. 在 Azure 门户中，打开 IoT 中心并单击“终结点” 。

3. 在“终结点”边栏选项卡中，选择“CriticalQueue”终结点，然后单击“删除”。 单击“是”，然后单击“添加”。 将终结点命名为 **StorageContainer**，从下拉列表中选择“Azure 存储容器”，然后创建**存储帐户**和**存储容器**。  记下名称。  完成后，单击底部的“确定”。 

 > [!NOTE]
   > 如果可以使用不止一个**终结点**，则不需删除 **CriticalQueue**。

4. 单击 IoT 中心的“路由”。 单击边栏选项卡顶部的“添加” ，创建将消息路由到刚添加的队列的路由规则。 选择“设备消息”作为数据源。 输入 `level="storage"` 作为条件，并选择自定义终结点 **StorageContainer** 作为路由规则终结点。 单击底部的“保存”。  

    请确保回退路由设为“开”。 此设置是 IoT 中心的默认配置。

1. 确保以前的应用程序仍在运行。 

1. 在 Azure 门户中，转到自己的存储帐户，在“Blob 服务”下面单击“浏览 Blob...”。选择容器，导航到 JSON 文件并单击该文件，然后单击“下载”查看数据。

## <a name="next-steps"></a>后续步骤
在本教程中，介绍了如何使用 IoT 中心的消息路由功能可靠地分派设备到云的消息。

通过[如何使用 IoT 中心发送云到设备的消息][lnk-c2d] ，了解如何将消息从解决方案后端发送到设备。

若要查看使用 IoT 中心完成端到端解决方案的示例，请参阅 [Azure IoT 远程监视解决方案加速器][lnk-suite]。

若要了解有关使用 IoT 中心开发解决方案的详细信息，请参阅 [IoT 中心开发人员指南]。

若要详细了解 IoT 中心的消息路由，请参阅[使用 IoT 中心发送和接收消息][lnk-devguide-messaging]。

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-java-java-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-java-java-process-d2c/route-creation.png
[33]: ./media/iot-hub-java-java-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-java]: ../service-bus-messaging/service-bus-java-how-to-use-queues.md

[Azure 存储]: /storage/
[Azure 服务总线]: /service-bus-messaging/

[IoT 中心开发人员指南]: ./iot-hub-devguide.md
[lnk-devguide-messaging]: ./iot-hub-devguide-messaging.md
[IoT 中心入门]: ./iot-hub-java-java-getstarted.md
[Azure IoT 开发人员中心]: https://www.azure.cn/develop/iot
[Transient Fault Handling]: https://msdn.microsoft.com/zh-cn/library/hh675232.aspx

<!-- Links -->
[Transient Fault Handling]: https://msdn.microsoft.com/zh-cn/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: ./iot-hub-java-java-c2d.md
[lnk-suite]: /iot-suite/
[lnk-free-trial]: https://www.azure.cn/pricing/1rmb-free/
<!--Update_Description:update wording and link references-->