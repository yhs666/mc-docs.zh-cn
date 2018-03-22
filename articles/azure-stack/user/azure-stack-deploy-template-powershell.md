---
title: 在 Azure Stack 中使用 PowerShell 部署模板 | Microsoft Docs
description: 了解如何使用资源管理器模板和 PowerShell 部署虚拟机。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/25/2017
ms.date: 03/08/2018
ms.author: v-junlch
ms.reviewer: ''
ms.openlocfilehash: afe3aac53f1322d52be80c82f030fb09a89ee220
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="deploy-templates-in-azure-stack-using-powershell"></a>使用 PowerShell 在 Azure Stack 中部署模板

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

使用 PowerShell 可将 Azure 资源管理器模板部署到 Azure Stack 开发工具包。  资源管理器 模板可通过单个协调操作部署和预配应用程序的所有资源。

## <a name="run-azurerm-powershell-cmdlets"></a>运行 AzureRM PowerShell cmdlet
在此示例中，将会运行脚本，使用资源管理器模板将虚拟机部署到 Azure Stack 开发工具包。  在继续之前，请确保已[配置 PowerShell](azure-stack-powershell-configure-user.md)  

在此示例模板中使用的 VHD 是 WindowsServer-2012-R2-Datacenter。

1. 前往 <http://aka.ms/AzureStackGitHub>，搜索 **101-simple-windows-vm** 模板，并将其保存到以下位置：c:\\templates\\azuredeploy-101-simple-windows-vm.json。
2. 在 PowerShell 中，运行以下部署脚本。 将 *username* 和 *password* 替换为你的用户名和密码。 后续使用时，递增 *$myNum* 参数的值，以避免覆盖你的部署。
   
   ```PowerShell
       # Set Deployment Variables
       $myNum = "001" #Modify this per deployment
       $RGName = "myRG$myNum"
       $myLocation = "local"
   
       # Create Resource Group for Template Deployment
       New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
       # Deploy Simple IaaS Template
       New-AzureRmResourceGroupDeployment `
           -Name myDeployment$myNum `
           -ResourceGroupName $RGName `
           -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
           -NewStorageAccountName mystorage$myNum `
           -DnsNameForPublicIP mydns$myNum `
           -AdminUsername <username> `
           -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
           -VmName myVM$myNum `
           -WindowsOSVersion 2012-R2-Datacenter
   ```
3. 打开 Azure Stack 门户，依次单击“浏览”、“虚拟机”，并查找新虚拟机 (*myDeployment001*)。


## <a name="next-steps"></a>后续步骤
[通过 Visual Studio 部署模板](azure-stack-deploy-template-visual-studio.md)


