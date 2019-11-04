---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/23/2019
ms.date: 10/28/2019
ms.author: v-junlch
ms.openlocfilehash: b201002074818734899bd323d515c3ba4cdf39cb
ms.sourcegitcommit: 7d2ea8a08ee329913015bc5d2f375fc2620578ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034428"
---
添加在 `context.bindings` 上使用 `msg` 输出绑定对象来创建队列消息的代码。 请在`context.res` 语句之前添加此代码。

```javascript
// Add a message to the Storage queue.
context.bindings.msg = "Name passed to the function: " + 
(req.query.name || req.body.name);
```

此时，你的函数应如下所示：

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    if (req.query.name || (req.body && req.body.name)) {
        // Add a message to the Storage queue.
        context.bindings.msg = "Name passed to the function: " + 
        (req.query.name || req.body.name);
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};
```

