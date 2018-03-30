---
title: 在 Azure Stack 中提供虚拟机规模集 | Microsoft Docs
description: 了解云操作员如何向 Azure Stack 应用商店中添加虚拟机规模集
services: azure-stack
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.topic: article
origin.date: 03/13/2018
ms.date: 03/22/2018
ms.author: v-junlch
ms.reviewer: anajod
keywords: ''
ms.openlocfilehash: d7e13b2f644a6f49b86da3036eb0999fccccfcbe
ms.sourcegitcommit: 61fc3bfb9acd507060eb030de2c79de2376e7dd3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2018
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>在 Azure Stack 中提供虚拟机规模集

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

虚拟机规模集是一种 Azure Stack 计算资源。 可以使用它们部署和管理一组相同的虚拟机。 由于所有虚拟机的配置都相同，因此规模集不需要预配虚拟机。 可以更方便地构建面向大型计算、大数据、容器化工作负荷的大规模服务。

本文将指导你完成在 Azure Stack Marketplace 中提供规模集的过程。 完成此过程之后，用户将可以将虚拟机规模集添加到其订阅。

Azure Stack 上的虚拟机规模集与 Azure 上的虚拟机规模集类似。 有关详细信息，请参阅以下视频：
- [Mark Russinovich talks Azure scale sets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)（Mark Russinovich 谈论 Azure 规模集）
- [Guy Bowerman 介绍虚拟机规模集](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

在 Azure Stack 上，虚拟机规模集不支持自动缩放。 可以使用 Azure Stack 门户、资源管理器模板或 PowerShell 将更多实例添加到规模集。

## <a name="prerequisites"></a>先决条件
- **Powershell 和工具**

   安装并配置适用于 Azure Stack 的 PowerShell 和 Azure Stack 工具。 请参阅[在 Azure Stack 中启动并运行 PowerShell](azure-stack-powershell-configure-quickstart.md)。

   安装 Azure Stack 工具后，请确保导入以下 PowerShell 模块（路径相对于 AzureStack-Tools-master 文件夹中的 .\ComputeAdmin 文件夹）：

        Import-Module .\AzureStack.ComputeAdmin.psm1

- **操作系统映像**

   若要支持 Linux，请下载 Ubuntu Server 16.04 并使用带以下参数的 ```Add-AzsVMImage``` 添加它：```-publisher "Canonical" -offer "UbuntuServer" -sku "16.04-LTS"```。

## <a name="add-the-virtual-machine-scale-set"></a>添加虚拟机规模集

针对环境编辑以下 PowerShell 脚本，然后运行该脚本，以便将虚拟机规模集添加到 Azure Stack 应用商店。 

``$User`` 是用于连接管理员门户的帐户。 例如，serviceadmin@contoso.partner.onmschina.cn。

```
$Arm = "https://adminmanagement.local.azurestack.external"
$Location = "local"

Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint $Arm

$Password = ConvertTo-SecureString -AsPlainText -Force "<your Azure Stack administrator password>"

$User = "<your Azure Stack service administrator user name>"

$Creds =  New-Object System.Management.Automation.PSCredential $User, $Password

$AzsEnv = Get-AzureRmEnvironment AzureStackAdmin
$AzsEnvContext = Add-AzureRmAccount -Environment $AzsEnv -Credential $Creds

Select-AzureRmSubscription -SubscriptionName "Default Provider Subscription"

Add-AzsVMSSGalleryItem -Location $Location
```

## <a name="remove-a-virtual-machine-scale-set"></a>删除虚拟机规模集

若要删除虚拟机规模集库项，请运行以下 PowerShell 命令：

    Remove-AzsVMSSGalleryItem

> [!NOTE]
> 库项可能不会立即删除。 可能需要刷新门户几次，该项才会从 Marketplace 中删除。


## <a name="next-steps"></a>后续步骤
[Azure Stack 常见问题解答](azure-stack-faq.md)


<!-- Update_Description: wording update -->