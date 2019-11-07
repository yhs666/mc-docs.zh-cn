---
title: Azure 自动化 Runbook 错误故障排除
description: 了解如何解决 Azure 自动化 Runbook 的问题
services: automation
author: WenJason
ms.author: v-jay
origin.date: 01/24/2019
ms.date: 10/21/2019
ms.topic: conceptual
ms.service: automation
manager: digimobile
ms.openlocfilehash: 2ac23a738c76d723a12223e70b9cb5f2ae5794ea
ms.sourcegitcommit: cb2caa72ec0e0922a57f2fa1056c25e32c61b570
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142097"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Runbook 错误故障排除

在 Azure 自动化中执行 Runbook 出现错误时，可使用以下步骤来诊断问题。

1. **确保 Runbook 脚本在本地计算机上执行成功：** 请参阅 [PowerShell 文档](https://docs.microsoft.com/powershell/scripting/overview)或 [Python 文档](https://docs.python.org/3/)，了解语言参考和学习模块。

   在本地执行脚本即可发现并解决常见错误，例如：

   - **缺少模块**
   - **语法错误**
   - **逻辑错误**

2. **调查**特定消息的 runbook [错误流](/automation/automation-runbook-output-and-messages#runbook-output)并将其与下面的错误进行比较。

3. **确保节点和自动化工作区具有所需的模块。**

如果 Runbook 挂起或意外失败：

* [检查作业状态](/automation/automation-runbook-execution#job-statuses)，其中阐明了 runbook 状态和一些可能的原因。
* [将更多输出添加到](/automation/automation-runbook-output-and-messages#message-streams) runbook，以确定 runbook 挂起之前发生了什么情况。
* [处理由作业引发的任何异常](/automation/automation-runbook-execution#handling-exceptions)。

## <a name="login-azurerm"></a>场景：运行 Login-AzureRMAccount 以登录

### <a name="issue"></a>问题

执行 runbook 时收到以下错误：

```error
Run Login-AzureRMAccount to login.
```

### <a name="cause"></a>原因

如果你使用的不是运行方式帐户或者运行方式帐户已过期，则可能会发生此错误。 请参阅[管理 Azure 自动化运行方式帐户](/automation/manage-runas-account)。

此错误有两个主要原因：

* AzureRM 模块版本不同。
* 你正在尝试访问不同订阅中的资源。

### <a name="resolution"></a>解决方法

如果在更新一个 AzureRM 模块后收到此错误，应将所有 AzureRM 模块更新为同一版本。

如果你正在尝试访问另一订阅中的资源，可遵循以下步骤配置权限。

1. 转到自动化帐户的运行方式帐户，并复制应用程序 ID 和指纹。
  ![复制应用程序 ID 和指纹](../media/troubleshoot-runbooks/collect-app-id.png)
1. 访问未托管自动化帐户的订阅的访问控制，并添加新的角色分配。
  ![访问控制](../media/troubleshoot-runbooks/access-control.png)
1. 添加在上一步骤中收集的应用程序 ID。 选择“参与者”权限。
   ![添加角色分配](../media/troubleshoot-runbooks/add-role-assignment.png)
1. 复制订阅名称，以便在下一步骤中使用。
1. 现在，可以使用以下 runbook 代码来测试从自动化帐户访问其他订阅的权限。

    将“\<CertificateThumbprint \>”替换为在步骤 #1 中复制的值，将“\<SubscriptionName\>”替换为在步骤 #4 中复制的值。

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint "<CertificateThumbprint>" -EnvironmentName AzureChinaCloud
    #Select the subscription you want to work with
    Select-AzureRmSubscription -SubscriptionName '<YourSubscriptionNameGoesHere>'

    #Test and get outputs of the subscriptions you granted access.
    $subscriptions = Get-AzureRmSubscription
    foreach($subscription in $subscriptions)
    {
        Set-AzureRmContext $subscription
        Write-Output $subscription.Name
    }
    ```

## <a name="unable-to-find-subscription"></a>场景：无法找到 Azure 订阅

### <a name="issue"></a>问题

使用 `Select-AzureSubscription` 或 `Select-AzureRmSubscription` cmdlet 时收到以下错误：

```error
The subscription named <subscription name> cannot be found.
```

### <a name="error"></a>错误

以下情况下可能发生此错误：

* 订阅名称无效

* 尝试获取订阅详细信息的 Azure Active Directory 用户未配置为订阅的管理员。

### <a name="resolution"></a>解决方法

执行以下步骤以确定是否已正确向 Azure 进行身份验证并有权访问尝试选择的订阅：

1. 在 Azure 自动化之外测试脚本，以确保它独立运行。
2. 请确保在运行 `Select-AzureSubscription` cmdlet 之前运行 `Add-AzureAccount` cmdlet。
3. 将 `Disable-AzureRmContextAutosave –Scope Process` 添加到 runbook 的开头。 此 cmdlet 可以确保任何凭据都仅适用于当前 runbook 的执行。
4. 如果仍然看到此错误消息，可通过在 `Add-AzureAccount` cmdlet 后添加 **AzureRmContext** 参数来修改代码，然后执行代码。

   ```powershell
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint -EnvironmentName AzureChinaCloud

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

## <a name="auth-failed-mfa"></a>场景：无法向 Azure 进行身份验证，因为已启用多重身份验证

### <a name="issue"></a>问题

使用 Azure 用户名和密码向 Azure 进行身份验证时收到以下错误：

```error
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

### <a name="cause"></a>原因

如果对 Azure 帐户设置了多重身份验证，则不能使用 Azure Active Directory 用户向 Azure 进行身份验证。 而只能使用证书或服务主体向 Azure 进行身份验证。

### <a name="resolution"></a>解决方法

要将证书用于 Azure 经典部署模型 cmdlet，请参阅[创建并添加管理 Azure 服务所需的证书](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx)。 若要将服务主体用于 Azure Resource Manager cmdlet，请参阅[使用 Azure 门户创建服务主体](../../active-directory/develop/howto-create-service-principal-portal.md)和[通过 Azure Resource Manager 对服务主体进行身份验证](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)。

## <a name="get-serializationsettings"></a>场景：作业流中出现了关于 get_SerializationSettings 方法的错误

### <a name="issue"></a>问题

执行 runbook 时，runbook 无法管理 Azure 资源。

### <a name="cause"></a>原因

Runbook 在运行时没有使用正确的上下文。

### <a name="resolution"></a>解决方法

如果使用多个订阅，则调用 Runbook 时可能会丢失订阅上下文。 若要确保将订阅上下文传递给 Runbook，请将 `AzureRmContext` 参数添加到 cmdlet 并将上下文传递给它。 还建议将 `Disable-AzureRmContextAutosave` cmdlet 与 **Process** 范围配合使用来确保你使用的凭据仅用于当前 runbook。

```azurepowershell
# Ensures that any credentials apply only to the execution of this runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint `
    -EnvironmentName AzureChinaCloud

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

有关详细信息，请参阅[使用多个订阅](../automation-runbook-execution.md#working-with-multiple-subscriptions)。

## <a name="not-recognized-as-cmdlet"></a>场景：字词未识别为 cmdlet、函数、脚本的名称

### <a name="issue"></a>问题

Runbook 失败并显示错误：

```error
The job was tried three times but it failed
```

### <a name="cause"></a>原因

发生此错误是由于以下问题之一：

* 内存限制。 可以在[自动化服务限制](../../azure-subscription-service-limits.md#automation-limits)中找到有关分配给沙盒的内存量的限制。 如果作业使用的内存超过 400 MB，则它可能会失败。

* 网络套接字。 如[自动化服务限制](../../azure-subscription-service-limits.md#automation-limits)中所述，Azure 沙盒限制为 1000 个并发网络套接字。

* 模块不兼容。 如果模块依赖关系不正确，则可能会发生此错误，并且如果它们不正确，则 runbook 通常会返回“找不到命令”或“无法绑定参数”消息。

* Runbook 尝试调用在 Azure 沙盒中运行的 Runbook 中的可执行文件或子过程。 Azure 沙盒不支持此方案。

* runbook 尝试向输出流写入太多异常数据。

### <a name="resolution"></a>解决方法

下述解决方案中的任何一种都可以解决此问题：

* 在内存限制内工作的建议方法是拆分多个 runbook 之间的工作负载，不作为内存中的诸多数据进行处理，不写入不必要的 runbook 输出，或考虑在 PowerShell 工作流 runbook 中写入多少个检查点。 可以使用 clear 方法（例如 `$myVar.clear()`）清除变量并使用 `[GC]::Collect()` 立即运行垃圾回收。 这将减少运行时期间 runbook 的内存占用情况。

* 在[混合 runbook 辅助角色](../automation-hrw-run-runbooks.md)中运行 runbook。 混合辅助角色不受内存和网络限制，而 Azure 沙盒则受限于此限制。

* 如果需要在 Runbook 中调用进程（如 .exe 或 subprocess.call），则需要在[混合 Runbook 辅助角色](../automation-hrw-run-runbooks.md)上运行 Runbook。

* 作业输出流限制为 1MB。 请确保在 try/catch 块中包含对可执行文件或子进程的调用。 如果这些调用引发异常，请将该异常中的消息写入自动化变量中。 这将防止将其写入作业输出流中。

## <a name="sign-in-failed"></a>场景：登录 Azure 帐户失败

### <a name="issue"></a>问题

使用 `Add-AzureAccount` 或 `Connect-AzureRmAccount` cmdlet 时收到以下错误之一：

```error
Unknown_user_type: Unknown User Type
```

```error
No certificate was found in the certificate store with thumbprint
```

### <a name="cause"></a>原因

如果凭据资产名称无效，将发生此错误。 如果用于设置自动化凭据资产的用户名和密码无效，也可能会出现此错误。

### <a name="resolution"></a>解决方法

要确定具体错误，请执行以下步骤：

1. 请确保没有包含任何特殊字符。 这些字符包括用于连接 Azure 的自动化凭据资产名称中的 \@ 字符  。  
2. 查看你是否能够在本地 PowerShell ISE 编辑器中使用存储在 Azure 自动化凭据中的用户名和密码。 可以通过在 PowerShell ISE 中运行以下 cmdlet 来检查用户名和密码是否正确：

   ```powershell
   $Cred = Get-Credential
   #Using Azure Service Management
   Add-AzureAccount -Environment AzureChinaCloud -Credential $Cred  
   #Using Azure Resource Manager
   Connect-AzureRmAccount -EnvironmentName AzureChinaCloud -Credential $Cred
   ```

3. 如果无法在本地进行身份验证，则意味着你尚未设置好 Azure Active Directory 凭据。 请参阅 [使用 Azure Active Directory 向 Azure 进行身份验证](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) 博客文章，了解如何正确设置 Azure Active Directory 帐户。

4. 如果它看起来是暂时性错误，请尝试向身份验证例程添加重试逻辑，使身份验证更加可靠。

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       #Logging in to Azure...
       $connectionResult = Connect-AzureRmAccount `
                              -EnvironmentName AzureChinaCloud `
                              -ServicePrincipal `
                              -TenantId $servicePrincipalConnection.TenantId `
                              -ApplicationId $servicePrincipalConnection.ApplicationId `
                              -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }
   ```

## <a name="child-runbook-object"></a>对象引用未设置为对象的实例

### <a name="issue"></a>问题

使用 `-Wait` 开关调用子 runbook 并且输出流包含对象时，会收到以下错误：

```error
Object reference not set to an instance of an object
```

### <a name="cause"></a>原因

有一个已知问题，如果 [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) 包含对象，则它无法正确处理输出流。

### <a name="resolution"></a>解决方法

要解决此问题，建议改为实现轮询逻辑并使用 [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) cmdlet 来检索输出。 下面的示例中定义了此逻辑的示例。

```powershell
$automationAccountName = "ContosoAutomationAccount"
$runbookName = "ChildRunbookExample"
$resourceGroupName = "ContosoRG"

function IsJobTerminalState([string] $status) {
    return $status -eq "Completed" -or $status -eq "Failed" -or $status -eq "Stopped" -or $status -eq "Suspended"
}

$job = Start-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName -Name $runbookName -ResourceGroupName $resourceGroupName
$pollingSeconds = 5
$maxTimeout = 10800
$waitTime = 0
while((IsJobTerminalState $job.Status) -eq $false -and $waitTime -lt $maxTimeout) {
   Start-Sleep -Seconds $pollingSeconds
   $waitTime += $pollingSeconds
   $job = $job | Get-AzureRmAutomationJob
}

$jobResults | Get-AzureRmAutomationJobOutput | Get-AzureRmAutomationJobOutputRecord | Select-Object -ExpandProperty Value
```

## <a name="fails-deserialized-object"></a>场景：Runbook 因反序列化的对象而失败

### <a name="issue"></a>问题

Runbook 失败并显示错误：

```error
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

### <a name="cause"></a>原因

如果 runbook 为 PowerShell 工作流，它会将复杂对象以反序列化格式进行存储，以便在工作流暂停的情况下保留 runbook 状态。

### <a name="resolution"></a>解决方法

下述三种解决方案中的任何一种都可以解决此问题：

* 如果要将复杂对象从一个 cmdlet 传送到另一个 cmdlet，则可将这两个 cmdlet 包装在 InlineScript 中。
* 传递复杂对象中你所需要的名称或值，不必传递整个对象。
* 使用 PowerShell Runbook，而不使用 PowerShell 工作流 Runbook。

## <a name="quota-exceeded"></a>场景：Runbook 作业失败，因为超过了分配的配额

### <a name="issue"></a>问题

Runbook 作业失败并显示错误：

```error
The quota for the monthly total job run time has been reached for this subscription
```

### <a name="cause"></a>原因

当作业执行时间超过你帐户的 500 分钟免费配额时，就会出现此错误。 此配额适用于所有类型的作业执行任务。 其中一些任务可能是测试作业、从门户启动作业、使用 Webhook 执行作业，以及通过 Azure 门户或数据中心计划要执行的作业。 若要详细了解自动化的定价，请参阅[自动化定价](https://azure.cn/pricing/details/automation/)。

### <a name="resolution"></a>解决方法

如果想要每月使用 500 分钟以上的处理时间，则需将订阅从免费层改为基本层。 可以通过下述步骤升级到基本层：

1. 登录到 Azure 订阅
2. 选择要升级的自动化帐户
3. 单击“设置” > “定价”   。
4. 单击页面底部的“启用”  ，以将帐户升级到“基本”  层。

## <a name="cmdlet-not-recognized"></a>场景：在执行 Runbook 时无法识别 cmdlet

### <a name="issue"></a>问题

Runbook 作业失败并显示错误：

```error
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

### <a name="cause"></a>原因

当 PowerShell 引擎找不到要在 runbook 中使用的 cmdlet 时，则会导致此错误。 此错误可能是因为，帐户中缺少包含该 cmdlet 的模块、与 Runbook 名称冲突，或者该 cmdlet 也存在于其他模块中，而自动化无法解析该名称。

### <a name="resolution"></a>解决方法

下述解决方案中的任何一种都可以解决此问题：

* 检查输入的 cmdlet 名称是否正确。
* 确保 cmdlet 存在于自动化帐户中，且没有冲突。 要验证 cmdlet 是否存在，请在编辑模式下打开 Runbook，并搜索希望在库中找到的 cmdlet，或者运行 `Get-Command <CommandName>`。 验证该 cmdlet 可供帐户使用且与其他 cmdlet 或 runbook 不存在名称冲突以后，可将其添加到画布上，并确保使用的是 runbook 中的有效参数集。
* 如果存在名称冲突且 cmdlet 可在两个不同的模块中使用，则可使用 cmdlet 的完全限定名称来解决此问题。 例如，可以使用 ModuleName\CmdletName  。
* 如果是在本地执行混合辅助角色组中的 runbook，则请确保模块和 cmdlet 已安装在托管混合辅助角色的计算机上。

## <a name="long-running-runbook"></a>场景：长时间运行的 Runbook 无法完成

### <a name="issue"></a>问题

运行 3 小时后，runbook 将显示处于“已停止”状态  。 可能还会收到错误：

```error
The job was evicted and subsequently reached a Stopped state. The job cannot continue running
```

此行为是 Azure 沙盒的设计使然，因为 Azure 自动化中会对进程进行“公平份额”监视。 “公平份额”会自动停止执行时间超过 3 小时的 Runbook。 超过公平份额时间限制的 runbook 的状态因 runbook 类型而异。 PowerShell 和 Python runbook 设置为“已停止”状态  。 PowerShell 工作流 runbook 设置为“失败”  。

### <a name="cause"></a>原因

Runbook 超出了 Azure 沙盒中公平份额允许的 3 小时限制。

### <a name="resolution"></a>解决方法

建议的解决方案是在[混合 Runbook 辅助角色](../automation-hrw-run-runbooks.md)上运行 runbook。

混合辅助角色不受[公平份额](../automation-runbook-execution.md#fair-share) 3 小时 runbook 限制，而 Azure 沙盒受限于此限制。 应开发在混合 Runbook 辅助角色上运行的 Runbook，以便在出现意外的本地基础结构问题时支持重启行为。

另一种选择是通过创建[子 runbook](../automation-child-runbooks.md) 来优化 runbook。 如果 runbook 在多个资源上遍历同一函数，例如在多个数据库上执行某个数据库操作，则可将该函数移到子 runbook。 各个子 runbook 是在单独的进程中并行执行的。 此行为降低了完成父 runbook 所需的时间总量。

启用子 runbook 方案的 PowerShell cmdlet 是：

[Start-AzureRMAutomationRunbook](https://docs.microsoft.com/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) - 此 cmdlet 用于启动 runbook 并向其传递参数

[Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) - 在子 runbook 完成后，如果需要执行操作，可使用此 cmdlet 检查每个子 runbook 的作业状态。

## <a name="expired webhook"></a>场景：状态：调用 Webhook 时显示 400 错误请求

### <a name="issue"></a>问题

在尝试调用 Azure 自动化 Runbook 的 Webhook 时，收到以下错误：

```error
400 Bad Request : This webhook has expired or is disabled
```

### <a name="cause"></a>原因

尝试调用的 Webhook 已禁用，或者已过期。

### <a name="resolution"></a>解决方法

如果 Webhook 处于禁用状态，可以通过 Azure 门户重新启用 Webhook。 如果 Webhook 已过期，需要删除并重新创建 Webhook。 如果尚未过期，只能[续订 Webhook](../automation-webhooks.md#renew-webhook)。

## <a name="429"></a>场景：429：当前的请求速率过大。 请重试

### <a name="issue"></a>问题

在运行 `Get-AzureRmAutomationJobOutput` cmdlet 时收到以下错误消息：

```error
429: The request rate is currently too large. Please try again
```

### <a name="cause"></a>原因

从具有多个[详细流](../automation-runbook-output-and-messages.md#verbose-stream)的 Runbook 中检索作业输出时，可能发生此错误。

### <a name="resolution"></a>解决方法

可通过两种方法来解决此错误：

* 编辑 Runbook，并减少它发出的作业流数量。
* 减少运行 cmdlet 时要检索的流数量。 若要遵循此行为，可以为 `Get-AzureRmAutomationJobOutput` cmdlet 指定 `-Stream Output` 参数以仅检索输出流。 

## <a name="cannot-invoke-method"></a>场景：PowerShell 作业失败，出现错误：无法调用方法

### <a name="issue"></a>问题

在 Runbook（在 Azure 中运行）中启动 PowerShell 作业时，收到以下错误消息：

```error
Exception was thrown - Cannot invoke method. Method invocation is supported only on core types in this language mode.
```

### <a name="cause"></a>原因

在 Runbook（在 Azure 中运行）中启动 PowerShell 作业时，可能会发生此错误。 出现此行为可能是因为在 Azure 沙盒中运行的 Runbook 可能未在[完整语言模式](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_language_modes)下运行。

### <a name="resolution"></a>解决方法

可通过两种方法来解决此错误：

* 使用 `Start-AzureRmAutomationRunbook` 而不是 `Start-Job` 来启动 Runbook
* 如果 Runbook 出现此错误消息，请在混合 Runbook 辅助角色上运行它

若要详细了解 Azure 自动化 Runbook 的此行为和其他行为，请参阅 [Runbook 行为](../automation-runbook-execution.md#runbook-behavior)。

## <a name="other"></a>上面未列出我的问题

为帮助你解决问题，以下部分列出了支持文档中未列出的其他常见错误。

## <a name="hybrid-runbook-worker-doesnt-run-jobs-or-isnt-responding"></a>混合 runbook 辅助角色不运行作业，或者没有响应

如果你使用混合辅助角色来运行作业，而不是在 Azure 自动化中运行作业，则可能需要[排查混合辅助角色本身的问题](/automation/troubleshoot/hybrid-runbook-worker)。

## <a name="runbook-fails-with-no-permission-or-some-variation"></a>Runbook 由于“无权限”或某个变动而失败

运行方式帐户对 Azure 资源的权限可能与当前帐户的权限不同。 请确保运行方式帐户[有权访问你的脚本中使用的任何资源](/role-based-access-control/role-assignments-portal)。

## <a name="runbooks-were-working-but-suddenly-stopped"></a>Runbook 在工作时突然停止

* 如果 runbook 之前在执行但突然停止，请[确保运行方式帐户未过期](/automation/manage-runas-account#cert-renewal)
* 如果你使用 webhook 来启动 runbook，请[确保 webhook 未过期](/automation/automation-webhooks#renew-webhook)。

## <a name="issues-passing-parameters-into-webhooks"></a>将参数传递到 webhook 时出现问题

如果在将参数传递到 webhook 时需要帮助，请参阅[从 webhook 启动 runbook](/automation/automation-webhooks#parameters)。

## <a name="inconsistent-behavior-in-runbooks"></a>Runbook 中的行为不一致

按 [Runbook 执行](/automation/automation-runbook-execution#runbook-behavior)中的指南操作，避免并发作业出现问题、资源被创建多次或 Runbook 中出现其他计时敏感逻辑。

## <a name="runbook-fails-with-the-errors-no-permission-forbidden-403-or-some-variation"></a>Runbook 失败并显示错误：“无权限”、“已禁止”、403 或其他错误

运行方式帐户对 Azure 资源的权限可能与当前帐户的权限不同。 请确保运行方式帐户[有权访问你的脚本中使用的任何资源](/role-based-access-control/role-assignments-portal)。

## <a name="runbooks-were-working-but-suddenly-stopped"></a>Runbook 在工作时突然停止

* 如果 runbook 之前在执行但突然停止，请确保运行方式帐户[未过期](/automation/manage-runas-account#cert-renewal)。
* 如果使用 webhook 来启动 runbook，请确保 webhook [未过期](/automation/automation-webhooks#renew-webhook)。

## <a name="passing-parameters-into-webhooks"></a>将参数传递到 webhook

如果在将参数传递到 webhook 时需要帮助，请参阅[从 webhook 启动 runbook](/automation/automation-webhooks#parameters)。

## <a name="using-self-signed-certificates"></a>使用自签名证书

若要使用自签名证书，必须遵循[创建新证书](/automation/shared-resources/certificates#creating-a-new-certificate)中的指导。

## <a name="recommended-documents"></a>建议的文档

* [在 Azure 自动化中启动 Runbook](/automation/automation-starting-a-runbook)
* [在 Azure 自动化中执行 Runbook](/automation/automation-runbook-execution)

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 通过 [Azure 论坛](https://social.msdn.microsoft.com/Forums/zh-cn/home)获取 Azure 专家的解答
* 如需更多帮助，可以提交 Azure 支持事件。 转到 [Azure 支持站点](https://www.azure.cn/support/)。