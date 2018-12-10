---
title: 示例：检测图像中的人脸 - 人脸 API
titleSuffix: Azure Cognitive Services
description: 使用人脸 API 检测图像中的人脸。
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: sample
origin.date: 03/01/2018
ms.date: 10/23/2018
ms.author: v-junlch
ms.openlocfilehash: 0b512fdd81ea01bbd65ae33291d4680f9de7f12a
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52650251"
---
# <a name="how-to-detect-faces-in-image"></a>如何检测图像中的人脸

本指南将演示如何使用提取的人脸属性（例如性别、年龄或姿势）检测图像中的人脸。 示例是在 C# 中使用人脸 API 客户端库编写而成。 

## <a name="concepts"></a> 概念

如果不熟悉本指南中的以下概念，请随时参阅[术语表](../Glossary.md)中的定义： 

- 人脸检测
- 人脸特征点
- 头部姿势
- 人脸属性

## <a name="preparation"></a> 准备工作

在此示例中，我们将演示以下功能： 

- 检测图像中的人脸，使用矩形框架对其进行标记
- 分析瞳孔、鼻子或嘴巴的位置，然后在图像中进行标记
- 分析人脸的头部姿势、性别和年龄

若要执行这些功能，需准备一张图像，其中至少有一个清晰的人脸。 

## <a name="step1"></a> 第 1 步：授权 API 调用

每次调用人脸 API 都需要提供订阅密钥。 需通过查询字符串参数传递此密钥，或者在请求标头中指定此密钥。 若要通过查询字符串传递订阅密钥，请参阅[“人脸 - 检测”](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)的请求 URL 作为示例：

```
https://api.cognitive.azure.cn/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription Key>
```

