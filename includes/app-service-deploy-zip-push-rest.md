---
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 11/03/2016
ms.date: 12/31/2018
ms.author: v-biyu
ms.openlocfilehash: 7146fe591da421dc6d03695c5fd51088fbe2b11d
ms.sourcegitcommit: b8aa5d05ef46f1db2df4f2653cdd8d150e847113
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2019
ms.locfileid: "54906290"
---
## <a name="rest"></a>使用 REST API 部署 ZIP 文件 
 
可以使用[部署服务 REST API](https://github.com/projectkudu/kudu/wiki/REST-API) 将 .zip 文件部署到 Azure 中的应用。 若要部署，请将 POST 请求发送到 https://<应用名称>.scm.chinacloudsites.cn/api/zipdeploy。 POST 请求必须在消息正文中包含此 .zip 文件。 应用的部署凭据是通过使用 HTTP BASIC 身份验证在请求中提供的。 有关详细信息，请参阅 [.zip 推送部署参考](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)。 

对于 HTTP 基本身份验证，需使用应用服务部署凭据。 若要了解如何设置部署凭据，请参阅[设置和重置用户级别凭据](../articles/app-service/deploy-configure-credentials.md#userscope)。

### <a name="with-curl"></a>使用 cURL

以下示例使用 cURL 工具部署 .zip 文件。 替换占位符 `<username>`、`<password>`、`<zip_file_path>` 和 `<app_name>`。 出现 cURL 提示时，键入密码。

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.chinacloudsites.cn/api/zipdeploy
```

此请求从已上传的 .zip 文件触发推送部署。 可以使用 https://<app_name>.scm.chinacloudsites.cn/api/deployments 终结点查看当前和之前的部署，如以下 cURL 示例所示。 同样，使用应用的名称替换 `<app_name>`；使用部署凭据的用户名替换 `<deployment_user>`。

```bash
curl -u <deployment_user> https://<app_name>.scm.chinacloudsites.cn/api/deployments
```

### <a name="with-powershell"></a>使用 PowerShell

以下示例使用 [Invoke-RestMethod](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-restmethod) 发送包含 .zip 文件的请求。 替换占位符 `<deployment_user>`、`<deployment_password>`、`<zip_file_path>` 和 `<app_name>`。

```PowerShell
#PowerShell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<zip_file_path>"
$apiUrl = "https://<app_name>.scm.chinacloudsites.cn/api/zipdeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
$userAgent = "powershell/1.0"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

此请求从已上传的 .zip 文件触发推送部署。 若要查看当前和之前的部署，请运行以下命令。 同样，请替换 `<app_name>` 占位符。

```bash
$apiUrl = "https://<app_name>.scm.chinacloudsites.cn/api/deployments"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method GET
```
