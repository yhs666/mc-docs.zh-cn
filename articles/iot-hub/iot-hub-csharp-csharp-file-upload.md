---
title: 使用 .NET 将文件从设备上传到 Azure IoT 中心 | Microsoft Docs
description: 如何使用用于 .NET 的 Azure IoT 设备 SDK 从设备将文件上传到云中。 上传的文件存储在 Azure 存储 Blob 容器中。
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: ''
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 07/04/2017
ms.date: 07/09/2018
ms.author: v-yiso
ms.openlocfilehash: 63c256ccf18d16aad67cbc8dcbe6e5af6b4128a6
ms.sourcegitcommit: 00c8a6a07e6b98a2b6f2f0e8ca4090853bb34b14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2018
ms.locfileid: "38938880"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-using-net"></a>通过 .NET 使用 IoT 中心将文件从设备上传到云

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

本教程的内容基于[使用 IoT 中心发送云到设备的消息](./iot-hub-csharp-csharp-c2d.md)教程中所述的代码，演示如何使用 IoT 中心的文件上载功能。 其中了说明了如何：

- 安全地为设备提供用于上传文件的 Azure Blob URI。
- 使用 IoT 中心文件上传通知在应用后端中触发对文件的处理。

[IoT 中心入门](iot-hub-csharp-csharp-getstarted.md)和[使用 IoT 中心发送云到设备的消息](iot-hub-csharp-csharp-c2d.md)教程介绍了 IoT 中心提供的基本的设备到云和云到设备的消息传送功能。 [处理设备到云的消息](tutorial-routing.md)教程介绍了一种在 Azure Blob 存储中可靠存储设备到云消息的方法。 但是，在某些情况下，无法轻松地将设备发送的数据映射为 IoT 中心接受的相对较小的设备到云消息。 例如：

* 包含图像的大型文件
* 视频
* 以高频率采样的振动数据
* 某种形式的预处理数据

通常使用 [Hadoop](/hdinsight/) 堆栈等工具在云中批处理这些文件。 需要从设备上传文件时，仍可以使用 IoT 中心的安全性和可靠性。

在本教程结束时，会运行 2 个 .NET 控制台应用：

* **SimulatedDevice**， [使用 IoT 中心发送云到设备消息](./iot-hub-csharp-csharp-c2d.md) 教程中创建的应用的修改版本。 该应用使用 IoT 中心提供的 SAS URI 将文件上传到存储。
* **ReadFileUploadNotification**，它可以接收来自 IoT 中心的文件上传通知。

> [!NOTE]
> IoT 中心通过 Azure IoT 设备 SDK 来支持许多设备平台和语言（包括 C、Java 和 Javascript）。 有关如何将设备连接到 Azure IoT 中心的分步说明，请参阅 [Azure IoT 开发人员中心]。
> 
> 

要完成本教程，需要以下各项：

* Visual Studio 2015 或 Visual Studio 2017
* 有效的 Azure 帐户。 如果没有帐户，可以创建一个[试用帐户][lnk-free-trial]，只需几分钟即可完成。

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>从设备应用上传文件

在本部分中，会修改在[使用 IoT 中心发送云到设备消息](./iot-hub-csharp-csharp-c2d.md)中创建的设备应用，以接收来自 IoT 中心的云到设备消息。

1. 在 Visual Studio 中，右键单击 **SimulatedDevice** 项目，单击“**添加**”，并单击“**现有项**”。 导航到某个图像文件并将它包含在项目中。 本教程假设图像名称为 `image.jpg`。
2. 右键单击该图像，并单击“**属性**”。 确保“**复制到输出目录**”设置为“**始终复制**”。

    ![][1]
3. 在 **Program.cs** 文件的顶部添加以下语句：

    ```csharp
    using System.IO;
    ```
4. 将以下方法添加到 **Program** 类：

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    `UploadToBlobAsync` 方法获取要上传的文件的文件名与流源，并处理上传到存储的任务。 控制台应用会显示上传文件所需的时间。
5. 在 **Main** 方法中的 `Console.ReadLine()` 行前面添加以下方法：

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> 为简单起见，本教程不实现任何重试策略。 在生产代码中，应按 MSDN 文章 [Transient Fault Handling]（暂时性故障处理）中所述实施重试策略（例如指数性的回退）。
> 
> 

## <a name="receive-a-file-upload-notification"></a>接收文件上传通知
在本部分中，会编写一个 .NET 控制台应用，用于接收来自 IoT 中心的文件上传通知消息。

1. 在当前的 Visual Studio 解决方案中，通过使用“**控制台应用程序**”项目模板创建 Visual C# Windows 项目。 将项目命名为 **ReadFileUploadNotification**。

    ![Visual Studio 中的新项目][2]
2. 在“解决方案资源管理器”中，右键单击 **ReadFileUploadNotification** 项目，并单击“管理 NuGet 包...”。

3. 在“NuGet 包管理器”窗口中，搜索“Microsoft.Azure.Devices”，并单击“安装”并接受使用条款。 

    此操作会下载、安装 [Azure IoT 服务 SDK NuGet 包]，并在 **ReadFileUploadNotification** 项目中添加对它的引用。

4. 在 **Program.cs** 文件的顶部添加以下语句：

    ```csharp
    using Microsoft.Azure.Devices;
    ```
5. 将以下字段添加到 **Program** 类。 将占位符值替换为在 [IoT 中心入门] 中获取的 IoT 中心连接字符串：

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```
6. 将以下方法添加到 **Program** 类：

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    请注意，此接收模式与用于从设备应用接收云到设备消息的模式相同。
7. 最后，在 **Main** 方法中添加以下行：

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>运行应用程序
现在，已准备就绪，可以运行应用程序了。

1. 在 Visual Studio 中，右键单击解决方案并选择“**设置启动项目**”。 选择“**多个启动项目**”，并针对 **ReadFileUploadNotification** 和 **SimulatedDevice** 应用选择“**启动**”操作。
2. 按 **F5**。 这两个应用程序应该都会启动。 你将在其中一个控制台应用中看到上传已完成，同时还会看到另一个控制台应用收到的上传通知消息。 可使用 [Azure 门户]或 Visual Studio 服务器资源管理器检查 Azure 存储帐户中是否存在上传的文件。

   ![][50]

## <a name="next-steps"></a>后续步骤
在本教程中，已学习了如何使用 IoT 中心的文件上传功能来简化从设备进行的文件上传。 可以使用以下文章继续探索 IoT 中心功能和方案：

* [以编程方式创建 IoT 中心][lnk-create-hub]
* [C SDK 简介][lnk-c-sdk]
* [Azure IoT SDK][lnk-sdks]

若要进一步探索 IoT 中心的功能，请参阅：

* [使用 Azure IoT Edge 将 AI 部署到边缘设备][lnk-iotedge]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png

<!-- Links -->

[Azure 门户]: https://portal.azure.cn/

[Azure IoT 开发人员中心]: https://www.azure.cn/develop/iot

[Transient Fault Handling]: https://msdn.microsoft.com/zh-cn/library/hh680901(v=pandp.50).aspx
[Azure IoT 服务 SDK NuGet 包]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: https://www.azure.cn/pricing/1rmb-trial/

[lnk-create-hub]: ./iot-hub-rm-template-powershell.md
[lnk-c-sdk]: ./iot-hub-device-sdk-c-intro.md
[lnk-sdks]: ./iot-hub-devguide-sdks.md

[lnk-iotedge]: ./iot-hub-windows-iot-edge-simulated-device.md


<!--Update_Description: update wording and code-->