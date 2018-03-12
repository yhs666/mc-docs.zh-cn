---
title: "将默认的 VM 映像添加到 Azure Stack Marketplace | Microsoft Docs"
description: "将 Windows Server 2016 VM 默认映像添加到 Azure Stack Marketplace。"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 2849E53F-3D58-48A5-8007-3238FC39F630
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 01/23/2018
ms.date: 03/01/2018
ms.author: v-junlch
ms.openlocfilehash: 94e9a45337d687033cedd86d8d466c825a1f2d54
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="add-the-windows-server-2016-vm-image-to-the-azure-stack-marketplace"></a>将 Windows Server 2016 VM 映像添加到 Azure Stack Marketplace

默认情况下，Azure Stack Marketplace 中不会提供任何虚拟机映像。 Azure Stack 操作员必须先将映像添加到 Marketplace，然后才能让用户访问该映像。 可使用以下方法之一，将 Windows Server 2016 映像添加到 Azure Stack Marketplace：

- [从 Azure Marketplace 下载映像](#add-the-image-by-downloading-it-from-the-azure-marketplace)。 如果在连接方案中执行操作，并且已向 Azure 注册 Azure Stack 实例，请使用此选项。

- [使用 PowerShell 添加映像](#add-the-image-by-using-powershell)。 如果在断开连接或连接受限的方案中部署 Azure Stack，请使用此选项。

## <a name="add-the-image-by-downloading-it-from-the-azure-marketplace"></a>通过从 Azure Marketplace 下载映像来添加映像

1. 部署 Azure Stack，然后登录到 Azure Stack 开发工具包。

2. 单击“更多服务” > “Marketplace 管理” > “从 Azure 添加”。 

3. 查找或搜索 **Windows Server 2016 Datacenter** 映像，然后选择“下载”。

   ![从 Azure 下载映像](./media/azure-stack-add-default-image/download-image.png)

下载完成后，“Marketplace 管理”中会显示该映像。 该映像也可以从“计算”获取，并可用于创建新的虚拟机。

## <a name="add-the-image-by-using-powershell"></a>使用 PowerShell 添加映像

### <a name="prerequisites"></a>先决条件 

如果已[通过 VPN 建立连接](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)，请通过[开发工具包](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop)或基于 Windows 的外部客户端运行以下先决条件操作：

1. 安装 [Azure Stack 兼容的 Azure PowerShell 模块](azure-stack-powershell-install.md)。  

2. 下载[使用 Azure Stack 所需的工具](azure-stack-powershell-download.md)。  

3. 在“Windows Server 评估”页上，[下载 Windows Server 2016 评估版](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016)。 出现提示时，选择下载项的 ISO 版本。 记录下载位置的路径，以便在本文稍后介绍的步骤中使用。 此步骤需要建立 Internet 连接。  

### <a name="add-the-image-to-the-azure-stack-marketplace"></a>将映像添加到 Azure Stack Marketplace
   
1. 使用以下命令导入 Azure Stack Connect 和 ComputeAdmin 模块：

   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # Import the Connect and ComputeAdmin modules.   
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   ```

2. 登录到 Azure Stack 环境。 运行以下脚本之一，具体取决于部署 Azure Stack 环境时是使用了 Azure Active Directory (Azure AD) 还是 Active Directory 联合身份验证服务 (AD FS)。 （根据环境配置替换 Azure AD `tenantName`、`GraphAudience` 终结点和 `ArmEndpoint` 值。）  

   * **Azure Active Directory**。 使用以下 cmdlet：

      ```PowerShell
      # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "<Resource Manager endpoint for your environment>"

      # For Azure Stack Development Kit, this value is set to https://graph.chinacloudapi.cn/. To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "<GraphAuidence endpoint for your environment>"
      
      # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
      Add-AzureRMEnvironment `
        -Name "AzureStackAdmin" `
        -ArmEndpoint $ArmEndpoint

      Set-AzureRmEnvironment `
        -Name "AzureStackAdmin" `
        -GraphAudience $GraphAudience

      $TenantID = Get-AzsDirectoryTenantId `
        -AADTenantName "<myDirectoryTenantName>.partner.onmschina.cn" `
        -EnvironmentName AzureStackAdmin

      Login-AzureRmAccount `
        -EnvironmentName "AzureStackAdmin" `
        -TenantId $TenantID 
      ```

   * **Active Directory 联合身份验证服务**。 使用以下 cmdlet：
    
      ```PowerShell
      # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "<Resource Manager endpoint for your environment>"

      # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "<GraphAuidence endpoint for your environment>"

      # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
      Add-AzureRMEnvironment `
        -Name "AzureStackAdmin" `
        -ArmEndpoint $ArmEndpoint

      Set-AzureRmEnvironment `
        -Name "AzureStackAdmin" `
        -GraphAudience $GraphAudience `
        -EnableAdfsAuthentication:$true

      $TenantID = Get-AzsDirectoryTenantId `
      -ADFS `
      -EnvironmentName "AzureStackAdmin" 

      Login-AzureRmAccount `
        -EnvironmentName "AzureStackAdmin" `
        -TenantId $TenantID 
      ```
   
3. 将 Windows Server 2016 映像添加到 Azure Stack Marketplace。 （将 *fully_qualified_path_to_ISO* 替换为下载的 Windows Server 2016 ISO 的路径。）

    ```PowerShell
    $ISOPath = "<fully_qualified_path_to_ISO>"

    # Add a Windows Server 2016 Evaluation VM image.
    New-AzsServer2016VMImage `
      -ISOPath $ISOPath

    ```

若要确保 Windows Server 2016 VM 映像具有最新的累积更新，请在运行 `New-AzsServer2016VMImage` cmdlet 时包含 `IncludeLatestCU` 参数。 有关 `New-AzsServer2016VMImage` cmdlet 允许的参数的信息，请参阅[参数](#parameters)。 将映像发布到 Azure Stack Marketplace 大约需要一小时。 

## <a name="parameters-for-new-azsserver2016vmimage"></a>New-AzsServer2016VMImage 的参数

### <a name="new-azsserver2016vmimage"></a>New-AzsServer2016VMImage 

创建并上传新的 Server 2016 核心 (Core) 映像或完整映像，并为其创建 Marketplace 项。

| parameters | 必须 | 示例 | 说明 |
|-----|-----|------|---- |
|ISOPath|是| N:\ISO\en_windows_16_x64_dvd | 下载的 Windows Server 2016 ISO 的完全限定路径。|
|Net35|否| True | 在 Windows Server 2016 映像中安装 .NET 3.5 运行时。 此值默认设置为 **true**。|
|版本|否| 完整 |  指定 **Core**、**Full** 或 **Both** Windows Server 2016 映像。 此值默认设置为 **Full**。|
|VHDSizeInMB|否| 40,960 | 设置要添加到 Azure Stack 环境的 VHD 映像的大小 (MB)。 此值默认设置为 40,960 MB。|
|CreateGalleryItem|否| True | 指定是否要为 Windows Server 2016 映像创建 Marketplace 项。 此值默认设置为 **true**。|
|location |否 | D:\ | 指定 Windows Server 2016 映像要发布到的位置。|
|IncludeLatestCU|否| False | 将最新的 Windows Server 2016 累积更新应用到新 VHD。 检查脚本，确保它指向最新的更新，或使用以下两个选项中的一个。 |
|CUUri |否 | https://yourupdateserver/winservupdate2016 | 设置从特定 URI 运行 Windows Server 2016 累积更新。 |
|CUPath |否 | C:\winservupdate2016 | 设置从本地路径运行 Windows Server 2016 累积更新。 如果在断开连接的环境中部署 Azure Stack 实例，此选项相当有用。|

## <a name="next-steps"></a>后续步骤

[预配虚拟机](azure-stack-provision-vm.md)

