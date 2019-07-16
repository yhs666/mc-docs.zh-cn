---
title: 适用于 Windows 的 Azure Monitor 虚拟机扩展 | Azure
description: 使用虚拟机扩展在 Windows 虚拟机上部署 Log Analytics 代理。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
origin.date: 04/29/2019
ms.date: 07/08/2019
ms.author: v-yeche
ms.openlocfilehash: ae723725db005a384a7349ce2448b22aead0974f
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67570532"
---
# <a name="azure-monitor-virtual-machine-extension-for-windows"></a>适用于 Windows 的 Azure Monitor 虚拟机扩展

Azure Monitor 日志提供跨云和本地资产的监视功能。 适用于 Windows 的 Log Analytics 代理虚拟机扩展由 Azure 发布和提供支持。 该扩展在 Azure 虚拟机上安装 Log Analytics 代理，并将虚拟机注册到现有的 Log Analytics 工作区中。 本文档详细介绍适用于 Windows 的 Azure Monitor 虚拟机扩展支持的平台、配置和部署选项。

<!--MOONCAKE: CORRECT ON supported by Azure-->

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>先决条件

### <a name="operating-system"></a>操作系统

Windows 的 Log Analytics 代理扩展支持以下版本的 Windows 操作系统：

- Windows Server 2019
- Windows Server 2008 R2、2012、2012 R2、2016，版本 1709 和 1803

### <a name="agent-and-vm-extension-version"></a>代理和 VM 扩展版本
下表提供每次发布的 Azure Monitor VM 扩展和 Log Analytics 代理捆绑包的版本映射。 

| Azure Monitor Linux VM 扩展版本 | Log Analytics 代理捆绑包版本 | 发布日期 | 发行说明 |
|--------------------------------|--------------------------|--------------------------|--------------------------|
| 8.0.11049.0 | 1.0.11049.1 | 2017 年 2 月 | |
| 8.0.11072.0 | 1.0.11072.1 | 2017 年 9 月 | |
| 8.0.11081.0 | 1.0.11081.5 | 2017 年 11 月 | | 
| 8.0.11103.0 | 不适用 |  2018 年 4 月 | |
| 8.0.11136.0 | 不适用 | 2018 年 9 月 |  <ul><li> 添加了对 VM 移动时检测资源 ID 更改的支持 </li><li> 添加了对使用非扩展安装时报告资源 ID 的支持 </li></ul>| 
| 10.19.10006.0 | 不适用 | 2018 年 12 月 | <ul><li> 次要稳定性修复 </li></ul> | 
| 10.19.13515.0 | 1.0.13515.1 | 2019 年 3 月 | <ul><li>次要稳定性修复 </li></ul> |

<!--Not Available on ### Azure Security Center-->

### <a name="internet-connectivity"></a>Internet 连接
适用于 Windows 的 Log Analytics 代理扩展要求目标虚拟机已连接到 Internet。 

## <a name="extension-schema"></a>扩展架构

以下 JSON 显示 Log Analytics 代理扩展的架构。 此扩展需要目标 Log Analytics 工作区的工作区 ID 和工作区密钥。 这些数据可在 Azure 门户的工作区设置中找到。 由于工作区密钥应视为敏感数据，因此将它存储在受保护的设置配置中。 Azure VM 扩展保护的设置数据已加密，并且只能在目标虚拟机上解密。 请注意，**workspaceId** 和 **workspaceKey** 区分大小写。

```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```
### <a name="property-values"></a>属性值

| 名称 | 值/示例 |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| publisher | Microsoft.EnterpriseCloud.Monitoring |
| type | MicrosoftMonitoringAgent |
| typeHandlerVersion | 1.0 |
| workspaceId (e.g)* | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (e.g) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |

\* workspaceId 在 Log Analytics API 中称为 consumerId。

## <a name="template-deployment"></a>模板部署

可使用 Azure Resource Manager 模板部署 Azure VM 扩展。 可以在 Azure 资源管理器模板中使用上一部分中详细介绍的 JSON 架构，以便在 Azure 资源管理器模板部署过程中运行 Log Analytics 代理扩展。 包含 Log Analytics 代理 VM 扩展的示例模板可以在 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm)中找到。 

>[!NOTE]
>需要将代理配置为向多个工作区报告时，此模板不支持指定多个工作区 ID 和工作区密钥。

<!--Not Available on  To configure the agent to report to multiple workspaces, see [Adding or removing a workspace](../../azure-monitor/platform/agent-manage.md#adding-or-removing-a-workspace)-->

虚拟机扩展的 JSON 可以嵌套在虚拟机资源内，或放置在 Resource Manager JSON 模板的根级别或顶级别。 JSON 的位置会影响资源名称和类型的值。 有关详细信息，请参阅[设置子资源的名称和类型](../../azure-resource-manager/resource-group-authoring-templates.md#child-resources)。 

以下示例假定 Azure Monitor 扩展嵌套在虚拟机资源内。 嵌套扩展资源时，JSON 放置在虚拟机的 `"resources": []` 对象中。

```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

将扩展 JSON 放置在模板的根部时，资源名称包括对父虚拟机的引用，并且类型反映了嵌套的配置。 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

## <a name="powershell-deployment"></a>PowerShell 部署

可以使用 `Set-AzVMExtension` 命令将 Log Analytics 代理虚拟机扩展部署到现有的虚拟机。 运行命令之前，需将公共和专用的配置存储在 PowerShell 哈希表中。 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzVMExtension -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location ChinaNorth 
```

## <a name="troubleshoot-and-support"></a>故障排除和支持

### <a name="troubleshoot"></a>故障排除

有关扩展部署状态的数据可以从 Azure 门户和使用 Azure PowerShell 模块进行检索。 若要查看给定 VM 的扩展部署状态，请使用 Azure PowerShell 模块运行以下命令。

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

扩展执行输出记录到在以下目录中发现的文件：

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a>支持

如果对本文中的任何观点存在疑问，可以联系 [Azure 支持](https://support.azure.cn/zh-cn/support/contact/)上的 Azure 专家。 或者，也可以提出 Azure 支持事件。 请转到 [Azure 支持站点](https://support.azure.cn/zh-cn/support/support-azure/)提交请求。 有关使用 Azure 支持的信息，请阅读 [Azure 支持常见问题](https://www.azure.cn/support/faq/)。

<!-- Update_Description: new article about oms windows-->
<!--ms.date: 04/20/2018-->