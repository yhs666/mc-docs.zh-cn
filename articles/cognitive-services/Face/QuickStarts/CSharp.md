---
title: "人脸 API C# 快速入门 | Microsoft Docs"
description: "获取信息和代码示例，帮助自己快速开始使用认知服务中的人脸 API 和 C#。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: face
ms.topic: article
origin.date: 06/21/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 9b14415986f6988e8a2db50378da6db3f07a85ff
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="face-api-c-quick-starts"></a>人脸 API C# 快速入门
本文提供信息和代码示例来帮助读者快速开始使用人脸 API 和 C# 来完成以下任务： 
- [检测图像中的人脸](#Detect) 
- [创建人员组](#Create)

## <a name="prerequisites"></a>先决条件
- 在[此处](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)获取 Microsoft 人脸 API Windows SDK
- 在[此处](../../Computer-vision/Vision-API-How-to-Topics/HowToSubscribe.md)详细了解如何获取免费订阅密钥

## 使用人脸 API 通过 C# 检测图像中的人脸 <a name="Detect"> </a>
使用[“人脸 - 检测”方法](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)可以检测图像中的人脸，并返回人脸属性，包括：
- 人脸 ID：在多种人脸 API 方案使用的唯一 ID。 
- 人脸矩形：左侧坐标、顶部坐标、宽度和高度，指示人脸在图像中的位置。
- 地标：27 点人脸地标数组，指向人脸组成部分的重要位置。
- 面部属性包括年龄、性别、笑容强度、头部姿势和面部毛发。 

#### <a name="face-detect-c-example-request"></a>人脸检测 C# 示例请求

本示例是使用人脸 API 客户端库以 C# 语言编写的。 

1. 在 Visual Studio 中创建新的控制台解决方案。
1. 将 Program.cs 替换为以下代码。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 更改 `uriBase` 值以使用订阅密钥的获取位置。
1. 运行该程序。
1. 输入硬盘上某个图像的路径。

```c#
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;

namespace CSHttpClientSample
{
    static class Program
    {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace the subscriptionKey string value with your valid subscription key.
        const string subscriptionKey = "13hc77781f7e4b19b5fcdd72a8df7156";

        const string uriBase = "https://api.cognitive.azure.cn/face/v1.0/detect";

        static void Main()
        {
            // Get the path and filename to process from the user.
            Console.WriteLine("Detect faces:");
            Console.Write("Enter the path to an image with faces that you wish to analzye: ");
            string imageFilePath = Console.ReadLine();

            // Execute the REST API call.
            MakeAnalysisRequest(imageFilePath);

            Console.WriteLine("\nPlease wait a moment for the results to appear. Then, press Enter to exit...\n");
            Console.ReadLine();
        }


        /// <summary>
        /// Gets the analysis of the specified image file by using the Computer Vision REST API.
        /// </summary>
        /// <param name="imageFilePath">The image file.</param>
        static async void MakeAnalysisRequest(string imageFilePath)
        {
            HttpClient client = new HttpClient();

            // Request headers.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request parameters. A third optional parameter is "details".
            string requestParameters = "returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";

            // Assemble the URI for the REST API Call.
            string uri = uriBase + "?" + requestParameters;

            HttpResponseMessage response;

            // Request body. Posts a locally stored JPEG image.
            byte[] byteData = GetImageAsByteArray(imageFilePath);

            using (ByteArrayContent content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");

                // Execute the REST API call.
                response = await client.PostAsync(uri, content);

                // Get the JSON response.
                string contentString = await response.Content.ReadAsStringAsync();

                // Display the JSON response.
                Console.WriteLine("\nResponse:\n");
                Console.WriteLine(JsonPrettyPrint(contentString));
            }
        }


        /// <summary>
        /// Returns the contents of the specified file as a byte array.
        /// </summary>
        /// <param name="imageFilePath">The image file to read.</param>
        /// <returns>The byte array of the image data.</returns>
        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }


        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            int offset = 0;
            int indentLength = 3;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\'':
                        if (quote) ignore = !ignore;
                        break;
                }

                if (quote)
                    sb.Append(ch);
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (ch != ' ') sb.Append(ch);
                            break;
                    }
                }
            }

            return sb.ToString().Trim();
        }
    }
}
```
#### <a name="face-detect-response"></a>人脸检测响应

成功响应将以 JSON 格式返回。 下面是成功响应的示例： 

```json
Response:

[
   {
      "faceId": "f7eda569-4603-44b4-8add-cd73c6dec644",
      "faceRectangle": {
         "top": 131,
         "left": 177,
         "width": 162,
         "height": 162
      },
      "faceAttributes": {
         "smile": 0.0,
         "headPose": {
            "pitch": 0.0,
            "roll": 0.1,
            "yaw": -32.9
         },
         "gender": "female",
         "age": 22.9,
         "facialHair": {
            "moustache": 0.0,
            "beard": 0.0,
            "sideburns": 0.0
         },
         "glasses": "NoGlasses",
         "emotion": {
            "anger": 0.0,
            "contempt": 0.0,
            "disgust": 0.0,
            "fear": 0.0,
            "happiness": 0.0,
            "neutral": 0.986,
            "sadness": 0.009,
            "surprise": 0.005
         },
         "blur": {
            "blurLevel": "low",
            "value": 0.06
         },
         "exposure": {
            "exposureLevel": "goodExposure",
            "value": 0.67
         },
         "noise": {
            "noiseLevel": "low",
            "value": 0.0
         },
         "makeup": {
            "eyeMakeup": true,
            "lipMakeup": true
         },
         "accessories": [

         ],
         "occlusion": {
            "foreheadOccluded": false,
            "eyeOccluded": false,
            "mouthOccluded": false
         },
         "hair": {
            "bald": 0.0,
            "invisible": false,
            "hairColor": [
               {
                  "color": "brown",
                  "confidence": 1.0
               },
               {
                  "color": "black",
                  "confidence": 0.87
               },
               {
                  "color": "other",
                  "confidence": 0.51
               },
               {
                  "color": "blond",
                  "confidence": 0.08
               },
               {
                  "color": "red",
                  "confidence": 0.08
               },
               {
                  "color": "gray",
                  "confidence": 0.02
               }
            ]
         }
      }
   }
]
```
## 使用人脸 API 通过 C# 创建人员组 <a name="Create"> </a>

使用[“人员组 - 创建人员组”方法](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)可以创建包含指定 personGroupId、名称和用户提供的 userData 的人员组。

#### <a name="person-group---create-a-person-group-c-example-request"></a>“人员组 - 创建人员组”C# 示例请求

在 Visual Studio 中创建一个新的控制台解决方案，然后将 Program.cs 替换为以下代码。 更改 `string uri` 以使用订阅密钥的获取区域，并将“Ocp-Apim-Subscription-Key”值替换为有效的订阅密钥。

```c#
using System;
using System.Net.Http.Headers;
using System.Net.Http;

namespace CSHttpClientSample
{
    static class Program
    {
        static void Main()
        {
            Console.WriteLine("Enter an ID for the group you wish to create:");
            Console.WriteLine("(Use numbers, lower case letters, '-' and '_'. The maximum length of the personGroupId is 64.)");

            string personGroupId = Console.ReadLine();
            MakeCreateGroupRequest(personGroupId);

            Console.WriteLine("\n\n\nWait for the result below, then hit ENTER to exit...\n\n\n");
            Console.ReadLine();
        }


        static async void MakeCreateGroupRequest(string personGroupId)
        {
            var client = new HttpClient();

            // Request headers - replace this example key with your valid key.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "6726adbabb494773a28a7a5a21d5974a");

            // Request URI string.
            string uri = "https://api.cognitive.azure.cn/face/v1.0/persongroups/" + personGroupId;

            // Here "name" is for display and doesn't have to be unique. Also, "userData" is optional.
            string json = "{\"name\":\"My Group\", \"userData\":\"Some data related to my group.\"}";
            HttpContent content = new StringContent(json);
            content.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            HttpResponseMessage response = await client.PutAsync(uri, content);

            // If the group was created successfully, you'll see "OK".
            // Otherwise, if a group with the same personGroupId has been created before, you'll see "Conflict".
            Console.WriteLine("Response status: " + response.StatusCode);
        }
    }
}
```

