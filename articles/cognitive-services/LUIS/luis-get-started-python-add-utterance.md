---
title: 更改、训练应用，Python
titleSuffix: Language Understanding - Azure Cognitive Services
description: 此 Python 快速入门会将示例话语添加到家庭自动化应用并训练该应用。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 04/19/19
ms.author: v-lingwu
ms.openlocfilehash: 9d2a4dac903ba4e3227ea72aa061bf4668abe3ef
ms.sourcegitcommit: bf4c3c25756ae4bf67efbccca3ec9712b346f871
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2019
ms.locfileid: "65556925"
---
# <a name="quickstart-change-model-using-python"></a>快速入门：使用 Python 更改模型

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>先决条件

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [Python 3.6](https://www.python.org/downloads/) 或更高版本。
* [Visual Studio Code](https://code.visualstudio.com/)

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>示例话语 JSON 文件

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>创建快速入门代码

1. 将以下代码片段复制到名为 `add-utterances-3-6.py` 的文件中：

```
   ########### Python 3.6 #############
   # -*- coding: utf-8 -*-

   import http.client, sys, os.path, json

   # Authoring key, available in luis.ai under Account Settings
   LUIS_authoringKey  = "YOUR-AUTHORING-KEY"

   # ID of your LUIS app to which you want to add an utterance
   LUIS_APP_ID      = "YOUR-APP-ID"

   # The version number of your LUIS app
   LUIS_APP_VERSION = "0.1"

   # Update the host if your LUIS subscription is not in the West US region
   LUIS_HOST       = "westus.api.cognitive.microsoft.com"

   # uploadFile is the file containing JSON for utterance(s) to add to the LUIS app.
   # The contents of the file must be in this format described at: https://aka.ms/add-utterance-json-format
   UTTERANCE_FILE   = "./utterances.json"
   RESULTS_FILE     = "./utterances.results.json"

   # LUIS client class for adding and training utterances
   class LUISClient:
      
      # endpoint method names
      TRAIN    = "train"
      EXAMPLES = "examples"

      # HTTP verbs
      GET  = "GET"
      POST = "POST"

      # Encoding
      UTF8 = "UTF8"

      # path template for LUIS endpoint URIs
      PATH     = "/luis/api/v2.0/apps/{app_id}/versions/{app_version}/"

      # default HTTP status information for when we haven't yet done a request
      http_status = 200
      reason = ""
      result = ""

      def __init__(self, host, app_id, app_version, key):
         self.key = key
         self.host = host
         self.path = self.PATH.format(app_id=app_id, app_version=app_version)

      def call(self, luis_endpoint, method, data=""):
         path = self.path + luis_endpoint
         headers = {'Ocp-Apim-Subscription-Key': self.key}
         conn = http.client.HTTPSConnection(self.host)
         conn.request(method, path, data.encode(self.UTF8) or None, headers)
         response = conn.getresponse()
         self.result = json.dumps(json.loads(response.read().decode(self.UTF8)),
                                    indent=2)
         self.http_status = response.status
         self.reason = response.reason
         print(self.result)
         return self

      def add_utterances(self, filename=UTTERANCE_FILE):
         with open(filename, encoding=self.UTF8) as utterance:
               data = utterance.read()
         return self.call(self.EXAMPLES, self.POST, data)
         
      def train(self):
         return self.call(self.TRAIN, self.POST)

      def status(self):
         return self.call(self.TRAIN, self.GET)

      def print(self):
         if self.result:
               print(self.result)
         return self

      def raise_for_status(self):
         if 200 <= self.http_status < 300:
               return self
         raise http.client.HTTPException("{} {}".format(
               self.http_status, self.reason))

   if __name__ == "__main__":

      luis = LUISClient(LUIS_HOST, LUIS_APP_ID, LUIS_APP_VERSION,
                        LUIS_authoringKey)

      try:
         print("Adding utterance(s).")
         luis.add_utterances().raise_for_status()

         print("Requesting training.")
         luis.train().raise_for_status()

         print("Requesting training status.")
         luis.status().raise_for_status()

      except Exception as ex:
         luis.print()    # JSON response may have more details
         print("{0.__name__}: {1}".format(type(ex), ex))
```

## <a name="run-code"></a>运行代码
使用 Python 3.6 在命令行中运行该应用程序。

### <a name="add-utterances-from-the-command-line"></a>从命令行添加话语

调用不带参数的 add-utterance 会将陈述添加到应用，但不训练应用。

```console
> python add-utterances-3-6.py
```

将话语添加到模型时，将返回以下响应。  

```json
    "response": [
        {
            "value": {
                "UtteranceText": "go to seattle",
                "ExampleId": -5123383
            },
            "hasError": false
        },
        {
            "value": {
                "UtteranceText": "book a flight",
                "ExampleId": -169157
            },
            "hasError": false
        }
    ]
```

以下示例显示了训练请求成功的结果：

```json
{
    "request": null,
    "response": {
        "statusId": 9,
        "status": "Queued"
    }
}
```

```json
Requested training status.
[
   {
      "modelId": "eb2f117c-e10a-463e-90ea-1a0176660acc",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c1bdfbfc-e110-402e-b0cc-2af4112289fb",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "863023ec-2c96-4d68-9c44-34c1cbde8bc9",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "82702162-73ba-4ae9-a6f6-517b5244c555",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "37121f4c-4853-467f-a9f3-6dfc8cad2763",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "de421482-753e-42f5-a765-ad0a60f50d69",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "80f58a45-86f2-4e18-be3d-b60a2c88312e",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c9eb9772-3b18-4d5f-a1e6-e0c31f91b390",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "2afec2ff-7c01-4423-bb0e-e5f6935afae8",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "95a81c87-0d7b-4251-8e07-f28d180886a1",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   }
]
```

## <a name="clean-up-resources"></a>清理资源
完成本快速入门后，删除在本快速入门中创建的所有文件。 

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"] 
> [以编程方式生成 LUIS 应用](luis-tutorial-node-import-utterances-csv.md)




