---
title: 快速入门：更改、训练应用，Go - LUIS
titleSuffix: Azure Cognitive Services
description: 在本 Go 语言快速入门中，你将向家庭自动化应用中添加示例话语并训练该应用。
author: lingliw
manager: digimobile
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
origin.date: 09/03/2019
ms.date: 09/23/2019
ms.author: v-lingwu
ms.openlocfilehash: b5cab38154e67e2f34892ac11c13b736f1793994
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71330350"
---
# <a name="quickstart-change-model-using-go"></a>快速入门：使用 Go 更改模型

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-intro-para.md)]

## <a name="prerequisites"></a>先决条件

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* 已安装 [Go](https://golang.org/) 编程语言。
* [VSCode](https://code.visualstudio.com) 

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>示例话语 JSON 文件

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>创建快速入门代码 

1. 使用 VSCode 创建 `add-utterances.go`。 

2. 添加依赖项。 

```
    // dependencies
    package main
    import (
        "fmt"
        "net/http"
        "io/ioutil"
        "log"
        "strings"
    )
```

3. 添加通用 HTTP 请求函数，这包括在标头中传递创作密钥。 

```
    // generic HTTP request
    // includes setting header with authoring key
    func httpRequest(httpVerb string, url string, authoringKey string, body string){

            client := &http.Client{}
        
            request, err := http.NewRequest(httpVerb, url, strings.NewReader(body))
            request.Header.Add("Ocp-Apim-Subscription-Key", authoringKey)

            fmt.Println("body")
            fmt.Println(body)

            response, err := client.Do(request)
            if err != nil {
                log.Fatal(err)
            } else {
                defer response.Body.Close()
                contents, err := ioutil.ReadAll(response.Body)
                if err != nil {
                    log.Fatal(err)
                }
                fmt.Println("   ", response.StatusCode)
                fmt.Println(string(contents))
            }
    }
```

4. 通过 JSON 文件添加示例话语。

```
    // get utterances from file and add to model
    func addUtterance(authoringKey string, appID string,  version string, fileOfLabeledExampleUtterances string){

        exampleUtterancesAsBytes, err := ioutil.ReadFile(fileOfLabeledExampleUtterances) // just pass the file name
        if err != nil {
            fmt.Print(err)
        }
        fmt.Println(string(exampleUtterancesAsBytes))


        // NOTE: region is chinaeast2
        var authoringUrl = fmt.Sprintf("https://chinaeast2.api.cognitive.azure.cn/luis/api/v2.0/apps/%s/versions/%s/examples", appID, version)

        httpRequest("POST", authoringUrl, authoringKey, (string(exampleUtterancesAsBytes)))
    }
```

5. 请求训练。 使用帮助程序函数将同一路由的谓词设置为训练状态。 

```
    func requestTraining(authoringKey string, appID string,  version string){

        trainApp("POST", authoringKey, appID, version)
    }
    func trainApp(httpVerb string, authoringKey string, appID string,  version string){

        var authoringUrl = fmt.Sprintf("https://chinaeast2.api.cognitive.azure.cn/luis/api/v2.0/apps/%s/versions/%s/train", appID, version)

        httpRequest(httpVerb,authoringUrl, authoringKey, "")
    }
```

6. 请求训练状态。 使用帮助程序函数将同一路由的谓词设置为请求训练。 

```
    func getTrainingStatus(authoringKey string, appID string, version string){

        trainApp("GET", authoringKey, appID, version)
    }
```

7. 添加主函数，以用于处理命令行分析。

```
    // main function
    func main() {

        // NOTE: change to your app ID
        var appID = "YOUR-APP-ID"

        // NOTE: change to your authoring key
        var authoringKey = "YOUR-AUTHORING-KEY"

        var version = "0.1"

        var exampleUtterances = "utterances.json"

        fmt.Println("add example utterances requested")
        addUtterance(authoringKey, appID, version, exampleUtterances)

        fmt.Println("training selected")
        requestTraining(authoringKey, appID,  version)

        fmt.Println("training status selected")
        getTrainingStatus(authoringKey, appID, version)

    }
```

## <a name="add-an-utterance-from-the-command-line-train-and-get-status"></a>从命令行添加话语，进行训练并获取状态

1. 在你创建 Go 文件的目录中，在命令提示符下输入 `go build add-utterances.go` 来编译 Go 文件。 对于成功的生成，命令提示符不会返回任何信息。

2. 通过在命令提示符下输入以下文本从命令行运行 Go 应用程序： 

    ```console
    add-utterances -appID <your-app-id> -authoringKey <add-your-authoring-key> -version <your-version-id> -region chinaeast -utteranceFile utterances.json

    ```

    将 `<add-your-authoring-key>` 替换为你的创作密钥的值（也称为初学者密钥）。 将 `<your-app-id>` 替换为你的应用 ID 的值。 将 `<your-version-id>` 替换为你的版本的值。 默认版本为 `0.1`。

    此命令提示符将显示以下结果：

    ```console
    add example utterances requested
    [
        {
            "text": "go lang 1",
            "intentName": "None",
            "entityLabels": []
        }
    ,
        {
            "text": "go lang 2",
            "intentName": "None",
            "entityLabels": []
        }
    ]
    201
    [
        {
            "value": {
                "ExampleId": 77783998,
                "UtteranceText": "go lang 1"
            },
            "hasError": false
        },
        {
            "value": {
                "ExampleId": 77783999,
                "UtteranceText": "go lang 2"
            },
            "hasError": false
        }
    ]
    training selected
    202
    {"statusId":9,"status":"Queued"}
    training status selected
    200
    [
        {
            "modelId": "c52d6509-9261-459e-90bc-b3c872ee4a4b",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "5119cbe8-97a1-4c1f-85e6-6449f3a38d77",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "01e6b6bc-9872-47f9-8a52-da510cddfafe",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "33b409b2-32b0-4b0c-9e91-31c6cfaf93fb",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "1fb210be-2a19-496d-bb72-e0c2dd35cbc1",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "3d098beb-a1aa-423f-a0ae-ce08ced216d6",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "cce854f8-8f8f-4ed9-a7df-44dfea562f62",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "4d97bf0d-5213-4502-9712-2d6e77c96045",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        }
    ]
    ```

    此响应包括三个 HTTP 调用中每一个的 HTTP 状态代码，以及在响应的正文中返回的任何 JSON 响应。 

## <a name="clean-up-resources"></a>清理资源
完成本快速入门后，删除在本快速入门中创建的所有文件。 

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"] 
> [生成具有自定义域的应用](luis-quickstart-intents-only.md) 




