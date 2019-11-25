---
title: 规划 Azure Stack 更新 | Microsoft Docs
description: 了解如何规划 Azure Stack 更新。
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
origin.date: 10/17/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.lastreviewed: 08/23/2019
ms.reviewer: ppacent
ms.openlocfilehash: 36cf42a02cd6688129699692ceae8fd377bb119f
ms.sourcegitcommit: 7dfb76297ac195e57bd8d444df89c0877888fdb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020021"
---
# <a name="plan-for-an-azure-stack-update"></a>规划 Azure Stack 更新

*适用于：Azure Stack 集成系统*

可以准备好 Azure Stack，使更新过程能够尽量顺畅地进行，并尽量减轻对用户造成的影响。 本文介绍通知用户可能发生服务中断的步骤，以及准备更新所要执行的步骤。

## <a name="notify-your-users-of-maintenance-operations"></a>在执行维护操作之前通知用户

应向用户通知任何维护操作，并尽可能将正常维护时段安排在非工作时间。 维护操作可能会同时影响租户工作负荷和门户操作。

## <a name="prepare-for-an-azure-stack-update"></a>准备 Azure Stack 更新

可按如下所述准备更新：确保已应用所有修补程序、安全修补程序和 OEM 更新，已验证 Azure Stack 实例的运行状况，已检查可用的容量，并已检查更新包。

1. 查看已知问题。 有关说明，请参阅 [Azure Stack 的已知问题](/azure-stack/operator/release-notes)。

2. 检查安全更新。 有关更新列表，请参阅 [Azure Stack 安全更新](/azure-stack/operator/release-notes-security-updates)。

3. 在开始安装此更新之前，请运行 [Test-AzureStack](/azure-stack/operator/azure-stack-diagnostic-test)，以验证 Azure Stack 的状态并解决发现的所有操作问题，包括所有警告和故障。 另外，请查看处于活动状态的警报并解决任何需要采取操作的警报。

    1. 从可以通过网络访问 Azure Stack ERCS VM 的计算机打开特权终结点会话。 有关使用 PEP 的说明，请参阅[使用 Azure Stack 中的特权终结点](/azure-stack/operator/azure-stack-privileged-endpoint)。

    2. Test-AzureStack -Group UpdateReadiness

    3. 查看输出并解决任何运行状况错误。 有关详细信息，请参阅[运行 Azure Stack 的验证测试](/azure-stack/operator/azure-stack-diagnostic-test)。

4. 解决问题。 有关详细了解带标记的问题的指导，请参阅“监视运行状况和警报”主题。 有关说明，请参阅[在 Azure Stack 中监视运行状况和警报](/azure-stack/operator/azure-stack-monitor-health)。

5. 应用最新的修补程序。 有关最新修补程序的列表，请参阅发行说明中的“修补程序”部分。 应用最新的修补程序后，重复步骤 3 和 4。

6. 确保 OEM 包与要更新到的 Azure Stack 版本兼容。 如果 OEM 包与要更新到的 Azure Stack 版本不兼容，则必须在运行 Azure Stack 更新之前，先执行 OEM 包更新。 有关说明，请参阅“应用 Azure Stack 原始设备制造商 (OEM) 更新”。 在应用 OEM 包更新后，重复步骤 3 和 4。

7. 运行容量规划器工具。 有关该工具的概述和用法说明，请参阅 [Azure Stack 容量规划概述](/azure-stack/operator/azure-stack-capacity-planning-overview)。

8. （可选）如果你看到**更新失败**等警报，可以[启用自动诊断日志收集](azure-stack-configure-automatic-diagnostic-log-collection.md)主动收集日志以进行客户支持分析。 

8. 检查更新包。 规划维护时段时，必须根据发行说明中所述，检查 Microsoft 发布的特定类型的更新包。

## <a name="next-steps"></a>后续步骤

[应用 Azure Stack 更新](azure-stack-apply-updates.md)。
