---
title: Azure Stack 更新活动清单 | Microsoft Docs
description: 为系统准备最新 Azure Stack 更新的清单。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/30/2019
ms.date: 10/21/2019
ms.author: v-jay
ms.reviewer: hectorl
ms.lastreviewed: 08/30/2019
ms.openlocfilehash: 38bdb04556739713d4224d6166ca53adf8fbeafd
ms.sourcegitcommit: 713bd1d1b476cec5ed3a9a5615cfdb126bc585f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72578443"
---
# <a name="azure-stack-update-activity-checklist"></a>Azure Stack 更新活动清单

*适用于：Azure Stack 集成系统*

查看此清单，以便准备 Azure Stack 更新。 本文包含 Azure Stack 操作员的更新相关活动的清单。

## <a name="prepare-for-azure-stack-update"></a>准备 Azure Stack 更新

| 活动                     | 详细信息                                                   |
|------------------------------|-----------------------------------------------------------|
| 查看已知问题     | [已知问题列表](azure-stack-release-notes-known-issues-1908.md)。                |
| 查看安全更新 | [安全更新列表](azure-stack-release-notes-security-updates.md)。      |
| 应用最新的 OEM 包 | 请与 OEM 联系，以确保你的系统满足系统要更新到的 Azure Stack 版本的最低 OEM 包要求。 |
| 运行 Test-AzureStack | 运行 `Test-AzureStack -Group UpdateReadiness` 确定操作问题。 |
| 解决问题 | 解决 `Test-AzureStack` 确定的任何操作问题。 |
| 应用最新修补程序 | 应用适用于当前安装版本的最新修补程序。 |
| 运行 Capacity Planner 工具 | 请确保使用最新版本的 [Azure Stack Capacity Planner 工具](azure-stack-capacity-planning-overview.md)来执行工作负荷规划和大小调整。 最新版本包含 bug 修复，并提供与每个 Azure Stack 更新一起发布的新功能。 |
| 可用更新 | 只有在联网场景中，Azure Stack 部署才会定期检查安全的终结点，并在已发布云更新的情况下自动通知你。 断开连接的客户可以根据[此处所述的过程](azure-stack-apply-updates.md)下载和导入新包。 |

## <a name="during-azure-stack-update"></a>在 Azure Stack 更新期间

| 活动 | 详细信息 |
|--------------------|------------------------------------------------------------------------------------------------------|
| 管理更新 |[使用运营商门户管理 Azure Stack 中的更新](azure-stack-updates.md)。 |
|  |  |
| 监视更新 | 如果运营商门户不可用，请[使用特权终结点监视 Azure Stack 中的更新](azure-stack-monitor-update.md)。 |
|  |  |
| 恢复更新 | 在修复失败的更新后，[使用特权终结点恢复 Azure Stack 中的更新](azure-stack-monitor-update.md)。 |

> [!Important]  
> 在更新期间不要运行 **Test-AzureStack**，因为这会导致更新停止。

## <a name="after-azure-stack-update"></a>Azure Stack 更新后

| 活动 | 详细信息 |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 应用最新修补程序 | 应用适用于已更新版本的最新修补程序。 |
| 检索加密密钥 | 检索静态数据加密密钥，并将其安全存储在 Azure Stack 部署的外部。 请遵照[有关如何检索密钥的说明](azure-stack-security-bitlocker.md)操作。 |
|  |  |
| 重新启用多租户 | 对于多租户 Azure Stack，请确保在成功更新后[配置所有来宾目录租户](azure-stack-enable-multitenancy.md#configure-guest-directory)。 |

## <a name="next-steps"></a>后续步骤

- [查看已知问题列表](azure-stack-release-notes-known-issues-1908.md)
- [查看安全更新列表](azure-stack-release-notes-security-updates.md)
