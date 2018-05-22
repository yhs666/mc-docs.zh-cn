---
title: 从 Web 应用程序使用 Azure 密钥保管库 | Microsoft Docs
description: 本教程介绍如何从 Web 应用程序使用 Azure 密钥保管库。
services: key-vault
author: adhurwit
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: article
origin.date: 09/15/2017
ms.date: 05/16/2018
ms.author: v-junlch
ms.openlocfilehash: 235f1b274cec2d90a5cc3f29c8b6224a569f2abb
ms.sourcegitcommit: 1804be2eacf76dd7993225f316cd3c65996e5fbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2018
---
# <a name="use-azure-key-vault-from-a-web-application"></a>从 Web 应用程序使用 Azure 密钥保管库

## <a name="introduction"></a>简介

本教程介绍如何从 Azure 中的 Web 应用程序使用 Azure 密钥保管库。 其中讲解了如何访问 Azure 密钥保管库中的机密，以便在 Web 应用程序中使用。

**估计完成时间：** 15 分钟

有关 Azure 密钥保管库的概述信息，请参阅 [什么是 Azure 密钥保管库？](key-vault-whatis.md)

## <a name="prerequisites"></a>先决条件

要完成本教程，必须具备以下先决条件：

- Azure 密钥保管库中机密的 URI
- 已在 Azure Active Directory 中注册且有权访问密钥保管库的 Web 应用程序的客户端 ID 和客户端机密
- Web 应用程序。 我们演示在 Azure 中作为 Web 应用程序部署的 ASP.NET MVC 应用程序的步骤。

> [!NOTE]
>* 必须已完成 [ 入门](key-vault-get-started.md)中列出的适用于本教程的步骤，以便获取 Web 应用程序的密钥、客户端 ID 和客户端密码的 URI。


要访问密钥保管库的 Web 应用程序已在 Azure Active Directory 中注册，有权访问密钥保管库。 否则，请返回入门教程中的“注册应用程序”部分，并重复列出的步骤。

本教程面向已了解在 Azure 上创建 Web 应用程序基本知识的 Web 开发人员。 有关 Azure Web 应用的详细信息，请参阅 [Web 应用概述](../app-service/app-service-web-overview.md)。

## <a id="packages"></a>添加 NuGet 包

需要在 Web 应用程序上安装两个包。

- Active Directory 身份验证库 - 包含与 Azure Active Directory 交互以及管理用户标识的方法
- Azure 密钥保管库库 - 包含与 Azure 密钥保管库交互的方法

可以在程序包管理器控制台中使用 Install-Package 命令安装这两个包。

```
// this is currently the latest stable version of ADAL
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202
Install-Package Microsoft.Azure.KeyVault
```

## <a id="webconfig"></a>修改 Web.Config

需要按如下所示将三个应用程序设置添加到 web.config 文件。

```
<!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
<add key="ClientId" value="clientid" />
<add key="ClientSecret" value="clientsecret" />

<!-- SecretUri is the URI for the secret in Azure Key Vault -->
<add key="SecretUri" value="secreturi" />
```

如果不打算将应用程序作为 Azure Web 应用程序托管，则应在 web.config 中添加实际客户端 ID、客户端机密和机密 URI 值。否则，请将这些虚构值，因为我们会在 Azure 门户中添加实际值以提高安全级别。

## <a id="gettoken"></a>添加获取访问令牌的方法

若要使用密钥保管库 API，则需要一个访问令牌。 密钥保管库客户端将处理对密钥保管库 API 的调用，但需要为该 API 提供一个用于获取访问令牌的函数。  

以下代码可从 Azure Active Directory 获取访问令牌。 可将此代码添加在应用程序中的任意位置。 我想要添加一个 Utils 或 EncryptionHelper 类。  

```cs
//add these using statements
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Threading.Tasks;
using System.Web.Configuration;

//this is an optional property to hold the secret after it is retrieved
public static string EncryptSecret { get; set; }

//the method that will be provided to the KeyVaultClient
public static async Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                WebConfigurationManager.AppSettings["ClientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

> [!NOTE]
>* 使用客户端 ID 和客户端机密是对 Azure AD 应用程序进行身份验证的一种方法。 在 Web 应用程序中使用该组合还可以实现职责分离，更好地控制密钥管理。 但它不依赖于将客户端机密放入配置设置中，对于某些人来说这可能就像将要保护的机密放入配置设置中一样具有风险。 有关如何使用客户端 ID 和证书（而非客户端 ID 和客户端机密）对 Azure AD 应用程序进行身份验证的讨论，请参阅下文。

## <a id="appstart"></a>在 Application Start 中检索机密

现在，我们需要添加代码来调用密钥保管库 API 并检索机密。 以下代码可添加到任何位置，只要在使用之前调用即可。 我已将此代码放在 Global.asax 中的 Application Start 事件内，这样，在启动应用程序时，该代码运行一次，从而让应用程序可使用该机密。

```cs
//add these using statements
using Microsoft.Azure.KeyVault;
using System.Web.Configuration;

// I put my GetToken method in a Utils class. Change for wherever you placed your method.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));
var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

//I put a variable in a Utils class to hold the secret for general  application use.
Utils.EncryptSecret = sec.Value;
```

## <a id="portalsettings"></a>在 Azure 门户中添加应用设置（可选）

如果有一个 Azure Web 应用程序，则现在可以在 Azure 门户中添加 AppSettings 的实际值。 如果这样做，实际值不会出现在 web.config 中，而是通过门户受到保护，在门户中拥有不同的访问控制功能。 这些值将取代在 web.config 中输入的值。请确保名称相同。

![Azure 门户中显示的应用程序设置][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>使用证书（而不是客户端密码）进行身份验证

另一种对 Azure AD 应用程序进行身份验证的方法是使用客户端 ID 和证书（而不是客户端 ID 和客户端机密）。 下面是在 Azure Web 应用中使用证书的步骤：

1. 获取或创建证书
2. 将证书与 Azure AD 应用程序相关联
3. 在 Web 应用中添加代码以使用证书
4. 将证书添加到 Web 应用

### <a name="get-or-create-a-certificate"></a>获取或创建证书

针对我们所需的用途，我们将生成一个测试证书。 下面是几个可在开发人员命令提示符下创建证书的命令。 将目录更改为要在其中创建证书文件的位置。  此外，对于证书的开始和结束日期，使用当前日期加上 1 年。

```
makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2017 -e 07/31/2018 -r
pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123
```

记下 .pfx 的结束日期和密码（在此示例中为：07/31/2018 和 test123）。 稍后需要使用这些信息。

有关创建测试证书的详细信息，请参阅 [如何：创建自己的测试证书](https://msdn.microsoft.com/library/ff699202.aspx)

### <a name="associate-the-certificate-with-an-azure-ad-application"></a>将证书与 Azure AD 应用程序相关联

现在你已有一个证书，需要将其与 Azure AD 应用程序相关联。 目前，Azure 门户不支持此工作流；可通过 PowerShell 完成此操作。 使用 Azure AD 应用程序运行以下命令，以便关联证书：

```ps
$x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$x509.Import("C:\data\KVWebApp.cer")
$credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())


$adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $x509.NotBefore -EndDate $x509.NotAfter


$sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId


Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName "http://kvwebapp" -PermissionsToSecrets all -ResourceGroupName 'contosorg'

# get the thumbprint to use in your app settings
$x509.Thumbprint
```

运行这些命令后，可以在 Azure AD 中看到该应用程序。 搜索时，请确保在搜索对话框中选择“我的公司拥有的应用程序”，而不是“我的公司使用的应用程序”。

若要了解有关 Azure AD 应用程序对象和 ServicePrincipal 对象的详细信息，请参阅[应用程序对象和服务主体对象](../active-directory/develop/active-directory-application-objects.md)

### <a name="add-code-to-your-web-app-to-use-the-certificate"></a>在 Web 应用中添加代码以使用证书

现在，我们将在 Web 应用中添加代码以访问证书并使用它进行身份验证。

首先是用于访问证书的代码。

```cs
public static class CertificateHelper
{
    public static X509Certificate2 FindCertificateByThumbprint(string findValue)
    {
        X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
        try
        {
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                findValue, false); // Don't validate certs, since the test root isn't installed.
            if (col == null || col.Count == 0)
                return null;
            return col[0];
        }
        finally
        {
            store.Close();
        }
    }
}
```

请注意，StoreLocation 是 CurrentUser，而不是 LocalMachine。 并且，我们为 Find 方法提供“false”，因为我们使用的是测试证书。

其次是使用 CertificateHelper 并创建 ClientAssertionCertificate 的代码，这是身份验证所需要的。

```cs
public static ClientAssertionCertificate AssertionCert { get; set; }

public static void GetCert()
{
    var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
    AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
}
```

以下是新代码，用于获取访问令牌。 这将替换以上示例中的 GetToken 方法。 为方便起见，我为它起了不同的名称。

```cs
public static async Task<string> GetAccessToken(string authority, string resource, string scope)
{
    var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
    var result = await context.AcquireTokenAsync(resource, AssertionCert);
    return result.AccessToken;
}
```

为了便于使用，我已将所有这些代码放到我的 Web 应用项目的 Utils 类中。

最后的代码更改是在 Application_Start 方法中。 首先，我们需要调用 GetCert() 方法以加载 ClientAssertionCertificate。 然后，我们更改在创建新 KeyVaultClient 时提供的回调方法。 注意，这将替换以上示例中的代码。

```cs
Utils.GetCert();
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));
```

### <a name="add-a-certificate-to-your-web-app-through-the-azure-portal"></a>通过 Azure 门户向 Web 应用中添加证书

将证书添加到 Web 应用是一个简单的两步过程。 首先，转到 Azure 门户并导航到 Web 应用。 在 Web 应用的“设置”边栏选项卡中，单击“自定义域和 SSL”对应的条目。 在打开的边栏选项卡中，你将能够上传在上面的示例中创建的证书 KVWebApp.pfx，请确保记住 pfx 的密码。

![将证书添加到 Azure 门户中的 Web 应用][2]

你需要执行的最后一项操作是将应用程序设置添加到 Web 应用中，该设置名为 WEBSITE\_LOAD\_CERTIFICATES，值为 *。 这会确保加载所有证书。 如果只想加载已上传的证书，则可以输入这些证书的指纹的逗号分隔列表。

要了解有关将证书添加到 Web 应用的详细信息，请参阅[在 Azure 网站应用程序中使用证书](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)

### <a name="add-a-certificate-to-key-vault-as-a-secret"></a>将证书作为机密添加到 Key Vault

如果不直接将证书上传到 Web 应用服务，可以在 Key Vault 中将它存储为机密，然后从那里部署该证书。 此过程包括两个步骤，以下博客文章对此做了概述： [Deploying Azure Web App Certificate through Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png

<!--Update_Description: wording update -->