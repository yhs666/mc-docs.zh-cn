---
title: 确定何时使用 Azure Blob、Azure 文件或 Azure 磁盘
description: 了解在 Azure 中存储和访问数据的不同方式有助于决定要使用的技术。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 11/28/2018
ms.date: 07/15/2019
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: d788d48de69578055779f2152bcf7c87d1c83eba
ms.sourcegitcommit: 80336a53411d5fce4c25e291e6634fa6bd72695e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844449"
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

## <a name="comparison-files-and-blobs"></a>比较：文件和 Blob

下表将 Azure 文件和 Azure Blob 进行了比较。  
  
||||  
|-|-|-|  
|属性 |Azure Blob |**Azure 文件**|  
|持久性选项|LRS、GRS、RA-GRS|LRS、GRS|  
|辅助功能|REST API|REST API<br /><br /> SMB 2.1 和 SMB 3.0（标准文件系统 API）|  
|连接|REST API -- 全球范围|REST API -- 全球范围<br /><br /> SMB 2.1 -- 区域内<br /><br /> SMB 3.0 -- 全球范围|  
|终结点|`http://myaccount.blob.core.chinacloudapi.cn/mycontainer/myblob`|`\\myaccount.file.core.chinacloudapi.cn\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.chinacloudapi.cn/myshare/myfile.txt`|  
|目录|平面命名空间|真正的目录对象|  
|名称区分大小写|区分大小写|不区分大小写，但保留大小写|  
|容量|最多 2 个 PiB 帐户限制 |5 TiB 文件共享|  
|吞吐量|每个块 Blob 高达 60 MiB/秒|每个共享高达 60 MiB/秒|  
|对象大小|每个块 blob 最多大约 4.75 TiB|每个文件最多为 1 TiB|  
|计费容量|基于写入的字节数|基于文件大小|  
|客户端库|多种语言|多种语言|  
  
## <a name="comparison-files-and-disks"></a>比较：文件和磁盘

Azure 文件是对 Azure 磁盘的补充。 一个磁盘每次只能附加到一个 Azure 虚拟机。 磁盘是作为页 blob 存储在 Azure 存储中的固定格式 VHD，由虚拟机用来存储持久性数据。 Azure 文件中的文件共享可采用与访问本地磁盘相同的方式进行访问（通过本机文件系统 API），并可跨多个虚拟机进行共享。  

下表将 Azure 文件和 Azure 磁盘进行比较。  

||||  
|-|-|-|  
|属性 |**Azure 磁盘**|**Azure 文件**|  
|作用域|专用于单个虚拟机|跨多个虚拟机共享访问|  
|快照和复制|是|是|  
|配置|虚拟机启动时连接|虚拟机启动后连接|  
|身份验证|内置|使用 net use 设置|  
|使用 REST 访问|无法访问 VHD 中的文件|可访问存储在共享中的文件|  
|最大大小|32 TiB 磁盘|5 TiB 文件共享，共享中可保存 1 TiB 文件|  
|最大 IOPS|20,000 IOps|1000 IOps|  
|吞吐量|每个磁盘高达 900 MiB/秒|目标是每个文件共享 60 MiB/秒（更高 IO 大小可以达到更高目标）|  

## <a name="next-steps"></a>后续步骤

决定如何存储和访问数据时，还应考虑涉及的成本。 有关详细信息，请参阅 [Azure 存储定价](https://www.azure.cn/pricing/details/storage/)。
  
某些 SMB 功能不适用于云。 有关详细信息，请参阅 [Features not supported by the Azure File service](https://docs.microsoft.com/rest/api/storageservices/features-not-supported-by-the-azure-file-service)（Azure 文件服务不支持的功能）。
  
有关磁盘的详细信息，请参阅[托管磁盘简介](../../virtual-machines/windows/managed-disks-overview.md)以及[如何将数据磁盘附加到 Windows 虚拟机](../../virtual-machines/windows/attach-managed-disk-portal.md)。
