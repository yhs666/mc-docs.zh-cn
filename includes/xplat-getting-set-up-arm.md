---
services: virtual-machines
title: 将 Azure CLI 与 Azure Resource Manager 配合使用
authors: squillace
solutions: ''
manager: timlt
editor: tysonn
ms.service: virtual-machine
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: linux
ms.workload: infrastructure
ms.date: 04/13/2015
ms.author: rasquill
ms.openlocfilehash: fe74be179565ca2026fa53b0cf68002ddaec9950
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
ms.locfileid: "20227815"
---
## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>将 Azure CLI 与 Azure Resource Manager (ARM) 配合使用

通过 Resource Manager 命令和模板使用 Azure CLI 以利用资源组部署 Azure 资源和工作负荷之前，需要有一个 Azure 帐户（这是当然的）。 如果没有帐户，可以[在此处获取免费 Azure](https://www.azure.cn/pricing/1rmb-trial/)。

> [!NOTE]
> 如果没有 Azure 帐户但有 MSDN 订阅，则可以使用试用帐户获取免费 Azure 信用额度。 

### <a name="step-1-verify-the-azure-cli-version"></a>步骤 1：验证 Azure CLI 版本

若要将 Azure CLI 用于强制性命令和 ARM 模板，至少需要安装 0.8.17 版。 要验证你的版本，请键入 `azure --version`。 你应看到类似如下的内容：

```
$ azure --version
0.8.17 (node: 0.10.25)
```

如果需要更新 Azure CLI 版本，请参阅 [Azure CLI](https://github.com/Azure/azure-xplat-cli)。

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>步骤 2：确保对 Azure 使用工作或学校标识

只有在使用 [Azure Active Directory 租户](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)或[服务主体名称](https://msdn.microsoft.com/library/azure/dn132633.aspx)时，才能使用 ARM 命令模式。 （这些也称为*组织 ID*。）

若要查看是否已经有一个这样的标识，请登录，方法是键入 `azure login`，然后在系统提示时使用工作单位或学校的用户名和密码。 如果确实有一个，则会看到下面这样的内容：

```
$ azure login
info:    Executing command login
warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
Username: ahmet@contoso.partner.onmschina.cn
Password: *********
|info:    Added subscription Visual Studio Ultimate with MSDN
info:    Setting subscription Visual Studio Ultimate with MSDN as default
info:    Added subscription Azure Free Trial
+
info:    login command OK
```

如果没有看到该内容，则必须使用 Microsoft 帐户标识创建一个新租户（或服务主体）。 （使用个人 MSDN 订阅或免费试用订阅时通常会发生这种情况。）若要通过使用 Microsoft ID 创建的 Azure 帐户创建一个工作或学校 ID，请参阅[将 Azure AD 目录与新的 Azure 订阅相关联](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)。 如果认为自己应该已经有一个组织 ID，则需向创建该帐户的人员反映情况。

### <a name="step-3-choose-your-azure-subscription"></a>步骤 3：选择 Azure 订阅

如果 Azure 帐户中只有一个订阅，则默认情况下，Azure CLI 会自行与该订阅相关联。 如果有多个订阅，需通过键入 `azure account set <subscription id or name> true` 来选择要使用的订阅，其中，_subscription id or name_ 是要在当前会话中使用的订阅 ID 或订阅名称。

你会看到下面这样的内容：

```
$ azure account set "Azure Free Trial" true
info:    Executing command account set
info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
info:    Changes saved
info:    account set command OK
```

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>步骤 4：将 Azure CLI 置于 ARM 模式下

若要在 Azure 资源管理 (ARM) 模式下使用 Azure CLI，请键入 `azure config mode arm`。 你会看到下面这样的内容：

```
$ azure config mode arm
info:    New mode is arm
```

> [!NOTE]
> 可以通过键入 `azure config mode asm` 切换回来，以便使用 Azure 服务管理命令。