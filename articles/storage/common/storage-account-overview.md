---
title: Azure 存储帐户概述 | Microsoft Docs
description: 了解创建和使用 Azure 存储帐户所需的选项。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 09/13/2018
ms.date: 09/24/2018
ms.author: v-jay
ms.component: common
ms.openlocfilehash: 87289294b1a16001ffc5603b3eeb69ce4a8bd8bf
ms.sourcegitcommit: 0081fb238c35581bb527bdd704008c07079c8fbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523745"
---
# <a name="azure-storage-account-overview"></a>Azure 存储帐户概述

Azure 存储帐户包含所有的 Azure 存储数据对象：Blob、文件、队列、表和磁盘。 Azure 存储帐户中的数据具有持久、高度可用、安全、可大规模缩放的特点，并且可以通过 HTTP 或 HTTPS 从世界上的任何位置访问。 

若要了解如何创建 Azure 存储帐户，请参阅[创建存储帐户](storage-quickstart-create-account.md)。

## <a name="types-of-storage-accounts"></a>存储帐户的类型

Azure 存储提供三种类型的存储帐户。 每种类型支持不同的功能，并且具有自己的定价模型。 在创建存储帐户之前，需考虑到这些差异，以便确定最适合应用程序的帐户类型。 存储帐户的类型包括：

* **常规用途 v2** 帐户（建议用于大多数情况）
* **常规用途 v1** 帐户
* **Blob 存储**帐户

下表介绍存储帐户的类型及其功能：

| 存储帐户类型 | 支持的服务                       | 支持的性能层 | 支持的访问层               | 复制选项                                                | 部署模型<sup>1</sup>  | 加密<sup>2</sup> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------------|--------------------------------------------------------------------|-------------------|------------|
| 常规用途 V2   | Blob、文件、队列、表和磁盘       | 标准、高级           | 热、冷 | LRS、GRS、RA-GRS | Resource Manager | 加密  |
| 常规用途 V1   | Blob、文件、队列、表和磁盘       | 标准、高级           | 不适用                                  | LRS、GRS、RA-GRS                                                   | 资源管理器、经典  | 加密  |
| Blob 存储         | Blob（仅块 Blob 和追加 Blob） | 标准                    | 热、冷                            | LRS、GRS、RA-GRS                                                   | Resource Manager  | 加密  |

<sup>1</sup>建议使用 Azure 资源管理器部署模型。 使用经典部署模型的存储帐户仍可在某些位置创建，而现有的经典帐户仍然会受支持。 有关详细信息，请参阅 [Azure 资源管理器与经典部署：了解部署模型和资源状态](../../azure-resource-manager/resource-manager-deployment-model.md)。

<sup>2</sup>所有存储帐户均使用存储服务加密 (SSE) 对静态数据进行加密。 有关详细信息，请参阅[静态数据的 Azure 存储服务加密](storage-service-encryption.md)。

### <a name="general-purpose-v2-accounts"></a>常规用途 v2 帐户

常规用途 v2 存储帐户支持最新的 Azure 存储功能，并纳入了常规用途 v1 存储帐户和 Blob 存储帐户的所有功能。 常规用途 v2 帐户提供适用于 Azure 存储的最低单 GB 容量价格，以及具有行业竞争力的事务价格。 常规用途 v2 存储帐户支持以下 Azure 存储服务：

- Blob（所有类型）
- 文件
- 磁盘
- 队列
- 表

大多数情况下，建议使用常规用途 v2 存储帐户。 可以轻松地将常规用途 v1 存储帐户或 Blob 存储帐户升级为常规用途 v2 帐户，不需停机，不需应用程序重写，也不需复制数据。 若要详细了解如何升级到常规用途 v2 帐户，请参阅[升级到常规用途 v2 存储帐户](storage-account-upgrade.md)。 

