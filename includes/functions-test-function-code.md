---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 04/24/2019
ms.date: 10/28/2019
ms.author: v-junlch
ms.openlocfilehash: 8d33186a41d1c9ad7160af21a0f5f9e4b89abdff
ms.sourcegitcommit: 7d2ea8a08ee329913015bc5d2f375fc2620578ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034420"
---
## <a name="test"></a>在 Azure 中验证函数

使用 cURL 验证已部署的函数。 使用从上一步复制的 URL（包括功能键），将查询字符串 `&name=<yourname>` 追加到该 URL。

![使用 cURL 在 Azure 中调用函数。](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

也可以将复制的 URL（包括功能键）粘贴到 Web 浏览器的地址栏中。 同样，在执行请求之前，先将查询字符串 `&name=<yourname>` 追加到该 URL。

![使用 Web 浏览器调用函数。](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  

