---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 11/30/2018
ms.date: 01/21/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 778300c34fca4e9cbd15b4cb1ac63d0642111e4f
ms.sourcegitcommit: 04392fdd74bcbc4f784bd9ad1e328e925ceb0e0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2019
ms.locfileid: "54333931"
---
使用提升的权限打开 PowerShell 控制台。



如果要在本地运行 Azure PowerShell，请连接到 Azure 帐户。 *Connect-AzureRmAccount* cmdlet 会提示你输入凭据。 进行身份验证后，它会下载帐户设置，以便 Azure PowerShell 可以使用这些设置。

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
