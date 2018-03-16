---
title: "注册 Azure Stack | Microsoft Docs"
description: "将 Azure Stack 注册到 Azure 订阅。"
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/08/2018
ms.date: 03/02/2018
ms.author: v-junlch
ms.openlocfilehash: c4ba38fb50d339ffe70ac284d7bbda7d87b0bc0e
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>将 Azure Stack 注册到 Azure 订阅

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可将 [Azure Stack](azure-stack-poc.md) 注册到 Azure，以便从 Azure 下载 Marketplace 项目，并设置向 Microsoft 报告商业数据的功能。

> [!NOTE]
>之所以建议注册，是因为这样可以测试重要的 Azure Stack 功能，例如 Marketplace 联合和使用情况报告。 注册 Azure Stack 之后，使用情况将报告给 Azure 商业组件。 用于注册的订阅下会显示此信息。 不会向 Azure Stack 开发工具包用户收取使用费用，不管报告的使用量是多少。


## <a name="get-azure-subscription"></a>获取 Azure 订阅

将 Azure Stack 注册到 Azure 之前，必须准备好：

- Azure 订阅的订阅 ID。 若要获取该 ID，请登录到 Azure，单击“更多服务” > “订阅”，单击要使用的订阅，然后就可以在“概要”下找到**订阅 ID**。 目前不支持中国、德国和美国政府云订阅。
- 订阅所有者的帐户用户名和密码（支持 MSA/2FA 帐户）。
- 从 Azure Stack 1712 更新版本 (180106.1) 开始，不一定要对 Azure 订阅使用 Azure Active Directory。 将鼠标悬停在 Azure 门户右上角的头像上即可在 Azure 中找到此目录。

