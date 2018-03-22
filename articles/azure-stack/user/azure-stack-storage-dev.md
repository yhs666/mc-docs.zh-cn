---
title: Azure Stack 存储开发工具入门
description: 使用 Azure Stack 存储开发工具的入门指南
services: azure-stack
author: xiaofmao
ms.author: v-junlch
origin.date: 09/25/2017
ms.date: 03/09/2018
ms.topic: get-started-article
ms.service: azure-stack
ms.openlocfilehash: 9c59f088bbf730c0a9e170bee9306bb0c0d5fe99
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-azure-stack-storage-development-tools"></a>Azure Stack 存储开发工具入门

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*


Azure Stack 提供一组存储服务，包括 Azure Blob、表和队列存储。

本文提供如何开始使用 Azure Stack 存储开发工具的快速指南。 可以在对应的 Azure 存储教程中，找到更多的详细信息和示例代码。

Azure 存储和 Azure Stack 存储之间有一些已知的差异，包括每个平台的一些特定需求。 例如，Azure Stack 有特定的客户端库以及特定的终结点后缀需求。 有关详细信息，请参阅 [Azure Stack 存储：差异和注意事项](azure-stack-acs-differences.md)。

## <a name="azure-client-libraries"></a>Azure 客户端库
Azure Stack 存储支持的 REST API 版本是 2015-04-05。 它不同于最新版的 Azure 存储 REST API。 因此，对于存储客户端库，需要知道与 REST API 2015-04-05 兼容的版本。


