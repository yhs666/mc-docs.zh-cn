---
title: Azure Data Lake Storage Gen2 的已知问题 | Microsoft Docs
description: 了解 Azure Data Lake Storage Gen2 的限制和已知问题
author: WenJason
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
origin.date: 10/11/2019
ms.date: 10/28/2019
ms.author: v-jay
ms.reviewer: jamesbak
ms.openlocfilehash: 6a0cd51d5bc36ef50f4c6cefca11bfb237aeefdd
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914534"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2 的已知问题

本文列出了使用分层命名空间的存储帐户 (Azure Data Lake Storage Gen2) 尚不支持或者仅部分支持的功能与工具。

<a id="blob-apis-disabled" />

## <a name="blob-storage-apis"></a>Blob 存储 API

Blob 存储 API 已禁用，以防止可能出现的功能可操作性问题，因为 Blob 存储 API 还不能与 Azure Data Lake Gen2 API 互操作。

> [!NOTE]
> 使用公共预览版 Data Lake Storage 多协议访问时，Blob API 和 Data Lake Storage Gen2 API 可以对相同数据进行操作。 若要了解详细信息，请参阅 [Data Lake Storage 多协议访问](data-lake-storage-multi-protocol-access.md)。

### <a name="what-to-do-with-existing-tools-applications-and-services"></a>对于现有工具、应用程序和服务要采取的措施

如果上述任何组件使用 Blob API，并且你希望使用它们处理帐户的所有内容，则可使用两个选项。

* **选项 1**：在 [Data Lake Storage 多协议访问](data-lake-storage-multi-protocol-access.md)公开发布且 Blob API 与 Azure Data Lake Gen2 API 实现完全的互操作之前，请勿在 Blob 存储帐户中启用分层命名空间。 [Data Lake Storage 多协议访问](data-lake-storage-multi-protocol-access.md)目前为公共预览版。  使用**未**启用分层命名空间的存储帐户意味着无法访问 Data Lake Storage Gen2 特定的功能，例如目录和容器访问控制列表。

* **选项 2**：启用分层命名空间。 有了公共预览版 [Data Lake Storage 多协议访问](data-lake-storage-multi-protocol-access.md)，用于调用 Blob API 的工具和应用程序以及 Blob 存储功能（例如诊断日志）就可以与具有分层命名空间的帐户配合使用。 请务必查看此文，了解已知问题和限制。

### <a name="what-to-do-if-you-used-blob-apis-to-load-data-before-blob-apis-were-disabled"></a>如果在禁用 Blob API 之前已使用 Blob API 上传了数据，该怎么办

如果在这些 API 被禁用之前使用了它们来加载数据，并且生产环境需要访问该数据，请联系 Azure 支持部门并提供以下信息：

> [!div class="checklist"]
> * 订阅 ID（GUID，不是名称）。
> * 存储帐户名称。
> * 在生产环境中是否频繁受到影响，如果是，针对的是哪些存储帐户？
> * 即使在生产环境中未频繁受到影响，也请告诉我们你是否出于某种原因需要将该数据复制到另一个存储帐户，如果是，为什么？

在这些情况下，我们可以在有限的时间段内恢复对 Blob API 的访问权限，以便你可以将此数据复制到未启用分层命名空间功能的存储帐户。

### <a name="issues-and-limitations-with-using-blob-apis-on-accounts-that-have-a-hierarchical-namespace"></a>在有分层命名空间的帐户上使用 Blob API 时存在的问题和限制

使用公共预览版 Data Lake Storage 多协议访问时，Blob API 和 Data Lake Storage Gen2 API 可以对相同数据进行操作。

本部分介绍使用 Blob API 和 Data Lake Storage Gen2 API 对相同的数据执行操作时存在的问题和限制。

* 不能同时使用 Blob API 和 Data Lake Storage API 向文件的同一实例写入数据。

