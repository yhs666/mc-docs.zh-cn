---
title: 计算机视觉 API C# 教程 | Microsoft Docs
description: 探讨一个使用 Microsoft 认知服务中的计算机视觉 API 的基本 Windows 应用。 执行 OCR，创建缩略图，并处理图像中的视觉特征。
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: computer-vision
ms.topic: article
origin.date: 05/22/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 0ea281b7f57993acf7165f8159fbb80a3e9bfc19
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
ms.locfileid: "23407604"
---
# <a name="computer-vision-api-c35-tutorial"></a>计算机视觉 API C&#35; 教程

探讨一个使用计算机视觉 API 执行光学字符识别 (OCR)、创建智能裁剪的缩略图，以及在图像中检测、分类、标记和描述视觉特征（包括人脸）的基本 Windows 应用程序。 在以下示例中，可以提交图像 URL 或本地存储的文件。 可以使用此开源示例作为模板，生成自己的使用视觉 API 和 WPF（Windows Presentation Foundation，.NET Framework 的一部分）的 Windows 应用。

### <a name="Prerequisites">先决条件</a>

#### <a name="platform-requirements"></a>平台要求

以下示例是使用 [Visual Studio 2015 Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs) 针对 .NET Framework 开发的。 

#### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>订阅计算机视觉 API 并获取订阅密钥 

在创建示例之前，必须订阅计算机视觉 API（Microsoft 认知服务的一部分，以前称为 Project Oxford）。 有关订阅和密钥管理的详细信息，请参阅[订阅](/cognitive-services/)。 在本教程中，可以使用主密钥和辅助密钥。 

#### <a name="get-the-client-library-and-example"></a>获取客户端库和示例

可以通过 [SDK](https://www.github.com/microsoft/cognitive-vision-windows) 将计算机视觉 API 客户端库和示例应用程序克隆到计算机。 不要下载 ZIP 格式的文件。

### <a name="Step1">步骤 1：安装示例</a>

在 GitHub Desktop 中，打开 Sample-WPF\VisionAPI-WPF-Samples.sln。

### <a name="Step2">步骤 2：生成示例</a>

- 按 Ctrl+Shift+B，或者单击功能区菜单中的“生成”，并选择“生成解决方案”。

### <a name="Step3">步骤 3：运行示例</a>

1. 完成生成后，按 **F5**，或单击功能区菜单中的“开始”运行示例。
2. 找到计算机视觉 API 用户界面窗口，其中包含带有“在此处粘贴订阅密钥以开始”字样的文本编辑框。
可以选择通过单击“保存密钥”按钮在个人电脑或笔记本电脑上保存订阅密钥。 如果想要从系统中删除订阅密钥，请单击“删除密钥”，将密钥从个人电脑或笔记本电脑中删除。

    ![视觉订阅密钥](../Images/Vision_UI_Subscription.PNG)

3. 在“选择方案”下面，通过单击选择使用六种方案中的一种，然后遵照屏幕说明操作。 Microsoft 会收到所上传的图像，并可能会使用这些图像来改进计算机视觉 API 和相关服务。 提交图像即表示你已确认遵守我们的[开发人员行为准则](https://azure.microsoft.com/en-us/support/legal/developer-code-of-conduct/)。

    ![分析图像接口](../Images/Analyze_Image_Example.PNG)

4. 将在此示例应用程序中使用示例图像。 可以在人脸 API Windows Github 存储库的[数据文件夹](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data)中找到这些图像。 请注意，需根据 [LICENSE-IMAGE](https://github.com/Microsoft/Cognitive-Face-Windows/blob/master/LICENSE-IMAGE.md) 协议的许可条款使用这些图像。

### <a name="Review">查看并学习</a>

运行应用程序后，让我们了解此示例应用如何与认知服务技术集成。 这样，便可以更轻松地使用 Microsoft 计算机视觉 API 在此应用中继续生成项目，或开发自己的应用。

此示例应用利用计算机视觉 API 客户端库 - Microsoft 计算机视觉 API 的精简 C# 客户端包装。 如上所述生成示例应用时，可以从 NuGet 包获取客户端库。 可以在“视觉”>“Windows”>“客户端库”下面的标题为“客户端库”的文件夹（包含在根据前面“先决条件”所述下载的文件存储库中）中查看客户端库源代码。

还可以了解如何在解决方案资源管理器中使用客户端库代码：在“VisionAPI WPF_Samples”下面展开“AnalyzePage.xaml”，找到用于将图像提交到图像分析终结点的“AnalyzePage.xaml.cs”。 双击 .xaml.cs 文件，在 Visual Studio 的新窗口中将其打开。

让我们看看 **AnalyzePage.xaml.cs** 中的两个代码片段，了解视觉客户端库在示例应用中的用法。 该文件包含指出“KEY SAMPLE CODE STARTS HERE”和“KEY SAMPLE CODE ENDS HERE”的代码注释，可帮助找到下面重现的代码片段。

分析终结点可将图像 URL 或二进制图像数据（八位字节流格式）作为输入进行处理。 首先，查找一条可让你使用视觉客户端库的 using 指令。

```
// ----------------------------------------------------------------------
// KEY SAMPLE CODE STARTS HERE
// Use the following namespace for VisionServiceClient 
// ---------------------------------------------------------------------- 
using Microsoft.ProjectOxford.Vision; 
using Microsoft.ProjectOxford.Vision.Contract; 
// ----------------------------------------------------------------------
// KEY SAMPLE CODE ENDS HERE 
// ----------------------------------------------------------------------
```
**UploadAndAnalyzeImage(...)** 此代码片段演示如何使用客户端库将订阅密钥和本地存储的图像提交到计算机视觉 API 服务的分析终结点。

```
private async Task<AnalysisResult> UploadAndAnalyzeImage(string imageFilePath)
{
    // -----------------------------------------------------------------------
    // KEY SAMPLE CODE STARTS HERE
    // -----------------------------------------------------------------------  
    //
    // Create Project Oxford Computer Vision API Service client
    //
    VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
    Log("VisionServiceClient is created");
    
    using (Stream imageFileStream = File.OpenRead(imageFilePath))
    {
    //
    // Analyze the image for all visual features
    //
    Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
    VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
    AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageFileStream, visualFeatures);
    return analysisResult;
    }
    
    // -----------------------------------------------------------------------
    // KEY SAMPLE CODE ENDS HERE
    // -----------------------------------------------------------------------
}
```
**AnalyzeUrl(...)** 此代码片段演示如何使用客户端库将订阅密钥和照片 URL 提交到计算机视觉 API 服务的分析终结点。

```
private async Task<AnalysisResult> AnalyzeUrl(string imageUrl)
{
    // -----------------------------------------------------------------------
    // KEY SAMPLE CODE STARTS HERE
    // -----------------------------------------------------------------------
    
    //
    // Create Project Oxford Computer Vision API Service client
    //
    VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
    Log("VisionServiceClient is created");
    
    //
    // Analyze the url for all visual features
    //
    Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
    VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
    AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageUrl, visualFeatures);
    return analysisResult;
}
// -----------------------------------------------------------------------
// KEY SAMPLE CODE ENDS HERE
// -----------------------------------------------------------------------
```
**其他页面和终结点** 查看示例中的其他页面，可以了解如何与计算机视觉 API 服务公开的其他终结点交互；例如，OCR 终结点显示为 OCRPage.xaml.cs 中包含的代码的一部分 

### <a name="Related">相关主题</a>
 - [人脸 API 入门](../../Face/Tutorials/FaceAPIinCSharpTutorial.md)
 
 



