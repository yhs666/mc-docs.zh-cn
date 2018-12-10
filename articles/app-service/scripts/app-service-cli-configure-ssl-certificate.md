---
title: Azure CLI 脚本示例 - 将自定义 SSL 证书绑定到 Web 应用 | Azure
description: Azure CLI 脚本示例 - 将自定义 SSL 证书绑定到 Web 应用
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
origin.date: 12/11/2017
ms.date: 04/02/2018
ms.author: v-yiso
ms.custom: mvc
ms.openlocfilehash: 805db8df97e5bbb683f23f5ebcb03a2b276ff6a2
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52662318"
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a>将自定义 SSL 证书绑定到 Web 应用

此示例脚本在应用服务中创建一个 Web 应用及其相关资源，并将自定义域名的 SSL 证书绑定到该应用。 在此示例中，需要以下项：

* 对域注册机构的 DNS 配置页的访问权限。
* 需要上传和绑定的 SSL 证书的有效 .PFX 文件及其密码。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

如果选择在本地安装并使用 CLI，则需使用 Azure CLI 2.0 或更高版本。 若要查找版本，请运行 `az --version`。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-lastest)。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

fqdn=<replace-with-www.{yourdomain}>
pfxPath=<replace-with-path-to-your-.PFX-file>
pfxPassword=<replace-with-your=.PFX-password>
resourceGroup=myResourceGroup
webappname=mywebapp$RANDOM

# Create a resource group.
az group create --location chinanorth --name $resourceGroup

# Create an App Service plan in Basic tier (minimum required by custom domains).
az appservice plan create --name $webappname --resource-group $resourceGroup --sku B1

# Create a web app.
az webapp create --name $webappname --resource-group $resourceGroup \
--plan $webappname

echo "Configure a CNAME record that maps $fqdn to $webappname.chinacloudsites.cn"
read -p "Press [Enter] key when ready ..."

# Before continuing, go to your DNS configuration UI for your custom domain and follow the 
# instructions at https://docs.azure.cn/zh-cn/app-service/app-service-web-tutorial-custom-domain#step-2-create-the-dns-records to configure a CNAME record for the 
# hostname "www" and point it your web app's default domain name.

# Map your prepared custom domain name to the web app.
az webapp config hostname add --webapp-name $webappname --resource-group $resourceGroup \
--hostname $fqdn

# Upload the SSL certificate and get the thumbprint.
thumbprint=$(az webapp config ssl upload --certificate-file $pfxPath \
--certificate-password $pfxPassword --name $webappname --resource-group $resourceGroup \
--query thumbprint --output tsv)

# Binds the uploaded SSL certificate to the web app.
az webapp config ssl bind --certificate-thumbprint $thumbprint --ssl-type SNI \
--name $webappname --resource-group $resourceGroup

echo "You can now browse to https://$fqdn"
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [`az group create`](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az_group_create) | 创建用于存储所有资源的资源组。 |
| [`az appservice plan create`](https://docs.azure.cn/zh-cn/cli/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | 创建应用服务计划。 |
| [`az webapp create`](https://docs.azure.cn/zh-cn/cli/webapp?view=azure-cli-latest#az_webapp_create) | 创建 Azure Web 应用。 |
| [`az webapp config hostname add`](https://docs.azure.cn/zh-cn/cli/webapp/config/hostname?view=azure-cli-latest#az_webapp_config_hostname_add) | 将自定义域映射到 Web 应用。 |
| [`az webapp config ssl upload`](https://docs.azure.cn/zh-cn/cli/webapp/config/ssl?view=azure-cli-latest#az_webapp_config_ssl_upload) | 将 SSL 证书上传到 Web 应用。 |
| [`az webapp config ssl bind`](https://docs.azure.cn/zh-cn/cli/webapp/config/ssl?view=azure-cli-latest#az_webapp_config_ssl_bind) | 将上传的 SSL 证书绑定到 Web 应用。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/overview?view=azure-cli-lastest)。

可以在 [Azure 应用服务文档](../app-service-cli-samples.md)中找到其他应用服务 CLI 脚本示例。

<!--Update_Description: add a note about Azure CLI 2.0 version-->