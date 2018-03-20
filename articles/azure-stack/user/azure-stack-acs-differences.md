---
title: "Azure Stack 存储：差异和注意事项"
description: "了解 Azure Stack 存储与 Azure 存储之间的差异，以及 Azure Stack 部署注意事项。"
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
ms.reviwer: xiaofmao
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 11/08/2017
ms.date: 03/08/2018
ms.author: v-junlch
ms.openlocfilehash: 0d7e9de1e244004e15cceabfb724806dd121a98a
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Azure Stack 存储：差异和注意事项

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

Azure Stack 存储是 Azure Stack 中的一组存储云服务。 Azure Stack 存储使用与 Azure 一致的语义来提供 Blob、表、队列和帐户管理功能。

此文汇总了 Azure Stack 存储与 Azure 存储之间的已知差异。 此外，还汇总了部署 Azure Stack 时要注意的其他事项。 有关 Azure Stack 与 Azure 之间大致差异的详细信息，请参阅[重要注意事项](azure-stack-considerations.md)主题。

## <a name="cheat-sheet-storage-differences"></a>速查表：存储差异

| 功能 | Azure（全局） | Azure Stack |
| --- | --- | --- |
|文件存储|支持基于云的 SMB 文件共享|尚不支持
|静态数据的 Azure 存储服务加密|256 位 AES 加密|尚不支持
|存储帐户类型|常规用途和 Azure Blob 存储帐户|仅限常规用途
|复制选项|本地冗余存储、异地冗余存储、读取访问异地冗余存储和区域冗余存储|本地冗余存储
|高级存储|完全支持|可预配，但无性能限制或保证
|托管磁盘|支持高级和标准版|尚不支持
|Blob 名称|1,024 个字符（2,048 字节）|880 个字符（1,760 字节）
|块 Blob 大小上限|4.75 TB（100 MB X 50,000 块）|50,000 X 4 MB（约 195 GB）
|页 Blob 快照复制|支持备份已附加到运行中 VM 的 Azure 非托管 VM 磁盘|尚不支持
|页 Blob 增量快照复制|支持高级和标准 Azure 页 Blob|尚不支持
|页 Blob 大小上限|8 TB|1 TB
|页 Blob 页面大小|512 字节|4 KB
|表分区键和行键大小|1,024 个字符（2,048 字节）|400 个字符（800 字节）

### <a name="metrics"></a>指标
存储指标也有一些差异：
- 存储指标中的事务数据不区分内部或外部网络带宽。
- 存储指标中的事务数据不包含虚拟机对所装载的驱动器的访问。

## <a name="api-version"></a>API 版本
使用 Azure Stack 存储时支持以下版本：

- Azure 存储数据服务：[2015-04-05 REST API 版本](https://docs.microsoft.com/rest/api/storageservices/Version-2015-04-05?redirectedfrom=MSDN)
- Azure 存储管理服务：[2015-05-01-preview、2015-06-15 和 2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN) 

## <a name="next-steps"></a>后续步骤

- [Azure Stack 存储开发工具入门](azure-stack-storage-dev.md)
- [Azure Stack 存储简介](azure-stack-storage-overview.md)


