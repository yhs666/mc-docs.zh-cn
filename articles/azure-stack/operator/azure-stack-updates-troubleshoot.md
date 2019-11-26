---
title: 在 Azure Stack 中排查更新问题 | Microsoft Docs
description: 作为 Azure Stack 操作员，了解如何解决更新问题，以便 Azure Stack 可以尽快恢复生产状态。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/23/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.lastreviewed: 09/23/2019
ms.reviewer: ppacent
ms.openlocfilehash: abfb08918035e4f9a4fb2dd460c6d3fdbad7fd5d
ms.sourcegitcommit: 7dfb76297ac195e57bd8d444df89c0877888fdb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020473"
---
# <a name="troubleshooting-patch-and-update-issues-for-azure-stack"></a>排查 Azure Stack 的修补和更新问题

*适用于：Azure Stack 集成系统*

可以使用本文中的指南解决更新 Azure Stack 时遇到的问题。

## <a name="preparationfailed"></a>PreparationFailed

**适用于**：此问题适用于所有支持的版本。

**原因：** 尝试安装 Azure Stack 更新时，更新状态可能会失败并将状态更改为 `PreparationFailed`。 对于连接到 Internet 的系统，这通常表明由于 Internet 连接不稳定，无法正确下载更新包。 

**补救措施**：可以通过再次单击“立即安装”  来解决此问题。 如果此问题仍然存在，建议按照[安装更新](azure-stack-apply-updates.md?#install-updates-and-monitor-progress)部分的说明手动上传更新包。

**发生率**：常见

## <a name="next-steps"></a>后续步骤

- [更新 Azure Stack](azure-stack-updates.md)  
- [Microsoft Azure Stack 帮助和支持](azure-stack-help-and-support-overview.md)
