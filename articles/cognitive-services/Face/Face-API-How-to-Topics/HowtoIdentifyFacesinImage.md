---
title: "使用人脸 API 识别图像中的人脸 | Microsoft Docs"
description: "使用认知服务中的人脸 API 在图像中识别人脸。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: face
ms.topic: article
origin.date: 02/06/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 4bb6952dbe2ec309c78506c9a4672c6e6e95e2a6
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="how-to-identify-faces-in-image"></a>如何识别图像中的人脸

本指南将演示如何使用事先根据已知人员创建的人员组识别未知的人脸。 这些示例是使用人脸 API 客户端库以 C# 语言编写的。

## <a name="concepts"></a> 概念

如果不熟悉本指南中的以下概念，请随时在[术语表](../Glossary.md)中搜索定义：

- 人脸 - 检测
- 人脸 - 识别
- 人员组

## <a name="preparation"></a>准备工作

在此示例中，我们将演示以下内容：

- 如何创建人员组 - 此人员组包含已知人员的列表。
- 如何将人脸分配给每个人 - 这些人脸用作识别人的基线。 建议使用清楚的正面人脸，就像带照片的身份证一样。 好的照片集应该包含同一人的多个人脸，但姿势、衣服颜色或发型并不相同。

若要演示此示例，需准备多个照片：

- 一些包含该人人脸的照片。 [单击此处下载示例照片](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data)，分别为 Anna、Bill 和 Clare。
- 一系列测试照片，其中可能包含（也可能不包含）Anna、Bill 或 Clare 的人脸，用于测试其识别效果。 也可从以上链接选择一些示例图像。

## <a name="step1"></a>步骤 1：授权 API 调用

每次调用人脸 API 都需要提供订阅密钥。 需通过查询字符串参数传递此密钥，或者在请求标头中指定此密钥。 若要通过查询字符串传递订阅密钥，请参阅 [Face - Detect](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)（人脸 - 检测）的请求 URL 示例：
```
https://api.cognitive.azure.cn/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Your subscription key>
```

订阅密钥也可在 HTTP 请求标头 ocp-apim-subscription-key: &lt;你的订阅密钥&gt; 中指定（替代方法）。使用客户端库时，订阅密钥通过 FaceServiceClient 类的构造函数传入。 例如：
 
```CSharp 
faceServiceClient = new FaceServiceClient("Your subscription key");
```
 