常规用途 v2 存储帐户提供多个访问层，可以根据使用模式来存储数据。 有关详细信息，请参阅 [Blob 数据的访问层](#access-tiers-for-blob-data)。

### <a name="general-purpose-v1-accounts"></a>常规用途 v1 帐户

常规用途 v1 帐户可以访问所有 Azure 存储服务，但可能没有最新功能，其单 GB 定价也可能不是最低的。 常规用途 v1 存储帐户支持以下 Azure 存储服务：

- Blob（所有类型）
- 文件
- 磁盘
- 队列
- 表

大多数情况下建议使用常规用途 v2 帐户，但以下情况最好是使用常规用途 v1 帐户： 

* 应用程序要求使用 Azure 经典部署模型。 常规用途 v2 帐户和 Blob 存储帐户只支持 Azure 资源管理器部署模型。

* 应用程序为事务密集型，或者使用很大的异地复制带宽，但不需要大的容量。 在这种情况下，常规用途 v1 可能是最经济的选择。

* 使用了早于 2014-02-14 的[存储服务 REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) 的版本或使用了版本低于 4.x 的客户端库，并且无法升级你的应用程序。

### <a name="blob-storage-accounts"></a>Blob 存储帐户

Blob 存储帐户是一种专用存储帐户，可以将非结构化对象数据作为块 Blob 存储。 Blob 存储帐户提供的持久性、可用性、可伸缩性和性能特性与常规用途 v2 存储帐户提供的相同。 Blob 存储帐户支持存储块 Blob和追加 Blob，但不支持存储页 Blob。

Blob 存储帐户提供多个访问层，可以根据使用模式来存储数据。 有关详细信息，请参阅 [Blob 数据的访问层](#access-tiers-for-blob-data)。

## <a name="naming-storage-accounts"></a>为存储帐户命名

为存储帐户命名时，请记住以下规则：

- 存储帐户名称必须为 3 到 24 个字符，并且只能包含数字和小写字母。
- 存储帐户名称在 Azure 中必须是唯一的。 没有两个存储帐户可以有相同的名称。

## <a name="performance-tiers"></a>性能层

可以针对下述两个性能层之一配置常规用途存储帐户：

* 标准存储性能层，用于存储 Blob、文件、表、队列和 Azure 虚拟机磁盘。
* 高级存储性能层，仅用于存储 Azure 虚拟机磁盘。 有关高级存储的详细概述，请参阅 [高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../../virtual-machines/windows/premium-storage.md) 。

## <a name="access-tiers-for-block-blob-data"></a>块 Blob 数据的访问层

Azure 存储提供不同的选项，适用于根据使用模型访问块 Blob 数据。 Azure 存储中的每个访问层都针对特定的数据使用模式进行了优化。 根据需要选择适当的访问层以后，即可以最经济有效的方式存储块 Blob 数据。

可用的访问层包括：

* **热**访问层，已针对存储帐户中的频繁访问的对象进行了优化。 访问热层中的数据最经济有效，但存储费用稍高。 新的存储帐户默认在热层中创建。
* **冷**访问层，适用于存储大量不常访问且存储时间至少为 30 天的数据。 将数据存储在冷层中更经济有效，但访问该数据的费用与访问热层中的数据相比，可能会稍高一些。

如果数据的使用模式有所更改，可以随时在这些访问层之间切换。 

> [!IMPORTANT]
> 更改现有存储帐户或 Blob 的访问层可能会产生额外费用。

有关访问层的详细信息，请参阅 [Azure Blob 存储：热、冷和存档存储层](../blobs/storage-blob-storage-tiers.md)。

## <a name="replication"></a>复制

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

有关存储复制的详细信息，请参阅 [Azure 存储复制](storage-redundancy.md)。

## <a name="encryption"></a>Encryption

存储帐户中的所有数据均在服务端加密。 有关加密的详细信息，请参阅[静态数据的 Azure 存储服务加密](storage-service-encryption.md)。

## <a name="storage-account-endpoints"></a>存储帐户终结点

存储帐户在 Azure 中为数据提供唯一的命名空间。 存储在 Azure 存储中的每个对象都有一个地址，其中包含唯一的帐户名称。 将帐户名称与 Azure 存储服务终结点组合在一起，即可构成适用于存储帐户的终结点。

例如，如果常规用途存储帐户名为 *mystorageaccount*，则该帐户的默认终结点为：

* Blob 存储： http://*mystorageaccount*.blob.core.chinacloudapi.cn
* 表存储： http://*mystorageaccount*.table.core.chinacloudapi.cn
* 队列存储： http://*mystorageaccount*.queue.core.chinacloudapi.cn
* Azure 文件： http://*mystorageaccount*.file.core.chinacloudapi.cn

> [!NOTE]
> Blob 存储帐户仅公开 Blob 服务终结点。

用于访问存储帐户中某个对象的 URL 是通过将对象在存储帐户中的位置追加到终结点后面而构造的。 例如，Blob 地址可能具有以下格式： http://*mystorageaccount*.blob.core.chinacloudapi.cn/*mycontainer*/*myblob*。

也可将存储帐户配置为对 Blob 使用自定义域。 有关详细信息，请参阅[为 Azure 存储帐户配置自定义域名](../blobs/storage-custom-domain-name.md)。  

## <a name="control-access-to-account-data"></a>控制对帐户数据的访问

默认情况下，只有你，即帐户所有者，才能使用帐户中的数据。 你可以控制哪些用户可以访问你的数据，以及这些用户可以有什么权限。

对存储帐户发出的每个请求都必须获得授权。 在服务级别，请求必须包含有效的 *Authorization* 标头，该标头包含服务在执行请求之前对其进行验证所需的所有信息。

可以通过下述任意方法授予对存储帐户中数据的访问权限：

- **共享密钥授权：** 使用存储帐户访问密钥构造一个连接字符串，供应用程序在运行时用于访问 Azure 存储。 连接字符串中的值用于构造传递给 Azure 存储的 *Authorization* 标头。 有关详细信息，请参阅[配置 Azure 存储连接字符串](storage-configure-connection-string.md)。
- **共享访问签名：** 使用共享访问签名对存储帐户中的资源进行委托访问。 共享访问签名是一个令牌，其中封装了对目标对象为 URL 上的 Azure 存储的请求进行授权所需的所有信息。 可以在共享访问签名中指定存储资源、授予的权限以及权限有效时间间隔。 有关详细信息，请参阅[使用共享访问签名 (SAS)](storage-dotnet-shared-access-signature-part-1.md)。

## <a name="copying-data-into-a-storage-account"></a>将数据复制到存储帐户中

我们提供的实用程序和库适用于从本地存储设备或第三方云存储提供商处导入数据。 使用哪种解决方案取决于要传输的数据量。 

从常规用途 v1 存储帐户或 Blob 存储帐户升级到常规用途 v2 帐户时，数据会自动迁移。 建议使用此路径来升级帐户。 但是，如果决定将数据从常规用途 v1 帐户移到 Blob 存储帐户，则需使用下述工具和库手动迁移数据。 

### <a name="azcopy"></a>AzCopy

AzCopy 是一个 Windows 命令行实用程序，用于将数据高性能复制到 Azure 存储（或从中进行复制）。 可以使用 AzCopy 将数据从现有的常规用途存储帐户复制到 Blob 存储帐户，或者将数据从本地存储设备上传。 有关详细信息，请参阅[使用 AzCopy 命令行实用程序传输数据](../common/storage-use-azcopy.md?toc=%2fstorage%2fblobs%2ftoc.json)。

### <a name="data-movement-library"></a>数据移动库

适用于 .NET 的 Azure 存储数据移动库基于为 AzCopy 提供技术支持的核心数据移动框架。 库旨在实现类似于 AzCopy 的高性能、可靠且简单的数据传输操作。 可以通过它以本机模式充分利用应用程序中 AzCopy 提供的功能，无需运行和监视 AzCopy 的外部实例。 有关详细信息，请参阅[适用于 .Net 的 Azure 存储数据移动库](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>REST API 或客户端库

可以创建自定义应用程序以使用其中一个 Azure 客户端库或 Azure 存储服务 REST API 将数据迁移到 Blob 存储帐户。 Azure 存储对多种语言和平台（如 .NET、Java、C++、Node.JS、PHP、Ruby 和 Python）提供了内容丰富的客户端库。 客户端库提供高级功能，如重试逻辑、日志记录和并行上传。 也可以直接针对 REST API（可发出 HTTP/HTTPS 请求的任何语言都可调用它）进行开发。

有关 Azure 存储 REST API 的详细信息，请参阅 [Azure Storage Services REST API Reference](https://docs.microsoft.com/rest/api/storageservices/)（Azure 存储服务 REST API 参考）。 

> [!IMPORTANT]
> 使用客户端加密进行加密的 Blob 会将与加密相关的元数据与 Blob 一起存储。 如果复制使用客户端加密来加密的 Blob，请确保复制操作保留 Blob 元数据，尤其是与加密相关的元数据。 如果复制不包含此加密元数据的 Blob，则不能再次检索 Blob 内容。 有关加密相关元数据的详细信息，请参阅 [Azure 存储客户端加密](../common/storage-client-side-encryption.md?toc=%2fstorage%2fblobs%2ftoc.json)。

### <a name="azure-importexport-service"></a>Azure 导入/导出服务

如果有大量需要导入到存储帐户中的数据，可考虑使用 Azure 导入/导出服务。 使用导入/导出服务，可将磁盘驱动器寄送到 Azure 数据中心，以便将大量数据安全地导出到 Azure Blob 存储和 Azure 文件。 

也可以使用导入/导出服务将数据从 Azure Blob 存储传输到磁盘驱动器，然后再寄送到本地站点。 可将单个或多个磁盘驱动器中的数据导入到 Azure Blob 存储或 Azure 文件。 有关详细信息，请参阅[什么是 Azure 导入/导出服务？](/storage/common/storage-import-export-service)

## <a name="storage-account-billing"></a>存储帐户计费

[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

## <a name="next-steps"></a>后续步骤

* 若要了解如何创建 Azure 存储帐户，请参阅[创建存储帐户](storage-quickstart-create-account.md)。
* 若要管理或删除现有的存储帐户，请参阅[管理 Azure 存储帐户](storage-account-manage.md)。
