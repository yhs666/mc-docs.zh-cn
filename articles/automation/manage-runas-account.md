---
title: 管理 Azure 自动化运行方式帐户
description: 本文介绍如何使用 PowerShell 或门户管理运行方式帐户。
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: WenJason
ms.author: v-jay
origin.date: 05/24/2019
ms.date: 12/02/2019
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: 6e5e0b98abf83855f272e24f952e05d78cce7823
ms.sourcegitcommit: 9597d4da8af58009f9cef148a027ccb7b32ed8cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2019
ms.locfileid: "74655378"
---
# <a name="manage-azure-automation-run-as-accounts"></a>管理 Azure 自动化运行方式帐户

Azure 自动化中的运行方式帐户用于提供身份验证，以使用 Azure cmdlet 管理 Azure 中的资源。

创建运行方式帐户时，将在 Azure Active Directory 中创建新的服务主体用户，并在订阅级别向此用户分配“参与者”角色。 对于在 Azure 虚拟机上使用混合 Runbook 辅助角色的 Runbook，可以使用 [Azure 资源托管标识](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources)而不是运行方式帐户来对 Azure 资源进行身份验证。

有两种类型的运行方式帐户：

* **Azure 运行方式帐户** - 此帐户用于管理[资源管理器部署模型](../azure-resource-manager/resource-manager-deployment-model.md)资源。
  * 将创建使用自签名证书的 Azure AD 应用程序，在 Azure AD 中为此应用程序创建服务主体帐户，并在当前订阅中为此帐户分配“参与者”角色。 可将此项设置更改为“所有者”或其他任何角色。 有关详细信息，请参阅 [Azure 自动化中基于角色的访问控制](automation-role-based-access-control.md)。
  * 在指定的自动化帐户中创建名为 *AzureRunAsCertificate* 的自动化证书资产。 该证书资产保存 Azure AD 应用程序使用的证书私钥。
  * 在指定的自动化帐户中创建名为 *AzureRunAsConnection* 的自动化连接资产。 该连接资产保存 applicationId、tenantId、subscriptionId 和证书指纹。

* **Azure 经典运行方式帐户** - 此帐户用于管理[经典部署模型](../azure-resource-manager/resource-manager-deployment-model.md)资源。
  * 在订阅中创建管理证书
  * 在指定的自动化帐户中创建名为 *AzureClassicRunAsCertificate* 的自动化证书资产。 该证书资产保存管理证书使用的证书私钥。
  * 在指定的自动化帐户中创建名为 *AzureClassicRunAsConnection* 的自动化连接资产。 该连接资产保存订阅名称、subscriptionId 和证书资产名称。
  * 必须是订阅的共同管理员才能进行创建或续订

  > [!NOTE]
  > 默认情况下，运行方式帐户的服务主体没有读取 Azure Active Directory 的权限。 如果希望添加读取或管理 Azure Active directory 的权限，需要在“API 权限”  下对服务主体授予该权限。 若要了解详细信息，请参阅[添加用于访问 Web API 的权限](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)。

## <a name="permissions"></a>配置运行方式帐户时所需的权限

若要创建或更新运行方式帐户，必须拥有特定的特权和权限。 Azure Active Directory 中的应用程序管理员和订阅中的所有者可以完成所有任务。 下表显示了在实施职责分离的情况下，所需的任务、等效 cmdlet 和权限的列表：

