---
title: 使用 PowerShell 创建 Azure AD 应用以访问 Azure 媒体服务 API | Azure
description: 了解如何使用 PowerShell 创建 Azure Active Directory (Azure AD) 应用，并安装该应用以访问 Azure 媒体服务 API。
services: media-services
documentationcenter: ''
author: forester123
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/17/2017
ms.date: 09/25/2017
ms.author: v-johch
ms.openlocfilehash: 3e11b61b55172c14ed57c10bac934d74cbd1e851
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>使用 PowerShell 创建要与 Azure 媒体服务 API 配合使用的 Azure AD 应用

了解如何使用 PowerShell 脚本创建 Azure Active Directory (Azure AD) 应用程序和服务主体，以访问 Azure 媒体服务资源。

## <a name="prerequisites"></a>先决条件

- 一个 Azure 帐户。 如果没有帐户，请从 [Azure 1 元试用](https://www.azure.cn/pricing/1rmb-trial/)入手。
- 一个媒体服务帐户。 有关详细信息，请参阅[在 Azure 门户中创建 Azure 媒体服务帐户](media-services-portal-create-account.md)。
- Azure PowerShell 版本 0.8.8 或更高版本。 有关详细信息，请参阅[如何使用 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。
- Azure Resource Manager cmdlets。

## <a name="create-an-azure-ad-app-by-using-powershell"></a>使用 PowerShell 创建 Azure AD 应用

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
Import-Module AzureRM.Resources
Set-AzureRmContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

有关详细信息，请参阅以下文章：

- [使用 Azure PowerShell 创建服务主体来访问资源](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [使用 Azure PowerShell 管理基于角色的访问控制](../active-directory/role-based-access-control-manage-access-powershell.md)
- [如何使用证书手动配置守护程序应用](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>后续步骤

开始[将文件上传到帐户](media-services-portal-upload-files.md)。

<!--Update_Description:add one link-->