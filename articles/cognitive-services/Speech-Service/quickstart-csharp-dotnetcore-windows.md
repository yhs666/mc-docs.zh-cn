---
title: 快速入门：识别语音，C# (.NET Core Windows) - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 Windows 上使用语音 SDK 通过 .NET Core 下的 C# 识别语音
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 12/13/2018
ms.date: 04/01/2019
ms.author: v-biyu
ms.openlocfilehash: 4347a20e2d926b8a52a96e39e4f131ca66ec4e4b
ms.sourcegitcommit: edce097f471b6e9427718f0641ee2b421e3c0ed2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348400"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-core"></a>快速入门：使用适用于 .NET Core 的语音 SDK 识别语音

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

在本文中，将使用认知服务[语音 SDK](speech-sdk.md) 为 Windows 上的 .NET Core 创建 C# 控制台应用程序。 可以通过电脑的麦克风实时将语音转录为文本。 该应用程序是使用[语音 SDK NuGet 包](https://aka.ms/csspeech/nuget)和 Microsoft Visual Studio 2017（任何版本）构建的。

> [!NOTE]
> .NET Core 是一个实现了 [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) 规范的开源跨平台 .NET 平台。

需要具有语音服务订阅密钥才能完成此快速入门。 你可以免费获得一个。 有关详细信息，请参阅[试用语音服务](get-started.md)。

## <a name="prerequisites"></a>先决条件

本快速入门需要：

* [.NET Core SDK](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* 语音服务的 Azure 订阅密钥。 [获取一个试用版](get-started.md)。

## <a name="create-a-visual-studio-project"></a>创建 Visual Studio 项目

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>添加示例代码

1. 打开 `Program.cs` 并将其中的所有代码替换为以下内容。

```C#
using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace helloworld
{
    class Program
    {
        public static async Task RecognizeSpeechAsync()
        {
            // Creates an instance of a speech config with specified subscription key and service region.
            // Replace with your own subscription key // and service region (e.g., "chinanorth").
            var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

            // Creates a speech recognizer.
            using (var recognizer = new SpeechRecognizer(config))
            {
                Console.WriteLine("Say something...");

                // Starts speech recognition, and returns after a single utterance is recognized. The end of a
                // single utterance is determined by listening for silence at the end or until a maximum of 15
                // seconds of audio is processed.  The task returns the recognition text as result. 
                // Note: Since RecognizeOnceAsync() returns only a single utterance, it is suitable only for single
                // shot recognition like command or query. 
                // For long-running multi-utterance recognition, use StartContinuousRecognitionAsync() instead.
                var result = await recognizer.RecognizeOnceAsync();

                // Checks result.
                if (result.Reason == ResultReason.RecognizedSpeech)
                {
                    Console.WriteLine($"We recognized: {result.Text}");
                }
                else if (result.Reason == ResultReason.NoMatch)
                {
                    Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                }
                else if (result.Reason == ResultReason.Canceled)
                {
                    var cancellation = CancellationDetails.FromResult(result);
                    Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

                    if (cancellation.Reason == CancellationReason.Error)
                    {
                        Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                        Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
                        Console.WriteLine($"CANCELED: Did you update the subscription info?");
                    }
                }
            }
        }

        static void Main()
        {
            RecognizeSpeechAsync().Wait();
            Console.WriteLine("Please press a key to continue.");
            Console.ReadLine();
        }
    }
}
```

1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 另请将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](regions.md) 

1. 保存对项目的更改。

## <a name="build-and-run-the-app"></a>生成并运行应用

1. 构建应用程序。 从菜单栏中，选择“构建” > “构建解决方案”。 编译代码时应不会出错。

    ![Visual Studio 应用程序的屏幕截图，其中突出显示了“生成解决方案”选项](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "成功生成")

1. 启动应用程序。 在菜单栏中，选择“调试” > “开始调试”，或按 F5。

    ![Visual Studio 应用程序的屏幕截图，其中突出显示了“启动调试”选项](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "启动应用进入调试")

1. 此时将显示控制台窗口，提示你说出任意内容。 说一个英语短语或句子。 你的语音将传输到语音服务并转录为文本，该文本将显示在同一窗口中。

    ![成功识别后的控制台输出的屏幕截图](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "成功识别后的控制台输出")

## <a name="next-steps"></a>后续步骤

GitHub 上提供了其他示例，例如如何从音频文件中读取语音。

> [!div class="nextstepaction"]
> [浏览 GitHub 上的 C# 示例](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>另请参阅

- [自定义声学模型](how-to-customize-acoustic-models.md)
- [自定义语言模型](how-to-customize-language-model.md)
