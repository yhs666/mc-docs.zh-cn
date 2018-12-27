---
title: 在 Azure 中将大量数据移入/移出云存储 | Microsoft Docs
description: 概述将数据移进和移出 Azure 存储的各种方法。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 08/26/2018
ms.date: 11/05/2018
ms.author: v-jay
ms.component: common
ms.openlocfilehash: abf9e0b15acc4152e9af90b90f36e88ebafe323f
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52652882"
---
# <a name="moving-data-to-and-from-azure-storage"></a>将数据移入和移出 Azure 存储
如果想将本地数据移动到 Azure 存储（或执行相反的操作），有许多种方式可以执行此操作。 最适合的方法因具体情况而异。 本文会提供不同方案以及针对每个方案的适当产品/服务的快速概述。

## <a name="building-applications"></a>构建应用程序
要构建应用程序，可针对 REST API 或我们众多客户端库之一进行开发，将数据移入和移出 Azure 存储。

Azure 存储器为许多常用语言（包括 .NET、Java、Android、Go、Xamarin、C++、Node.JS、PHP、Ruby、Python 和 iOS）提供丰富的客户端库。 客户端库提供高级功能，如重试逻辑、日志记录和并行上传。 也可以直接针对 REST API（可发出 HTTP/HTTPS 请求的任何语言都可调用它）进行开发。

请参阅 [Azure Blob 存储入门](../blobs/storage-dotnet-how-to-use-blobs.md)了解详细信息。

此外，我们还提供了 [Azure 存储数据移动库](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) ，该库专为高性能设计，用于将数据复制到 Azure 和从 Azure 复制数据。 请参阅数据移动库 [文档](https://github.com/Azure/azure-storage-net-data-movement) ，以了解详细信息。 

## <a name="quickly-viewinginteracting-with-your-data"></a>快速查看数据/与数据进行交互
如果希望轻松查看 Azure 存储数据，同时能够上传和下载数据，请考虑使用 Azure 存储资源管理器。

请查看 [Azure 存储资源管理器](../storage-explorers.md)列表了解详细信息。

## <a name="system-administration"></a>系统管理
如果需要或更熟悉命令行实用程序（例如系统管理员），以下是可考虑的一些选项：

### <a name="azcopy"></a>AzCopy
AzCopy 是一个命令行实用程序，旨在实现高性能地将数据复制到 Azure 存储和从 Azure 存储中复制。 还可在存储帐户内或在存储帐户间复制数据。 AzCopy 在 [Windows](storage-use-azcopy.md) 和 [Linux](storage-use-azcopy-linux.md) 上可用。

若要了解如何将本地数据迁移到 Azure 存储，请参阅[教程：使用 AzCopy 将本地数据迁移到云存储](storage-use-azcopy-migrate-on-premises-data.md)。

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell 是一个模块，它提供用于管理 Azure 上的服务的 cmdlet。 这是一种基于任务的命令行 Shell 和脚本语言，专为系统管理而设计。

请参阅[通过 Azure 存储使用 Azure PowerShell](storage-powershell-guide-full.md) 了解详细信息。

### <a name="azure-cli"></a>Azure CLI
Azure CLI 提供了一组开源的跨平台命令，可以用于 Azure 服务。 Azure CLI 在 Windows、OSX 和 Linux 上可用。

请参阅[通过 Azure 存储使用 Azure CLI](../storage-azure-cli.md) 了解详细信息。

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>在慢速网络上移动大量数据
与移动大量数据相关的最大挑战之一是传输时间。 若要从 Azure 存储放入/取出数据而不支付网络成本或编写代码，那么 Azure 导入/导出是合适的解决方案。

请参阅 [Azure 导入/导出](../storage-import-export-service.md)了解详细信息。

## <a name="backing-up-your-data"></a>备份数据
如果只需将数据备份到 Azure 存储，请使用 Azure 备份。 它是用于备份本地数据和 Azure VM 的强大解决方案。

请参阅 [Azure 备份](../../backup/backup-introduction-to-azure-backup.md)了解详细信息。

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>访问本地和云中的数据
如果需要用于访问本地和云中的数据的解决方案，则应考虑使用 Azure 的混合云存储解决方案 StorSimple。 此解决方案包含一个物理 StorSimple 设备，该设备智能地将频繁使用的数据存储在 SSD 上，将偶尔使用的数据存储在 HDD 上，并将非活动/备份/存档数据存储在 Azure 存储上。

## <a name="recovering-your-data"></a>恢复数据
拥有本地工作负荷和应用程序时，需要让业务能够在发生灾难时继续运行的解决方案。 Azure Site Recovery 可以处理虚拟机和物理服务器的复制、故障转移与恢复。 复制的数据存储在 Azure 存储中，因此不再需要辅助现场数据中心。

请参阅 [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) 了解详细信息。
### <a name="moving-data-faq"></a>移动数据常见问题：
## <a name="can-i-migrate-vhds-from-one-region-to-another-without-copying"></a>能否在不复制的情况下，将 VHD 从一个区域迁移到另一个区域？
若要在区域之间复制 VHD，唯一方式是在每个区域的存储帐户之间复制数据。 为此，可以使用 AZCopy。 请参阅“使用 AzCopy 命令行实用程序传输数据”，了解详细信息。 如果是特别大量的数据，也可以使用 Azure 导入/导出。 请参阅 [Azure 导入/导出](/storage/storage-import-export-service)了解详细信息。