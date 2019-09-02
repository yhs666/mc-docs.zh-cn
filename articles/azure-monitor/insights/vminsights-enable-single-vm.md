---
title: 启用用于 VM 的 Azure Monitor（预览版）进行评估 | Microsoft Docs
description: 了解如何在单个 Azure 虚拟机或虚拟机规模集上评估用于 VM 的 Azure Monitor。
services: azure-monitor
documentationcenter: ''
author: lingliw
manager: digimobile
origin.date: 08/22/2019
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2019
ms.author: v-lingwu
ms.openlocfilehash: 0b914f7ef2948d6857e0d7354938fe0acf7ed14f
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70103832"
---
# <a name="enable-azure-monitor-for-vms-preview-for-evaluation"></a>启用用于 VM 的 Azure Monitor（预览版）进行评估

可以在少量 Azure 虚拟机 (VM) 或单个 VM 或虚拟机规模集上评估用于 VM 的 Azure Monitor（预览版）。 若要启用监视，最简单直观的方式是使用 Azure 门户。 你的目标是监视 VM，发现性能或可用性问题。 

在开始前，请查看[先决条件](vminsights-enable-overview.md)，确保订阅和资源满足要求。  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>为单个 Azure VM 启用监视
若要启用对 Azure VM 的监视，请执行以下操作：

1. 登录到 [Azure 门户](https://portal.azure.cn)。

1. 选择“虚拟机”。 

1. 从列表中选择一个虚拟机。

1. 在“VM”页上的“监视”部分，选择“见解(预览版)”。  

1. 在“见解(预览版)”页上，选择“立即试用”。  

    ![为 VM 启用用于 VM 的 Azure Monitor](./media/vminsights-enable-single-vm/enable-vminsights-vm-portal-01.png)

1. 在“Azure Monitor Insights 载入”页上，如果现有的 Log Analytics 工作区与群集在同一订阅中，请从下拉列表中选择该工作区  。  

    该列表预先选择了 VM 在订阅中部署到的默认工作区和位置。 

    >[!NOTE]
    >若要创建新的 Log Analytics 工作区以存储来自 VM 的监视数据，请参阅[创建 Log Analytics 工作区](../../azure-monitor/learn/quick-create-workspace.md)。 Log Analytics 工作区必须属于某个[受支持的区域](vminsights-enable-overview.md#log-analytics)。

启用监视后，可能需要等待约 10 分钟才能查看 VM 的运行状况指标。

![启用用于 VM 的 Azure Monitor 来监视部署处理](./media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>为单个虚拟机规模集启用监视

若要启用对 Azure 虚拟机规模集的监视，请执行以下操作：

1. 登录到 [Azure 门户](https://portal.azure.cn)。

2. 选择“虚拟机规模集”。 

3. 从列表中选择虚拟机规模集。

4. 在“虚拟机规模集”页上的“监视”部分，选择“见解(预览版)”。  

5. 在“见解(预览版)”页上，若要使用现有的 Log Analytics 工作区，请在下拉列表中选择它  。

    列表预先选择了 VM 在订阅中部署到的默认工作区和位置。 

    ![为虚拟机规模集启用用于 VM 的 Azure Monitor](./media/vminsights-enable-single-vm/enable-vminsights-vmss-portal-01.png)

    >[!NOTE]
    >若要创建新的 Log Analytics 工作区以存储来自虚拟机规模集的监视数据，请参阅[创建 Log Analytics 工作区](../learn/quick-create-workspace.md)。 Log Analytics 工作区必须属于某个[受支持的区域](vminsights-enable-overview.md#log-analytics)。

启用监视后，可能需要等待约 10 分钟才能查看规模集的监视数据。

>[!NOTE]
>如果使用规模集的手动升级模型，请升级实例以完成设置。 可以从“设置”部分的“实例”页启动升级。  

![启用用于 VM 的 Azure Monitor 来监视部署处理](./media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status-01.png)

现已为 VM 或虚拟机规模集启用监视，可以在用于 VM 的 Azure Monitor 中使用监视信息进行分析了。 

## <a name="next-steps"></a>后续步骤

* 若要了解如何使用运行状况功能，请参阅[了解 Azure Monitor VM 的运行状况](vminsights-health.md)。 
* 若要查看已发现的应用程序依赖项，请参阅[使用用于 VM 的 Azure Monitor 映射](vminsights-maps.md)。 
* 若了解瓶颈、整体利用率和 VM 的性能，请参阅[查看 Azure VM 性能](vminsights-performance.md)。