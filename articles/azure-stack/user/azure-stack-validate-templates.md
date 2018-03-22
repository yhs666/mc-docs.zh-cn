---
title: 使用模板验证程序检查 Azure Stack 的模板 | Microsoft Docs
description: 检查要部署到 Azure Stack 的模板
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/20/2018
ms.date: 03/09/2018
ms.author: v-junlch
ms.reviewer: jeffgo
ms.openlocfilehash: de232042da3193205e53d2860ce6f79e7c958353
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="check-your-templates-for-azure-stack-with-template-validator"></a>使用模板验证程序检查 Azure Stack 的模板

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可以使用模板验证工具检查 Azure 资源管理器[模板](azure-stack-arm-templates.md)是否准备好供 Azure Stack 使用。 模板验证工具是 Azure Stack 工具的一部分。 使用[从 GitHub 下载工具](azure-stack-powershell-download.md)一文中所述的步骤下载 Azure Stack 工具。 

若要验证模板，可以使用 **TemplateValidator** 和 **CloudCapabilities** 文件夹中的以下 PowerShell 模块： 

 - AzureRM.CloudCapabilities.psm1 会创建云功能 JSON 文件，该文件表示云（例如 Azure Stack）中的服务和版本。
 - AzureRM.TemplateValidator.psm1 使用云功能 JSON 文件来测试要在 Azure Stack 中部署的模板。
 
在本文中，将生成云功能文件，然后运行验证程序工具。

## <a name="build-cloud-capabilities-file"></a>生成云功能文件
使用模板验证程序之前，先运行 AzureRM.CloudCapabilities PowerShell 模块以生成 JSON 文件。 如果更新集成系统，或添加任何新服务或 VM 扩展，也应重新运行该模块。

1.  请确保已连接到 Azure Stack。 这些步骤可从 Azure Stack 开发工具包主机执行，也可以使用 [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) 从工作站连接。 
2.  导入 AzureRM.CloudCapabilities PowerShell 模块：

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ``` 

3.  使用 Get-CloudCapabilities cmdlet 检索服务版本，并创建云功能 JSON 文件。 如果未指定 -OutputPath，将在当前目录中创建文件 AzureCloudCapabilities.Json。 使用实际位置：

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```             

## <a name="validate-templates"></a>验证模板
在这些步骤中，使用 AzureRM.TemplateValidator PowerShell 模块验证模板。 可以使用自己的模板，或验证 [Azure Stack 快速入门模板](https://github.com/Azure/AzureStack-QuickStart-Templates)。

1.  导入 AzureRM.TemplateValidator.psm1 PowerShell 模块：
    
    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2.  运行模板验证程序：

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

所有模板验证警告或错误都会记录到 PowerShell 控制台和源目录中的 HTML 文件。 下面是验证报告的示例：

![示例验证报告](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>parameters

| 参数 | 说明 | 必须 |
| ----- | -----| ----- |
| TemplatePath | 指定要在其中递归查找 Azure 资源管理器模板的路径 | 是 | 
| TemplatePattern | 指定要匹配的模板文件的名称。 | 否 |
| CapabilitiesPath | 指定云功能 JSON 文件的路径 | 是 | 
| IncludeComputeCapabilities | 包括 IaaS 资源（如 VM 大小和 VM 扩展）的评估 | 否 |
| IncludeStorageCapabilities | 包括存储资源（如 SKU 类型）的评估 | 否 |
| 报表 | 指定生成的 HTML 报告的名称 | 否 |
| 详细 | 将错误和警告记录到控制台 | 否|


### <a name="examples"></a>示例
此示例会验证所有下载到本地的 [Azure Stack 快速入门模板](https://github.com/Azure/AzureStack-QuickStart-Templates)，并且还会验证 VM 大小和扩展是否符合 Azure Stack 开发工具包功能。

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json.json `
-TemplatePattern MyStandardTemplateName.json`
-IncludeComputeCapabilities`
-Report TemplateReport.html
```


## <a name="next-steps"></a>后续步骤
 - [将模板部署到 Azure Stack](azure-stack-arm-templates.md)
 - [为 Azure Stack 开发模板](azure-stack-develop-templates.md)


