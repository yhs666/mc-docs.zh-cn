---
title: 获取应用身份验证的值 - Azure SQL 数据库 | Microsoft Docs
description: 创建服务主体以便从代码访问 SQL 数据库。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
origin.date: 10/23/2018
ms.date: 03/25/2019
ms.openlocfilehash: 4391f7e6e03a3b757a1a43f34a4891e7838a2eb1
ms.sourcegitcommit: 02c8419aea45ad075325f67ccc1ad0698a4878f4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318930"
---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>获取对应用程序进行身份验证所需的值以便从代码访问 SQL 数据库

若要在代码中创建并管理 SQL 数据库，必须在创建 Azure 资源的订阅中的 Azure Active Directory (AAD) 域内注册你的应用。

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>创建服务主体以便从应用程序访问资源

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

需要安装并运行最新的 [Azure PowerShell](https://docs.microsoft.com/powershell/azure) 。 有关详细信息，请参阅[如何安装 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)。

以下 PowerShell 脚本将创建对 C# 应用进行身份验证所需的 Active Directory (AD) 应用程序和服务主体。 该脚本输出我们需要用于前面 C# 示例的值。 有关详细信息，请参阅[使用 Azure PowerShell 创建服务主体以访问资源](../active-directory/develop/howto-authenticate-service-principal-powershell.md)。

    # Sign in to Azure.
    Connect-AzAccount -EnvironmentName AzureChinaCloud

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>另请参阅
* [使用 C# 创建 SQL 数据库](sql-database-get-started-csharp.md)
* [使用 Azure Active Directory 身份验证连接到 SQL 数据库](sql-database-aad-authentication.md)

