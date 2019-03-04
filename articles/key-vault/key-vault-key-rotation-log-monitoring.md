---
title: 使用端到端密钥轮换和审核设置 Azure Key Vault | Azure Docs
description: 借助本操作指南设置密钥轮换和监视 Key Vault 日志。
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: ''
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 06/12/2018
ms.date: 03/11/2019
ms.author: v-biyu
ms.openlocfilehash: c5b365f64c7b1ec21110827e6aecb902951b4fa2
ms.sourcegitcommit: 1e5ca29cde225ce7bc8ff55275d82382bf957413
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "56903151"
---
# <a name="set-up-azure-key-vault-with-key-rotation-and-auditing"></a>使用密钥轮换和审核设置 Azure Key Vault

## <a name="introduction"></a>简介

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

有了密钥保管库以后，即可用它来存储密钥和机密。 应用程序不再需要保存密钥或机密，但可以根据需要从保管库请求它们。 使用 Key Vault 可以更新密钥和机密，而不会影响应用程序，同时可以各种可能的方法管理密钥和机密。

>[!IMPORTANT]
> 本文中的示例仅用于说明目的， 不应在生产环境中使用。 

本文逐步讲解：

- 使用 Azure Key Vault 存储机密的示例。 在本文中，存储的机密是应用程序访问的 Azure 存储帐户密钥。 
- 如何实现该存储帐户机密的计划轮换。
- 如何监视 Key Vault 审核日志，并在收到意外的请求时发出警报。

> [!NOTE]
> 本文不详细说明 Key Vault 的初始设置。 有关信息，请参阅[什么是 Azure 密钥保管库？](key-vault-overview.md)。 有关跨平台命令行接口的说明，请参阅[使用 Azure CLI 管理 Key Vault](key-vault-manage-with-cli2.md)。

## <a name="set-up-key-vault"></a>设置密钥保管库

要使应用程序能够从 Key Vault 检索机密，必须先创建机密并将其上传到保管库。

启动 Azure PowerShell 会话，并使用以下命令登录用户的 Azure 帐户：

```powershell
Connect-AzAccount -Environment AzureChinaCloud
```

在弹出的浏览器窗口中，输入 Azure 帐户的用户名和密码。 PowerShell 会获取与此帐户关联的所有订阅。 PowerShell 默认使用第一个订阅。

如果有多个订阅，可能需要指定创建 Key Vault 时所用的订阅。 输入以下命令查看帐户的订阅：

```powershell
Get-AzSubscription
```

若要指定与要记录的 Key Vault 关联的订阅，请输入：

```powershell
Set-AzContext -SubscriptionId <subscriptionID>
```

因为本文介绍了如何将存储帐户密钥存储为机密，因此，必须获取该存储帐户密钥。

```powershell
Get-AzStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

检索用户的机密（在本例中，为存储帐户密钥）后，必须将该密钥转换为安全字符串，并在 Key Vault 中使用该值创建机密。

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```

接下来，获取你创建的机密的 URI。 在稍后的步骤中调用 Key Vault 和检索机密时，需要用到此 URI。 运行以下 PowerShell 命令，并记下 ID 值（即机密 URI）：

```powershell
Get-AzKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>设置应用程序

存储机密后，可以在执行几个附加步骤后，使用代码检索并使用该机密。

首先必须将应用程序注册到 Azure Active Directory。 然后向 Key Vault 告知应用程序信息，使其允许来自应用程序的请求。

> [!NOTE]
> 必须在与 Key Vault 相同的 Azure Active Directory 租户上创建应用程序。

1. 打开“Azure Active Directory”。
2. 选择“应用注册” 。 
3. 选择“新建应用程序注册”，以将一个应用程序添加到 Azure Active Directory。

    ![在 Azure Active Directory 中打开应用程序](./media/keyvault-keyrotation/azure-ad-application.png)

4. 在“创建”下，将应用程序类型保留为“Web 应用/API”，并为应用程序命名。 为应用程序指定“登录 URL”。 此 URL 可以是任意 URL，适合本演示即可。

    ![创建应用程序注册](./media/keyvault-keyrotation/create-app.png)

5. 将应用程序添加到 Azure Active Directory 后，应用程序页将会打开。 依次选择“设置”、“属性”。 复制“应用程序 ID”值。 后面的步骤需要用到。

接下来，为应用程序生成密钥，使其可与 Azure Active Directory 交互。 若要创建密钥，请在“设置”下选择“密钥”。 记下为 Azure Active Directory 应用程序生成的新密钥。 后面的步骤需要用到。 从此部分导航出来以后，该密钥将不可用。 

![Azure Active Directory 应用密钥](./media/keyvault-keyrotation/create-key.png)

在建立从应用程序到 Key Vault 的任何调用之前，必须让 Key Vault 知道应用程序及其权限。 以下命令使用 Azure Active Directory 应用中的保管库名称和应用程序 ID 为应用程序授予对 Key Vault 的 **Get** 访问权限。

```powershell
Set-AzKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

现在可以开始生成应用程序调用。 在应用程序中，必须安装所需的 NuGet 包，以便与 Azure Key Vault 和 Azure Active Directory 交互。 从 Visual Studio 包管理器控制台输入以下命令。 在编写本文时，Azure Active Directory 包的最新版本为 3.10.305231913，请确认最新版本并视需要进行更新。

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

在应用程序代码中，创建一个类来保存 Azure Active Directory 身份验证的方法。 在本示例中，该类名为 **Utils**。 添加以下 `using` 语句：

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

接下来，添加以下方法，从 Azure Active Directory 检索 JWT 令牌。 为了方便维护，请将硬编码的字符串值移到 Web 或应用程序配置。

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

添加所需的代码，调用密钥保管库并检索机密值。 首先，必须添加以下 `using` 语句：

```csharp
using Microsoft.Azure.KeyVault;
```

添加方法调用，调用密钥保管库并检索机密。 在此方法中，提供在前面步骤中保存的机密 URI。 请注意如何使用前面创建的 **Utils** 类中的 **GetToken** 方法。

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

现在，运行应用程序时，用户应该会向 Azure Active Directory 进行身份验证，并从 Azure Key Vault 中检索机密值。

## <a name="key-rotation-using-azure-automation"></a>使用 Azure 自动化进行密钥轮替

现在，对于存储为 Key Vault 机密的值，可以设置轮换策略。 可通过多种方式轮换机密：

- 手动轮换
- 使用 API 调用以编程方式轮换
- 通过 Azure 自动化脚本轮换

本文结合使用 Azure PowerShell 和 Azure 自动化来更改 Azure 存储帐户的访问密钥。 然后使用新密钥更新 Key Vault 机密。

若要允许 Azure 自动化在 Key Vault 中设置机密值，必须获取名为 **AzureRunAsConnection** 的连接的客户端 ID。 此连接是建立 Azure 自动化实例时创建的。 若要查找此 ID，请在 Azure 自动化实例中选择“资产”。 在此处选择“连接”，然后选择“AzureRunAsConnection”服务主体。 记下“ApplicationId”值。

![Azure 自动化客户端 ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

在“资产”中选择“模块”。 选择“库”，然后搜索并导入以下每个模块的更新版本：

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage

> [!NOTE]
> 在撰写本文时，只需要针对以下脚本更新上面记下的模块。 如果自动化作业失败，请确认已导入所有必要的模块及其依赖项。

检索 Azure 自动化连接的应用程序 ID 之后，必须让 Key Vault 知道此应用程序有权更新保管库中的机密。 使用以下 PowerShell 命令：

```powershell
Set-AzKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

接下来，选择 Azure 自动化实例下的“Runbook”，然后选择“添加 Runbook”。 选择“快速创建”。 为 Runbook 命名，然后选择“PowerShell”作为 Runbook 类型。 可以添加说明。 最后，选择“创建”。

![创建 Runbook](./media/keyvault-keyrotation/Create_Runbook.png)

将以下 PowerShell 脚本粘贴在新 Runbook 的编辑器窗格中：

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection"
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Connect-AzAccount -Environment AzureChinaCloud`
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

# Optionally you can set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

在编辑器窗格中，选择“测试”窗格以测试脚本。 正常运行脚本后，可以选择“发布”，并在 Runbook 配置窗格中应用 Runbook 的计划。

<!-- Update_Description: wording update -->