|客户端库|Azure Stack 支持的版本|链接|终结点规范|
|---------|---------|---------|---------|
|.NET     |6.2.0|Nuget 包：<br>[https://www.nuget.org/packages/WindowsAzure.Storage/6.2.0](https://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)<br><br>GitHub 版本：<br>[https://github.com/Azure/azure-storage-net/releases/tag/v6.2.1](https://github.com/Azure/azure-storage-net/releases/tag/v6.2.1)|app.config 文件|
|Java|4.1.0|Maven 包：<br>[http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/4.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/4.1.0)<br><br>GitHub 版本：<br> [https://github.com/Azure/azure-storage-java/releases/tag/v4.1.0](https://github.com/Azure/azure-storage-java/releases/tag/v4.1.0)|连接字符串设置|
|Node.js     |1.1.0|NPM 链接：<br>[https://www.npmjs.com/package/azure-storage](https://www.npmjs.com/package/azure-storage)<br>（运行：`npm install azure-storage@1.1.0)`<br><br>Github 版本：<br>[https://github.com/Azure/azure-storage-node/releases/tag/1.1.0](https://github.com/Azure/azure-storage-node/releases/tag/1.1.0)|服务实例声明||C++|2.4.0|Nuget 包：<br>[https://www.nuget.org/packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>GitHub 版本：<br>[https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|连接字符串设置|
|C++|2.4.0|Nuget 包：<br>[https://www.nuget.org/packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>GitHub 版本：<br>[https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|连接字符串设置|
|PHP|0.15.0|GitHub 版本：<br>[https://github.com/Azure/azure-storage-php/releases/tag/v0.15.0](https://github.com/Azure/azure-storage-php/releases/tag/v0.15.0)<br><br>通过编辑器安装（请参阅下面的详细信息）|连接字符串设置|
|Python     |0.30.0|PIP 包：<br> [https://pypi.python.org/pypi/azure-storage/0.30.0](https://pypi.python.org/pypi/azure-storage/0.30.0)<br>（运行：`pip install -v azure-storage==0.30.0)`<br><br>GitHub 版本：<br> [https://github.com/Azure/azure-storage-python/releases/tag/v0.30.0](https://github.com/Azure/azure-storage-python/releases/tag/v0.30.0)|服务实例声明|
|Ruby|0.12.1<br>预览|RubyGems 包：<br> [https://rubygems.org/gems/azure-storage/versions/0.12.1.preview](https://rubygems.org/gems/azure-storage/versions/0.12.1.preview)<br><br>GitHub 版本：<br> [https://github.com/Azure/azure-storage-ruby/releases/tag/v0.12.1](https://github.com/Azure/azure-storage-ruby/releases/tag/v0.12.1)|连接字符串设置|

> [!NOTE]
> PHP 详细信息<br><br>
>若要通过编辑器安装，请执行以下操作：
>1. 在项目的根目录中，使用以下代码创建一个名为 `composer.json` 的文件：<br>
>
>   ```
>   {
>       "require":{
>           "Microsoft/azure-storage":"0.15.0"
>        }
>    }
>   ```
>
>2. 将 [composer.phar](http://getcomposer.org/composer.phar) 下载到项目根目录中。
>3. 运行：`php composer.phar install`。
>


## <a name="endpoint-declaration"></a>终结点声明
Azure Stack 终结点包含两个部分：区域的名称和 Azure Stack 域。
在 Azure Stack 开发工具包中，默认终结点是 **local.azurestack.external**。
如果不确定你的终结点，请与云管理员联系。

## <a name="examples"></a>示例


### <a name="net"></a>.NET

对于 Azure Stack，在 app.config 文件中指定终结点后缀：

```
<add key="StorageConnectionString" 
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=local.azurestack.external;" />
```
### <a name="java"></a>Java

对于 Azure Stack，在连接字符串的设置中指定终结点后缀：

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=local.azurestack.external";
```

### <a name="nodejs"></a>Node.js

对于 Azure Stack，在声明实例中指定终结点后缀：

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob.local.azurestack.external');
```
### <a name="c"></a>C++

对于 Azure Stack，在连接字符串的设置中指定终结点后缀：

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=local.azurestack.external"));
```

### <a name="php"></a>PHP

对于 Azure Stack，在连接字符串的设置中指定终结点后缀：

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.local.azurestack.external/;
QueueEndpoint=http:// <storage account name>.queue.local.azurestack.external/;
TableEndpoint=http:// <storage account name>.table.local.azurestack.external/;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

对于 Azure Stack，在声明实例中指定终结点后缀：

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix='local.azurestack.external')
```
### <a name="ruby"></a>Ruby

对于 Azure Stack，在连接字符串的设置中指定终结点后缀：

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=local.azurestack.external
```

## <a name="blob-storage"></a>Blob 存储

以下 Azure Blob 存储教程适用于 Azure Stack。 请注意前面[示例](#examples)部分中所述的 Azure Stack 特定终结点后缀需求。

- [通过 .NET 开始使用 Azure Blob 存储](../../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [如何通过 Java 使用 Blob 存储](../../storage/blobs/storage-java-how-to-use-blob-storage.md)
- [如何通过 Node.js 使用 Blob 存储]../../storage/blobs/storage-nodejs-how-to-use-blob-storage.md)
- [如何通过 C++ 使用 Blob 存储](../../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
- [如何通过 PHP 使用 Blob 存储](../../storage/blobs/storage-php-how-to-use-blobs.md)
- [如何通过 Python 使用 Azure Blob 存储](../../storage/blobs/storage-python-how-to-use-blob-storage.md)
- [如何通过 Ruby 使用 Blob 存储](../../storage/blobs/storage-ruby-how-to-use-blob-storage.md)

## <a name="queue-storage"></a>队列存储

以下 Azure 队列存储教程适用于 Azure Stack。 请注意前面[示例](#examples)部分中所述的 Azure Stack 特定终结点后缀需求。

- [通过 .NET 开始使用 Azure 队列存储](../../storage/queues/storage-dotnet-how-to-use-queues.md)
- [如何通过 Java 使用队列存储](../../storage/queues/storage-java-how-to-use-queue-storage.md)
- [如何通过 Node.js 使用队列存储](../../storage/queues/storage-nodejs-how-to-use-queues.md)
- [如何通过 C++ 使用队列存储](../../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
- [如何通过 PHP 使用队列存储](../../storage/queues/storage-php-how-to-use-queues.md)
- [如何通过 Python 使用队列存储](../../storage/queues/storage-python-how-to-use-queue-storage.md)
- [如何通过 Ruby 使用队列存储](../../storage/queues/storage-ruby-how-to-use-queue-storage.md)


## <a name="table-storage"></a>表存储

以下 Azure 表存储教程适用于 Azure Stack。 请注意前面[示例](#examples)部分中所述的 Azure Stack 特定终结点后缀需求。

- [通过 .NET 开始使用 Azure 表存储](../../cosmos-db/table-storage-how-to-use-dotnet.md)
- [如何通过 Java 使用表存储](../../cosmos-db/table-storage-how-to-use-java.md)
- [如何通过 Node.js 使用 Azure 表存储](../../cosmos-db/table-storage-how-to-use-nodejs.md)
- [如何通过 C++ 使用表存储](../../cosmos-db/table-storage-how-to-use-c-plus.md)
- [如何通过 PHP 使用表存储](../../cosmos-db/table-storage-how-to-use-php.md)
- [如何在 Python 中使用表存储](../../cosmos-db/table-storage-how-to-use-python.md)
- [如何通过 Ruby 使用表存储](../../cosmos-db/table-storage-how-to-use-ruby.md)

## <a name="next-steps"></a>后续步骤

- [Azure 存储简介](../../storage/common/storage-introduction.md)

