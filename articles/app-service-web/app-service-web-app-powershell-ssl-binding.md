---
title: 使用 PowerShell 创建 SSL 证书绑定
description: 了解如何使用 PowerShell 将 SSL 证书绑定到 Web 应用。
services: app-service\web
documentationcenter: ''
author: ahmedelnably
manager: stefsch
editor: ''
ms.assetid: 8ce32508-e982-48d3-b878-0e526afda537
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/13/2016
ms.date: 09/26/2016
ms.author: v-dazen
ms.openlocfilehash: d05dea44519cbec156d53a8efbe4e8182e9384e2
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52661911"
---
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>使用 PowerShell 创建 Azure App Service SSL 证书绑定
发行的 Azure PowerShell 版本 1.1.0 中添加了新的 cmdlet，可让用户将现有的或新的 SSL 证书绑定到现有的 Web 应用。

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>上传和绑定新的 SSL 证书
情景：用户要将 SSL 证书绑定到某个 Web 应用。

如果知道包含 Web 应用的资源组名称、Web 应用名称、用户计算机上的证书 .pfx 文件路径、证书的密码以及自定义主机名，我们就可以使用以下 PowerShell 命令创建 SSL 绑定：

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

请注意，在将 SSL 绑定添加到 Web 应用之前，必须已经配置主机名（自定义域）。 如果未配置主机名，则在运行 New-AzureRmWebAppSSLBinding 时会收到“‘主机名’不存在”错误。 可以直接通过门户或使用 Azure PowerShell 添加主机名。 以下 PowerShell 代码片段可以在运行 New-AzureRmWebAppSSLBinding 之前配置主机名。   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

请务必了解，Set-AzureRmWebApp cmdlet 会覆盖 Web 应用的主机名。 因此，上述 PowerShell 代码片段会追加到 Web 应用现有的主机名列表。  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>上传和绑定现有的 SSL 证书
情景：用户要将以前上传的 SSL 证书绑定到某个 Web 应用。

我们可以使用以下命令获取已上传到特定资源组的证书列表

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

请注意，证书位于特定位置和资源组的本地，如果配置的 Web 应用与所需的证书位于不同的位置和资源组，用户需要重新上传证书 

如果知道包含 Web 应用的资源组名称、Web 应用名称、证书指纹以及自定义主机名，我们就可以使用以下 PowerShell 命令创建该 SSL 绑定：

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>删除现有的 SSL 绑定
情景：用户想要删除现有的 SSL 绑定。

如果知道包含 Web 应用的资源组名称、Web 应用名称以及自定义主机名，我们就可以使用以下 PowerShell 命令删除该 SSL 绑定：

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

请注意，如果删除的 SSL 绑定是在该位置使用该证书的最后一个绑定，则会按默认删除该证书；如果用户想要保留证书，可以使用 DeleteCertificate 选项保留证书

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>参考
* [将 Azure PowerShell 与 Azure Resource Manager 结合使用](../powershell-azure-resource-manager.md)