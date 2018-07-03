---
title: 使应用程序数据在 Azure 中高度可用 | Azure
description: 利用读取访问异地冗余存储，使应用程序数据高度可用
services: storage
author: forester123
manager: josefree
ms.service: storage
ms.workload: web
ms.topic: tutorial
origin.date: 03/26/2018
ms.date: 07/02/2018
ms.author: v-nany
ms.custom: mvc
ms.openlocfilehash: 78af9c93eed9baf98e2fd57bdbe3d756586bf8c3
ms.sourcegitcommit: 3583af94b935af10fcd4af3f4c904cf0397af798
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2018
ms.locfileid: "37103079"
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>使应用程序数据在 Azure 存储中高度可用

本教程是一个教程系列的第一部分，介绍了如何在 Azure 中实现应用程序数据的高可用性。 完成后，你会有一个控制台应用程序，用于将 blob 上传到[读取访问异地冗余](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) 存储帐户并对其进行检索。 RA-GRS 的工作方式是将事务从主要区域复制到次要区域。 此复制过程可确保次要区域中的数据最终一致。 应用程序使用[断路器](/azure/architecture/patterns/circuit-breaker)模式来确定要连接到的终结点。 对故障进行模拟时，应用程序切换到辅助终结点。

在该系列的第一部分中，你将学习如何：

> [!div class="checklist"]
> * 创建存储帐户
> * 下载示例
> * 设置连接字符串
> * 运行控制台应用程序

## <a name="prerequisites"></a>先决条件

完成本教程：
 
# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)
* 使用以下工作负荷安装 [Visual Studio 2017](https://www.visualstudio.com/downloads/)：
  - **Azure 开发**

  ![Azure 开发（位于“Web 和云”下）](media/storage-create-geo-redundant-storage/workloads.png)

* （可选）下载并安装 [Fiddler](https://www.telerik.com/download/fiddler)
 
# <a name="python-tabpython"></a>[Python] (#tab/python) 

* [安装 Python](https://www.python.org/downloads/)
* 下载并安装[用于 Python 的 Azure 存储 SDK](https://github.com/Azure/azure-storage-python)
* （可选）下载并安装 [Fiddler](https://www.telerik.com/download/fiddler)

# <a name="java-tabjava"></a>[Java] (#tab/java)

* 从命令行安装和配置要使用的 [Maven](http://maven.apache.org/download.cgi)
* 安装并配置 [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

---

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>登录到 Azure 门户

登录到 [Azure 门户](https://portal.azure.cn/)。

## <a name="create-a-storage-account"></a>创建存储帐户

存储帐户提供唯一的命名空间来存储和访问 Azure 存储数据对象。

按以下步骤创建读取访问异地冗余存储帐户：

1. 选择 Azure 门户左上角的“创建资源”按钮。

2. 在“新建”页中选择“存储”，然后在“特别推荐”下选择“存储帐户 - blob、文件、表、队列”。
3. 使用以下信息填充存储帐户窗体（如下图所示），然后选择“创建”：

   | 设置       | 建议的值 | 说明 |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **名称** | mystorageaccount | 存储帐户的唯一值 |
   | 部署模型 | Resource Manager  | 资源管理器包含最新功能。|
   | 帐户类型 | StorageV2 | 有关帐户类型的详细信息，请参阅[存储帐户的类型](../common/storage-introduction.md#types-of-storage-accounts) |
   | **性能** | 标准 | 对于示例方案，“标准”已足够。 |
   | **复制**| 读取访问异地冗余存储 (RA-GRS) | 这是运行示例所必需的。 |
   |需要安全传输 | 已禁用| 此方案不需要安全传输。 |
   |**订阅** | 你的订阅 |有关订阅的详细信息，请参阅[订阅](https://account.windowsazure.cn/Subscriptions)。 |
   |**ResourceGroup** | MyResourceGroup |如需有效的资源组名称，请参阅 [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)（命名规则和限制）。 |
   |**位置** | 中国东部 | 选择一个位置。 |

![创建存储帐户](media/storage-create-geo-redundant-storage/createragrsstracct.png)

## <a name="download-the-sample"></a>下载示例

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

[下载示例项目](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip)并解压缩 storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip 文件。 也可使用 [git](https://git-scm.com/) 将应用程序的副本下载到开发环境。 示例项目包含一个控制台应用程序。

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.git 
```
# <a name="python-tabpython"></a>[Python] (#tab/python) 

[下载示例项目](https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip)并解压缩 storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.zip 文件。 也可使用 [git](https://git-scm.com/) 将应用程序的副本下载到开发环境。 示例项目包含一个基本的 Python 应用程序。

```bash
git clone https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="java-tabjava"></a>[Java] (#tab/java)
[下载示例项目](https://github.com/Azure-Samples/storage-java-ha-ra-grs)并解压缩 storage-java-ragrs.zip 文件。 也可使用 [git](https://git-scm.com/) 将应用程序的副本下载到开发环境。 示例项目包含一个基本的 Java 应用程序。

```bash
git clone https://github.com/Azure-Samples/storage-java-ha-ra-grs.git
```
---


## <a name="set-the-connection-string"></a>设置连接字符串

在应用程序中，必须为存储帐户提供连接字符串。 建议将此连接字符串存储在运行应用程序的本地计算机上的环境变量中。 根据操作系统按照下面的某个示例的做法创建环境变量。

在 Azure 门户中导航到存储帐户。 在存储帐户中选择“设置”下的“访问密钥”。 复制主密钥或辅助密钥中的**连接字符串**。 将 \<yourconnectionstring\> 替换为实际的连接字符串，只需根据操作系统运行以下命令之一即可。 此命令将一个环境变量保存到本地计算机。 在 Windows 中，此环境变量不可用，除非重新加载正使用的**命令提示符**或 shell。 替换以下示例中的 **\<storageConnectionString\>**：

# <a name="linux-tablinux"></a>[Linux] (#tab/linux) 
export storageconnectionstring=\<yourconnectionstring\> 

# <a name="windows-tabwindows"></a>[Windows] (#tab/windows) 
setx storageconnectionstring "\<yourconnectionstring\>"

---


## <a name="run-the-console-application"></a>运行控制台应用程序

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)
在 Visual Studio 中，按 F5 或选择“开始”即可开始调试应用程序。 Visual studio 自动还原缺失的 NuGet 包（如果已配置）。若要了解详细信息，请访问 [Installing and reinstalling packages with package restore](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview)（通过包还原安装和重新安装包）。

此时会启动一个控制台窗口，应用程序开始运行。 应用程序将 HelloWorld.png 图像从解决方案上传到存储帐户。 应用程序会进行检查，确保图像已复制到辅助 RA-GRS 终结点， 然后开始下载图像（最多 999 次）。 每次读取都用 P 或 S 来表示。其中，P 表示主终结点，S 表示辅助终结点。

![正在运行的控制台应用](media/storage-create-geo-redundant-storage/figure3.png)

在示例代码中，`Program.cs` 文件中的 `RunCircuitBreakerAsync` 任务用于通过 [DownloadToFileAsync](https://docs.azure.cn/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet) 方法下载存储帐户中的图像。 下载前，将会先定义 [OperationContext](https://docs.azure.cn/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet)。 操作上下文定义事件处理程序，此类程序在下载成功完成时或者下载失败并重试时触发。

# <a name="python-tabpython"></a>[Python] (#tab/python) 
若要在终端或命令提示符处运行应用程序，请转到 **circuitbreaker.py** 目录，然后输入 `python circuitbreaker.py`。 应用程序将 HelloWorld.png 图像从解决方案上传到存储帐户。 应用程序会进行检查，确保图像已复制到辅助 RA-GRS 终结点， 然后开始下载图像（最多 999 次）。 每次读取都用 P 或 S 来表示。其中，P 表示主终结点，S 表示辅助终结点。

![正在运行的控制台应用](media/storage-create-geo-redundant-storage/figure3.png)

在示例代码中，使用了 `circuitbreaker.py` 文件中的 `run_circuit_breaker` 方法通过 [get_blob_to_path](https://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html) 方法来下载存储帐户中的映像。 

存储对象重试函数设置为线性重试策略。 重试函数决定了是否重试某个请求，并指定了在重试该请求之前需要等待的秒数。 如果应该在针对主终结点的初始请求失败以后重试针对次要终结点的请求，请将 **retry\_to\_secondary** 值设置为 true。 在示例应用程序的存储对象的 `retry_callback` 函数中，定义了自定义重试策略。
 
在下载之前，定义了服务对象的 [retry_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) 和 [response_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) 函数。 这些函数定义了在下载成功完成或下载失败并重试时触发的事件处理程序。  

# <a name="java-tabjava"></a>[Java] (#tab/java)
若要运行应用程序，可以打开一个终端或命令提示符，其作用域为下载的应用程序所在的文件夹。 在该处输入 `mvn compile exec:java` 即可运行应用程序。 然后，应用程序会将 **HelloWorld.png** 映像从该目录上传到存储帐户并进行检查，确保映像已复制到辅助 RA-GRS 终结点。 检查完成以后，应用程序就会开始重复下载该映像，同时会将从其下载映像的终结点报告回来。

存储对象重试函数设置为使用线性重试策略。 重试函数决定了是否重试某个请求，并指定了在每次重试之前需要等待的秒数。 **BlobRequestOptions** 的 **LocationMode** 属性设置为 **PRIMARY\_THEN\_SECONDARY**。 这样，如果应用程序在尝试下载 **HelloWorld.png** 时无法访问主位置，则会自动将其切换到辅助位置。

---

## <a name="understand-the-sample-code"></a>了解示例代码

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

### <a name="retry-event-handler"></a>Retry 事件处理程序

当映像下载失败并设置为重试时，将调用 `OperationContextRetrying` 事件处理程序。 如果达到应用程序中定义的重试次数上限，请求的 [LocationMode](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) 会变为 `SecondaryOnly`。 此设置强制应用程序从辅助终结点尝试下载该图像。 此配置可减少请求图像所花的时间，因为不会无限重试主终结点。
 
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

# <a name="python-tabpython"></a>[Python] (#tab/python) 

### <a name="retry-event-handler"></a>Retry 事件处理程序

当映像下载失败并设置为重试时，将调用 `retry_callback` 事件处理程序。 如果达到应用程序中定义的重试次数上限，请求的 [LocationMode](https://docs.microsoft.com/python/api/azure.storage.common.models.locationmode?view=azure-python) 会变为 `SECONDARY`。 此设置强制应用程序从辅助终结点尝试下载该图像。 此配置可减少请求图像所花的时间，因为不会无限重试主终结点。  

```python
def retry_callback(retry_context):
    global retry_count
    retry_count = retry_context.count
    sys.stdout.write("\nRetrying event because of failure reading the primary. RetryCount= {0}".format(retry_count))
    sys.stdout.flush()

    # Check if we have more than n-retries in which case switch to secondary
    if retry_count >= retry_threshold:

        # Check to see if we can fail over to secondary.
        if blob_client.location_mode != LocationMode.SECONDARY:
            blob_client.location_mode = LocationMode.SECONDARY
            retry_count = 0
        else:
            raise Exception("Both primary and secondary are unreachable. "
                            "Check your application's network connection.")
```

### <a name="request-completed-event-handler"></a>RequestCompleted 事件处理程序

当图像下载成功时，会调用 `response_callback` 事件处理程序。 如果应用程序使用的是辅助终结点，则应用程序会继续使用该终结点，最多 20 次。 20 次以后，应用程序会将 [LocationMode](https://docs.microsoft.com/python/api/azure.storage.common.models.locationmode?view=azure-python) 重新设置为 `PRIMARY` 并重试主终结点。 如果请求成功，应用程序会继续从主终结点读取。

```python
def response_callback(response):
    global secondary_read_count
    if blob_client.location_mode == LocationMode.SECONDARY:

        # You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        # then switch back to the primary and see if it is available now.
        secondary_read_count += 1
        if secondary_read_count >= secondary_threshold:
            blob_client.location_mode = LocationMode.PRIMARY
            secondary_read_count = 0
```

# <a name="java-tabjava"></a>[Java] (#tab/java)

使用 Java 时，如果 **BlobRequestOptions** 的 **LocationMode** 属性设置为 **PRIMARY\_THEN\_SECONDARY**，则不需定义回调处理程序。 这样，如果应用程序在尝试下载 **HelloWorld.png** 时无法访问主位置，则会自动将其切换到辅助位置。

```java
    BlobRequestOptions myReqOptions = new BlobRequestOptions();
    myReqOptions.setRetryPolicyFactory(new RetryLinearRetry(deltaBackOff, maxAttempts));
    myReqOptions.setLocationMode(LocationMode.PRIMARY_THEN_SECONDARY);
    blobClient.setDefaultRequestOptions(myReqOptions);

    blob.downloadToFile(downloadedFile.getAbsolutePath(),null,blobClient.getDefaultRequestOptions(),opContext);
```
---


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
<!--Update_Description: add Java install, configuration, code sample-->