|任务|Cmdlet  |最低权限  |设置权限的位置|
|---|---------|---------|---|
|创建 Azure AD 应用程序|[New-AzureRmADApplication](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermadapplication)     | 应用程序开发人员角色<sup>1</sup>        |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>“主页”>“Azure Active Directory”>“应用注册” |
|将凭据添加到应用程序。|[New-AzureRmADAppCredential](https://docs.microsoft.com/powershell/module/AzureRM.Resources/New-AzureRmADAppCredential)     | 应用程序管理员或全局管理员<sup>1</sup>         |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>“主页”>“Azure Active Directory”>“应用注册”|
|创建和获取 Azure AD 服务主体|[New-AzureRMADServicePrincipal](https://docs.microsoft.com/powershell/module/AzureRM.Resources/New-AzureRmADServicePrincipal)</br>[Get-AzureRmADServicePrincipal](https://docs.microsoft.com/powershell/module/AzureRM.Resources/Get-AzureRmADServicePrincipal)     | 应用程序管理员或全局管理员<sup>1</sup>        |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>“主页”>“Azure Active Directory”>“应用注册”|
|分配或获取指定主体的 RBAC 角色|[New-AzureRMRoleAssignment](https://docs.microsoft.com/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment)</br>[Get-AzureRMRoleAssignment](https://docs.microsoft.com/powershell/module/AzureRM.Resources/Get-AzureRmRoleAssignment)      | 你必须具有以下权限：</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br>或者是：</br></br>用户访问管理员或所有者        | [订阅](../role-based-access-control/role-assignments-portal.md)</br>“主页”>“订阅”> \<订阅名称\> -“访问控制(IAM)”|
|创建或删除自动化证书|[New-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/AzureRM.Automation/New-AzureRmAutomationCertificate)</br>[Remove-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationCertificate)     | 资源组中的参与者         |自动化帐户资源组|
|创建或删除自动化连接|[New-AzureRmAutomationConnection](https://docs.microsoft.com/powershell/module/AzureRM.Automation/New-AzureRmAutomationConnection)</br>[Remove-AzureRmAutomationConnection](https://docs.microsoft.com/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationConnection)|资源组中的参与者 |自动化帐户资源组|

<sup>1</sup> Azure AD 租户中的非管理员用户可以[注册 AD 应用程序](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)，前提是 Azure AD 租户的“用户设置”页中的“用户可以注册应用程序”选项已设置为“是”。    如果“应用注册设置”设置为“否”，则执行此操作的用户必须是上表中定义的用户  。

如果你在被添加到订阅的“全局管理员”  角色之前不是订阅的 Active Directory 实例的成员，则会将你添加为来宾。 在这种情况下，“添加自动化帐户”页上会显示 `You do not have permissions to create…` 警告。  可以先从订阅的 Active Directory 实例中删除已添加到“全局管理员”  角色的用户，然后重新添加，使其成为 Active Directory 中的完整用户。 若要验证这种情况，可在 Azure 门户的“Azure Active Directory”窗格中依次选择“用户”、“所有用户”，并选择特定的用户。    用户配置文件下的“用户类型”属性值不应等于“来宾”。  

## <a name="permissions-classic"></a>配置经典运行方式帐户时所需的权限

若要配置或续订经典运行方式帐户，必须在订阅级别具有**共同管理员**角色。 若要了解有关经典权限的详细信息，请参阅 [Azure 经典订阅管理员](../role-based-access-control/classic-administrators.md#add-a-co-administrator)。

## <a name="create-a-run-as-account-in-the-portal"></a>在门户中创建运行方式帐户

在本部分，请执行以下步骤，在 Azure 门户中更新 Azure 自动化帐户。 可以单独创建运行方式帐户和经典运行方式帐户。 如果不需管理经典资源，可以只创建 Azure 运行方式帐户。

1. 以订阅管理员角色成员和订阅共同管理员的帐户登录 Azure 门户。
2. 在 Azure 门户中，单击“所有服务”  。 在资源列表中，键入“自动化”  。 开始键入时，会根据输入筛选该列表。 选择“自动化帐户”  。
3. 在“自动化帐户”页的自动化帐户列表中选择自动化帐户。 
4. 在左侧窗格的“帐户设置”部分下，选择“运行方式帐户”   。
5. 根据所需帐户，选择“Azure 运行方式帐户”或“Azure 经典运行方式帐户”   。 选择后，便会出现“添加 Azure 运行方式帐户”或“添加 Azure 经典运行方式帐户”页。查看概述信息后，单击“创建”，继续创建运行方式帐户    。
6. 在 Azure 创建运行方式帐户时，可以在菜单的“通知”下面跟踪进度  。 此外还显示一个横幅，指出正在创建帐户。 此过程可能需要几分钟才能完成。

## <a name="create-run-as-account-using-powershell"></a>使用 PowerShell 创建运行方式帐户

## <a name="prerequisites"></a>先决条件

以下列表提供了在 PowerShell 中创建运行方式帐户所要满足的要求：

* 装有 Azure 资源管理器模块 3.4.1 和更高版本的 Windows 10 或 Windows Server 2016。 PowerShell 脚本不支持早期版本的 Windows。
* Azure PowerShell 1.0 和更高版本。 有关 PowerShell 1.0 版本的信息，请参阅[如何安装和配置 Azure PowerShell](https://azure.microsoft.com/powershell/azureps-cmdlets-docs)。
* 作为  –AutomationAccountName 和  -ApplicationDisplayName 参数的值引用的自动化帐户。
* 与[配置运行方式帐户时所需的权限](#permissions)中所列权限相当的权限

若要获取脚本的必需参数 SubscriptionID、ResourceGroup 和 AutomationAccountName 的值，请完成以下步骤    ：

1. 在 Azure 门户中，单击“所有服务”  。 在资源列表中，键入“自动化”  。 开始键入时，会根据输入筛选该列表。 选择“自动化帐户”  。
1. 在“自动化帐户”页中选择自动化帐户，然后在“帐户设置”下  选择“属性”  。
1. 记下“属性”页上的“订阅 ID”、“名称”和“资源组”值。    

   ![自动化帐户的“属性”页](media/manage-runas-account/automation-account-properties.png)

此 PowerShell 脚本包括对以下配置的支持：

* 使用自签名证书创建运行方式帐户。
* 使用自签名证书创建运行方式帐户和经典运行方式帐户。
* 使用企业证书颁发机构 (CA) 颁发的证书创建运行方式帐户和经典运行方式帐户。
* 在 Azure 中国云中使用自签名证书创建运行方式帐户和经典运行方式帐户。

>[!NOTE]
> 如果选择任一选项来创建经典运行方式帐户，则在执行脚本后，请将公共证书（.cer 文件扩展名）上传到创建自动化帐户的订阅的管理存储中。

1. 将以下脚本保存到计算机。 在本示例中，请使用文件名 *New-RunAsAccount.ps1*保存。

   该脚本使用多个 Azure 资源管理器 cmdlet 来创建资源。 以上[权限](#permissions)表显示了 cmdlet 及其所需的权限。

    ```powershell
    #Requires -RunAsAdministrator
    Param (
        [Parameter(Mandatory = $true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory = $true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory = $true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory = $true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory = $true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory = $true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory = $false)]
        [string] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [ValidateSet("AzureChinaCloud")]
        [string]$EnvironmentName = "AzureChinaCloud",

        [Parameter(Mandatory = $false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
    )

    function CreateSelfSignedCertificate([string] $certificateName, [string] $selfSignedCertPlainPassword,
        [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired) -HashAlgorithm SHA256

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
    }

    function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $keyId = (New-Guid).Guid

        # Create an Azure AD application, AD App Credential, AD ServicePrincipal

        # Requires Application Developer Role, but works with Application administrator or GLOBAL ADMIN
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId)
        # Requires Application administrator or GLOBAL ADMIN
        $ApplicationCredential = New-AzureRmADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $PfxCert.NotBefore -EndDate $PfxCert.NotAfter
        # Requires Application administrator or GLOBAL ADMIN
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        # Requires User Access Administrator or Owner.
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6) {
            Sleep -s 10
            New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
        return $Application.ApplicationId.ToString();
    }

    function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName, [string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
    }

    function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
    }

    Import-Module AzureRM.Profile
    Import-Module AzureRM.Resources

    $AzureRMProfileVersion = (Get-Module AzureRM.Profile).Version
    if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 4) -or ($AzureRMProfileVersion.Major -gt 3))) {
        Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
        return
    }


    Connect-AzureRmAccount -Environment $EnvironmentName
    $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

    # Create a Run As account by using a service principal
    $CertifcateAssetName = "AzureRunAsCertificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionTypeName = "AzureServicePrincipal"

    if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
    }
    else {
        $CertificateName = $AutomationAccountName + $CertifcateAssetName
        $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
        $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
        $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
        CreateSelfSignedCertificate $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
    }

    # Create a service principal
    $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
    $ApplicationId = CreateServicePrincipal $PfxCert $ApplicationDisplayName

    # Create the Automation certificate asset
    CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

    # Populate the ConnectionFieldValues
    $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1
    $Thumbprint = $PfxCert.Thumbprint
    $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

    # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

    if ($CreateClassicRunAsAccount) {
        # Create a Run As account by using a service principal
        $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
        $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
        $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
        $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
        "Log in to the Azure portal (https://portal.azure.cn) and select Subscriptions -> Management Certificates." + [Environment]::NewLine +
        "Then click Upload and upload the .cer format of #CERT#"

        if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        }
        else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName + $ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName   $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red       $UploadMessage
    }
    ```

    > [!IMPORTANT]
    > Add-AzureRmAccount 现在是 Connect-AzureRMAccount 的别名   。 搜索库项时，如果未看到 **Connect-AzureRMAccount**，可以使用 **Add-AzureRmAccount**。

1. 在计算机上，从“开始”屏幕以提升的用户权限启动 **Windows PowerShell**  。
1. 在提升权限的命令行外壳中，转到包含步骤 1 所创建脚本的文件夹。
1. 使用所需配置的参数值执行该脚本。

    **使用自签名证书创建运行方式帐户**

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
    ```

    **使用自签名证书创建运行方式帐户和经典运行方式帐户**

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
    ```

    **使用企业证书创建运行方式帐户和经典运行方式帐户**

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
    ```

    **在 Azure 中国云中使用自签名证书创建运行方式帐户和经典运行方式帐户**

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureChinaCloud
    ```

    > [!NOTE]
    > 执行脚本后，系统会提示在 Azure 上进行身份验证。 请以订阅管理员角色成员和订阅共同管理员的帐户登录。

成功执行脚本后，请注意以下事项：

* 如果使用自签名公共证书（.cer 文件）创建了经典运行方式帐户，该脚本将创建该帐户，并将其保存到计算机上用于执行 PowerShell 会话的用户配置文件下方的临时文件夹（ *%USERPROFILE%\AppData\Local\Temp*）。

* 如果使用企业公共证书（.cer 文件）创建了经典运行方式帐户，则使用此证书。 按照[将管理 API 证书上传到 Azure 门户](../azure-api-management-certs.md)的说明进行操作。

## <a name="delete-a-run-as-or-classic-run-as-account"></a>删除运行方式帐户或经典运行方式帐户

本部分介绍如何删除并重新创建运行方式帐户或经典运行方式帐户。 执行此操作时，将保留自动化帐户。 删除运行方式帐户或经典运行方式帐户后，可在 Azure 门户中重新创建它。

1. 在 Azure 门户中，打开自动化帐户。

2. 在“自动化帐户”  页上，选择“运行方式帐户”  。

3. 在“运行方式帐户”属性页上，选择要删除的运行方式帐户或经典运行方式帐户。  然后，在所选帐户的“属性”窗格中单击“删除”。  

   ![删除运行方式帐户](media/manage-runas-account/automation-account-delete-runas.png)

4. 帐户删除过程中，可以在菜单的“通知”  下面跟踪进度。

5. 删除该帐户后，可以通过在“运行方式帐户”属性页中选择创建选项“Azure 运行方式帐户”来重新创建该帐户。  

   ![重新创建自动化运行方式帐户](media/manage-runas-account/automation-account-create-runas.png)

## <a name="cert-renewal"></a>自签名证书续订

在运行方式帐户过期之前的某个时间点，需要续订证书。 如果认为运行方式帐户已遭到入侵，可以删除然后重新创建它。 本部分介绍如何执行这些操作。

为运行方式帐户创建的自签名证书自创建日期算起的一年后过期。 可以在该证书过期之前的任何时间续订。 续订时，将保留当前的有效证书，以确保已排队等候或正在主动运行且使用运行方式帐户进行身份验证的任何 Runbook 不会受到负面影响。 该证书在过期之前将保持有效。

> [!NOTE]
> 如果已将自动化运行方式帐户配置为使用企业证书颁发机构颁发的证书并使用此选项，该企业证书会被自签名证书替换。

若要续订证书，请执行以下操作：

1. 在 Azure 门户中，打开自动化帐户。

1. 在“帐户设置”下选择“运行方式帐户”。  

    ![自动化帐户属性窗格](media/manage-runas-account/automation-account-properties-pane.png)

1. 在“运行方式帐户”属性页上，选择要为其续订证书的运行方式帐户或经典运行方式帐户。 

1. 在所选帐户的“属性”窗格中，单击“续订证书”。  

    ![续订运行方式帐户的证书](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. 证书续订过程中，可以在菜单的“通知”  下面跟踪进度。

## <a name="auto-cert-renewal"></a>使用自动化 Runbook 设置自动证书续订

若要自动续订证书，可以使用自动化 Runbook。 [GitHub](https://github.com/ikanni/PowerShellScripts/blob/master/AzureAutomation/RunAsAccount/GrantPermissionToRunAsAccountAADApplication-ToRenewCertificateItself-CreateSchedule.ps1) 上的以下脚本可在自动化帐户中启用此功能。

- `GrantPermissionToRunAsAccountAADApplication-ToRenewCertificateItself-CreateSchedule.ps1` 脚本创建每周续订运行方式帐户证书的计划。
- 该脚本将 **Update-AutomationRunAsCredential** Runbook 添加到自动化帐户。
  - 你还可以在该脚本中查看 GitHub 上的 Runbook 代码：[Update-AutomationRunAsCredential.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AutomationRunAsCredential.ps1)。
  - 还可以根据需要，使用该文件中的 PowerShell 代码手动续订证书。

若要立即测试续订过程，请使用以下步骤：

1. 编辑 **Update-AutomationRunAsCredential** Runbook，并在第 122 行的 `Exit(1)` 命令前面添加一个注释字符 (`#`)，如下所示。

   ```powershell
   #Exit(1)
   ```

2. 发布 Runbook。
3. 启动 Runbook。
4. 使用以下代码验证续订是否成功：

   ```powershell
   (Get-AzAutomationCertificate -AutomationAccountName TestAA
                                -Name AzureRunAsCertificate
                                -ResourceGroupName TestAutomation).ExpiryTime.DateTime
   ```

   ```Output
   Thursday, November 7, 2019 7:00:00 PM
   ```

5. 测试后编辑 Runbook，删除在“步骤 1”中添加的注释字符。 
6. **发布** Runbook。

> [!NOTE]
> 只有 Azure Active Directory 中的“全局管理员”或“公司管理员”才能执行该脚本。  

## <a name="limiting-run-as-account-permissions"></a>限制运行方式帐户权限

若要针对 Azure 中的资源控制自动化目标，可以运行 PowerShell 库中的 [Update-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug8) 脚本，以将现有的运行方式帐户服务主体更改为创建并使用自定义角色定义。 此角色对除 [Key Vault](/key-vault/) 以外的所有资源拥有权限。

> [!IMPORTANT]
> 运行 `Update-AutomationRunAsAccountRoleAssignments.ps1` 脚本后，使用运行方式帐户访问 KeyVault 的 Runbook 将不再可以正常运行。 应在帐户中查看调用 Azure KeyVault 的 Runbook。
>
> 若要从 Azure 自动化 Runbook 访问 KeyVault，需要[添加运行方式帐户对 KeyVault 的权限](#add-permissions-to-key-vault)。

如果需要限制运行方式服务主体可以进一步执行的操作，可将其他资源类型添加到自定义角色定义的 `NotActions`。 以下示例限制对 `Microsoft.Compute` 的访问。 如果将此资源添加到角色定义的 **NotActions**，此角色将无法访问任何计算资源。 若要详细了解角色定义，请参阅[了解 Azure 资源的角色定义](../role-based-access-control/role-definitions.md)。

```powershell
$roleDefinition = Get-AzureRmRoleDefinition -Name 'Automation RunAs Contributor'
$roleDefinition.NotActions.Add("Microsoft.Compute/*")
$roleDefinition | Set-AzureRMRoleDefinition
```

若要确定运行方式帐户使用的服务主体是否在“参与者”或自定义角色定义中，请转到你的自动化帐户，然后在“帐户设置”下选择“运行方式帐户” > “Azure 运行方式帐户”。     在“角色”下，可以看到正在使用的角色定义。 

[![](media/manage-runas-account/verify-role.png "Verify the Run As Account role")](media/manage-runas-account/verify-role-expanded.png#lightbox)

若要确定自动化运行方式帐户对多个订阅或自动化帐户使用的角色定义，可以使用 PowerShell 库中的 [Check-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug5) 脚本。

### <a name="add-permissions-to-key-vault"></a>将权限添加到 Key Vault

若要允许 Azure 自动化管理 Key Vault，而你的运行方式帐户服务主体正在使用自定义角色定义，则你需要执行额外的步骤才能实现此行为：

* 授予对 Key Vault 的权限
* 设置访问策略

可以使用 PowerShell 库中的 [Extend-AutomationRunAsAccountRoleAssignmentToKeyVault.ps1](https://aka.ms/AA5hugb) 脚本向运行方式帐户授予对 KeyVault 的权限，或者访问[授予应用程序对 Key Vault 的访问权限](../key-vault/key-vault-group-permissions-for-apps.md)来详细了解如何设置 KeyVault 权限。

## <a name="misconfiguration"></a>配置错误

在初始设置期间，运行方式帐户或经典运行方式帐户所需的某些配置项可能已被删除或未正确创建。 这些项包括：

* 证书资产
* 连接资产
* 运行方式帐户已从参与者角色中删除
* Azure AD 中的服务主体或应用程序

在上述和其他错误配置的实例中，自动化将检测更改，并在该帐户的“运行方式帐户”属性窗格中显示“不完整”状态。  

![不完整的运行方式帐户配置状态](media/manage-runas-account/automation-account-runas-incomplete-config.png)

选择该运行方式帐户时，该帐户的“属性”窗格中会显示以下错误消息： 

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

可通过删除并重新创建运行方式帐户来快速解决该帐户的问题。

## <a name="next-steps"></a>后续步骤

* 有关服务主体的详细信息，请参阅 [Application Objects and Service Principal Objects](../active-directory/develop/app-objects-and-service-principals.md)（应用程序对象和服务主体对象）。
* 有关证书和 Azure 服务的详细信息，请参阅 [Azure 云服务证书概述](../cloud-services/cloud-services-certs-create.md)。
