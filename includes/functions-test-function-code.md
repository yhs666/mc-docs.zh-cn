---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 04/24/2019
ms.date: 09/27/2019
ms.author: v-junlch
ms.openlocfilehash: 628cd0bef3f74eb0d4fc0e5c0514c96474bcf720
ms.sourcegitcommit: c72fba1cacef1444eb12e828161ad103da338bb1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71695954"
---
## <a name="test"></a>在 Azure 中测试函数

使用 cURL 测试已部署的函数。 使用从上一步复制的 URL（包括功能键），将查询字符串 `&name=<yourname>` 追加到该 URL。

![使用 cURL 在 Azure 中调用函数。](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

也可以将复制的 URL（包括功能键）粘贴到 Web 浏览器的地址中。 同样，在执行请求之前，先将查询字符串 `&name=<yourname>` 追加到该 URL。

![使用 Web 浏览器调用函数。](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  

