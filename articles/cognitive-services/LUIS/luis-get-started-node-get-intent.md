---
title: 快速入门：获取意向，Node.js - LUIS
titleSuffix: Azure Cognitive Services
description: 本快速入门使用可用的公共 LUIS 应用从会话文本中确定用户的意向。 使用 Node.js 将用户的意向作为文本发送到公共应用的 HTTP 预测终结点。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
origin.date: 09/04/2019
ms.date: 09/23/2019
ms.author: v-lingwu
ms.openlocfilehash: 9f8fa7c051be05ac23e692bf0e7c8ab048eccaac
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71329743"
---
# <a name="quickstart-get-intent-using-nodejs"></a>快速入门：使用 Node.js 获取意向

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>先决条件

* [Node.js](https://nodejs.org/) 编程语言 
* [Visual Studio Code](https://code.visualstudio.com/)
* 公共应用 ID：df67dcdb-c37d-46af-88e1-8b97951ca1c2


> [!NOTE] 
> [**Azure-Samples** GitHub 存储库](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/analyze-text/node)中提供了完整的 Node.js 解决方案。

## <a name="get-luis-key"></a>获取 LUIS 密钥

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>使用浏览器获取意向

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>以编程方式获取意向

可以使用 Node.js 来访问上一步骤中浏览器窗口显示的相同结果。

1. 复制以下代码片段：

```
    require('dotenv').config();

    var request = require('request');
    var querystring = require('querystring');

    // Analyze text
    //
    // utterance = user's text
    //
    function getLuisIntent(utterance) {

        // endpoint URL
        var endpoint =
            "https://chinaeast2.api.cognitive.azure.cn/luis/v2.0//apps/";

        // Set the LUIS_APP_ID environment variable 
        // to df67dcdb-c37d-46af-88e1-8b97951ca1c2, which is the ID
        // of a public sample application.    
        var luisAppId = process.env.LUIS_APP_ID;

        // Read LUIS key from environment file ".env"
        // You can use the authoring key instead of the endpoint key. 
        // The authoring key allows 1000 endpoint queries a month.
        var endpointKey = process.env.LUIS_ENDPOINT_KEY;

        // Create query string 
        var queryParams = {
            "verbose":  true,
            "q": utterance,
            "subscription-key": endpointKey
        }

        // append query string to endpoint URL
        var luisRequest =
            endpoint + luisAppId +
            '?' + querystring.stringify(queryParams);

        // HTTP Request
        request(luisRequest,
            function (err,
                response, body) {

                // HTTP Response
                if (err)
                    console.log(err);
                else {
                    var data = JSON.parse(body);

                    console.log(`Query: ${data.query}`);
                    console.log(`Top Intent: ${data.topScoringIntent.intent}`);
                    console.log('Intents:');
                    console.log(JSON.stringify(data.intents));
                }
            });
    }

    // Pass an utterance to the sample LUIS app
    getLuisIntent('turn on the left light');
```

2. 创建包含以下文本的 `.env` 文件，或者在系统环境中设置以下变量：

    ```CMD
    LUIS_APP_ID=df67dcdb-c37d-46af-88e1-8b97951ca1c2
    LUIS_ENDPOINT_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```

3. 将 `LUIS_ENDPOINT_KEY` 环境变量设置为你的密钥。

4. 通过在命令行上运行以下命令来安装依赖项：`npm install`。

5. 通过 `npm start` 运行代码。 其中会显示前面在浏览器窗口中显示的相同值。

## <a name="luis-keys"></a>LUIS 密钥

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>清理资源

删除 Node.js 文件。

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [添加表达](luis-get-started-node-add-utterance.md)




