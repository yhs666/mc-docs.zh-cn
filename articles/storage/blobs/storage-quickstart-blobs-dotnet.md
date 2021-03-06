---
title: 快速入门：Azure Blob 存储库 v12 - .NET
description: 本快速入门介绍如何使用适用于 .NET 的 Azure Blob 存储客户端库版本 12 在 Blob（对象）存储中创建容器和 blob。 接下来，介绍如何将 blob 下载到本地计算机，以及如何列出容器中的所有 blob。
author: WenJason
ms.author: v-jay
origin.date: 11/05/2019
ms.date: 11/25/2019
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.openlocfilehash: 64cb73a566444856c9a3d3c0cd66209fae185d8c
ms.sourcegitcommit: 6a19227dcc0c6e0da5b82c4f69d0227bf38a514a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74328653"
---
# <a name="quickstart-azure-blob-storage-client-library-v12-for-net"></a>快速入门：适用于 .NET 的 Azure Blob 存储客户端库 v12

适用于 .NET 的 Azure Blob 存储客户端库 v12 入门。 Azure Blob 存储是 Azure 的适用于云的对象存储解决方案。 请按照步骤操作，安装程序包并试用基本任务的示例代码。 Blob 存储最适合存储巨量的非结构化数据。

> [!NOTE]
> 若要开始使用以前的 SDK 版本，请参阅[快速入门：适用于 .NET 的 Azure Blob 存储客户端库](storage-quickstart-blobs-dotnet-legacy.md)。

使用适用于 .NET 的 Azure Blob 存储客户端库 v12 完成以下操作：

* 创建容器
* 将 blob 上传到 Azure 存储
* 列出容器中所有的 blob
* 将 blob 下载到本地计算机
* 删除容器

