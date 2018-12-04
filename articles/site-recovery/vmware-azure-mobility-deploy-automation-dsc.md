---
title: 使用 Azure Automation DSC 部署 Site Recovery 移动服务 | Azure
description: 介绍如何使用 Azure Automation DSC 将适用于 VMware VM 和物理机服务器复制的 Azure Site Recovery 移动服务与 Azure 代理自动部署到 Azure
services: site-recovery
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: article
origin.date: 03/05/2018
ms.date: 04/02/2018
ms.author: v-yeche
ms.openlocfilehash: 8bf435a1472598846f6c77d9af4b1db728f3d81a
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52645910"
---
# <a name="deploy-the-mobility-service-with-azure-automation-dsc"></a>使用 Azure Automation DSC 部署移动服务

使用 [Azure Site Recovery](site-recovery-overview.md) 将移动服务部署到要复制到 Azure 的 VMware VM 和物理服务器上。

本文介绍如何使用 Azure 自动化 Desired State Configuration (DSC) 在 Windows 计算机上部署服务，以确保：

## <a name="prerequisites"></a>先决条件

* 一个存储库，用于存储所需设置
* 一个存储库，用于存储注册到管理服务器时所需的密码。 将为特定配置服务器生成唯一密码。 
* 应在要启用保护的计算机上安装 Windows Management Framework (WMF) 5.0。 这是 Automation DSC 的必要条件。
如需将 DSC 用于安装了 WMF 4.0 的 Windows 计算机，请参阅[在断开连接的环境中使用 DSC](#use-dsc-in-disconnected-environments)。
<!-- Notice: Archor Should be #use-dsc-in-disconnected-environments-->

## <a name="extract-binaries"></a>提取二进制文件

移动服务可通过命令行安装，接受多个参数。 从安装中提取二进制文件后，需要获取这些文件，并将其存储在能够使用 DSC 配置检索它们的某个位置。

1. 若要提取进行此安装所需的文件，请在管理服务器上浏览到以下目录：**\Azure Site Recovery\home\svsystems\pushinstallsvc\repository**
2. 在此文件夹中，验证是否有一个具有以下名称的 MSI 文件：**Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**。
3. 使用以下命令提取安装程序：**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**
2. 选择所有文件，并将其发送到压缩的文件夹 (zip)。

现已获得所需的二进制文件，可以使用 Automation DSC 自动安装移动服务了。

## <a name="store-the-passphrase"></a>存储密码

需要确定在何处放置这个压缩的文件夹。 可以使用稍后所述的 Azure 存储帐户来存储设置所需的通行短语。 然后，代理将在此过程中注册到管理服务器。

1. 将部署配置服务器时获得的密码保存到文本文件 (passphrase.txt) 中。
2. 将压缩的文件夹和通行短语放置在 Azure 存储帐户的专用容器中。

也可以将这些文件保存在网络上的共享文件夹中。 只需确保以后要使用的 DSC 资源可以访问和获取该设置与通行短语即可。

## <a name="create-the-dsc-configuration"></a>创建 DSC 配置

设置依赖于 WMF 5.0。 为了使计算机能够成功通过 Automation DSC 应用配置，需要安装 WMF 5.0。

环境使用以下示例 DSC 配置：

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.chinacloudapi.cn/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.chinacloudapi.cn/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
配置会执行以下操作：

* 变量告诉配置要在何处获取移动服务和 Azure VM 代理的二进制文件、在何处获取通行短语，以及在何处存储输出。
* 配置将导入 xPSDesiredStateConfiguration DSC 资源，以便使用 `xRemoteFile` 下载存储库中的文件。
* 配置将创建需存储二进制文件的目录。
* 存档资源将从已压缩文件夹中提取文件。
* 包安装资源将使用特定参数安装 UNIFIEDAGENT.EXE 安装程序的移动服务。 （需要根据实际环境对构造参数的变量进行更改。）
* 包 AzureAgent 资源将安装 Azure VM 代理（推荐在 Azure 中运行的每个 VM 上安装此代理）。 Azure VM 代理还可以在故障转移后向 VM 添加扩展。
* 服务资源可确保相关的移动服务和 Azure 服务始终运行。

将配置保存为 **ASRMobilityService**。

> [!NOTE]
> 请记住，需根据实际的管理服务器替换配置中的 CSIP，这样代理才会正确进行连接，并使用正确的通行短语。
>
>

## <a name="upload-to-automation-dsc"></a>上传到 Automation DSC
由于所进行的 DSC 配置会导入所需的 DSC 资源模块 (xPSDesiredStateConfiguration)，因此，需要先将该模块导入自动化中，然后才能上传 DSC 配置。

1. 登录到自己的自动化帐户，浏览到“资产” > “模块”，然后单击“浏览库”。
2. 搜索模块，并将其导入到帐户中。
3. 完成此操作后，请转到已安装 Azure Resource Manager 模块的计算机，并继续导入新建的 DSC 配置。

## <a name="import-cmdlets"></a>导入 cmdlet

1. 在 PowerShell 中，登录到 Azure 订阅。
2. 修改 Cmdlet 以反映环境，并捕获变量中的 Automation 帐户信息：

    ```powershell
    $AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
    ```

3. 使用以下 cmdlet 将配置上传到 Automation DSC：

    ```powershell
    $ImportArgs = @{
        SourcePath = 'C:\ASR\ASRMobilityService.ps1'
        Published = $true
        Description = 'DSC Config for Mobility Service'
    }
    $AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
    ```

## <a name="compile-the-configuration-in-automation-dsc"></a>在 Automation DSC 中编译配置

1. 在 Automation DSC 中编译配置，以便可以开始向其注册节点。 为此，请运行以下 cmdlet：

    ```    powershell
    $AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
    ```

这可能需要数分钟的时间，因为基本上是将配置部署到托管的 DSC 拉取服务。

2. 编译配置完成后，即可使用 PowerShell (Get-AzureRmAutomationDscCompilationJob) 或 [Azure 门户](https://portal.azure.cn/)来检索作业信息。

    ![检索作业](./media/vmware-azure-mobility-deploy-automation-dsc/retrieve-job.png)

现已成功发布 DSC 配置并将其上传到 Automation DSC。

## <a name="onboard-machines-to-automation-dsc"></a>将计算机加入 Automation DSC

1. 确保 Windows 计算机已更新为最新版本的 WMF。 可以从 [下载中心](https://www.microsoft.com/download/details.aspx?id=50395)下载并安装适用于平台的正确版本。
2. 为 DSC 创建将应用于节点的元配置。 为成功操作，需要检索适用于 Azure 中所选自动化帐户的终结点 URL 和主密钥。 可以在自动化帐户的“所有设置”边栏选项卡上的“密钥”下找到这些值。

    ![密钥值](./media/vmware-azure-mobility-deploy-automation-dsc/key-values.png)

在本示例中，需要使用 Site Recovery 来保护一台 Windows Server 2012 R2 物理服务器。

## <a name="check-for-any-pending-file-rename-operations"></a>检查是否有任何未完成的文件重命名操作

在开始将服务器关联到 Automation DSC 终结点之前，建议查看注册表中是否有待完成的文件重命名操作。 这些操作可能导致设置无法完成，因为系统等待重新启动。

1. 运行以下 cmdlet，确保服务器上没有待完成的重新启动操作：

    ```powershell
    Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
    ```
2. 如果显示的结果为空，则可继续操作。 否则，应在维护时段内重新启动服务器以解决此问题。
3. 若要在服务器上应用配置，请启动 PowerShell 集成脚本环境 (ISE) 并运行以下脚本。 这实质上是一个 DSC 本地配置，将指示本地配置管理器引擎注册到 Automation DSC 服务并检索特定的配置 (ASRMobilityService.localhost)。

    ```powershell
    [DSCLocalConfigurationManager()]
    configuration metaconfig {
        param (
            $URL,
            $Key
        )
        node localhost {
            Settings {
                RefreshFrequencyMins = '30'
                RebootNodeIfNeeded = $true
                RefreshMode = 'PULL'
                ActionAfterReboot = 'ContinueConfiguration'
                ConfigurationMode = 'ApplyAndMonitor'
                AllowModuleOverwrite = $true
            }

            ResourceRepositoryWeb AzureAutomationDSC {
                ServerURL = $URL
                RegistrationKey = $Key
            }

            ConfigurationRepositoryWeb AzureAutomationDSC {
                ServerURL = $URL
                RegistrationKey = $Key
                ConfigurationNames = 'ASRMobilityService.localhost'
            }

            ReportServerWeb AzureAutomationDSC {
                ServerURL = $URL
                RegistrationKey = $Key
            }
        }
    }
    metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

    Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
    ```

此配置导致本地配置管理器引擎向 Automation DSC 注册自身。 它还将决定引擎的正确工作方式、存在配置偏差 (ApplyAndAutoCorrect) 时应采取的操作以及需要重启时应如何处理配置。

1. 运行此脚本后，节点就会启动注册到 Automation DSC 的过程。

    ![正在注册节点](./media/vmware-azure-mobility-deploy-automation-dsc/register-node.png)

2. 返回到 Azure 门户后，可以看到新注册的节点已出现在门户中。

    ![门户中已注册的节点](./media/vmware-azure-mobility-deploy-automation-dsc/registered-node.png)

3. 在服务器上，可以运行以下 PowerShell cmdlet 来验证节点是否已正确注册：

    ```powershell
    Get-DscLocalConfigurationManager
    ```

4. 拉取配置并将其应用到服务器以后，可运行以下 cmdlet 对其进行验证：

    ```powershell
    Get-DscConfigurationStatus
    ```

输出显示服务器已成功拉取其配置：

![输出](./media/vmware-azure-mobility-deploy-automation-dsc/successful-config.png)

此外，移动服务安装程序自带的日志位于 *SystemDrive*\ProgramData\ASRSetupLogs 中。

就这么简单。 现在，已在要使用 Site Recovery 保护的计算机上成功部署和注册移动服务。 DSC 将确保所需的服务始终运行。

![成功部署](./media/vmware-azure-mobility-deploy-automation-dsc/successful-install.png)

管理服务器检测到成功部署后，即可使用 Site Recovery 配置保护措施并允许在计算机上进行复制。

<a name="use-dsc-in-disconnected-environments"></a>
## <a name="use-dsc-in-disconnected-environments"></a>在断开连接的环境中使用 DSC

即使计算机未连接到 Internet，也可通过 DSC 在需要保护的工作负荷上部署和配置移动服务。

可以在环境中将自己的 DSC 实例化，这样，基本上就能获得 Automation DSC 所提供的相同功能。 即，客户端将请求到 DSC 终结点的配置（在其注册后）。 另一种做法是将 DSC 配置手动推送到计算机（无论是在本地还是远程）。

请注意，本示例为计算机名添加了一个参数。 远程文件现在位于想要保护的计算机应可访问的远程共享上。 脚本结束时，会启用配置，然后开始将 DSC 配置应用到目标计算机。

### <a name="prerequisites"></a>先决条件
确定已安装 xPSDesiredStateConfiguration PowerShell 模块。 对于安装了 WMF 5.0 的 Windows 计算机，可在目标计算机上运行以下 cmdlet 来安装 xPSDesiredStateConfiguration 模块：

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

如果需要将此模块分发到装有 WMF 4.0 的 Windows 计算机，则也可以下载并保存该模块。 在安装了 PowerShellGet (WMF 5.0) 的计算机上运行此 cmdlet：

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

此外，对于 WMF 4.0，请确保计算机上已安装 [Windows 8.1 更新 KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) 。

以下配置可以推送到装有 WMF 5.0 和 WMF 4.0 的 Windows 计算机：

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

若要在公司网络中实例化自己的 DSC 拉取服务器来模拟 Automation DSC 的功能，请参阅 [Setting up a DSC web pull server](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396)（设置 DSC Web 拉服务器）。

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>可选：使用 Azure Resource Manager 模板部署 DSC 配置
本文重点介绍如何通过创建自己的 DSC 配置来自动部署移动服务和 Azure VM 代理，以及如何确保它们在要保护的计算机上处于运行状态。 也可以使用 Azure Resource Manager 模板，将此 DSC 配置部署到新的或现有的 Azure 自动化帐户。 此模板使用输入参数来创建包含环境变量的自动化资产。

部署模板后，只需参阅本指南中的步骤 4 即可登记计算机。

该模板将执行以下操作：

1. 使用现有的自动化帐户或创建新的自动化帐户
2. 获取以下各项的输入参数：
   * ASRRemoteFile -- 存储移动服务设置的位置
   * ASRPassphrase -- 存储 passphrase.txt 文件的位置
   * ASRCSEndpoint -- 管理服务器的 IP 地址
3. 导入 xPSDesiredStateConfiguration PowerShell 模块
4. 创建和编译 DSC 配置

上述所有步骤按正确的顺序进行，以便能够开始登记所要保护的计算机。

[GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC)上提供了该模板及其部署说明。

使用 PowerShell 部署模板：

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.chinacloudapi.cn/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.chinacloudapi.cn/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>后续步骤
部署移动服务代理后，可为虚拟机[启用复制](vmware-azure-tutorial.md)。

<!--Update_Description: update meta properties, wording update， update link -->
