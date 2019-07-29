---
title: 在 Azure 自动化中管理模块
description: 本文介绍如何在 Azure 自动化中管理模块。
services: automation
ms.service: automation
author: WenJason
ms.author: v-jay
origin.date: 06/05/2019
ms.date: 07/22/2019
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: a59ca52ac1c8edd8c435cc3b700ea64a54c2c529
ms.sourcegitcommit: 2a020ee232b901b13c9f1c4d27ad65228a34d58b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2019
ms.locfileid: "68391974"
---
# <a name="manage-modules-in-azure-automation"></a>在 Azure 自动化中管理模块

Azure 自动化提供相应的功能用于将 PowerShell 模块导入到基于 PowerShell 的 Runbook 所用的自动化帐户。 这些模块可以是创建的自定义模块，或适用于 Azure 的 AzureRM 和 Az 模块。

## <a name="import-modules"></a>导入模块

可通过多种方法将模块导入到自动化帐户。 以下部分介绍了不同的模块导入方法。

> [!NOTE]
> 在 Azure 自动化中使用的模块中文件的最大路径长度为 140 个字符。 如果任何路径的长度超过 140 个字符，则无法使用 `Import-Module` 将其导入到 PowerShell 会话。

### <a name="powershell"></a>PowerShell

可以使用 [New-AzureRmAutomationModule](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationmodule) 将模块导入到自动化帐户。 该 cmdlet 采用某个模块 zip 包的 URL。

