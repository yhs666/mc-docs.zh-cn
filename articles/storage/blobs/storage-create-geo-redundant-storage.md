---
title: "使应用程序数据在 Azure 中高度可用 | Microsoft Docs"
description: "利用读取访问异地冗余存储，使应用程序数据高度可用"
services: storage
documentationcenter: 
author: yunan2016
manager: digimobile
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
origin.date: 11/15/2017
ms.date: 01/01/2018
ms.author: v-nany
ms.custom: mvc
ms.openlocfilehash: 1004e2cc5d3e8e1ab12e740355df91a51e3a284b
ms.sourcegitcommit: 0b0d3b61e91a97277de8eda8d7a8e114b7c4d8c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>使应用程序数据在 Azure 存储中高度可用

本教程是一个系列中的第一部分。 本教程介绍如何使应用程序数据在 Azure 中高度可用。 完成后，会有一个 .NET Core 控制台应用程序，用于将 Blob 上传到[读取访问异地冗余](../common/storage-redundancy.md#read-access-geo-redundant-storage) (RA-GRS) 存储帐户并对其进行检索。 RA-GRS 的工作方式是将事务从主要区域复制到次要区域。 此复制过程可确保次要区域中的数据最终一致。 应用程序使用[断路器](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker)模式来确定要连接到的终结点。 对故障进行模拟时，应用程序切换到辅助终结点。

在该系列的第一部分中，你将学习如何：

> [!div class="checklist"]
> * 创建存储帐户
> * 下载示例
> * 设置连接字符串
> * 运行控制台应用程序

## <a name="prerequisites"></a>先决条件

完成本教程：

* 使用以下工作负荷安装 [Visual Studio 2017](https://www.visualstudio.com/downloads/)：
  - **Azure 开发**

  ![Azure 开发（位于“Web 和云”下）](media/storage-create-geo-redundant-storage/workloads.png)

* 下载并安装 [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>登录到 Azure 门户

登录到 [Azure 门户](https://portal.azure.cn/)。

## <a name="create-a-storage-account"></a>创建存储帐户

存储帐户提供唯一的命名空间来存储和访问 Azure 存储数据对象。

按以下步骤创建读取访问异地冗余存储帐户：

1. 选择 Azure 门户左上角的“新建”按钮。

2. 在“新建”页中选择“存储”，然后在“特别推荐”下选择“存储帐户 - blob、文件、表、队列”。
3. 使用以下信息填充存储帐户窗体（如下图所示），然后选择“创建”：

   | 设置       | 建议的值 | 说明 |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **名称** | mystorageaccount | 存储帐户的唯一值 |
   | 部署模型 | Resource Manager  | 资源管理器包含最新功能。|
   | 帐户类型 | 常规用途 | 有关帐户类型的详细信息，请参阅[存储帐户的类型](../common/storage-introduction.md#types-of-storage-accounts) |
   | **性能** | 标准 | 对于示例方案，“标准”已足够。 |
   | **复制**| 读取访问异地冗余存储 (RA-GRS) | 这是运行示例所必需的。 |
   |需要安全传输 | 已禁用| 此方案不需要安全传输。 |
   |**订阅** | 你的订阅 |有关订阅的详细信息，请参阅[订阅](https://account.windowsazure.cn/Subscriptions)。 |
   |**ResourceGroup** | MyResourceGroup |如需有效的资源组名称，请参阅 [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)（命名规则和限制）。 |
   |**位置** | 中国东部 | 选择一个位置。 |

![创建存储帐户](media/storage-create-geo-redundant-storage/figure1.png)

## <a name="download-the-sample"></a>下载示例

[下载示例项目](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip)。

提取（解压缩）storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip 文件。
示例项目包含一个控制台应用程序。

## <a name="set-the-connection-string"></a>设置连接字符串

在应用程序中，必须为存储帐户提供连接字符串。 建议将此连接字符串存储在运行应用程序的本地计算机上的环境变量中。 根据操作系统按照下面的某个示例的做法创建环境变量。

在 Azure 门户中导航到存储帐户。 在存储帐户中选择“设置”下的“访问密钥”。 复制主密钥或辅助密钥中的**连接字符串**。 将 \<yourconnectionstring\> 替换为实际的连接字符串，只需根据操作系统运行以下命令之一即可。 此命令将一个环境变量保存到本地计算机。 在 Windows 中，此环境变量不可用，除非重新加载正使用的**命令提示符**或 shell。 替换以下示例中的 **\<storageConnectionString\>**：

### <a name="linux"></a>Linux

```bash
export storageconnectionstring=<yourconnectionstring>
```

### <a name="windows"></a>Windows

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

![应用配置文件](media/storage-create-geo-redundant-storage/figure2.png)

## <a name="run-the-console-application"></a>运行控制台应用程序

在 Visual Studio 中，按 F5 或选择“开始”即可开始调试应用程序。 Visual studio 自动还原缺失的 NuGet 包（如果已配置）。若要了解详细信息，请访问 [Installing and reinstalling packages with package restore](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview)（通过包还原安装和重新安装包）。

此时会启动一个控制台窗口，应用程序开始运行。 应用程序将 HelloWorld.png 图像从解决方案上传到存储帐户。 应用程序会进行检查，确保图像已复制到辅助 RA-GRS 终结点， 然后开始下载图像（最多 999 次）。 每次读取都用 P 或 S 来表示。其中，P 表示主终结点，S 表示辅助终结点。

![正在运行的控制台应用](media/storage-create-geo-redundant-storage/figure3.png)

在示例代码中，`Program.cs` 文件中的 `RunCircuitBreakerAsync` 任务用于通过 [DownloadToFileAsync](https://docs.azure.cn/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet) 方法下载存储帐户中的图像。 在下载之前，会定义 [OperationContext](https://docs.azure.cn/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet)。 操作上下文定义事件处理程序，此类程序在下载成功完成时或者下载失败并重试时触发。

### <a name="retry-event-handler"></a>Retry 事件处理程序

当映像下载失败并设置为重试时，将调用 `OperationContextRetrying` 事件处理程序。 如果达到应用程序中定义的重试次数上限，请求的 [LocationMode](https://docs.azure.cn/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) 变为 `SecondaryOnly`。 此设置强制应用程序从辅助终结点尝试下载该图像。 此配置可减少请求图像所花的时间，因为不会无限重试主终结点。

```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

### <a name="request-completed-event-handler"></a>RequestCompleted 事件处理程序

当图像下载成功时，会调用 `OperationContextRequestCompleted` 事件处理程序。 如果应用程序使用的是辅助终结点，则应用程序会继续使用该终结点，最多 20 次。 20 次以后，应用程序会将 [LocationMode](https://docs.azure.cn/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) 重新设置为 `PrimaryThenSecondary` 并重试主终结点。 如果请求成功，应用程序会继续从主终结点读取。

```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times, 
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

## <a name="next-steps"></a>后续步骤

此系列的第一部分介绍了如何通过 RA-GRS 存储帐户使应用程序高度可用，例如，如何：

> [!div class="checklist"]
> * 创建存储帐户
> * 下载示例
> * 设置连接字符串
> * 运行控制台应用程序

若要了解如何模拟故障并强制应用程序使用辅助 RA-GRS 终结点，请转到此系列的第二部分。

> [!div class="nextstepaction"]
> [模拟连接到主存储终结点时出现的故障](storage-simulate-failure-ragrs-account-app.md)
