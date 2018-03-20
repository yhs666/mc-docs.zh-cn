---
title: "在 Azure Stack 中轮换机密 | Microsoft Docs"
description: "了解如何在 Azure Stack 中轮换机密。"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 49071044-6767-4041-9EDD-6132295FA551
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/08/2018
ms.date: 03/04/2018
ms.author: v-junlch
ms.openlocfilehash: b5c35ca81e7fbe97c37442fa351f84fee1976b6f
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>在 Azure Stack 中轮换机密

*适用于：Azure Stack 集成系统*

请定期更新 Azure Stack 组件的密码。

## <a name="updating-the-passwords-for-the-baseboard-management-controller-bmc"></a>更新基板管理控制器 (BMC) 的密码

基板管理控制器 (BMC) 监视服务器的物理状态。 有关更新 BMC 密码的规范和说明会根据原始设备制造商 (OEM) 硬件供应商而有所不同。

1. 按照 OEM 说明在 Azure Stack 的物理服务器上更新 BMC。 环境中每个 BMC 的密码必须相同。
2. 在 Azure Stack 会话中打开特权终结点。 有关说明，请参阅[使用 Azure Stack 中的特权终结点](azure-stack-privileged-endpoint.md)。
3. 在 PowerShell 提示符更改为 **[IP 地址或 ERCS VM 名称]: PS>** 或更改为 **[azs-ercs01]: PS>**（取决于环境）后，通过运行 `invoke-command` 来运行 `Set-BmcPassword`。 将特权终结点会话变量作为参数传递。 例如：

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    ```
    
    也可以将静态 PowerShell 版本与密码搭配使用，如以下代码行所示：
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for exmaple Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    ```

## <a name="next-steps"></a>后续步骤

若要详细了解安全性和 Azure Stack，请参阅 [Azure Stack 基础结构安全状况](azure-stack-security-foundations.md)。

