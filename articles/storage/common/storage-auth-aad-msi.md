---
title: 使用 Azure Active Directory 和 Azure 资源的托管标识授权对 Blob 和队列的访问权限 - Azure 存储
description: Azure Blob 和队列存储支持使用 Azure Active Directory 和 Azure 资源的托管标识授权对资源的访问权限。 可以使用 Azure 资源的托管标识在应用程序中授权对 Blob 和队列的访问权限，此类应用程序可以运行在 Azure 虚拟机、函数应用、虚拟机规模集等位置中。
services: storage
author: WenJason
ms.service: storage
ms.topic: conceptual
origin.date: 10/17/2019
ms.date: 10/28/2019
ms.author: v-jay
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 8d92eb3456bd5e633a6380ad300d35a5feba6d53
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914438"
---
# <a name="authorize-access-to-blobs-and-queues-with-azure-active-directory-and-managed-identities-for-azure-resources"></a>使用 Azure Active Directory 和 Azure 资源的托管标识授权对 Blob 和队列的访问权限

Azure Blob 和队列存储支持使用 [Azure 资源的托管标识](../../active-directory/managed-identities-azure-resources/overview.md)进行 Azure Active Directory (Azure AD) 身份验证。 Azure 资源的托管标识可以从 Azure 虚拟机 (VM)、函数应用、虚拟机规模集和其他服务中运行的应用程序使用 Azure AD 凭据授权对 Blob 和队列数据的访问权限。 将 Azure 资源的托管标识与 Azure AD 身份验证结合使用，可避免将凭据随在云中运行的应用程序一起存储。  

本文介绍如何使用 Azure 资源的托管标识对访问授权，以便对 Azure VM 中的 Blob 或队列数据进行访问， 另外还介绍如何在开发环境中对代码进行测试。

## <a name="enable-managed-identities-on-a-vm"></a>在 VM 上启用托管标识

在使用 Azure 资源的托管标识对 VM 中 Blob 和队列的访问权限进行授权之前，必须首先在 VM 上启用针对 Azure 资源的托管标识。 若要了解如何为 Azure 资源启用托管标识，请参阅下述文章之一：

- [Azure 门户](/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager 模板](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure 资源管理器客户端库](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

有关托管标识的详细信息，请参阅 [Azure 资源的托管标识](../../active-directory/managed-identities-azure-resources/overview.md)。

## <a name="authenticate-with-the-azure-identity-library-preview"></a>使用 Azure 标识库（预览版）进行身份验证

用于 .NET 的 Azure 标识客户端库（预览版）可以对安全主体进行身份验证。 代码在 Azure 中运行时，安全主体是 Azure 资源的托管标识。

代码在开发环境中运行时，可能会自动处理身份验证，也可能需要浏览器登录才能进行身份验证，具体取决于使用哪些工具。

其他开发工具可能会提示你通过 Web 浏览器登录。 也可使用服务主体从开发环境进行身份验证。 有关详细信息，请参阅[在门户中创建 Azure 应用标识](../../active-directory/develop/howto-create-service-principal-portal.md)。

进行身份验证后，Azure 标识客户端库会获得令牌凭据。 然后，此令牌凭据会封装在服务客户端对象中。该对象由你创建，用于对 Azure 存储执行操作。 库会获取相应的令牌凭据，为你无缝处理这一切。

有关 Azure 标识客户端库的详细信息，请参阅[用于 .NET 的 Azure 标识客户端库](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity)。

## <a name="assign-rbac-roles-for-access-to-data"></a>分配用于访问数据的 RBAC 角色

当 Azure AD 安全主体尝试访问 Blob 或队列数据时，该安全主体必须有资源访问权限。 不管安全主体是 Azure 中的托管标识还是在开发环境中运行代码的 Azure AD 用户帐户，都必须为安全主体分配一个 RBAC 角色，由该角色授权访问 Azure 存储中的 Blob 或队列数据。 若要了解如何通过 RBAC 分配权限，请参阅[使用 Azure Active Directory 授权访问 Azure Blob 和队列](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights)中标题为“为访问权限分配 RBAC 角色”的部分。 

## <a name="install-the-preview-packages"></a>安装预览版包

本文中的示例使用用于 Blob 存储的 Azure 存储客户端库的最新预览版。 若要安装预览版包，请在 NuGet 包管理器控制台中运行以下命令：

```powershell
Install-Package Azure.Storage.Blobs -IncludePrerelease
```

本文中的示例还使用[用于 .NET 的 Azure 标识客户端库](https://www.nuget.org/packages/Azure.Identity/)的最新预览版通过 Azure AD 凭据进行身份验证。 若要安装预览版包，请在 NuGet 包管理器控制台中运行以下命令：

```powershell
Install-Package Azure.Identity -IncludePrerelease
```

## <a name="net-code-example-create-a-block-blob"></a>.NET 代码示例：创建块 Blob

向代码添加以下 `using` 指令，以便使用预览版 Azure 标识和 Azure 存储客户端库。

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Azure.Identity;
using Azure.Storage;
using Azure.Storage.Sas;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
```

若要获取令牌凭据，以便代码用它来授权对 Azure 存储的请求，请创建 [DefaultAzureCredential](https://docs.microsoft.com/dotnet/api/azure.identity.defaultazurecredential) 类的实例。 以下代码示例演示了如何获取经身份验证的令牌凭据并使用它来创建服务客户端对象，然后使用服务客户端来上传新的 Blob：

```csharp
async static Task CreateBlockBlobAsync(string accountName, string containerName, string blobName)
{
    // Construct the blob container endpoint from the arguments.
    string containerEndpoint = string.Format("https://{0}.blob.core.chinacloudapi.cn/{1}",
                                                accountName,
                                                containerName);

    // Get a credential and create a client object for the blob container.
    BlobContainerClient containerClient = new BlobContainerClient(new Uri(containerEndpoint),
                                                                    new DefaultAzureCredential());

    try
    {
        // Create the container if it does not exist.
        await containerClient.CreateIfNotExistsAsync();

        // Upload text to a new block blob.
        string blobContents = "This is a block blob.";
        byte[] byteArray = Encoding.ASCII.GetBytes(blobContents);

        using (MemoryStream stream = new MemoryStream(byteArray))
        {
            await containerClient.UploadBlobAsync(blobName, stream);
        }
    }
    catch (StorageRequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

> [!NOTE]
> 若要使用 Azure AD 授权针对 Blob 或队列数据的请求，必须对这些请求使用 HTTPS。

## <a name="next-steps"></a>后续步骤

- 若要详细了解 Azure 存储中的 RBAC 角色，请参阅[使用 RBAC 管理存储数据的访问权限](storage-auth-aad-rbac.md)。
- 若要了解如何从存储应用程序内授予容器和队列访问权限，请参阅[将 Azure AD 与存储应用程序配合使用](storage-auth-aad-app.md)。
- 若要了解如何使用 Azure AD 凭据运行 Azure CLI 和 PowerShell 命令，请参阅[使用 Azure AD 凭据运行 Azure CLI 或 PowerShell 命令以访问 Blob 或队列数据](storage-auth-aad-script.md)。
