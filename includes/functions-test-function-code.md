---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/04/2018
ms.date: 02/22/2019
ms.author: v-junlch
ms.openlocfilehash: 564555f4304044de47baa9047bbcb61a6f78487c
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56666718"
---
## <a name="test"></a>测试函数

在 Mac 或 Linux 计算机上使用 cURL 或者在 Windows 上使用 Powershell 测试已部署的函数。 执行以下 cURL 命令（请将 `<app_name>` 占位符替换为 Function App 的名称）。 在 URL 的后面附加查询字符串 `&name=<yourname>`。

```powershell
Invoke-WebRequest -Uri "https://<app_name>.chinacloudsites.cn/api/MyHttpTrigger?name=<yourname>"
```

```bash
curl https://<app_name>.chinacloudsites.cn/api/MyHttpTrigger?name=<yourname>
```  

![函数响应会显示在浏览器中。](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

如果无法在命令行中使用 `cURL` 或 `Invoke-WebRequest`，请在 Web 浏览器的地址栏中输入相同的 URL。 同样，请将 `<app_name>` 占位符替换为 Function App 的名称，在 URL 的后面附加查询字符串 `&name=<yourname>`，然后执行请求。

    https://<app_name>.chinacloudsites.cn/api/MyHttpTrigger?name=<yourname>
   
![函数响应会显示在浏览器中。](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  

<!-- ms.date: 02/22/2019 -->