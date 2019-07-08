---
title: 教程 - 监视和更新 Azure 中的 Windows 虚拟机 | Azure
description: 本教程介绍如何在 Windows 虚拟机上监视启动诊断和性能指标，以及管理程序包更新
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 12/05/2018
ms.date: 07/01/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 9088acaef402c3e9f94b31d1dbc45b9b979a4e1d
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569813"
---
# <a name="tutorial-monitor-and-update-a-windows-virtual-machine-in-azure"></a>教程：监视和更新 Azure 中的 Windows 虚拟机

Azure 监视使用代理从 Azure VM 收集启动和性能数据，将此数据存储在 Azure 存储中，并使其可供通过门户、Azure PowerShell 模块和 Azure CLI 进行访问。 使用更新管理可以管理 Azure Windows VM 的更新和修补程序。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 在 VM 上启用启动诊断
> * 查看启动诊断
> * 查看 VM 主机指标
> * 安装诊断扩展
> * 查看 VM 指标
> * 创建警报
<!-- Not Available on> * Manage Windows updates -->
<!-- Not Available on> * Monitor changes and inventory -->
<!-- Not Available on> * Set up advanced monitoring -->

## <a name="launch-azure-powershell"></a>启动 Azure PowerShell

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="create-virtual-machine"></a>创建虚拟机

若要在本教程中配置 Azure 监视和更新管理，需要 Azure 中的 Windows VM。 首先，使用 [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) 设置 VM 的管理员用户名和密码：

```powershell
# Sign in the Azure China Cloud
Connect-AzAccount -Environment AzureChinaCloud

$cred = Get-Credential
```

现在，使用 [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) 创建 VM。 以下示例在“ChinaEast”位置  创建一个名为 myVM  的 VM。 如果资源组 *myResourceGroupMonitorMonitor* 和支持的网络资源不存在，则会创建它们：

```powershell
New-AzVm `
    -ResourceGroupName "myResourceGroupMonitor" `
    -Name "myVM" `
    -Location "China East" `
    -Credential $cred
```

创建资源和 VM 需要几分钟的时间。

## <a name="view-boot-diagnostics"></a>查看启动诊断

当 Windows 虚拟机启动时，启动诊断代理将捕获屏幕输出，可以使用该输出进行故障排除。 此功能是默认启用的。 捕获的屏幕截图存储在一个 Azure 存储帐户中，该帐户也是默认创建的。

可以使用 [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/az.compute/get-azvmbootdiagnosticsdata) 命令获取启动诊断数据。 在下面的示例中，启动诊断下载到了 *c:\* 驱动器的根目录中。

```powershell
Get-AzVMBootDiagnosticsData -ResourceGroupName "myResourceGroupMonitor" -Name "myVM" -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>查看主机指标

Windows VM 在 Azure 中有一个与它交互的专用主机 VM。 系统会自动收集该主机的指标，可以在 Azure 门户中查看这些指标。

1. 在 Azure 门户中单击“资源组”，选择“myResourceGroupMonitor”，并在资源列表中选择“myVM”。   
2. 要查看主机 VM 的性能情况，请在 VM 边栏选项卡上单击“指标”，并选择“可用指标”下的任一主机指标。  

    ![查看主机指标](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>安装诊断扩展

可以使用基本的主机指标，但若要查看更详细的指标和 VM 特定的指标，需在 VM 上安装 Azure 诊断扩展。 使用 Azure 诊断扩展可从 VM 检索其他监视数据和诊断数据。 可以查看这些性能指标，并根据 VM 的性能情况创建警报。 诊断扩展是通过 Azure 门户安装的，如下所述：

1. 在 Azure 门户中单击“资源组”，选择“myResourceGroupMonitor”，并在资源列表中选择“myVM”。   
2. 单击“诊断设置”。  列表中将显示已在上一部分中启用的“启动诊断”。  单击“基本指标”对应的复选框。 
3. 单击“启用来宾级监视”按钮。 

    ![查看诊断指标](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>查看 VM 指标

可以像查看主机 VM 指标一样查看 VM 指标：

1. 在 Azure 门户中单击“资源组”，选择“myResourceGroupMonitor”，并在资源列表中选择“myVM”。   
2. 要查看 VM 的性能情况，请在 VM 边栏选项卡上单击“指标”，并选择“可用指标”下的任一诊断指标。  

    ![查看 VM 指标](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>创建警报

可以根据特定的性能指标创建警报。 例如，当平均 CPU 使用率超过特定的阈值或者可用磁盘空间低于特定的空间量时，警报可用于发出通知。 警报显示在 Azure 门户中，也可以通过电子邮件发送。 还可以触发 Azure 自动化 Runbook 或 Azure 逻辑应用来响应生成的警报。

以下示例针对平均 CPU 使用率创建警报。

1. 在 Azure 门户中单击“资源组”，选择“myResourceGroupMonitor”，并在资源列表中选择“myVM”。   
2. 在 VM 边栏选项卡上单击“警报规则”，然后单击警报边栏选项卡顶部的“添加指标警报”。  
3. 为警报提供**名称**，例如 *myAlertRule*
4. 若要在 CPU 百分比持续 5 分钟超过 1.0 时触发警报，请选中其他所有默认值。
5. （可选）选中“电子邮件所有者、参与者和读者”对应的框，以便向他们发送电子邮件通知。  默认操作是在门户中显示通知。
6. 单击“确定”  按钮。

<!-- Metric select is no option for user choice-->


<!-- Not Available on ## Manage Windows updates -->
<!-- Not Available on ## Monitor changes and inventory-->
<!-- Not Availabel on(Log Analytics) ## Advanced monitoring-->

## <a name="next-steps"></a>后续步骤

在本教程中，你已使用 Azure 安全中心配置并查看了 VM。 你已了解如何：

> [!div class="checklist"]
> * 创建虚拟网络
> * 创建资源组和 VM
> * 在 VM 上启用启动诊断
> * 查看启动诊断
> * 查看主机指标
> * 安装诊断扩展
> * 查看 VM 指标
> * 创建警报

<!-- Not Available on > * Manage Windows updates -->
<!-- Not Available on > * Monitor changes and inventory -->
<!-- Not Available on > * Set up advanced monitoring -->

<!-- Not Avaialble on [Manage VM security](./tutorial-azure-security.md)-->
<!--Update_Description: update meta properties, wording update-->
