---
title: include 文件
description: include 文件
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: e7269dabc492a79720ea3c333b18dd8d5a4b7c0b
ms.sourcegitcommit: 5a57f99d978b78c1986c251724b1b04178c12d8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66195253"
---
## <a name="enable-event-grid-resource-provider"></a>启用事件网格资源提供程序

如果以前未在 Azure 订阅中使用过事件网格，则可能需要注册事件网格资源提供程序。 运行以下命令：

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

完成注册可能需要一些时间。 若要检查状态，请运行：

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

当 `RegistrationStatus` 为 `Registered` 后，即可继续。
