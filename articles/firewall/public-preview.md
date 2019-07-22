---
title: 启用 Azure 防火墙公共预览版
description: 使用 Azure PowerShell 启用 Azure 防火墙公共预览版
author: rockboyfor
ms.service: firewall
services: firewall
ms.topic: article
origin.date: 07/11/2018
ms.date: 07/22/2019
ms.author: v-yeche
ms.openlocfilehash: 9f2fd97c0cb57d8067dccca66ce8160bb58d6eb4
ms.sourcegitcommit: 5fea6210f7456215f75a9b093393390d47c3c78d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68337329"
---
<!--PENDING ON REGISTRY ON POWERSHELL-->
# <a name="enable-the-azure-firewall-public-preview"></a>启用 Azure 防火墙公共预览版

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

## <a name="enable-using-azure-powershell"></a>使用 Azure PowerShell 启用

若要启用 Azure 防火墙公共预览版，请使用以下 Azure PowerShell 命令：

```PowerShell
Register-AzProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network
Register-AzProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```

功能注册最多需要 30 分钟才能完成。 可以通过运行以下 Azure PowerShell 命令检查注册状态：

```powershell

Get-AzProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Get-AzProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```
注册完成后，运行以下命令：

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="next-steps"></a>后续步骤

- [教程：使用 Azure 门户部署和配置 Azure 防火墙](tutorial-firewall-deploy-portal.md)

<!-- Update_Description: new article about public preview-->
<!--ms.date: 07/22/2019-->