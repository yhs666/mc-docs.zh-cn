---
title: include 文件
description: include 文件
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
origin.date: 05/31/2019
ms.date: 07/17/2019
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 5f8d1d1ca772d408b88c49e5e20225511a240a5e
ms.sourcegitcommit: c61b10764d533c32d56bcfcb4286ed0fb2bdbfea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68331863"
---
## <a name="run-the-function-locally"></a>在本地运行函数

使用 Azure Functions Core Tools 可以在本地开发计算机上运行 Azure Functions 项目。

1. 若要测试函数，请在函数代码中设置断点并按 F5 启动函数应用项目。 来自 Core Tools 的输出会显示在“终端”  面板中。

1. 在“终端”  面板中，复制 HTTP 触发的函数的 URL 终结点。 此 URL 包括函数密钥，该密钥将传递给 `code` 查询参数。

    ![Azure 本地输出](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. 将 HTTP 请求的 URL 粘贴到浏览器的地址栏中。 将查询字符串 `?name=<yourname>` 追加到此 URL 并执行请求。 执行将在命中断点时暂停。

1. 下面显示了当继续执行时浏览器中对 GET 请求的响应：

    ![浏览器中的函数 localhost 响应](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. 若要停止调试，请按 Shift + F5。

