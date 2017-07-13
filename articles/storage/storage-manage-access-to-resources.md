---
title: "启用对 Azure Blob 存储中容器和 blob 的公共读取访问 | Azure"
description: "了解如何使容器和 blob 可供匿名访问，以及如何对其进行程序式访问。"
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a2cffee6-3224-4f2a-8183-66ca23b2d2d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: v-johch
ms.openlocfilehash: 147a2f898b129144f01e8a189e1973a1b39f8e8a
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# 管理对容器和 Blob 的匿名读取访问
<a id="manage-anonymous-read-access-to-containers-and-blobs" class="xliff"></a>
可以启用对 Azure Blob 存储中的容器及其 Blob 的匿名公共读取访问。 这样做可以授予对这些资源的只读访问权限，无需共享帐户密钥，也无需共享访问签名 (SAS)。

如果想要始终允许对某些 Blob 进行匿名读取访问，最好的方法是启用公共读取访问。 可以创建共享访问签名，实现更精细的控制。 利用共享访问签名，可以针对特定时间段提供使用不同权限的受限访问。 有关创建共享访问签名的详细信息，请参阅[在 Azure 存储中使用共享访问签名 (SAS)](storage-dotnet-shared-access-signature-part-1.md)。

## 授予对容器和 Blob 的匿名用户权限
<a id="grant-anonymous-users-permissions-to-containers-and-blobs" class="xliff"></a>
默认情况下，仅存储帐户的所有者能够访问容器以及其中的所有 Blob。 要授予匿名用户对容器及其 Blob 的读取权限，可以设置容器权限以允许公共访问。 匿名用户可以读取可公开访问的容器中的 Blob，而无需对请求进行身份验证。

可为容器配置以下权限：

* 非公共读取访问：只有存储帐户所有者可以访问容器及其 Blob。 这是所有新容器的默认权限。
* 仅针对 Blob 的公共读取访问：可通过匿名请求读取容器中的 Blob，但容器数据不可用。 匿名客户端无法枚举容器中的 Blob。
* 完全公共读取访问：可通过匿名请求读取所有容器和 Blob 数据。 客户端可以通过匿名请求枚举容器中的 Blob，但无法枚举存储帐户中的容器。

可以通过以下方式来设置容器权限：

* [Azure 门户](https://portal.azure.cn)
* [Azure PowerShell](storage-powershell-guide-full.md#how-to-manage-azure-blobs)
* [Azure CLI 2.0](storage-azure-cli.md#create-and-manage-blobs)
* 通过使用一个存储客户端库或 REST API 以编程方式设置

### 在 Azure 门户中设置容器权限
<a id="set-container-permissions-in-the-azure-portal" class="xliff"></a>
若要在 [Azure 门户](https://portal.azure.cn)中设置容器权限，请按以下步骤操作：

1. 在门户中打开“存储帐户”边栏选项卡。 通过在主门户菜单边栏选项卡中选择“存储帐户”，可以查找存储帐户。
1. 在“菜单”边栏选项卡的“Blob 服务”下，选择“容器”。
1. 在容器行上右键单击或选择省略号，打开容器的“上下文菜单”。
1. 在上下文菜单中选择“访问策略”。
1. 从下拉菜单中选择“访问类型”。

    ![编辑容器元数据对话框](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### 使用 .NET 设置容器权限
<a id="set-container-permissions-with-net" class="xliff"></a>
若要使用 C# 和适用于 .NET 的存储客户端库设置容器的权限，请先调用 GetPermissions 方法以检索容器的现有权限。 然后，设置 GetPermissions 方法返回的 BlobContainerPermissions 对象的 PublicAccess 属性。 最后，使用更新的权限调用 **SetPermissions** 方法。

以下示例将容器的权限设置为完全公共读取访问。 若要将权限设置为仅针对 blob 的公共读取访问，请将 PublicAccess 属性设置为 BlobContainerPublicAccessType.Blob。 若要删除匿名用户的所有权限，请将该属性设置为 **BlobContainerPublicAccessType.Off**。

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## 匿名访问容器和 Blob
<a id="access-containers-and-blobs-anonymously" class="xliff"></a>
如果某个客户端需要以匿名方式访问容器和 Blob，该客户端则可以使用不需要凭据的构造函数。 以下示例显示了如何通过多种不同的方法以匿名方式引用 Blob 服务资源。

### 创建匿名客户端对象
<a id="create-an-anonymous-client-object" class="xliff"></a>
通过提供帐户的 Blob 服务终结点，即可以创建一个用于匿名访问的新的服务客户端对象。 但是，也必须要知道该帐户中允许进行匿名访问的容器的名称。

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.Chinacloudapi.cn"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### 以匿名方式引用容器
<a id="reference-a-container-anonymously" class="xliff"></a>
如果拥有可以通过匿名方式使用的容器的 URL，则可使用该 URL 来直接引用容器。

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.Chinacloudapi.cn/sample-container"));

    // List blobs in the container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### 以匿名方式引用 Blob
<a id="reference-a-blob-anonymously" class="xliff"></a>
如果拥有允许进行匿名访问的 Blob 的 URL，则可使用该 URL 来直接引用 Blob：

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.Chinacloudapi.cn/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## 对匿名用户可用的功能
<a id="features-available-to-anonymous-users" class="xliff"></a>
下表显示了在将容器的 ACL 设置为允许公共访问时匿名用户可调用的操作。

| REST 操作 | 具有完全公共读取访问权限 | 具有仅针对 Blob 的公共读取访问权限 |
| --- | --- | --- |
| 列出容器 |仅所有者 |仅所有者 |
| 创建容器 |仅所有者 |仅所有者 |
| 获取容器属性 |全部 |仅所有者 |
| 获取容器元数据 |全部 |仅所有者 |
| 设置容器元数据 |仅所有者 |仅所有者 |
| 获取容器 ACL |仅所有者 |仅所有者 |
| 设置容器 ACL |仅所有者 |仅所有者 |
| 删除容器 |仅所有者 |仅所有者 |
| 列出 Blob |全部 |仅所有者 |
| 放置 Blob |仅所有者 |仅所有者 |
| 获取 Blob |全部 |全部 |
| 获取 Blob 属性 |全部 |全部 |
| 设置 Blob 属性 |仅所有者 |仅所有者 |
| 获取 Blob 元数据 |全部 |全部 |
| 设置 Blob 元数据 |仅所有者 |仅所有者 |
| 放置块 |仅所有者 |仅所有者 |
| 获取块列表（仅提交的块） |全部 |全部 |
| 获取块列表（仅未提交的块或所有块） |仅所有者 |仅所有者 |
| 放置块列表 |仅所有者 |仅所有者 |
| 删除 Blob |仅所有者 |仅所有者 |
| 复制 Blob |仅所有者 |仅所有者 |
| 快照 Blob |仅所有者 |仅所有者 |
| 租用 Blob |仅所有者 |仅所有者 |
| 放置页面 |仅所有者 |仅所有者 |
| 获取页面范围 |全部 |全部 |
| 追加 Blob |仅所有者 |仅所有者 |

## 后续步骤
<a id="next-steps" class="xliff"></a>

* [Authentication for the Azure Storage Services](https://msdn.microsoft.com/library/azure/dd179428.aspx)（Azure 存储服务的身份验证）
* [使用共享访问签名 (SAS)](storage-dotnet-shared-access-signature-part-1.md)
* [使用共享访问签名委托访问](https://msdn.microsoft.com/library/azure/ee395415.aspx)
