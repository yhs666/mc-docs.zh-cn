---
title: "使用 .NET Standard 将事件发送到 Azure 事件中心 | Azure"
description: "在 .NET Standard 中将事件发送到事件中心入门"
services: event-hubs
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 03/27/2017
ms.date: 07/03/2017
ms.author: v-yeche
ms.openlocfilehash: c32305aaa21ee395f46cde517e760222a9168022
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 .NET Standard 将消息发送到 Azure 事件中心入门
<a id="get-started-sending-messages-to-azure-event-hubs-in-net-standard" class="xliff"></a>

> [!NOTE]
> [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) 上提供了此示例。

本教程演示如何编写将一组消息发送到事件中心的 .NET Core 控制台应用程序。 可以按原样运行 [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) 解决方案，将 `EhConnectionString` 和 `EhEntityPath` 字符串替换为事件中心的值。 或者，可以按照本教程中的步骤创建自己的解决方案。

## 先决条件
<a id="prerequisites" class="xliff"></a>

* [Microsoft Visual Studio 2015 或 2017](http://www.visualstudio.com)。 本教程中的示例使用 Visual Studio 2017，但也支持 Visual Studio 2015。
* [.NET Core Visual Studio 2015 或 2017 工具](http://www.microsoft.com/net/core)。
* Azure 订阅。
* 事件中心命名空间。

为了将消息发送到事件中心，我们将使用 Visual Studio 编写 C# 控制台应用程序。

## 创建事件中心命名空间和事件中心
<a id="create-an-event-hubs-namespace-and-an-event-hub" class="xliff"></a>

第一步是使用 [Azure 门户](https://portal.azure.cn)创建事件中心类型的命名空间，并获取应用程序与事件中心进行通信所需的管理凭据。 若要创建命名空间和事件中心，请按照[本文](event-hubs-create.md)中的步骤操作，然后继续执行以下步骤。

## 创建控制台应用程序
<a id="create-a-console-application" class="xliff"></a>

启动 Visual Studio。 在“文件”菜单中，单击“新建”，然后单击“项目”。 创建 .NET Core 控制台应用程序。

![新建项目][1]

## 添加事件中心 NuGet 包
<a id="add-the-event-hubs-nuget-package" class="xliff"></a>

将 [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) NuGet 包添加到项目。

## 编写一些代码以将消息发送到事件中心
<a id="write-some-code-to-send-messages-to-the-event-hub" class="xliff"></a>

1. 在 Program.cs 文件顶部添加以下 `using` 语句。

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. 向 `Program` 类添加常量作为事件中心连接字符串和实体路径（单个事件中心名称）。 将括号中的占位符替换为在创建事件中心时获得的相应值。

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. 将名为 `MainAsync` 的新方法添加到 `Program` 类，如下所示：

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. 将名为 `SendMessagesToEventHub` 的新方法添加到 `Program` 类，如下所示：

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. 在 `Program` 类的 `Main` 方法中添加以下代码。

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   Program.cs 文件的内容如下所示。

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. 运行程序，并确保没有任何错误。

祝贺你！ 现在已向事件中心发送消息。

## 后续步骤
<a id="next-steps" class="xliff"></a>
访问以下链接可以了解有关事件中心的详细信息：

* [从事件中心接收事件](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [事件中心概述](event-hubs-what-is-event-hubs.md)
* [创建事件中心](event-hubs-create.md)
* [事件中心常见问题](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png