---
title: "编写使用 Azure 服务总线队列的程序 | Azure"
description: "如何编写用于服务总线消息传送的 C# 控制台应用程序"
services: service-bus
documentationCenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
origin.date: 03/23/2017
ms.author: v-yiso
ms.date: 07/17/2017
ms.openlocfilehash: 173a245b7e0b87fb3d3e7fa7161b7788cc7fd765
ms.sourcegitcommit: d5d647d33dba99fabd3a6232d9de0dacb0b57e8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# 服务总线队列入门
<a id="get-started-with-service-bus-queues" class="xliff"></a>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## 将要完成的任务
<a id="what-will-be-accomplished" class="xliff"></a>

在本教程中我们将完成以下任务：

1. 使用 Azure 门户创建服务总线命名空间。

2. 使用 Azure 门户创建服务总线消息传递队列。

3. 编写一个控制台应用程序用于发送消息。

4. 编写一个控制台应用程序用于接收消息。

## 先决条件
<a id="prerequisites" class="xliff"></a>
1. [Visual Studio 2015 或更高版本](http://www.visualstudio.com)。 本教程中的示例使用 Visual Studio 2015。
2. Azure 订阅。

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## 1.使用 Azure 门户创建命名空间
<a id="1-create-a-namespace-using-the-azure-portal" class="xliff"></a>

如果你已创建服务总线命名空间，请跳转到 [使用 Azure 门户创建队列](#2-create-a-queue-using-the-azure-portal) 部分。

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## 2.使用 Azure 门户创建队列
<a id="2-create-a-queue-using-the-azure-portal" class="xliff"></a>

如果你已创建服务总线队列，请跳转到 [将消息发送到队列](#3-send-messages-to-the-queue) 部分。

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## 3.将消息发送到队列
<a id="3-send-messages-to-the-queue" class="xliff"></a>

为了将消息发送到队列中，我们将使用 Visual Studio 编写一个 C# 控制台应用程序。

### 创建控制台应用程序
<a id="create-a-console-application" class="xliff"></a>

- 启动 Visual Studio 并创建新的控制台应用程序。

### 添加服务总线 NuGet 包
<a id="add-the-service-bus-nuget-package" class="xliff"></a>

1. 右键单击新创建的项目，然后选择“管理 NuGet 包” 。

2. 单击“浏览”选项卡，然后搜索“Microsoft Azure 服务总线”，并选择“Microsoft Azure 服务总线”项。 单击“安装”以完成安装，然后关闭此对话框。

    ![选择 NuGet 包][nuget-pkg]

### 编写一些代码以向队列发送消息
<a id="write-some-code-to-send-a-message-to-the-queue" class="xliff"></a>

1. 在 Program.cs 文件的顶部添加以下 using 语句。
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```

2. 将以下代码添加到 `Main` 方法，并将 **connectionString** 变量设置为创建命名空间时获取的连接字符串，以及将 **queueName** 设置为创建队列时使用的队列名称。
   
    ```csharp
    var connectionString = "<Your connection string>";
    var queueName = "<Your queue name>";

    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");
    client.Send(message);
    ```

    Program.cs 文件的内容如下所示。
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;

    namespace GettingStartedWithQueues
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "<Your connection string>";
                var queueName = "<Your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                client.Send(message);
            }
        }
    }
    ```

3. 运行该程序，并检查 Azure 门户。 单击命名空间“概述”  边栏选项卡中的队列名称。 请注意，“活动消息计数”  值现在应为 1。

      ![消息计数][queue-message]

## 4.从队列接收消息
<a id="4-receive-messages-from-the-queue" class="xliff"></a>

1. 创建新的控制台应用程序并添加对服务总线 NuGet 程序包的引用，类似于前面的发送应用程序。

2. 在 Program.cs 文件顶部添加以下 `using` 语句。
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```

3. 将以下代码添加到 `Main` 方法，并将 **connectionString** 变量设置为创建命名空间时获取的连接字符串，以及将 **queueName** 设置为创建队列时使用的队列名称。
   
    ```csharp
    var connectionString = "";
    var queueName = "samplequeue";

    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });

    Console.ReadLine();
    ```

    Program.cs 文件的内容如下所示：
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;

    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "";
          var queueName = "samplequeue";

          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.ReadLine();
        }
      }
    }
    ```

4. 运行该程序，并检查门户。 请注意，“队列长度”  值现在应为 0。

    ![队列长度][queue-message-receive]

祝贺你！ 你已创建队列、发送和接收消息。

## 后续步骤
<a id="next-steps" class="xliff"></a>

查看 [GitHub 存储库](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) 中的示例，了解 Azure 服务总线消息传送的一些更高级的功能。

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples