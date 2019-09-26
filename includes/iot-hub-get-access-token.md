---
ms.openlocfilehash: e51148124eaae9cfd1bc815fc2c3fb854fc7b643
ms.sourcegitcommit: 6a62dd239c60596006a74ab2333c50c4db5b62be
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2019
ms.locfileid: "71156560"
---
## <a name="obtain-an-azure-resource-manager-token"></a>获取 Azure 资源管理器令牌
Azure Active Directory 必须使用 Azure Resource Manager 来验证所有针对资源执行的任务。 此处显示的示例使用密码身份验证。有关其他方法，请参阅[对 Azure 资源管理器请求进行身份验证][lnk-authenticate-arm]。

1. 将以下代码添加到 Program.cs 中的 **Main** 方法，以使用应用程序 ID 和密码从 Azure AD 中检索令牌。
   
    ```csharp
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;

    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. 创建一个 **ResourceManagementClient** 对象，该对象通过在 **Main** 方法的末尾添加以下代码来使用令牌：
   
    ```csharp
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. 创建或获取对你使用的资源组的引用：
   
    ```csharp
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/zh-cn/library/azure/dn790557.aspx