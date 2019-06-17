---
title: 获取意向，Python
titleSuffix: Language Understanding - Azure Cognitive Services
description: 在本快速入门中，你将向 LUIS 终结点传递话语并返回意向和实体。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 04/19/19
ms.author: v-lingwu
ms.openlocfilehash: ff9f94ab190629c6b6468f7b23ab1331e819c67b
ms.sourcegitcommit: e77582e79df32272e64c6765fdb3613241671c20
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "67135993"
---
# <a name="quickstart-get-intent-using-python"></a>快速入门：使用 Python 获取意向
在本快速入门中，你将向 LUIS 终结点传递话语并返回意向和实体。

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>先决条件

* [Python 3.6](https://www.python.org/downloads/) 或更高版本。
* [Visual Studio Code](https://code.visualstudio.com/)

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>获取 LUIS 密钥

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>使用浏览器获取意向

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent--programmatically"></a>以编程方式获取意向

可以使用 Python 来访问上一步骤中浏览器窗口显示的相同结果。

1. 将以下代码片段之一复制到名为 `quickstart-call-endpoint.py` 的文件中：

```
    ########### Python 2.7 #############
    import httplib, urllib, base64

    headers = {
        # Request headers includes endpoint key
        # You can use the authoring key instead of the endpoint key.
        # The authoring key allows 1000 endpoint queries a month.
        'Ocp-Apim-Subscription-Key': 'YOUR-KEY',
    }

    params = urllib.urlencode({
        # Text to analyze
        'q': 'turn on the left light',
        # Optional request parameters, set to default values
        'verbose': 'false',
    })

    # HTTP Request
    try:
        # LUIS endpoint HOST for chinaeast region
        conn = httplib.HTTPSConnection('chinaeast.api.cognitive.microsoft.com')

        # LUIS endpoint path
        # includes public app ID
        conn.request("GET", "/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?%s" % params, "{body}", headers)

        response = conn.getresponse()
        data = response.read()

        # print HTTP response to screen
        print(data)
        conn.close()
    except Exception as e:
        print("[Errno {0}] {1}".format(e.errno, e.strerror))

    ####################################
```

```
    ########### Python 3.6 #############
    import requests

    headers = {
        # Request headers
        'Ocp-Apim-Subscription-Key': 'YOUR-KEY',
    }

    params ={
        # Query parameter
        'q': 'turn on the left light',
        # Optional request parameters, set to default values
        'timezoneOffset': '0',
        'verbose': 'false',
        'spellCheck': 'false',
        'staging': 'false',
    }

    try:
        r = requests.get('https://chinaeast2.api.cognitive.azure.cn/luis/v2.0//apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2',headers=headers, params=params)
        print(r.json())

    except Exception as e:
        print("[Errno {0}] {1}".format(e.errno, e.strerror))

    ####################################
```

2. 将 `Ocp-Apim-Subscription-Key` 字段的值替换为你的 LUIS 终结点密钥。

3. 使用 `pip install requests` 安装依赖项。

4. 使用 `python ./quickstart-call-endpoint.py` 运行该脚本。 其中会显示前面在浏览器窗口中显示的相同 JSON。

## <a name="luis-keys"></a>LUIS 密钥

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>清理资源
删除 python 文件。 

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [添加表达](luis-get-started-python-add-utterance.md)