也可在 HTTP 请求头中指定订阅密钥：**ocp-apim-subscription-key：&lt;订阅密钥&gt;**。使用客户端库时，订阅密钥通过 FaceServiceClient 类的构造函数传入。 例如：
```CSharp
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

## <a name="step2"></a> 步骤 2：将图像上传到服务，然后执行人脸检测操作

进行人脸检测最基本的方法是直接上传图像。 为此，请发送“POST”请求，其中包含 application/octet-stream 内容类型和从 JPEG 图像中读取的数据。 图像的大小上限为 4MB。

如果使用客户端库，则可传入流对象，通过上传方式进行人脸检测。 请参阅以下示例：

```CSharp
using (Stream s = File.OpenRead(@"D:\MyPictures\image1.jpg"))
{
    var faces = await faceServiceClient.DetectAsync(s, true, true);
 
    foreach (var face in faces)
    {
        var rect = face.FaceRectangle;
        var landmarks = face.FaceLandmarks;
    }
}
```

请注意，FaceServiceClient 的 DetectAsync 方法是异步的。 也应将调用方法标记为异步，才能使用 await 子句。
如果图像已发布在 Web 上且有 URL，还可以通过提供 URL 来执行人脸检测。 在本示例中，请求正文为包含 URL 的 JSON 字符串。
使用客户端库时，若要通过提供 URL 来执行人脸检测，可以使用 DetectAsync 方法的另一个重载来轻松执行。

```CSharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.DetectAsync(imageUrl, true, true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
}
``` 

随检测到的人脸一起返回的 FaceRectangle 属性实质上是人脸上的位置，以像素表示。 通常情况下，此矩形包含眼睛、眉毛、鼻子和嘴巴，不包括头顶、耳朵和下巴。 如果裁剪完整的头部或中景肖像（带照片身份证类型的图像），可能需要扩展矩形人脸框架区域，因为该人脸区域对某些应用程序来说可能过小。 若要更精确地定位人脸，请使用下一部分中介绍的人脸特征点（定位人脸特征或计算人脸方向机制），经证明非常有用。

## <a name="step3"></a> 第 3 步：了解并使用人脸特征点

人脸特征点是人脸上的一系列具体特征点；通常是瞳孔、眼角或鼻子等人脸组成部分特征点。 人脸特征点是可在人脸检测期间分析的可选属性。 若要将人脸特征点包括在检测结果中，可以在调用[人脸 - 检测](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)时将“true”作为布尔值传递给 returnFaceLandmarks 查询参数，也可以使用 FaceServiceClient 类 DetectAsync 方法的 returnFaceLandmarks 可选参数。

默认情况下，有 27 个预定义的特征点。 下图显示了如何定义所有 27 个点：

![HowToDetectFace](../Images/landmarks.1.jpg)

返回的点以像素为单位，就像人脸矩形框一样。 这样可以更容易地在图像中标记特定的兴趣点。 以下代码演示了如何检索鼻子和瞳孔的位置：

```CSharp
var faces = await faceServiceClient.DetectAsync(imageUrl, returnFaceLandmarks:true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
 
    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;
 
    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;
 
    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
``` 

除了标记图像中的人脸特征，人脸特征点还可以用来准确地计算人脸的朝向。 例如，可以将人脸的朝向定义为一个从嘴巴中心到眼睛中心的矢量。 以下代码对此进行了详细说明：

```CSharp
var landmarks = face.FaceLandmarks;
 
var upperLipBottom = landmarks.UpperLipBottom;
var underLipTop = landmarks.UnderLipTop;
 
var centerOfMouth = new Point(
    (upperLipBottom.X + underLipTop.X) / 2,
    (upperLipBottom.Y + underLipTop.Y) / 2);
 
var eyeLeftInner = landmarks.EyeLeftInner;
var eyeRightInner = landmarks.EyeRightInner;
 
var centerOfTwoEyes = new Point(
    (eyeLeftInner.X + eyeRightInner.X) / 2,
    (eyeLeftInner.Y + eyeRightInner.Y) / 2);
 
Vector faceDirection = new Vector(
    centerOfTwoEyes.X - centerOfMouth.X,
    centerOfTwoEyes.Y - centerOfMouth.Y);
``` 

知道人脸的朝向以后，即可旋转矩形人脸框，使之对其人脸。 很明显，使用人脸特征点可以提供更多详细信息，也更实用。

## <a name="step4"></a> 步骤 4：使用其他人脸属性

除了人脸特征点，人脸 - 检测 API 还可以分析人脸上的多个其他属性。 这些属性包括：

- Age
- 性别
- 笑容程度
- 面部毛发
- 3D 头部姿势

这些属性使用统计学算法进行预测，并非始终 100% 准确。 但是，在需要通过这些属性对人脸分类时，这些属性仍然有用。 有关每个属性的详细信息，请参阅[术语表](../Glossary.md)。

下面是一个简单的示例，介绍了如何在人脸检测过程中提取人脸属性：

```CSharp
var requiredFaceAttributes = new FaceAttributeType[] {
                FaceAttributeType.Age,
                FaceAttributeType.Gender,
                FaceAttributeType.Smile,
                FaceAttributeType.FacialHair,
                FaceAttributeType.HeadPose,
                FaceAttributeType.Glasses
            };
var faces = await faceServiceClient.DetectAsync(imageUrl,
    returnFaceLandmarks: true,
    returnFaceAttributes: requiredFaceAttributes);

foreach (var face in faces)
{
    var id = face.FaceId;
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
}
``` 
## <a name="summary"></a> 总结

本指南介绍了[人脸 - 检测](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API 的功能：如何通过本地上传的图像或网络上的图像 URL 进行人脸检测；如何通过返回矩形人脸框进行人脸检测；以及如何分析人脸特征点、3D 头部姿势和其他人脸属性。

有关 API 的详细信息，请参阅 API 参考指南中的[人脸 - 检测](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)。

## <a name="related"></a> 相关主题

[如何识别图像中的人脸](HowtoIdentifyFacesinImage.md)

