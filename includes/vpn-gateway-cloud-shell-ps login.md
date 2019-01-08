---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 11/30/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: f095b3087d514fd919b4b3e267cfef5e2b554cc1
ms.sourcegitcommit: 0a5a7daaf864ef787197f2b8e62539786b6835b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2018
ms.locfileid: "53656646"
---
使用提升的权限打开 PowerShell 控制台。



如果要在本地运行 Azure PowerShell，请连接到 Azure 帐户。 *Connect-AzureRmAccount -EnvironmentName AzureChinaCloud* cmdlet 会提示你输入凭据。 进行身份验证后，它会下载帐户设置，以便 Azure PowerShell 可以使用这些设置。

```azurepowershell
Connect-AzureRmAccount -EnvironmentName AzureChinaCloud
```

如果有多个订阅，请获取 Azure 订阅的列表。

```azurepowershell
Get-AzureRmSubscription
```

指定要使用的订阅。

```azurepowershell
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```