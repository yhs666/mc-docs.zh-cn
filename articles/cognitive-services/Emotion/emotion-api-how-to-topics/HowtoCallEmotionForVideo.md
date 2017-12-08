---
title: "调用视频情感 API | Microsoft Docs"
description: "了解如何调用认知服务中的视频情感 API。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: emotion
ms.topic: article
origin.date: 02/06/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 3eba19f994bd6f0697329576e1d274b516e5fc78
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="how-to-call-emotion-api-for-video"></a>如何调用视频情感 API

本指南演示如何调用视频情感 API。 这些示例是使用视频情感 API 客户端库以 C# 语言编写的。

### <a name="Prep">准备工作</a> 
若要使用视频情感 API，需要一部包含人员的视频，最好是人正对着相机的视频。

### <a name="Step1">步骤 1：授权 API 调用</a> 
每次调用视频情感 API 都需要提供订阅密钥。 需通过查询字符串参数传递此密钥，或者在请求标头中指定此密钥。 若要通过查询字符串传递订阅密钥，请参考下面的视频情感 API 请求 URL 示例：

```
https://api.cognitive.azure.cn/emotion/v1.0/recognizeInVideo&subscription-key=<Your subscription key>
```

或者，也可以在 HTTP 请求标头中指定订阅密钥：

```
ocp-apim-subscription-key: <Your subscription key>
```

使用客户端库时，订阅密钥通过 VideoServiceClient 类的构造函数传入。 例如：

```
var emotionServiceClient = new emotionServiceClient("Your subscription key");
```

### <a name="Step2">步骤 2：将视频上传到服务并检查状态</a>
执行任何视频情感 API 调用的最基本方法是直接上传视频。 为此，可将包含应用程序/八进制流内容类型的“POST”请求连同从视频文件中读取的数据一起发送。 视频的最大大小为 100MB。

使用客户端库时，可以传入流对象来通过上传执行稳定化。 请参阅以下示例：

```
Operation videoOperation;
using (var fs = new FileStream(@"C:\Videos\Sample.mp4", FileMode.Open))
{
      videoOperation = await videoServiceClient.CreateOperationAsync(fs, OperationType.recognizeInVideo);
}
```

请注意，VideoServiceClient 的 CreateOperationAsync 方法是异步的。 也应该将调用方法标记为异步，以便能够使用 await 子句。
如果视频已在 Web 中并包含公共 URL，可以通过提供该 URL 来访问视频情感 API。 在此示例中，请求正文是包含 URL 的 JSON 字符串。

使用客户端库时，可以使用 CreateOperationAsync 方法的另一个重载，通过 URL 轻松执行稳定化。


```
var videoUrl = "http://www.example.com/sample.mp4";
Operation videoOperation = await videoServiceClient.CreateOperationAsync(videoUrl, OperationType. recognizeInVideo);

```

此上传方法对于所有视频情感 API 调用是相同的。 

上传视频后，需要执行的下一个操作是检查其状态。 由于视频文件通常比其他文件更大且更多样化，用户在执行此步骤时预期需要花费较长的处理时间。 具体时间取决于文件的大小和长度。

使用客户端库可以通过 GetOperationResultAsync 方法检索操作状态和结果。


```
var operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);

```
通常，客户端应定期检索操作状态，直到状态显示为“Succeeded”或“Failed”。

```
OperationResult operationResult;
while (true)
{
      operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);
      if (operationResult.Status == OperationStatus.Succeeded || operationResult.Status == OperationStatus.Failed)
      {
           break;
      }

      Task.Delay(30000).Wait();
}

```

当 VideoOperationResult 的状态显示为“Succeeded”时，可以通过将 VideoOperationResult 强制转换为 VideoOperationInfoResult<VideoAggregateRecognitionResult> 并访问 ProcessingResult 字段来检索结果。

```
var emotionRecognitionJsonString = ((VideoOperationInfoResult<VideoAggregateRecognitionResult>)operationResult).ProcessingResult;
```

### <a name="Step3">步骤 3：检索和了解情感识别并跟踪 JSON 输出</a>

输出结果以 JSON 格式包含给定文件中人脸的元数据。

如步骤 2 中所述，当 OperationResult 的状态显示为“Succeeded”时，OperationResult 的 ProcessingResult 字段中会提供 JSON 输出。

面部检测和跟踪 JSON 包括以下属性：

属性 | 说明
-------------|-------------
版本 | 指视频情感 API JSON 的版本。
时间刻度 | 视频每秒的“时钟周期”数。
Offset  |时间戳的时间偏移量。 在视频情感 API 版本 1.0 中，此属性的值始终为 0。 在将来支持的方案中，此值可能会有变化。
Framerate | 视频的每秒帧数。
Fragments   | 元数据分解成称为“片段”的不同较小部分。 每个片段包含开始时间、持续时间、间隔数字和事件。
开始   | 第一个事件的开始时间，以时钟周期为单位。
持续时间 |  片段的长度，以时钟周期为单位。
时间间隔 |  片段中每个事件的长度，以时钟周期为单位。
事件  | 事件的数组。 外部数组代表一个时间间隔。 内部数组包含在该时间点发生的 0 个或多个事件。
windowFaceDistribution |    发生事件期间具有特定情感的人脸百分比。
windowMeanScores |  图像中人脸的每种情感的平均评分。

以这种方式设置 JSON 格式的原因是为了针对将来的方案设置 API，在这些方案中，快速检索元数据和管理较大的结果流非常重要。 此格式同时使用分片（分解基于时间的部分中的元数据，从而可以做到只下载所需的内容）和分段（如果事件过大，可将其分解）。 一些简单的计算可帮助转换数据。 例如，如果事件从 6300（时钟周期）开始，其时间刻度为 2997（时钟周期/秒），帧速率为 29.97（帧/秒），那么：

*   开始时间/时间刻度 = 2.1 秒
*   秒数 x (帧速率/时间刻度) = 63 帧

下面是针对面部检测和跟踪，将 JSON 提取为每帧格式的简单示例：

```
var emotionRecognitionTrackingResultJsonString = operationResult.ProcessingResult;
var emotionRecognitionTracking = JsonConvert.DeserializeObject<EmotionRecognitionResult>(emotionRecognitionTrackingResultJsonString, settings);
```
由于情感会随着时间的变化而变得平缓，因此，如果曾经生成了一个视觉对象用于将结果层叠在原始视频的顶层，请从提供的时间戳中减去 250 毫秒。

### <a name="Summary">摘要</a>
本指南已介绍视频情感 API 的功能：如何上传视频，检查其状态，以及检索情感识别元数据。

