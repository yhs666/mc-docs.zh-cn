---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 07/05/2019
ms.date: 09/05/2019
ms.author: v-junlch
ms.openlocfilehash: aa6883b85f5f7377242ed3a17db3407696645c18
ms.sourcegitcommit: 4f1047b6848ca5dd96266150af74633b2e9c77a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70805737"
---
1. 若要运行函数，请按 F5  。 可能需要启用一个防火墙例外，以便这些工具能够处理 HTTP 请求。 在本地运行时，永远不会强制实施授权级别。

2. 从 Azure Functions 运行时输出复制函数的 URL。

    ![Azure 本地运行时](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. 将 HTTP 请求的 URL 粘贴到浏览器的地址栏中。 将查询字符串 `?name=<YOUR_NAME>` 追加到此 URL 并执行请求。 下面演示浏览器中函数返回的对本地 GET 请求的响应： 

    ![浏览器中的函数 localhost 响应](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. 若要停止调试，请按 **Shift + F5**。

