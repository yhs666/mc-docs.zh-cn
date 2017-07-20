---
title: "如何通过 Python 使用 Azure 文件存储 | Azure"
description: "了解如何通过 Python 使用 Azure 文件存储上传、列出、下载和删除文件。"
services: storage
documentationcenter: python
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
origin.date: 12/08/2016
ms.date: 01/06/2017
ms.author: v-johch
ms.openlocfilehash: 420a5bc304151ada6bb4597ebc673a0cdf9cfddb
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="how-to-use-azure-file-storage-from-python"></a>如何通过 Python 使用 Azure 文件存储

[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>概述
本文将演示如何使用文件存储执行常见方案。 这些示例用 Python 编写并使用 [Microsoft Azure Storage SDK for Python]。 涉及的方案包括上传、列出、下载以及删除文件。

[!INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>创建共享
通过 **FileService** 对象，可使用共享、目录和文件。 以下代码创建 **FileService** 对象。 在要在其中以编程方式访问 Azure 存储的 Python 文件中，在顶部附近添加以下内容：

```python
from azure.storage.file import FileService
```

以下代码使用存储帐户名称和帐户密钥创建 **FileService** 对象。  将“myaccount”和“mykey”替换为你的帐户名称和密钥。

```python
file_service = **FileService** (account_name='myaccount', account_key='mykey',endpoint_suffix='core.chinacloudapi.cn')
```

在以下代码示例中，如果共享不存在，可以使用 **FileService** 对象来创建它。

```python
file_service.create_share('myshare')
```

## <a name="upload-a-file-into-a-share"></a>将文件上传到共享

Azure 文件存储共享至少包含文件所在的根目录。 在本部分，你将学习如何将文件从本地存储上传到共享所在的根目录。

若要创建文件和上传数据，请使用 **create\_file\_from\_path**、**create\_file\_from\_stream**、**create\_file\_from\_bytes** 或 **create\_file\_from\_text** 方法。 这些方法属于高级方法，用于在数据大小超过 64 MB 时执行必要的分块。

**create\_file\_from\_path** 用于从指定路径上传文件中的内容，**create\_file\_from\_stream** 用于从打开的文件/流上传内容。 **create\_file\_from\_bytes** 用于上传一组字节，**create\_file\_from\_text** 可使用指定的编码（默认为 UTF-8）上传指定的文本值。

下面的示例将 **sunset.png** 文件的内容上传到 **myfile** 文件中。

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

## <a name="how-to-create-a-directory"></a>如何：创建目录

也可将文件置于子目录中，而不将其全部置于根目录中，以便对存储进行有效的组织。 Azure 文件存储服务允许创建帐户所允许任意数目的目录。 以下代码将在根目录下创建名为 **sampledir** 的子目录。

```python
file_service.create_directory('myshare', 'sampledir')
```

## <a name="how-to-list-files-and-directories-in-a-share"></a>如何：列出共享中的文件和目录

若要列出共享中的文件和目录，请使用 **list\_directories\_and\_files** 方法。 此方法会返回一个生成器。 以下代码将共享中每个文件和目录的 **名称** 输出到控制台。

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

## <a name="download-files"></a>下载文件

若要从文件下载数据，请使用 **get\_file\_to\_path**、**get\_file\_to\_stream**、**get\_file\_to\_bytes** 或 **get\_file\_to\_text**。 这些方法属于高级方法，用于在数据大小超过 64 MB 时执行必要的分块。

以下示例演示了如何使用 **get\_file\_to\_path** 下载 **myfile** 文件的内容，并将其存储到 **out-sunset.png** 文件。

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

## <a name="delete-a-file"></a>删除文件

最后，若要删除文件，请调用 **delete_file**。

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a>后续步骤

现在，你已了解文件存储的基础知识，可单击下面的链接了解详细信息。

- [Python 开发人员中心](/develop/python/)
- [Azure 存储服务 REST API](http://msdn.microsoft.com/zh-cn/library/azure/dd179355)
- [Azure 存储团队博客]
- [Microsoft Azure Storage SDK for Python]

[Azure 存储团队博客]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK for Python]: https://github.com/Azure/azure-storage-python