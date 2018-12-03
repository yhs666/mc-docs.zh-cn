---
title: Azure Stack 中托管磁盘的差异和注意事项 | Microsoft Docs
description: 了解使用 Azure Stack 中的托管磁盘时的差异和注意事项。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/05/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.reviewer: jiahan
ms.openlocfilehash: 884b22c7bb71b992bc0854f252fcaa394cad87e0
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52656870"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack 托管磁盘：差异和注意事项
本文总结了 Azure Stack 托管磁盘和 Azure 托管磁盘之间的已知差异。 有关 Azure Stack 与 Azure 之间的大致差异的详细信息，请参阅[重要注意事项](azure-stack-considerations.md)一文。

托管磁盘通过管理与 VM 磁盘关联的[存储帐户](/azure-stack/azure-stack-manage-storage-accounts)简化了 IaaS VM 的磁盘管理。
  

## <a name="cheat-sheet-managed-disk-differences"></a>速查表：托管磁盘差异

| 功能 | Azure（中国） | Azure Stack |
| --- | --- | --- |
|静态数据加密 |Azure 存储服务加密 (SSE)、Azure 磁盘加密 (ADE)     |BitLocker 128 位 AES 加密      |
|映像          | 支持托管自定义映像 |尚不支持|
|备份选项 |支持 Azure 备份服务 |尚不支持 |
|灾难恢复选项 |支持 Azure Site Recovery |尚不支持|
|磁盘类型     |高级 SSD 和标准 HDD |高级 SSD、标准 HDD |
|高级磁盘  |完全支持 |可部署，但无性能限制或保证  |
|高级磁盘 IOPS  |取决于磁盘大小  |每个磁盘 2300 IOPS |
|高级磁盘吞吐量 |取决于磁盘大小 |每个磁盘 145 MB/秒 |
|磁盘最大大小  |4 TB       |1 TB       |
|磁盘性能分析 |支持的聚合指标和每磁盘指标 |尚不支持 |
|迁移      |提供从现有非托管 Azure 资源管理器 VM 迁移的工具，而无需重新创建 VM  |尚不支持 |


## <a name="metrics"></a>指标
存储指标也有一些差异：
- 使用 Azure Stack，存储指标中的事务数据不区分内部或外部网络带宽。
- 存储指标中的 Azure Stack 事务数据不包含虚拟机对所装载磁盘的访问。


## <a name="api-versions"></a>API 版本
Azure Stack 托管磁盘支持以下 API 版本：
- 2017-03-30


## <a name="next-steps"></a>后续步骤
[了解 Azure Stack 虚拟机](azure-stack-compute-overview.md)