注册人脸 API 服务以后，订阅密钥可在 [Azure 门户](https://portal.azure.cn)中获取。

## <a name="step2"></a> 步骤 2：创建人员组

在此步骤中，我们创建了一个名为“MyFriends”的人员组，其中包含三人：Anna、Bill 和 Chare。 每个人都会注册多个人脸。 需要从图像中检测这些人脸。 完成所有这些步骤后，就会获得一个如下图所示的人员组：

![HowToIdentify1](../Images/group.image.1.jpg)

### <a name="step2-1"></a> 2.1 定义人员组的人
人是基本的识别单位。 一个人可以注册一个或多个已知人脸。 但是，人员组是人的集合，每个人在特定的人员组中定义。 是针对人员组完成识别的。 因此，此任务是先创建一个人员组，然后在其中创建人员，例如 Anna、Bill、Clare。

首先，需创建新的人员组。 这是通过[人员组 - 创建人员组](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API 执行的。 相应的客户端库 API 是用于 FaceServiceClient 类的 CreatePersonGroupAsync 方法。 指定用来创建组的组 ID 对于每个订阅来说是唯一的 - 也可使用其他人员组 API 来获取、更新或删除人员组。 定义某个组以后，即可使用[人员 - 创建人员](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API 在其中定义人员。 客户端库方法是 CreatePersonAsync。 创建人员以后，即可向每个人员添加人脸。

```CSharp 
// Create an empty person group
string personGroupId = "myfriends";
await faceServiceClient.CreatePersonGroupAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceServiceClient.CreatePersonAsync(
    // Id of the person group that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step2-2"></a> 2.2 检测人脸并将其注册到正确的人员
进行检测时，可以将一个“POST”Web 请求发送到[人脸 - 检测](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API，将图像文件置于 HTTP 请求正文中。 使用客户端库时，人脸检测通过用于 FaceServiceClient 类的 DetectAsync 方法来执行。

每检测到一个人脸，即可调用[人员 - 添加人脸](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)将其添加到正确的人员。

以下代码演示了从图像中检测人脸并将其添加到人员的具体过程：
```CSharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceServiceClient.AddPersonFaceAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
请注意，如果图像包含多个人脸，则只会添加最大的人脸。 可以向该人添加其他人脸，方法是将字符串以“targetFace = left, top, width, height”格式传递到[人员 - 添加人脸](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API 的 targetFace 查询参数，或者使用 AddPersonFaceAsync 方法的 targetFace 可选参数添加其他人脸。 添加到该人的每个人脸都会获得一个唯一且持久的人脸 ID，可以将其用在[人员 - 删除人脸](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e)和[人脸 - 识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)中。
## <a name="step3"></a> 步骤 3：人员组定型

人员组必须先定型，然后才能使用它来进行识别。 另外，在添加或删除人员后必须重新定型，在编辑某个人员的已注册人脸后也是如此。 定型由[人员组 - 人员组定型](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API 完成。 使用客户端库时，只需调用 TrainPersonGroupAsync 方法即可：
 
```CSharp 
await faceServiceClient.TrainPersonGroupAsync(personGroupId);
```
 
请注意，定型是一个异步过程。 即使已返回 TrainPersonGroupAsync 方法，该过程也未必就完成了。 可能需要通过[人员组 - 获取人员组定型状态](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) API 或客户端库的 GetPersonGroupTrainingStatusAsync 方法来查询定型状态。 以下代码演示了一个简单的逻辑，即如何等待人员组定型完成：
 
```CSharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceServiceClient.GetPersonGroupTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != "running")
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step4"></a> 步骤 4：根据定义的人员组来识别人脸
进行识别时，人脸 API 可以计算测试人脸与某个组中所有人脸的相似度，然后返回与该测试人脸最匹配的人。 这是通过[人脸 - 识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API 或客户端库的 IdentifyAsync 方法完成的。

需使用前述步骤检测测试人脸，然后将人脸 ID 作为辅助参数传递给识别 API。 一次可以确定多个人脸 ID，结果会包含所有识别结果。 默认情况下，识别操作只返回一个与测试人脸最匹配的人。 可以根据自己的偏好指定可选参数 maxNumOfCandidatesReturned，让识别操作返回更多的候选人。

以下代码演示了识别过程：
```CSharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceServiceClient.DetectAsync(s);
    var faceIds = faces.Select(face => face.FaceId).ToArray();
 
    var results = await faceServiceClient.IdentifyAsync(personGroupId, faceIds);
    foreach (var identifyResult in results)
    {
        Console.WriteLine("Result of face: {0}", identifyResult.FaceId);
        if (identifyResult.Candidates.Length == 0)
        {
            Console.WriteLine("No one identified");
        }
        else
        {
            // Get top 1 among all candidates returned
            var candidateId = identifyResult.Candidates[0].PersonId;
            var person = await faceServiceClient.GetPersonAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

完成这些步骤后，可以尝试识别不同的人脸，看能否根据上传的用于人脸检测的图像正确识别 Anna、Bill 或 Clare 的人脸。 请查看以下示例：

![HowToIdentify2](../Images/identificationResult.1.jpg )

## <a name="summary"></a> 摘要

本指南介绍了创建人员组并对人进行识别的过程。 下面是前面解释和演示的功能的简要提醒：

- 使用[人脸 - 检测](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d) API 检测人脸
- 使用[人员组 - 创建人员组](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API 创建人员组
- 使用[人员 - 创建人员](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API 创建人员
- 使用[人员组 - 人员组定型](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API 进行人员组定型
- 使用[人脸 - 识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API 根据人员组来识别未知人脸

## <a name="related"></a> 相关主题

[如何检测图像中的人脸](HowtoDetectFacesinImage.md)