```powershell
New-AzureRmAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

### <a name="azure-portal"></a>Azure 门户

在 Azure 门户中导航到你的自动化帐户，然后选择“共享资源”下的“模块”。   单击“+ 添加模块”。  选择包含你的模块的 **.zip** 文件，然后单击“确定”开始执行导入过程。 

## <a name="internal-cmdlets"></a>内部 cmdlet

下面是导入到每个自动化帐户的内部 `Orchestrator.AssetManagement.Cmdlets` 模块中的 cmdlet 列表。 可在 Runbook 和 DSC 配置中访问这些 cmdlet，使用它们可与自动化帐户中的资产进行交互。 此外，使用内部 cmdlet 还可以从加密的“变量”值、“凭据”和加密的“连接”字段中检索机密。    Azure PowerShell cmdlet 无法检索这些机密。 无需隐式连接到 Azure 即可使用这些 cmdlet。 如果需要使用某个连接（例如运行方式帐户）在 Azure 中进行身份验证，则这些内部 cmdlet 就非常有利。

|Name|说明|
|---|---|
|Get-AutomationCertificate|`Get-AutomationCertificate [-Name] <string> [<CommonParameters>]`|
|Get-AutomationConnection|`Get-AutomationConnection [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]` |
|Get-AutomationPSCredential|`Get-AutomationPSCredential [-Name] <string> [<CommonParameters>]` |
|Get-AutomationVariable|`Get-AutomationVariable [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]`|
|Set-AutomationVariable|`Set-AutomationVariable [-Name] <string> -Value <Object> [<CommonParameters>]` |
|Start-AutomationRunbook|`Start-AutomationRunbook [-Name] <string> [-Parameters <IDictionary>] [-RunOn <string>] [-JobId <guid>] [<CommonParameters>]`|
|Wait-AutomationJob|`Wait-AutomationJob -Id <guid[]> [-TimeoutInMinutes <int>] [-DelayInSeconds <int>] [-OutputJobsTransitionedToRunning] [<CommonParameters>]`|

## <a name="module-best-practices"></a>模块最佳做法

PowerShell 模块可导入到 Azure 自动化中，这样其 cmdlet 就可以在 runbook 中使用，其 DSC 资源就可以在 DSC 配置中使用。 Azure 自动化在后台存储这些模块，在执行 runbook 作业和 DSC 编译作业时将其载入 Azure 自动化沙盒中，并在其中执行 Runbook 以及编译 DSC 配置。 模块中的所有 DSC 资源还会自动放置在 Automation DSC 拉取服务器上。 它们可以在应用 DSC 配置时被计算机拉取。

创作一个要在 Azure 自动化中使用的 PowerShell 模块时，我们建议考虑以下事项：

* 在模块中为每个 cmdlet 提供摘要、说明和帮助 URI。 可以在 PowerShell 中为 cmdlet 定义特定的帮助信息，使用户可以通过 **Get-Help** cmdlet 获取这些 cmdlet 的使用帮助。 以下示例演示如何定义要在 .psm1 模块文件中使用的摘要和帮助 URI：

  ```powershell
  <#
       .SYNOPSIS
        Gets a Contoso User account
  #>
  function Get-ContosoUser {
  [CmdletBinding](DefaultParameterSetName='UseConnectionObject', `
  HelpUri='https://www.contoso.com/docs/information')]
  [OutputType([String])]
  param(
     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $UserName,

     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $Password,

     [Parameter(ParameterSetName='ConnectionObject', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [Hashtable]
     $Connection
  )

  switch ($PSCmdlet.ParameterSetName) {
     "UserAccount" {
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $UserName, $Password
        Connect-Contoso -Credential $cred
     }
     "ConnectionObject" {
        Connect-Contoso -Connection $Connection
    }
  }
  }
  ```

  提供此信息可在 PowerShell 控制台中使用 **Get-Help** cmdlet 显示此帮助。 Azure 门户中也会显示此说明。

  ![集成模块帮助](../media/modules/module-activity-description.png)

* 在模块中定义所有 cmdlet 的输出类型。 定义 cmdlet 的输出类型以后，就可以利用设计时 IntelliSense 来确定 cmdlet 的输出属性，供创作时使用。 这在自动化 runbook 的图形创作过程中特别有用，这种创作过程要求掌握一定程度的设计时知识，以改进用户对模块的使用体验。

  可以通过添加 `[OutputType([<MyOutputType>])]`（其中，MyOutputType 是有效类型）来实现此目的。 若要详细了解 OutputType，请参阅[关于函数 OutputTypeAttribute](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute)。 以下代码示例演示如何将 `OutputType` 添加到 cmdlet：

  ```powershell
  function Get-ContosoUser {
  [OutputType([String])]
  param(
     [string]
     $Parameter1
  )
  # <script location here>
  }
  ```

  ![图形 Runbook 输出类型](../media/modules/runbook-graphical-module-output-type.png)

  此行为类似于 PowerShell ISE 中 cmdlet 输出的“预输入”功能，但不需要运行。

  ![POSH IntelliSense](../media/modules/automation-posh-ise-intellisense.png)

* 使模块中的所有 cmdlet 以无状态方式呈现。 多个 Runbook 作业可在同一个 AppDomain 和同一个进程与沙盒中同时运行。 如果在这两些级别共享了任何状态，则作业可能会相互影响。 此行为可能导致间歇性的且难以诊断的问题。  下面是应避免的处理方式的示例。

  ```powershell
  $globalNum = 0
  function Set-GlobalNum {
     param(
         [int] $num
     )

     $globalNum = $num
  }
  function Get-GlobalNumTimesTwo {
     $output = $globalNum * 2

     $output
  }
  ```

* 该模块应完全包含在能够进行 xcopy 操作的包中。 需要执行 Runbook 时，Azure 自动化模块将分发到自动化沙盒中。 这些模块需要独立于它们在其上运行的​​主机工作。 你应能够压缩并移动模块包，并在导入到其他主机的 PowerShell 环境时使其正常运行。 为了实现这一点，模块不应该依赖于模块文件夹以外的任何文件。 此文件夹是将模块导入 Azure 自动化时压缩的文件夹。 该模块还不应依赖于主机上的任何唯一注册表设置，例如安装产品时设置的那些设置。 该模块中所有文件的路径长度应小于 140 个字符。 长度超过 140 个字符的任何路径将导致导入 Runbook 时出现问题。 如果不遵循此最佳做法，则无法在 Azure 自动化中使用该模块。  

* 如果在模块中引用 [Azure Powershell Az 模块](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.1.0)，请确保没有同时引用 `AzureRM`。 `Az` 模块不能与 `AzureRM` 模块一起使用。 Runbook 支持 `Az`，但默认情况下不会导入。

## <a name="default-modules"></a>默认模块

下表列出了创建自动化帐户时默认导入的模块。 下面列出的模块可以导入其更新版本的模块，但即使你删除了它们的新版本，也无法从自动化帐户中删除原始版本。

|模块名称|版本|
|---|---|
| AuditPolicyDsc | 1.1.0.0 |
| Azure | 1.0.3 |
| Azure.Storage | 1.0.3 |
| AzureRM.Automation | 1.0.3 |
| AzureRM.Compute | 1.2.1 |
| AzureRM.Profile | 1.0.3 |
| AzureRM.Resources | 1.0.3 |
| AzureRM.Sql | 1.0.3 |
| AzureRM.Storage | 1.0.3 |
| ComputerManagementDsc | 5.0.0.0 |
| GPRegistryPolicyParser | 0.2 |
| Microsoft.PowerShell.Core | 0 |
| Microsoft.PowerShell.Diagnostics |  |
| Microsoft.PowerShell.Management |  |
| Microsoft.PowerShell.Security |  |
| Microsoft.PowerShell.Utility |  |
| Microsoft.WSMan.Management |  |
| Orchestrator.AssetManagement.Cmdlets | 1 |
| PSDscResources | 2.9.0.0 |
| SecurityPolicyDsc | 2.1.0.0 |
| StateConfigCompositeResources | 1 |
| xDSCDomainjoin | 1.1 |
| xPowerShellExecutionPolicy | 1.1.0.0 |
| xRemoteDesktopAdmin | 1.1.0.0 |

## <a name="next-steps"></a>后续步骤

* 若要详细了解如何创建 PowerShell 模块，请参阅 [编写 Windows PowerShell 模块](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)