[API 参考文档](https://docs.microsoft.com/dotnet/api/azure.storage.blobs) | [库源代码](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs) | [包 (NuGet)](https://www.nuget.org/packages/Azure.Storage.Blobs/12.0.0) | [示例](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples)

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [创建一个 1 元试用帐户](https://www.azure.cn/pricing/1rmb-trial-full/?form-type=identityauth)
* Azure 存储帐户 - [创建存储帐户](/storage/common/storage-quickstart-create-account)
* 适用于操作系统的最新 [NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)。 确保获取 SDK，而不是运行时。

## <a name="setting-up"></a>设置

本部分逐步指导如何准备一个项目，使其与适用于 .NET 的 Azure Blob 存储客户端库 v12 配合使用。

### <a name="create-the-project"></a>创建项目

创建名为 BlobQuickstartV12 的 .NET Core 应用程序  。

1. 在控制台窗口（例如 cmd、PowerShell 或 Bash）中，使用 `dotnet new` 命令创建名为 BlobQuickstartV12 的新控制台应用  。 此命令将创建包含单个源文件的简单“Hello World”C# 项目：*Program.cs*。

   ```console
   dotnet new console -n BlobQuickstartV12
   ```

1. 切换到新创建的 BlobQuickstartV12 目录  。

   ```console
   cd BlobQuickstartV12
   ```

1. 在 BlobQuickstartV12 目录中，创建名为 data 的另一个目录   。 将在这里创建和存储 blob 数据文件。

    ```console
    mkdir data
    ```

### <a name="install-the-package"></a>安装包

仍在应用程序目录中时，使用 `dotnet add package` 命令安装适用于 .NET 包的 Azure Blob 存储客户端库。

```console
dotnet add package Azure.Storage.Blobs
```

### <a name="set-up-the-app-framework"></a>设置应用框架

从项目目录中执行以下操作：

1. 在编辑器中打开 Program.cs  文件
1. 删除 `Console.WriteLine("Hello World!");` 语句
1. 添加 `using` 指令
1. 更新 `Main` 方法声明以支持异步代码

代码如下：

```csharp
using Azure.Storage;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
using System;
using System.IO;
using System.Threading.Tasks;

namespace BlobQuickstartV12
{
    class Program
    {
        static async Task Main()
        {
        }
    }
}
```

### <a name="copy-your-credentials-from-the-azure-portal"></a>从 Azure 门户复制凭据

当示例应用程序向 Azure 存储发出请求时，必须对其进行授权。 若要对请求进行授权，请将存储帐户凭据以连接字符串形式添加到应用程序中。 按照以下步骤查看存储帐户凭据：

1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 找到自己的存储帐户。
3. 在存储帐户概述的“设置”部分，选择“访问密钥”。   在这里，可以查看你的帐户访问密钥以及每个密钥的完整连接字符串。
4. 找到“密钥 1”下面的“连接字符串”值，选择“复制”按钮复制该连接字符串。    下一步需将此连接字符串值添加到某个环境变量。

    ![显示如何从 Azure 门户复制连接字符串的屏幕截图](../../../includes/media/storage-copy-connection-string-portal/portal-connection-string.png)

### <a name="configure-your-storage-connection-string"></a>配置存储连接字符串

复制连接字符串以后，请将其写入运行应用程序的本地计算机的新环境变量中。 若要设置环境变量，请打开控制台窗口，并遵照适用于操作系统的说明。 将 `<yourconnectionstring>` 替换为实际的连接字符串。

#### <a name="windows"></a>Windows

```cmd
setx CONNECT_STR "<yourconnectionstring>"
```

在 Windows 中添加环境变量后，必须启动命令窗口的新实例。

#### <a name="linux"></a>Linux

```bash
export CONNECT_STR="<yourconnectionstring>"
```

#### <a name="macos"></a>macOS

```bash
export CONNECT_STR="<yourconnectionstring>"
```

#### <a name="restart-programs"></a>重新启动程序

添加环境变量后，重启需要读取环境变量的任何正在运行的程序。 例如，重启开发环境或编辑器，然后再继续。

## <a name="object-model"></a>对象模型

Azure Blob 存储最适合存储巨量的非结构化数据。 非结构化数据是不遵循特定数据模型或定义（如文本或二进制数据）的数据。 Blob 存储提供了三种类型的资源：

* 存储帐户
* 存储帐户中的容器
* 容器中的 blob

以下图示显示了这些资源之间的关系。

![Blob 存储体系结构的图示](./media/storage-blob-introduction/blob1.png)

使用以下 .NET 类与这些资源进行交互：

* [BlobServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobserviceclient)：`BlobServiceClient` 类可用于操纵 Azure 存储资源和 blob 容器。
* [BlobContainerClient](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient)：`BlobContainerClient` 类可用于操纵 Azure 存储容器及其 blob。
* [BlobClient](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobclient)：`BlobClient` 类可用于操纵 Azure 存储 blob。
* [BlobDownloadInfo](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.models.blobdownloadinfo)：`BlobDownloadInfo` 类表示从下载 blob 返回的属性和内容。

## <a name="code-examples"></a>代码示例

这些示例代码片段演示如何使用适用于 .NET 的 Azure Blob 存储客户端库执行以下步骤：

* [获取连接字符串](#get-the-connection-string)
* [创建容器](#create-a-container)
* [将 blob 上传到容器中](#upload-blobs-to-a-container)
* [列出容器中的 blob](#list-the-blobs-in-a-container)
* [下载 blob](#download-blobs)
* [删除容器](#delete-a-container)

### <a name="get-the-connection-string"></a>获取连接字符串

下面的代码从[配置存储连接字符串](#configure-your-storage-connection-string)部分中创建的环境变量中检索存储帐户的连接字符串。

在 `Main` 方法内添加此代码：

```csharp
Console.WriteLine("Azure Blob storage v12 - .NET quickstart sample\n");

// Retrieve the connection string for use with the application. The storage
// connection string is stored in an environment variable on the machine
// running the application called CONNECT_STR. If the
// environment variable is created after the application is launched in a
// console or with Visual Studio, the shell or application needs to be closed
// and reloaded to take the environment variable into account.
string connectionString = Environment.GetEnvironmentVariable("CONNECT_STR");
```

### <a name="create-a-container"></a>创建容器

确定新容器的名称。 以下代码将 GUID 值追加到容器名称，确保其是唯一的。

> [!IMPORTANT]
> 容器名称必须为小写。 有关命名容器和 Blob 的详细信息，请参阅[命名和引用容器、Blob 和元数据](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)。

创建 [BlobServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobserviceclient) 类的实例。 然后，调用 [CreateBlobContainerAsync](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobserviceclient.createblobcontainerasync) 方法在存储帐户中创建容器。

将此代码添加到 `Main` 方法的末尾：

```csharp
// Create a BlobServiceClient object which will be used to create a container client
BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

//Create a unique name for the container
string containerName = "quickstartblobs" + Guid.NewGuid().ToString();

// Create the container and return a container client object
BlobContainerClient containerClient = await blobServiceClient.CreateBlobContainerAsync(containerName);
```

### <a name="upload-blobs-to-a-container"></a>将 blob 上传到容器中

以下代码片段：

1. 在本地 data 目录中创建文本文件  。
1. 对在[创建容器](#create-a-container)部分创建的容器调用 [GetBlobClient](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobclient) 方法，获取对 [BlobClient](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobclient) 对象的引用。
1. 通过调用 [UploadAsync](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobclient.uploadasync) 方法将本地文本文件上传到 blob。 此方法将创建 Blob（如果该 Blob 尚不存在），或者覆盖 Blob（如果该 Blob 已存在）。

将此代码添加到 `Main` 方法的末尾：

```csharp
// Create a local file in the ./data/ directory for uploading and downloading
string localPath = "./data/";
string fileName = "quickstart" + Guid.NewGuid().ToString() + ".txt";
string localFilePath = Path.Combine(localPath, fileName);

// Write text to the file
await File.WriteAllTextAsync(localFilePath, "Hello, World!");

// Get a reference to a blob
BlobClient blobClient = containerClient.GetBlobClient(fileName);

Console.WriteLine("Uploading to Blob storage as blob:\n\t {0}\n", blobClient.Uri);

// Open the file and upload its data
using FileStream uploadFileStream = File.OpenRead(localFilePath);
await blobClient.UploadAsync(uploadFileStream);
uploadFileStream.Close();
```

### <a name="list-the-blobs-in-a-container"></a>列出容器中的 Blob

通过调用 [GetBlobsAsync](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsasync) 方法，列出容器中的 blob。 在这种情况下，只向容器添加了一个 blob，因此列表操作只返回那个 blob。

将此代码添加到 `Main` 方法的末尾：

```csharp
Console.WriteLine("Listing blobs...");

// List all blobs in the container
await foreach (BlobItem blobItem in containerClient.GetBlobsAsync())
{
    Console.WriteLine("\t" + blobItem.Name);
}
```

### <a name="download-blobs"></a>下载 Blob

通过调用 [DownloadAsync](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.downloadasync) 方法，下载以前创建的 blob。 示例代码将向文件名添加后缀“DOWNLOADED”，这样你就可以在本地文件系统中看到这两个文件。

将此代码添加到 `Main` 方法的末尾：

```csharp
// Download the blob to a local file
// Append the string "DOWNLOAD" before the .txt extension so you can see both files in MyDocuments
string downloadFilePath = localFilePath.Replace(".txt", "DOWNLOAD.txt");

Console.WriteLine("\nDownloading blob to\n\t{0}\n", downloadFilePath);

// Download the blob's contents and save it to a file
BlobDownloadInfo download = await blobClient.DownloadAsync();

using FileStream downloadFileStream = File.OpenWrite(downloadFilePath);
await download.Content.CopyToAsync(downloadFileStream);
downloadFileStream.Close();
```

### <a name="delete-a-container"></a>删除容器

以下代码使用 [DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync) 来删除整个容器，从而清除该应用所创建的资源。 它还会删除由应用创建的本地文件。

在删除 blob、容器和本地文件之前，应用会调用 `Console.ReadLine` 以暂停并等待用户输入。 可以通过这种方式验证是否已正确创建资源，然后再删除该资源。

将此代码添加到 `Main` 方法的末尾：

```csharp
// Clean up
Console.Write("Press any key to begin clean up");
Console.ReadLine();

Console.WriteLine("Deleting blob container...");
await containerClient.DeleteAsync();

Console.WriteLine("Deleting the local source and downloaded files...");
File.Delete(localFilePath);
File.Delete(downloadFilePath);

Console.WriteLine("Done");
```

## <a name="run-the-code"></a>运行代码

此应用在本地 *MyDocuments* 文件夹中创建一个测试文件，并将其上传到 Blob 存储。 然后，该示例会列出容器中的 blob，并使用新名称下载文件，这样便可对旧文件和新文件进行比较。

导航到应用程序目录，然后生成并运行应用程序。

```console
dotnet build
```

```console
dotnet run
```

应用的输出类似于以下示例：

```output
Azure Blob storage v12 - .NET quickstart sample

Uploading to Blob storage as blob:
         https://mystorageacct.blob.core.chinacloudapi.cn/quickstartblobs60c70d78-8d93-43ae-954d-8322058cfd64/quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31.txt

Listing blobs...
        quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31.txt

Downloading blob to
        ./data/quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31DOWNLOADED.txt

Press any key to begin clean up
Deleting blob container...
Deleting the local source and downloaded files...
Done
```

在开始清理过程之前，请在“MyDocuments”文件夹中查找这两个文件  。 可以打开它们，然后就会观察到它们完全相同。

验证文件后，按 Enter 键以删除测试文件并完成演示  。

## <a name="next-steps"></a>后续步骤

本快速入门介绍了如何使用 .NET 上传、下载和列出 Blob。

若要查看 Blob 存储示例应用，请继续执行以下操作：

> [!div class="nextstepaction"]
> [Azure Blob 存储 SDK v12 .NET 示例](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples)

* 有关教程、示例、快速入门和其他文档，请访问[面向 .NET 和 .NET Core 开发人员的 Azure](/dotnet/)。
* 若要详细了解 .NET Core，请参阅 [Get started with .NET in 10 minutes](https://www.microsoft.com/net/learn/get-started/)（.NET 10 分钟入门）。
