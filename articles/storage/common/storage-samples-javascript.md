---
title: 使用 JavaScript 的 Azure 存储示例 | Microsoft 文档
description: 查看、下载和运行 Azure 存储的示例代码和应用程序。 使用 JavaScript/Node.js 存储客户端库发现 Blob、队列、表和文件的入门示例。
author: WenJason
ms.author: v-jay
origin.date: 09/26/2019
ms.date: 11/25/2019
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: 92d2b5c6e584760b115e3ed157e943bf3dfb62ce
ms.sourcegitcommit: 6a19227dcc0c6e0da5b82c4f69d0227bf38a514a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74328752"
---
# <a name="azure-storage-samples-using-javascript"></a>使用 JavaScript 的 Azure 存储示例

下表概述了示例存储库和每个示例中涉及的方案。 单击链接可查看 GitHub 中相应的示例代码。

## <a name="blob-samples"></a>Blob 示例

| **方案** | **示例代码** |
|--------------|-----------------|
| 块 blob | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L43) |
| 客户端加密 | [通过 JavaScript 管理 Azure Key Vault 中的存储帐户密钥](https://github.com/Azure-Samples/key-vault-node-storage-accounts) |
| 复制 Blob | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L73) |
| 创建容器 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L54) |
| 删除 Blob | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L103) |
| 删除容器 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L110) |
| Blob 元数据 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L538) |
| Blob 属性 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L478) |
| 容器 ACL | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L444) |
| 容器元数据 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L409) |
| 容器属性 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L377) |
| 获取页面范围 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L170) |
| 租用 Blob | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L216) |
| 租赁容器 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L185) |
| 列出 Blob/容器 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L134) |
| 页 blob | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L129) |
| SAS | [JavaScript 中的共享访问签名](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L257) |
| 服务属性 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L308) |
| 设置 Cors 规则 | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L152) |
| 快照 Blob | [在 JavaScript 中开始使用 Azure Blob 服务](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L79) |

## <a name="file-samples"></a>文件示例

| **方案** | **示例代码** |
|--------------|-----------------|
| 创建共享/目录/文件 | [在 JavaScript 中开始使用 Azure 文件服务](https://github.com/Azure-Samples/storage-file-node-getting-started/blob/master/fileSample.js#L97) |
| 删除共享/目录/文件 | [在 JavaScript 中开始使用 Azure 文件服务](https://github.com/Azure-Samples/storage-file-node-getting-started/blob/master/fileSample.js#L135) |
| 下载文件 | [在 JavaScript 中开始使用 Azure 文件服务](https://github.com/Azure-Samples/storage-file-node-getting-started/blob/master/fileSample.js#L128) |
| 列出目录和文件 | [在 JavaScript 中开始使用 Azure 文件服务](https://github.com/Azure-Samples/storage-file-node-getting-started/blob/master/fileSample.js#L115) |
| 列出共享 | [在 JavaScript 中开始使用 Azure 文件服务](https://github.com/Azure-Samples/storage-file-node-getting-started/blob/master/fileSample.js#L187) |

## <a name="queue-samples"></a>队列示例

| **方案** | **示例代码** |
|--------------|-----------------|
| 添加消息 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L142) |
| 客户端加密 | [通过 JavaScript 管理 Azure Key Vault 中的存储帐户密钥](https://github.com/Azure-Samples/key-vault-node-storage-accounts) |
| 创建队列 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L57) |
| 删除消息 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L164) |
| 删除队列 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L203) |
| 列出队列 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L111) |
| 速览消息 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L170) |
| 队列 ACL | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/advanced.js#L192) |
| 队列 Cors 规则 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/advanced.js#L55) |
| 队列元数据 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/advanced.js#L161) |
| 队列服务属性 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/advanced.js#L94) |
| 队列统计信息 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/advanced.js#L149) |
| 更新消息 | [在 JavaScript 中开始使用 Azure 队列服务](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L176) |

## <a name="table-samples"></a>表示例

| **方案** | **示例代码** |
|--------------|-----------------|
| Batch 实体 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L87) |
| 创建表 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L41) |
| 删除实体/表 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L67) |
| 插入/合并/替换实体 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L49) |
| 列出表 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L63) |
| 查询实体 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L59) |
| 查询表 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L140) |
| 范围查询 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L102) |
| SAS | [JavaScript 中的共享访问签名](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L87) |
| 表 ACL | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L255) |
| 表 Cors 规则 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L149) |
| 表属性 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L188) |
| 表统计信息 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L243) |
| 更新实体 | [在 JavaScript 中开始使用 Azure 表服务](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L49) |

## <a name="azure-code-samples-library"></a>Azure 代码示例库

若要查看完整的示例库，请访问 [Azure 代码示例](https://azure.microsoft.com/resources/samples/?service=storage) 库，其中包括 Azure 存储的示例，可以下载并在本地运行。 代码示例库提供 .zip 格式的示例代码。 此外，用户也可以浏览 GitHub 存储库以获取每个示例并进行克隆。

[!INCLUDE [storage-node-samples-include](../../../includes/storage-node-samples-include.md)]

## <a name="getting-started-guides"></a>入门指南

有关 Azure 存储客户端库的安装和入门说明，请查看以下指南。

* [在 JavaScript 中开始使用 Azure Blob 服务](../blobs/storage-quickstart-blobs-nodejs.md)
* [在 JavaScript 中开始使用 Azure 队列服务](../queues/storage-nodejs-how-to-use-queues.md)
* [在 JavaScript 中开始使用 Azure 表服务](../../cosmos-db/table-storage-how-to-use-nodejs.md)

## <a name="next-steps"></a>后续步骤

了解其他语言的示例：

* .NET:[使用 .NET 的 Azure 存储示例](storage-samples-dotnet.md)
* Java:[使用 Java 的 Azure 存储示例](storage-samples-java.md)
* Python:[使用 Python 的 Azure 存储示例](storage-samples-python.md)
* 所有其他语言：[Azure 存储示例](storage-samples.md)
