---
title: 使用 PowerShell 自动执行 Azure Stack 验证 | Azure
description: 可以使用 PowerShell 自动执行 Azure Stack 验证。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
origin.date: 07/24/2018
ms.date: 08/27/2018
ms.author: v-jay
ms.reviewer: johnhas
ms.openlocfilehash: 7fd8f7dfe7d7a1ee4785617e5b096b1cb73bac6c
ms.sourcegitcommit: bc7679a5ad24ea9120c44fc771e88a08b5d8b207
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998374"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>使用 PowerShell 自动执行 Azure Stack 验证 

验证即服务 (VaaS) 提供了使用 **LaunchVaaSTests.ps1** 脚本自动启动测试的功能。

可以使用 PowerShell 执行以下工作流：

- 测试轮次工作流

此脚本包括工作流的四个元素：

- 安装先决条件。
- 安装并启动本地代理。
- 启动某种类别的测试，例如集成、功能、可靠性。
- 报告每个测试轮次结果以用于监视和报表文件生成。

## <a name="launch-the-test-pass-workflow"></a>启动测试轮次工作流

1. 打开提升的 PowerShell 命令提示符。

2. 运行以下脚本来下载自动化脚本：

    ````PowerShell  
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://vaastestpacksprodeastus.blob.core.windows.net/packages/Microsoft.VaaS.Scripts.3.0.0.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ````

3. 使用你的值运行以下脚本：

    ````PowerShell  
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>"  -AsPlainText -Force)
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<ServiceAdminUser>", (ConvertTo-SecureString "<ServiceAdminPassword>" -AsPlainText -Force)
    $TenantAdminCreds = New-Object System.Management.Automation.PSCredential "<TenantAdminUser>", (ConvertTo-SecureString "<TenantAdminPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
        -VaaSAccountTenantId <VaaSAccountTenantId> `
        -VaaSSolutionName <VaaSSolutionName> `
        -VaaSTestPassName <VaaSTestPassName> `
        -VaaSTestCategories Integration,Functional `
        -MaxScriptWaitTimeInHours 12 `
        -ServiceAdminCreds $ServiceAdminCreds `
    ````

    **Parameters**

    | 参数 | 说明 |
    | --- | --- |
    | VaaSUserld | VaaS 用户 ID。 | 
    | VaaSUserPassword | VaaS 密码。 |
    | ServiceAdminUser | Azure Stack 服务管理员帐户。  |
    | ServiceAdminPassword | Azure Stack 服务密码。  |
    | TenantAdminUser | 主租户的管理员。  |
    | TenantAdminPassword | 主租户的密码。  |
    | FunctionalCategory| Integration、Functional 或  Reliability。 如果使用多个值，请用逗号分隔各个值。  |

4. 查看测试结果。 有关阅读测试结果的信息，请参阅[监视测试](azure-stack-vaas-monitor-test.md)。

## <a name="next-steps"></a>后续步骤

 - 详细了解 [Azure Stack 验证即服务](/azure-stack/partner)。
 - 若要了解有关 Azure Stack 上的 PowerShell 的详细信息，请参阅 [Azure Stack 模块](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0)参考。