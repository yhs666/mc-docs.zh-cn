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
origin.date: 05/04/2017
ms.date: 07/30/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 9b6a7fa6a1a1352112eefd9a0522f30d7e43407d
ms.sourcegitcommit: 720d22231ec4b69082ca03ac0f400c983cb03aa1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/27/2018
ms.locfileid: "39306967"
---
# <a name="tutorial-monitor-and-update-a-windows-virtual-machine-in-azure"></a>教程：监视和更新 Azure 中的 Windows 虚拟机

Azure 监视使用代理从 Azure VM 收集启动和性能数据，将此数据存储在 Azure 存储中，并使其可供通过门户、Azure PowerShell 模块和 Azure CLI 进行访问。 使用更新管理可以管理 Azure Windows VM 的更新和修补程序。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 在 VM 上启用启动诊断
> * 查看启动诊断
> * 查看 VM 主机指标
> * 安装诊断扩展 <!-- Not Available on> * View VM metrics -->
<!-- Not Available on> * Create an alert -->
<!-- Not Available on> * Manage Windows updates -->
<!-- Not Available on> * Monitor changes and inventory -->
<!-- Not Available on> * Set up advanced monitoring -->

本教程需要 Azure PowerShell 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。

## <a name="create-virtual-machine"></a>创建虚拟机

若要在本教程中配置 Azure 监视和更新管理，需要 Azure 中的 Windows VM。 首先，使用 [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) 设置 VM 的管理员用户名和密码：

```powershell
$cred = Get-Credential
```

现在，使用 [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) 创建 VM。 以下示例在“ChinaEast”位置创建一个名为 myVM 的 VM。 如果资源组 *myResourceGroupMonitorMonitor* 和支持的网络资源不存在，则会创建它们：

```powershell
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupMonitor" `
    -Name "myVM" `
    -Location "China East" `
    -Credential $cred
```

创建资源和 VM 需要几分钟的时间。

## <a name="view-boot-diagnostics"></a>查看启动诊断

当 Windows 虚拟机启动时，启动诊断代理将捕获屏幕输出，可以使用该输出进行故障排除。 此功能是默认启用的。 捕获的屏幕截图存储在一个 Azure 存储帐户中，该帐户也是默认创建的。

可以使用 [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) 命令获取启动诊断数据。 在下面的示例中，启动诊断下载到了 *c:\* 驱动器的根目录中。

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName "myResourceGroupMonitor" -Name "myVM" -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>查看主机指标

Windows VM 在 Azure 中有一个与它交互的专用主机 VM。 系统会自动收集该主机的指标，可以在 Azure 门户中查看这些指标。

1. 在 Azure 门户中单击“资源组”，选择“myResourceGroupMonitor”，并在资源列表中选择“myVM”。
2. 要查看主机 VM 的性能情况，请在 VM 边栏选项卡上单击“指标”，并选择“可用指标”下的任一主机指标。

    ![查看主机指标](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>安装诊断扩展

可以使用基本的主机指标，但若要查看更详细的指标和 VM 特定的指标，需在 VM 上安装 Azure 诊断扩展。 使用 Azure 诊断扩展可从 VM 检索其他监视数据和诊断数据。 可以查看这些性能指标，并根据 VM 的性能情况创建警报。 诊断扩展是通过 Azure 门户安装的，如下所述：

1. 在 Azure 门户中单击“资源组”，选择“myResourceGroupMonitor”，并在资源列表中选择“myVM”。
2. 单击“诊断设置”。 列表中将显示已在上一部分中启用的“启动诊断”。 单击“基本指标”对应的复选框。
3. 单击“启用来宾级监视”按钮。

    ![查看诊断指标](./media/tutorial-monitoring/enable-diagnostics-extension.png)

<!-- Not Available on ## View VM metrics -->
<!-- Not Available on ## Create alerts -->
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

<!-- Not Available on > * View VM metrics -->
<!-- Not Available on > * Create an alert -->
<!-- Not Available on > * Manage Windows updates -->
<!-- Not Available on > * Monitor changes and inventory -->
<!-- Not Available on > * Set up advanced monitoring -->

<!--Update_Description: update meta properties, wording update-->

