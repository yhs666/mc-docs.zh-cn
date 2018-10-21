---
title: Azure CLI 脚本示例 - 将自定义 SSL 证书绑定到 Function App | Microsoft Docs
description: Azure CLI 脚本示例 - 将自定义 SSL 证书绑定到 Azure 中的 Function App
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: azure-functions
ms.devlang: azurecli
ms.topic: sample
origin.date: 07/03/2013
ms.date: 10/18/2018
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: f75170b5889393621adbbf672f209895bdfadd11
ms.sourcegitcommit: 2d33477aeb0f2610c23e01eb38272a060142c85d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49453938"
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>将自定义 SSL 证书绑定到 Function App

此示例脚本在应用服务中创建一个 Function App 及其相关资源，然后将自定义域名的 SSL 证书绑定到该应用。 在此示例中，需要以下项：

- 对域注册机构的 DNS 配置页的访问权限。
- 需要上传和绑定的 SSL 证书的有效 .PFX 文件及其密码。
- 已在自定义域中配置了一个指向 Web 应用的默认域名的 A 记录。 有关详细信息，请参阅[适用于 Azure 应用服务的映射自定义域说明](https://aka.ms/appservicecustomdns)。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

如果选择本地安装和使用 CLI，必须运行 Azure CLI 版本 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Function app and storage account names must be unique.
$storageName="mystorageaccount01"
$functionAppName="myconsumptionfunc01"

# TODO:
# Before starting, go to your DNS configuration UI for your custom domain and follow the 
# instructions at https://docs.azure.cn/zh-cn/app-service/app-service-web-tutorial-custom-domain#step-2-create-the-dns-records to configure an A record 
# and point it your web app's default domain name. 
fqdn=<Replace with www.{yourcustomdomain}>
pfxPath=<Replace with path to your .PFX file>
pfxPassword=<Replace with your .PFX password>

# Create a resource resourceGroupName
az group create `
  --name myResourceGroup `
  --location chinanorth

# Create an azure storage account
az storage account create `
  --name $storageName `
  --location chinanorth `
  --resource-group myResourceGroup `
  --sku Standard_LRS

# Create an App Service plan in Basic tier (minimum required by custom domains).
az appservice plan create `
  --name FunctionAppWithAppServicePlan `
  --location chinanorth `
  --resource-group myResourceGroup `
  --sku B1

# Create a Function App
az functionapp create `
  --name $functionAppName `
  --storage-account $storageName `
  --plan FunctionAppWithAppServicePlan `
  --resource-group myResourceGroup

# Map your prepared custom domain name to the function app.
az functionapp config hostname add `
  --name $functionAppName `
  --resource-group myResourceGroup `
  --hostname $fqdn

# Upload the SSL certificate and get the thumbprint.
thumbprint=$(az functionapp config ssl upload --certificate-file $pfxPath `
--certificate-password $pfxPassword --name $functionAppName --resource-group myResourceGroup `
--query thumbprint --output tsv)

# Binds the uploaded SSL certificate to the function app.
az functionapp config ssl bind `
  --certificate-thumbprint $thumbprint `
  --ssl-type SNI `
  --name $functionAppName `
  --resource-group myResourceGroup

echo "You can now browse to https://$fqdn"
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az storage account create](/cli/storage/account#az-storage-account-create) | 创建 Function App 所需的存储帐户。 |
| [az appservice plan create](/cli/appservice/plan#az-appservice-plan-create) | 创建绑定 SSL 证书所需的应用服务计划。 |
| [az functionapp create](/cli/functionapp#az-functionapp-create) | 在应用服务计划中创建函数应用。 |
| [az functionapp config hostname add](/cli/functionapp/config/hostname#az-functionapp-config-hostname-add) | 将自定义域映射到 Function App。 |
| [az functionapp config ssl upload](/cli/functionapp/config/ssl#az-functionapp-config-ssl-upload) | 将 SSL 证书上传到 Function App。 |
| [az functionapp config ssl bind](/cli/functionapp/config/ssl#az-functionapp-config-ssl-bind) | 将上传的 SSL 证书绑定到 Function App。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可以在 [Azure 应用服务文档](../functions-cli-samples.md)中找到其他应用服务 CLI 脚本示例。

