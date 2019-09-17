---
title: 确定何时使用 Azure Blob、Azure 文件或 Azure 磁盘
description: 了解在 Azure 中存储和访问数据的不同方式有助于决定要使用的技术。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 11/28/2018
ms.date: 09/09/2019
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: d92166ff86b347bd2a028531ab56175f8a738123
ms.sourcegitcommit: 66a77af2fab8a5f5b34723dc99e4d7ce0c380e78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2019
ms.locfileid: "70209352"
---
# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-disks"></a>确定何时使用 Azure Blob、Azure 文件或 Azure 磁盘

Azure 在 Azure 存储中提供多种功能，用于在云中存储和访问数据。 本文介绍 Azure 文件、Blob 和磁盘，旨在帮助用户选择合适的功能。

## <a name="scenarios"></a>方案

下表比较了文件、Blob 和磁盘，并显示每种技术适合的示例情景。

| 功能 | 说明 | 何时使用 |
|--------------|-------------|-------------|
| Azure 文件  | 提供 SMB 接口、客户端库和允许从任何位置访问存储文件的 [REST 接口](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api)。 | 希望将应用程序“提升和移动”到已使用本机文件系统 API 的云中，以此在该应用程序和 Azure 中运行的其他应用程序之间共享数据时。<br/><br/>希望存储需要从多个虚拟机访问的开发和调试工具时。 |
| Azure Blob  | 提供客户端库，以及一个可用于在块 blob 中大规模存储和访问非结构化数据的 [REST 接口](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api)。<br/><br/>还支持 [Azure Data Lake Storage Gen2](../blobs/data-lake-storage-introduction.md)，用于企业大数据分析解决方案。 | 希望应用程序支持流式处理和随机访问方案时。<br/><br/>希望可以从任何位置访问应用程序数据时。<br/><br/>想要在 Azure 上生成企业数据湖并执行大数据分析。 |
| **Azure 磁盘** | 提供客户端库和 [REST 接口](https://docs.microsoft.com/rest/api/compute/manageddisks/disks/disks-rest-api)，借助该接口可通过附加的虚拟硬盘永久地存储和访问数据。 | 希望提升和移动使用本机文件系统 API 将数据读写到永久性磁盘中应用程序时。<br/><br/>希望存储不要求从附加磁盘的虚拟机外进行访问的数据时。 |


## <a name="next-steps"></a>后续步骤

决定如何存储和访问数据时，还应考虑涉及的成本。 有关详细信息，请参阅 [Azure 存储定价](https://www.azure.cn/pricing/details/storage/)。
  
某些 SMB 功能不适用于云。 有关详细信息，请参阅 [Features not supported by the Azure File service](https://docs.microsoft.com/rest/api/storageservices/features-not-supported-by-the-azure-file-service)（Azure 文件服务不支持的功能）。
 
有关 Azure Blob 的详细信息，请参阅[什么是 Azure Blob 存储？](../blobs/storage-blobs-overview.md)一文。

有关磁盘存储的详细信息，请参阅[托管磁盘简介](../../virtual-machines/windows/managed-disks-overview.md)。

有关 Azure 文件存储的详细信息，请参阅 [Azure 文件存储部署规划](../files/storage-files-planning.md)一文。