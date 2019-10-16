---
title: 快速入门：更改、训练应用，Node.js - LUIS
titleSuffix: Azure Cognitive Services
description: 在本 Node.js 快速入门中，你将向家庭自动化应用中添加示例话语并训练该应用。
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
ms.openlocfilehash: 8a52722020d1270eef283176078134ea3b7d7019
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71329741"
---
# <a name="quickstart-change-model-using-nodejs"></a>快速入门：使用 Node.js 更改模型

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>先决条件

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* 包含 NPM 的最新 [**Node.js**](https://nodejs.org/en/download/)。
* 本文的 NPM 依赖项包括：[**request**](https://www.npmjs.com/package/request)、[**request-promise**](https://www.npmjs.com/package/request-promise)、[**fs-extra**](https://www.npmjs.com/package/fs-extra)。  
* [Visual Studio Code](https://code.visualstudio.com/)。

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>示例话语 JSON 文件

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>创建快速入门代码 

将 NPM 依赖项添加到名为 `add-utterances.js` 的文件。

```
   // NPM Dependencies
   var rp = require('request-promise');
   var fse = require('fs-extra');
   var path = require('path');
```

将 LUIS 常量添加到文件。 复制以下代码，并将相应的占位符更改为自己的创作密钥、应用程序 ID 和版本 ID。

```
   // To run this sample, change these constants.

   // Authoring key, available in https://luis.azure.cn under Account Settings
   const LUIS_authoringKey = "YOUR-AUTHORING-KEY";

   // ID of your LUIS app to which you want to add an utterance
   const LUIS_appId = "YOUR-APP-ID";

   // The version number of your LUIS app
   const LUIS_versionId = "0.1";
```

添加包含陈述的上传文件的名称和位置。 

```
   // uploadFile is the file containing JSON for utterance(s) to add to the LUIS app.
   // The contents of the file must be in this format described at: https://aka.ms/add-utterance-json-format
   const uploadFile = "./utterances.json"
```

添加 `addUtterance` 函数的方法和对象。

```
   // upload configuration 
   var configAddUtterance = {
      LUIS_authoringKey: LUIS_authoringKey,
      LUIS_appId: LUIS_appId,
      LUIS_versionId: LUIS_versionId,
      inFile: path.join(__dirname, uploadFile),
      uri: "https://chinaeast2.api.cognitive.azure.cn/luis/api/v2.0/apps/{appId}/versions/{versionId}/examples".replace("{appId}", LUIS_appId).replace("{versionId}", LUIS_versionId)
   };


   // Call add-utterance
   var addUtterance = async (config) => {

      try {

         // Extract the JSON for the request body
         // The contents of the file to upload need to be in this format described in the comments above.
         var jsonUtterance = await fse.readJson(config.inFile);

         // Add an utterance
         var utterancePromise = sendUtteranceToApi({
               uri: config.uri,
               method: 'POST',
               headers: {
                  'Ocp-Apim-Subscription-Key': config.LUIS_authoringKey
               },
               json: true,
               body: jsonUtterance
         });

         let results = await utterancePromise;

         console.log(JSON.stringify(results));

      } catch (err) {
         console.log(`Error adding utterance:  ${err.message} `);
         //throw err;
      }

   }
```

添加 `train` 函数的方法和对象。

```
   // training configuration 
   var configTrain = {
      LUIS_authoringKey: LUIS_authoringKey,
      LUIS_appId: LUIS_appId,
      LUIS_versionId: LUIS_versionId,
      uri: "https://chinaeast2.api.cognitive.azure.cn/luis/api/v2.0/apps/{appId}/versions/{versionId}/train".replace("{appId}", LUIS_appId).replace("{versionId}", LUIS_versionId),
      method: 'POST', // POST to request training, GET to get training status
   };

   // Call train
   var train = async (config) => {

      try {

         var trainingPromise = sendUtteranceToApi({
               uri: config.uri,
               method: config.method, // Use POST to request training, GET to get training status 
               headers: {
                  'Ocp-Apim-Subscription-Key': config.LUIS_authoringKey
               },
               json: true,
               body: null      // The body can be empty for a training request
         });

         let results = await trainingPromise;
         console.log(JSON.stringify(results));
         
      } catch (err) {
         console.log(`Error in Training:  ${err.message} `);
         // throw err;
      }

   }
```

添加函数 `sendUtteranceToApi`，以发送和接收 HTTP 调用。 

```
   // Send JSON as the body of the POST request to the API
   var sendUtteranceToApi = async (options) => {
      try {

         var response; 
         if (options.method === 'POST') {
               response = await rp.post(options);
         } else if (options.method === 'GET') {
               response = await rp.get(options);
         }
         
         return { request: options.body, response: response };

      } catch (err) {
         throw err;
      }
   }
```

添加用于选择操作的 **main** 代码。

```
   var main = async() =>{
      try{

         console.log("Add utterances complete.");
         await addUtterance(configAddUtterance);

         console.log("Train");
         configTrain.method = 'POST';
         await train(configTrain, false);

         console.log("Train status.");
         configTrain.method = 'GET';
         await train(configTrain, true);

         console.log("process done");

      }catch(err){
         throw err;
      }
   }

   // MAIN
   main();
```

### <a name="install-dependencies"></a>安装依赖项

创建具有以下文本的 `package.json` 文件：

```
   {
   "name": "node",
   "version": "1.0.0",
   "description": "",
   "main": "add-utterances.js",
   "scripts": {
      "start":"node add-utterances.js",
      "test": "echo \"Error: no test specified\" && exit 1"
   },
   "author": "",
   "license": "ISC",
   "dependencies": {
      "fs-extra": "^5.0.0",
      "request": "^2.83.0",
      "request-promise": "^4.2.2"
   }
   }
```

在命令行上，从包含 package.json 的目录中使用 NPM 安装依赖项：`npm install`。

## <a name="run-code"></a>运行代码

使用 Node.js 在命令行中运行该应用程序。

调用 `npm start` 将添加话语、进行训练并获取训练状态。

```console
> npm start 
```

此命令行显示调用“添加陈述”API 的结果。 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]


## <a name="clean-up-resources"></a>清理资源

完成本快速入门后，删除在本快速入门中创建的所有文件。 

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"] 
> [以编程方式生成 LUIS 应用](luis-tutorial-node-import-utterances-csv.md)