* 如果使用 Data Lake Storage Gen2 API 向某个文件写入数据，则在调用[获取块列表](https://docs.microsoft.com/rest/api/storageservices/get-block-list) Blob API 时，该文件的块将不可见。

* 覆盖某个文件时，可以使用 Data Lake Storage Gen2 API 或 Blob API。 这不会影响文件属性。

* 如果在使用[列出 Blob](https://docs.microsoft.com/rest/api/storageservices/list-blobs) 操作时不指定分隔符，则结果会包含目录和 Blob。

  如果选择使用分隔符，请只使用正斜杠 (`/`)。 这是唯一支持的分隔符。

* 如果使用[删除 Blob](https://docs.microsoft.com/rest/api/storageservices/delete-blob) API 来删除目录，则只能在该目录为空的情况下将其删除。

  这意味着，不能使用 Blob API 以递归方式删除目录。

这些 Blob REST API 不受支持：

* [放置 Blob（页）](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [放置页](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [获取页面范围](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [增量复制 Blob](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [从 URL 放置页](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [放置 Blob（追加）](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [追加块](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [从 URL 追加块](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

## <a name="issues-with-unmanaged-virtual-machine-vm-disks"></a>非托管虚拟机 (VM) 磁盘的问题

在有分层命名空间的帐户中，非托管 VM 磁盘不受支持。 若要在存储帐户中启用分层命名空间，请将非托管 VM 磁盘放到未启用分层命名空间功能的存储帐户中。


## <a name="support-for-other-blob-storage-features"></a>支持其他 Blob 存储功能

下表列出了使用分层命名空间的存储帐户 (Azure Data Lake Storage Gen2) 尚不支持或者仅部分支持的所有其他功能与工具。

| 功能/工具    | 详细信息    |
|--------|-----------|
| **适用于 Data Lake Storage Gen2 存储帐户的 API** | 部分支持 <br><br>Data Lake Storage 多协议访问目前为公共预览版。 此预览版允许将以 .NET、Java、Python SDK 编写的 Blob API 与具有分层命名空间的帐户配合使用。  这些 SDK 尚不包含允许你与目录交互或设置访问控制列表 (ACL) 的 API。 若要执行这些功能，可以使用 Data Lake Storage Gen2 **REST** API。 |
| **AzCopy** | 特定于版本的支持 <br><br>仅使用最新版本的 AzCopy ([AzCopy v10](/storage/common/storage-use-azcopy-v10?toc=%2fstorage%2ftables%2ftoc.json))。 不支持早期版本的 AzCopy，例如 AzCopy v8.1。|
| **Azure Blob 存储生命周期管理策略** | 受 [Data Lake Storage 多协议访问](data-lake-storage-multi-protocol-access.md)预览版的支持。 冷访问层和存档访问层仅受预览版的支持。 尚不支持删除 Blob 快照的功能。 |
| **Azure 存储资源管理器** | 特定于版本的支持 <br><br>仅使用版本 `1.6.0` 或更高版本。 <br>版本 `1.6.0` 可[免费下载](https://azure.microsoft.com/features/storage-explorer/)。|
| **Blob 容器 ACL** |尚不支持|
| **Blobfuse** |尚不支持|
| **自定义域** |尚不支持|
| **文件系统资源管理器** | 有限支持 |
| **诊断日志记录** |诊断日志受 [Data Lake Storage 多协议访问](data-lake-storage-multi-protocol-access.md)预览版的支持。 <br><br>目前不支持在 Azure 门户中启用日志。 以下示例演示如何使用 PowerShell 来启用日志。 <br><br>`$storageAccount = Get-AzStorageAccount -ResourceGroupName <resourceGroup> -Name <storageAccountName>`<br><br>`Set-AzStorageServiceLoggingProperty -Context $storageAccount.Context -ServiceType Blob -LoggingOperations read,write,delete -RetentionDays <days>`。 <br><br>请务必将 `Blob` 指定为 `-ServiceType` 参数的值，如此示例所示。 <br><br>目前，Azure 存储资源管理器不能用于查看诊断日志。 若要查看日志，请使用 AzCopy 或 SDK。
| **不可变存储** |尚不支持 <br><br>使用不可变存储可以 [WORM（一次写入，多次读取）](/storage/blobs/storage-blob-immutable-storage)状态存储数据。|
| **对象级层** |冷访问层和存档访问层受 [Data Lake Storage 多协议访问](data-lake-storage-multi-protocol-access.md)预览版的支持。 <br><br> 所有其他访问层目前均不受支持。|
| **Powershell 和 CLI 支持** | 受限功能 <br><br>支持管理操作（例如创建帐户）。 数据平面操作（例如上传和下载文件）为公共预览版，是 [Data Lake Storage 多协议访问](data-lake-storage-multi-protocol-access.md)的一部分。 处理目录以及设置访问控制列表 (ACL) 的功能尚不受支持。 |
| **第三方应用程序** | 有限支持 <br><br>对于使用 REST API 保持正常运行的第三方应用程序，如果在 Data Lake Storage Gen2 中使用这些应用程序，则它们可继续正常运行。 <br>调用 Blob API 的应用程序将可能兼容公共预览版 [Data Lake Storage 多协议访问](data-lake-storage-multi-protocol-access.md)。 |
|**软删除** |尚不支持|
| **版本控制功能** |尚不支持 <br><br>这包括[软删除](/storage/blobs/storage-blob-soft-delete)和其他版本控制功能，例如[快照](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob)。|


