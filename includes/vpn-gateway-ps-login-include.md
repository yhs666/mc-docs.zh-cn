---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: ff998a3db4461393867a3a97fd7afc6560ac6d12
ms.sourcegitcommit: 15a80d044339dab8bce43eb7be110ba01f630056
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2019
ms.locfileid: "69578655"
---
在开始此配置之前，必须登录到 Azure 帐户。 该 cmdlet 会提示提供 Azure 帐户的登录凭据。 登录后，它会下载帐户设置，供 Azure PowerShell 使用。 有关详细信息，请参阅[将 Windows PowerShell 与资源管理器配合使用](../articles/powershell-azure-resource-manager.md)。

若要登录，请使用提升的权限打开 PowerShell 控制台，并连接到帐户。 使用下面的示例来帮助连接：

```powershell
Connect-AzAccount -Environment AzureChinaCloud
```

如果有多个 Azure 订阅，请查看该帐户的订阅。

```powershell
Get-AzSubscription
```

指定要使用的订阅。

```powershell
Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```
