---
title: "确定何时使用 Azure Blob、Azure 文件或 Azure 数据磁盘"
description: "了解在 Azure 中存储和访问数据的不同方式有助于决定要使用的技术。"
services: storage
documentationcenter: 
author: hayley244
manager: digimobile
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/13/2017
ms.date: 
ms.author: v-haiqya
ms.openlocfilehash: 3e3524c6754d79895f6196b0434aeaff133927b7
ms.sourcegitcommit: 0f2694b659ec117cee0110f6e8554d96ee3acae8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-data-disks"></a>确定何时使用 Azure Blob、Azure 文件或 Azure 数据磁盘

Azure 在 Azure 存储中提供多种功能，用于在云中存储和访问数据。 本文介绍 Azure 文件、Blob 和数据磁盘，旨在帮助你在这些功能中进行选择。

## <a name="scenarios"></a>方案

下表将文件、Blob 和数据磁盘进行比较，并显示各自适用的示例方案。

| 功能 | 说明 | 何时使用 |
|--------------|-------------|-------------|
| Azure 文件 | 提供 SMB 接口、客户端库和允许从任何位置访问存储文件的 [REST 接口](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api)。 | 希望将应用程序“提升和移动”到已使用本机文件系统 API 的云中，以此在该应用程序和 Azure 中运行的其他应用程序之间共享数据时。<br/><br/>希望存储需要从多个虚拟机访问的开发和调试工具时。 |
| Azure Blob | 提供客户端库和允许在块 blob 中大规模存储和访问非结构化数据的 [REST 接口](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api)。 | 希望应用程序支持流式处理和随机访问方案时。<br/><br/>希望可以从任何位置访问应用程序数据时。 |
| Azure 数据磁盘 | 提供客户端库和允许数据永久存储在附加虚拟硬盘中并可从中进行访问的 [REST 接口](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-create-or-update)。 | 希望提升和移动使用本机文件系统 API 将数据读写到永久性磁盘中应用程序时。<br/><br/>希望存储不要求从附加磁盘的虚拟机外进行访问的数据时。 |

## <a name="comparison-files-and-blobs"></a>比较：文件和 Blob

下表将 Azure 文件和 Azure Blob 进行了比较。  
  
||||  
|-|-|-|  
|属性|Azure Blob|Azure 文件|  
|持久性选项|LRS、ZRS、GRS（和实现更高可用性的 RA-GRS）|LRS、GRS|  
|辅助功能|REST API|REST API<br /><br /> SMB 2.1 和 SMB 3.0（标准文件系统 API）|  
|连接|REST API -- 全球范围|REST API -- 全球范围<br /><br /> SMB 2.1 -- 区域内<br /><br /> SMB 3.0 -- 全球范围|  
|终结点|`http://myaccount.blob.core.chinacloudapi.cn/mycontainer/myblob`|`\\myaccount.file.core.chinacloudapi.cn\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.chinacloudapi.cn/myshare/myfile.txt`|  
|目录|平面命名空间|真正的目录对象|  
|名称区分大小写|区分大小写|不区分大小写，但保留大小写|  
|容量|最多 500 TB 容器|5 TB 文件共享|  
|吞吐量|每个块 blob 最高 60 MB/秒|每个共享最高 60 MB/秒|  
|对象大小|最多 200 GB/块 blob|最多 1 TB/文件|  
|计费容量|基于写入的字节数|基于文件大小|  
|客户端库|多种语言|多种语言|  
  
## <a name="comparison-files-and-data-disks"></a>比较：文件和数据磁盘

Azure 文件是 Azure 数据磁盘的补充。 一次只可以向一个 Azure 虚拟机附加一个数据磁盘。 数据磁盘是格式固定的 VHD（在 Azure 存储中存储为页 blob），可被虚拟机用来存储持久数据。 Azure 文件中的文件共享可采用与访问本地磁盘相同的方式进行访问（通过本机文件系统 API），并可跨多个虚拟机进行共享。  
 
下表将 Azure 文件和 Azure 数据磁盘进行了比较。  
 
||||  
|-|-|-|  
|属性|Azure 数据磁盘|Azure 文件|  
|范围|专用于单个虚拟机|跨多个虚拟机共享访问|  
|快照和复制|是|否|  
|配置|虚拟机启动时连接|虚拟机启动后连接|  
|身份验证|内置|使用 net use 设置|  
|清理|自动|手动|  
|使用 REST 访问|无法访问 VHD 中的文件|可访问存储在共享中的文件|  
|最大大小|1 TB 磁盘|5 TB 文件共享，共享内 1 TB 的文件|  
|最大 8KB IOps|500 IOps|1000 IOps|  
|吞吐量|每个磁盘最高 60 MB/秒|每个文件共享最高 60 MB/秒|  

## <a name="next-steps"></a>后续步骤

决定如何存储和访问数据时，还应考虑涉及的成本。 有关详细信息，请参阅 [Azure 存储定价](https://www.azure.cn/pricing/details/storage/)。
  
某些 SMB 功能不适用于云。 有关详细信息，请参阅 [Features not supported by the Azure File service](https://docs.microsoft.com/rest/api/storageservices/features-not-supported-by-the-azure-file-service)（Azure 文件服务不支持的功能）。

有关数据磁盘的详细信息，请参阅[管理磁盘和映像](../../virtual-machines/windows/about-disks-and-vhds.md)和[如何将数据磁盘附加到 Windows 虚拟机](../../virtual-machines/windows/classic/attach-disk.md)。

<!--Update_Description: update link-->