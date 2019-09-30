---
title: 快速入门：获取意向，Go - LUIS
titleSuffix: Azure Cognitive Services
description: 此 Go 快速入门使用可用的公共 LUIS 应用从会话文本中确定用户的意向。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
origin.date: 09/03/2019
ms.date: 09/23/2019
ms.author: v-lingwu
ms.openlocfilehash: adff16a9959add82838d805f021538a0ab9d24aa
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71330353"
---
# <a name="quickstart-get-intent-using-go"></a>快速入门：使用 Go 获取意向

在本快速入门中，你将向 LUIS 终结点传递话语并返回意向和实体。

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>先决条件

* [Go](https://golang.org/) 编程语言  
* [Visual Studio Code](https://code.visualstudio.com/)
* 公共应用 ID：df67dcdb-c37d-46af-88e1-8b97951ca1c2

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>获取 LUIS 密钥

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>使用浏览器获取意向

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>以编程方式获取意向

可以使用 GO 来访问上一步骤中浏览器窗口显示的相同结果。 

1. 创建名为 `endpoint.go` 的新文件。 添加以下代码：
    
```
        package main

        /* Do dependencies */
        import (
            "fmt"
            "flag"
            "net/http"
            "net/url"
            "io/ioutil"
            "log"
        )

        /* 
            Analyze text

            appID = public app ID = be402ffc-57f4-4e1f-9c1d-f0d9fa520aa4
            endpointKey = Azure Language Understanding key, or Authoring key if it still has quota
            region = endpoint region
            utterance = text to analyze

        */
        func endpointPrediction(appID string, endpointKey string, region string, utterance string) {

            var endpointUrl = fmt.Sprintf("https://%s.api.cognitive.azure.cn/luis/v2.0/apps/%s?subscription-key=%s&verbose=false&q=%s", region, appID, endpointKey, url.QueryEscape(utterance))
            
            response, err := http.Get(endpointUrl)

            // 401 - check value of 'subscription-key' - do not use authoring key!
            if err!=nil {
                // handle error
                fmt.Println("error from Get")
                log.Fatal(err)
            }
            
            response2, err2 := ioutil.ReadAll(response.Body)

            if err2!=nil {
                // handle error
                fmt.Println("error from ReadAll")
                log.Fatal(err2)
            }

            fmt.Println("response")
            fmt.Println(string(response2))
        }

        func main() {
            
            var appID = flag.String("appID", "df67dcdb-c37d-46af-88e1-8b97951ca1c2", "LUIS appID")
            var endpointKey = flag.String("endpointKey", "", "LUIS endpoint key")
            var region = flag.String("region", "", "LUIS app publish region")
            var utterance = flag.String("utterance", "turn on the bedroom light", "utterance to predict")

            flag.Parse()
            
            fmt.Println("appID has value", *appID)
            fmt.Println("endpointKey has value", *endpointKey)
            fmt.Println("region has value", *region)
            fmt.Println("utterance has value", *utterance)

            endpointPrediction(*appID, *endpointKey, *region, *utterance)

        }
```

2. 在你创建该文件的同一目录中，在命令提示符下输入 `go build endpoint.go` 来编译 Go 文件。 对于成功的生成，命令提示符不会返回任何信息。

3. 通过在命令提示符下输入以下文本从命令行运行 Go 应用程序： 

    ```CMD
    go run endpoint.go -appID df67dcdb-c37d-46af-88e1-8b97951ca1c2 -endpointKey <add-your-key> -region chinaeast2
    ```
    
    将 `<add-your-key>` 替换为你的密钥的值。  
    
    命令提示符响应为： 
    
    ```CMD
    appID has value df67dcdb-c37d-46af-88e1-8b97951ca1c2
    endpointKey has value xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    region has value chinaeast2
    utterance has value turn on the bedroom light
    response
    {
        "query": "turn on the bedroom light",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.809439957
        },
        "entities": [
            {
            "entity": "bedroom",
            "type": "HomeAutomation.Room",
            "startIndex": 12,
            "endIndex": 18,
            "score": 0.8065475
            }
        ]
    }
    ```
    
## <a name="luis-keys"></a>LUIS 密钥

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>清理资源
关闭 Go 文件并将其从文件系统中删除。 

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [添加表达](luis-get-started-go-add-utterance.md)




