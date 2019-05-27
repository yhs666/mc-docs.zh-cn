---
title: 使用 HeadPose 调整人脸矩形
titleSuffix: Azure Cognitive Services
description: 了解如何使用 HeadPose 属性来自动旋转人脸矩形。
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
origin.date: 04/26/2019
ms.date: 05/14/2019
ms.author: v-junlch
ms.openlocfilehash: 02f814dfdb25229b23da8519dcd2226ce7ea3045
ms.sourcegitcommit: 71172ca8af82d93d3da548222fbc82ed596d6256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65669042"
---
# <a name="use-the-headpose-attribute-to-adjust-the-face-rectangle"></a>使用 HeadPose 属性调整人脸矩形

在本指南中，你将使用检测到的人脸属性 HeadPose 来旋转 Face 对象的矩形。 本指南中的示例代码摘自[认知服务人脸 WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) 示例应用，使用 .NET SDK。

连同检测到的每个人脸一起返回的人脸矩形标记了该人脸在图像中的位置和大小。 默认情况下，该矩形始终与图像对齐（它的四条边是完全垂直和水平的）；这样可能不能有效地定格倾斜的人脸。 如果你想要以编程方式裁剪图像中的人脸，最好是能够旋转该矩形。

## <a name="explore-the-sample-code"></a>探索示例代码

可以使用 HeadPose 属性以编程方式旋转人脸矩形。 如果在检测人脸（请参阅[如何检测人脸](HowtoDetectFacesinImage.md)）时指定此属性，则稍后可以查询此属性。 [认知服务人脸 WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) 应用中的以下方法采用 **DetectedFace** 对象的列表，并返回 **[Face](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/Face.cs)** 对象的列表。 此处的 **Face** 是一个自定义类，它用于存储人脸数据，包括更新的矩形坐标。 将计算**顶边缘**、**左边缘**、**宽度**和**高度**的新值；新字段 **FaceAngle** 指定旋转角度。

```csharp
/// <summary>
/// Calculate the rendering face rectangle
/// </summary>
/// <param name="faces">Detected face from service</param>
/// <param name="maxSize">Image rendering size</param>
/// <param name="imageInfo">Image width and height</param>
/// <returns>Face structure for rendering</returns>
public static IEnumerable<Face> CalculateFaceRectangleForRendering(IList<DetectedFace> faces, int maxSize, Tuple<int, int> imageInfo)
{
    var imageWidth = imageInfo.Item1;
    var imageHeight = imageInfo.Item2;
    var ratio = (float)imageWidth / imageHeight;
    int uiWidth = 0;
    int uiHeight = 0;
    if (ratio > 1.0)
    {
        uiWidth = maxSize;
        uiHeight = (int)(maxSize / ratio);
    }
    else
    {
        uiHeight = maxSize;
        uiWidth = (int)(ratio * uiHeight);
    }

    var uiXOffset = (maxSize - uiWidth) / 2;
    var uiYOffset = (maxSize - uiHeight) / 2;
    var scale = (float)uiWidth / imageWidth;

    foreach (var face in faces)
    {
        var left = (int)(face.FaceRectangle.Left * scale + uiXOffset);
        var top = (int)(face.FaceRectangle.Top * scale + uiYOffset);

        // Angle of face rectangles, default value is 0 (not rotated).
        double faceAngle = 0;

        // If head pose attributes have been obtained, re-calculate the left & top (X & Y) positions.
        if (face.FaceAttributes?.HeadPose != null)
        {
            // Head pose's roll value acts directly as the face angle.
            faceAngle = face.FaceAttributes.HeadPose.Roll;
            var angleToPi = Math.Abs((faceAngle / 180) * Math.PI);

            // _____       | / \ |
            // |____|  =>  |/   /|
            //             | \ / |
            // Re-calculate the face rectangle's left & top (X & Y) positions.
            var newLeft = face.FaceRectangle.Left +
                face.FaceRectangle.Width / 2 -
                (face.FaceRectangle.Width * Math.Sin(angleToPi) + face.FaceRectangle.Height * Math.Cos(angleToPi)) / 2;

            var newTop = face.FaceRectangle.Top +
                face.FaceRectangle.Height / 2 -
                (face.FaceRectangle.Height * Math.Sin(angleToPi) + face.FaceRectangle.Width * Math.Cos(angleToPi)) / 2;

            left = (int)(newLeft * scale + uiXOffset);
            top = (int)(newTop * scale + uiYOffset);
        }

        yield return new Face()
        {
            FaceId = face.FaceId?.ToString(),
            Left = left,
            Top = top,
            OriginalLeft = (int)(face.FaceRectangle.Left * scale + uiXOffset),
            OriginalTop = (int)(face.FaceRectangle.Top * scale + uiYOffset),
            Height = (int)(face.FaceRectangle.Height * scale),
            Width = (int)(face.FaceRectangle.Width * scale),
            FaceAngle = faceAngle,
        };
    }
}
```

## <a name="display-the-updated-rectangle"></a>显示更新的矩形

在此处，可以使用显示画面中返回的 **Face** 对象。 [FaceDetectionPage.xaml](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/FaceDetectionPage.xaml) 中的以下行显示如何基于此数据呈现新矩形：

```xaml
 <DataTemplate>
    <Rectangle Width="{Binding Width}" Height="{Binding Height}" Stroke="#FF26B8F4" StrokeThickness="1">
        <Rectangle.LayoutTransform>
            <RotateTransform Angle="{Binding FaceAngle}"/>
        </Rectangle.LayoutTransform>
    </Rectangle>
</DataTemplate>
```

## <a name="next-steps"></a>后续步骤

有关旋转的人脸矩形的有效示例，请参阅 GitHub 上的[认知服务人脸 WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) 应用。 或者，请参阅[人脸 API HeadPose 示例](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples)应用，该应用会实时跟踪 HeadPose 属性，以检测各种头部动作（点头、摇头）。