如果没有符合这些要求的 Azure 订阅，可[在此处创建一个 Azure 帐户](https://www.azure.cn/pricing/1rmb-trial)。 注册 Azure Stack 不会对 Azure 订阅收取任何费用。

## <a name="register-azure-stack-with-azure"></a>将 Azure Stack 注册到 Azure  
> [!NOTE]
> 所有这些步骤都必须在可以访问特权终结点的计算机上执行。 就 Azure Stack 开发工具包来说，该计算机就是主机。 如果使用的是集成系统，请联系 Azure Stack 操作员。
>

1. 以管理员身份打开 PowerShell 控制台，然后[安装用于 Azure Stack 的 PowerShell](azure-stack-powershell-install.md)。  

2. 添加用于注册 Azure Stack 的 Azure 帐户。 若要添加此帐户，请运行已将 EnvironmentName 参数设置为 **AzureChinaCloud** 的 `Add-AzureRmAccount` cmdlet。 系统会提示输入 Azure 帐户凭据。可能必须使用双重身份验证，具体取决于帐户的配置。

   ```Powershell
   Add-AzureRmAccount -EnvironmentName "AzureChinaCloud"
   ```

3. 如果有多个订阅，请运行以下命令，选择要使用的那个订阅：  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. 运行以下命令，在 Azure 订阅中注册 Azure Stack 资源提供程序：

   ```Powershell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. 删除与注册相对应的 PowerShell 模块的任何现有版本，然后[从 GitHub 下载最新版本](azure-stack-powershell-download.md)。  

6. 从上一步创建的“AzureStack-Tools-master”目录导航到“Registration”文件夹，然后导入“.\RegisterWithAzure.psm1”模块：  

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

7. 在同一 PowerShell 会话中运行以下脚本：当系统提示输入凭据时，请指定 `azurestack\cloudadmin` 作为用户和密码，该凭据与部署过程中用于本地管理员的凭据相同。  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Set-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development
   ```

   | 参数 | 说明 |  
   |--------|-------------|
   | CloudAdminCredential | 用于[访问特权终结点](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)的云域凭据。 用户名采用 **\<Azure Stack 域\>\cloudadmin** 格式。 使用开发工具包时，用户名设置为 **azurestack\cloudadmin**。 如果使用的是集成系统，请联系 Azure Stack 操作员以获取此值。|  
   | PrivilegedEndpoint | 预先配置的远程 PowerShell 控制台，提供的功能包括日志收集和其他部署后任务。 使用开发工具包时，特权终结点托管在“AzS-ERCS01”虚拟机上。 如果使用的是集成系统，请联系 Azure Stack 操作员以获取此值。 有关详细信息，请参阅[使用特权终结点](azure-stack-privileged-endpoint.md)一文。|  
   | BillingModel | 订阅使用的计费模型。 此参数允许的值：**Capacity**、**PayAsYouUse** 和 **Development**。 使用开发工具包时，此值设置为 **Development**。 如果使用的是集成系统，请联系 Azure Stack 操作员以获取此值。 |

8. 脚本完成后，会看到消息“正在激活 Azure Stack (此步骤可能需要长达 10 分钟的时间才能完成)。”




## <a name="verify-the-registration"></a>验证注册  

1. 登录到管理员门户 (https://adminportal.local.azurestack.external)。

2. 单击“更多服务” > “Marketplace 管理” > “从 Azure 添加”。

3. 如果看到 Azure 提供的项列表（例如 WordPress），则表示激活成功。

> [!NOTE]
> 完成注册后，将不再显示提示未注册的活动警告。


## <a name="modify-the-registration"></a>修改注册

### <a name="change-the-subscription-you-use"></a>更改使用的订阅
若要更改使用的订阅，必须先运行 Remove-AzsRegistration，确保登录到正确的 Azure PowerShell 上下文，然后使用任何已更改的参数来运行 Set-AzsRegistration。

  ```Powershell   
  Remove-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```
### <a name="change-the-billing-model-or-syndication-features"></a>更改计费模型或联合功能
若要更改安装的计费模型或联合功能，可以调用注册函数来设置新值。 不需要先删除当前注册。  

  ```Powershell
     Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```


## <a name="disconnected-registration"></a>断开连接的注册
*本部分的信息适用于 Azure Stack 1712 更新版 (180106.1) 和更高版本，不支持更低的版本。*

若要在断开连接的环境中注册 Azure Stack，需要从 Azure Stack 环境获取注册令牌，然后在可连接到 Azure 的计算机上使用该令牌进行注册。  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>从 Azure Stack 环境获取注册令牌
  1. 若要获取注册令牌，请运行以下命令：  

     ```Powershell
        $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
        $RegistrationToken = Get-AzsRegistrationToken -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
      ```
      > [!TIP]  
      > 注册令牌保存在指定的与 *$env:SystemDrive\RegistrationToken.txt* 相对应的文件中。

  2. 保存此注册令牌，以便在连接 Azure 的计算机上使用。


### <a name="connect-to-azure-and-register"></a>连接到 Azure 并注册
在可以连接到 Azure 的计算机上运行此过程的步骤。

  1. [下载与 GitHub 中的注册相对应的最新 PowerShell 模块](azure-stack-powershell-download.md)。  

  2. 从上一步创建的“AzureStack-Tools-master”目录导航到“Registration”文件夹，然后导入“.\RegisterWithAzure.psm1”模块：  

     ```powershell
     Import-Module .\RegisterWithAzure.psm1
     ```

  3. 将 [RegisterWithAzure.psm1](https://go.microsoft.com/fwlink/?linkid=842959) 复制到某个文件夹，例如 *C:\Temp*。

  4. 以管理员身份启动 PowerShell ISE，然后导入 RegisterWithAzure 模块。  

  5. 确保登录到正确的 Azure PowerShell 上下文，以便通过其注册 Azure Stack 环境：  

     ```Powershell
      Set-AzureRmContext -SubscriptionId $YourAzureSubscriptionId   
     ```
  6. 指定可注册到 Azure 的注册令牌：

     ```Powershell  
       $registrationToken = "*Your Registration Token*"
       Register-AzsEnvironment -RegistrationToken $registrationToken  
     ```
      （可选）可以使用 Get-Content cmdlet，使之指向包含注册令牌的文件：

      ```Powershell  
       $registrationToken = Get-Content -Path 'C:\Temp\<Registration Token File>'
       Register-AzsEnvironment -RegistrationToken $registrationToken  
      ```
> [!NOTE]  
> 保存注册资源名称或注册令牌，供以后参考。

### <a name="remove-a-registered-resource"></a>删除注册的资源
若要删除注册资源，必须使用 UnRegister-AzsEnvironment，并传入用于 Register-AzsEnvironment 的注册资源名称或注册令牌。
- **注册资源名称：**

  ```Powershell    
     UnRegister-AzsEnvironment -RegistrationName "*Name of the registration resource*"
  ```
- **注册令牌：**    

  ```Powershell
     $registrationToken = "*Your copied registration token*"
     UnRegister-AzsEnvironment -RegistrationToken $registrationToken
   ```


## <a name="next-steps"></a>后续步骤
[连接到 Azure Stack](azure-stack-connect-azure-stack.md)

