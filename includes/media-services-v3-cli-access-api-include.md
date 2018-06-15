---
title: include 文件
description: include 文件
services: media-services
author: WenJason
ms.service: media-services
ms.topic: include
origin.date: 04/13/2018
ms.date: 05/28/2018
ms.author: v-nany
ms.custom: include file
ms.openlocfilehash: 284e438f3592f4fe566aff7afcbad079ee80f425
ms.sourcegitcommit: 6f42cd6478fde788b795b851033981a586a6db24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2018
ms.locfileid: "34695569"
---
## <a name="access-the-media-services-api"></a>访问媒体服务 API

若要连接到 Azure 媒体服务 API，请使用 Azure AD 服务主体身份验证。 以下命令创建 Azure AD 应用程序并将服务主体附加到帐户。 应使用返回的值来配置应用。

如果是在 Visual Studio Code 或 Visual Studio 中开发，则通常会将这些值添加到 App.config。如果使用的是 Postman，则可能需要创建环境变量，然后将其设置为执行以下脚本后获得的值。  

在运行脚本之前，可以将 `amsaccount` 和 `amsResourceGroup` 替换为在创建这些资源时选择的名称。 `amsaccount` 是要向其附加服务主体的 Azure 媒体服务帐户的名称。 <br/>接下来的命令使用 `xml` 选项，该选项返回的 xml 可以粘贴到 app.config 中。如果省略 `xml` 选项，响应将采用 `json` 格式。

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```

此命令会生成如下响应：

```xml
<add key="Region" value="chinaeast" />
<add key="ResourceGroup" value="amsResourceGroup" />
<add key="AadEndpoint" value="https://login.partner.microsoftonline.cn" />
<add key="AccountName" value="amsaccount" />
<add key="SubscriptionId" value="111111111-0000-2222-3333-55555555555" />
<add key="ArmAadAudience" value="https://management.core.chinacloudapi.cn/" />
<add key="AadTenantId" value="2222222222-0000-2222-3333-6666666666666" />
<add key="AadSecret" value="33333333-0000-2222-3333-55555555555" />
<add key="AadClientId" value="44444444-0000-2222-3333-55555555555" />
<add key="ArmEndpoint" value="https://management.azure.cn/" />
```

### <a name="configure-your-net-core-app"></a>配置 .NET Core 应用

1. 打开 Visual Studio Code。
2. 浏览到 App.config 文件并将其打开。
3. 将 appSettings 值替换为在前一步骤获得的值。

 ```xml
 <add key="Region" value="value" />
 <add key="ResourceGroup" value="value" />
 <add key="AadEndpoint" value="value" />
 <add key="AccountName" value="value" />
 <add key="SubscriptionId" value="value" />
 <add key="ArmAadAudience" value="value" />
 <add key="AadTenantId" value="value" />
 <add key="AadSecret" value="value" />
 <add key="AadClientId" value="value" />
 <add key="ArmEndpoint" value="value" />
 ```    
