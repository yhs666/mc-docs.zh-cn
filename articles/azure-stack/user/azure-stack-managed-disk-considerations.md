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
ms.date: 01/14/2019
ms.author: v-jay
ms.reviewer: jiahan
ms.openlocfilehash: 17a92c53bbae1de5899af6c6a27fb41710a63fa3
ms.sourcegitcommit: f9da1fd49933417cf75de8649af92fe27876da64
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/07/2019
ms.locfileid: "54058995"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack 托管磁盘：差异和注意事项
本文总结了 Azure Stack 托管磁盘和 Azure 托管磁盘之间的已知差异。 有关 Azure Stack 与 Azure 之间的大致差异的详细信息，请参阅[重要注意事项](azure-stack-considerations.md)一文。

托管磁盘通过管理与 VM 磁盘关联的[存储帐户](/azure-stack/azure-stack-manage-storage-accounts)简化了 IaaS VM 的磁盘管理。

> [!Note]  
> 从 1808 开始，推出了 Azure Stack 上的托管磁盘。
  

## <a name="cheat-sheet-managed-disk-differences"></a>速查表：托管磁盘差异

| 功能 | Azure（中国） | Azure Stack |
| --- | --- | --- |
|静态数据加密 |Azure 存储服务加密 (SSE)、Azure 磁盘加密 (ADE)     |BitLocker 128 位 AES 加密      |
|映像          | 支持托管自定义映像 |尚不支持|
|备份选项 |支持 Azure 备份服务 |尚不支持 |
|灾难恢复选项 |支持 Azure Site Recovery |尚不支持|
|磁盘类型     |高级 SSD、标准 SSD（预览版）和标准 HDD。 |高级 SSD、标准 HDD |
|高级磁盘  |完全支持 |可部署，但无性能限制或保证  |
|高级磁盘 IOPS  |取决于磁盘大小  |每个磁盘 2300 IOPS |
|高级磁盘吞吐量 |取决于磁盘大小 |每个磁盘 145 MB/秒 |
|磁盘大小  |Azure 高级磁盘：P4 (32 GiB) 到 P80 (32 TiB)<br>Azure 标准 SSD 磁盘：E10 (128 GiB) 到 E80 (32 TiB)<br>Azure 标准 HDD 磁盘：S4 (32 GiB) 到 S80 (32 TiB) |M4：32 GiB<br>M6：64 GiB<br>M10：128 GiB<br>M15：256 GiB<br>M20：512 GiB<br>M30：1024 GiB |
|磁盘快照复制|支持附加到正在运行的 VM 的快照 Azure 托管磁盘|尚不支持 |
|磁盘性能分析 |支持的聚合指标和每磁盘指标 |尚不支持 |
|迁移      |提供从现有非托管 Azure 资源管理器 VM 迁移的工具，而无需重新创建 VM  |尚不支持 |

> [!Note]  
> Azure Stack 中的托管磁盘 IOP 和吞吐量是一个上限数字而非预配的数字，这可能会受在 Azure Stack 中运行的硬件和工作负荷影响。


## <a name="metrics"></a>指标
存储指标也有一些差异：
- 使用 Azure Stack，存储指标中的事务数据不区分内部或外部网络带宽。
- 存储指标中的 Azure Stack 事务数据不包含虚拟机对所装载磁盘的访问。


## <a name="api-versions"></a>API 版本
Azure Stack 托管磁盘支持以下 API 版本：
- 2017-03-30

## <a name="known-issues"></a>已知问题
应用 1809 更新后，在部署带托管磁盘的 VM 时可能会遇到以下问题：

   - 如果订阅是在 1808 更新之前创建的，则部署具有托管磁盘的 VM 可能会失败并出现内部错误消息。 若要解决此错误，请针对每个订阅执行以下步骤：
      1. 在租户门户中转到“订阅”，找到相应订阅。 依次单击“资源提供程序”、“Microsoft.Compute”、“重新注册”。
      2. 在同一订阅下，转到“访问控制(标识和访问管理)”，验证“Azure Stack - 托管磁盘”是否已列出。
   - 如果已配置多租户环境，在与来宾目录相关联的订阅中部署 VM 可能会失败并出现内部错误消息。 若要解决错误，请执行[此文章](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory)中的步骤来重新配置每个来宾目录。


## <a name="next-steps"></a>后续步骤
[了解 Azure Stack 虚拟机](azure-stack-compute-overview.md)
