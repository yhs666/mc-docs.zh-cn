---
title: 快速入门：合成语音，C# (.NET Core) - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 Windows 上使用语音 SDK 通过 .NET Core 下的 C# 合成语音
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 06/24/2019
ms.date: 09/23/2019
ms.author: v-tawe
ms.openlocfilehash: 6615d5ce834847bb46e3609aec26227a9f6c5740
ms.sourcegitcommit: aea45739ba114a6b069f782074a70e5dded8a490
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275555"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-core"></a>快速入门：使用适用于 .NET Core 的语音 SDK 合成语音

在本文中，将使用认知服务[语音 SDK](speech-sdk.md) 为 Windows 上的 .NET Core 创建 C# 控制台应用程序。 将文本中的语音实时合成到电脑的扬声器中。 该应用程序是使用[语音 SDK NuGet 包](https://aka.ms/csspeech/nuget)和 Microsoft Visual Studio 2017 或更高版本（任何版本）生成的。

> [!NOTE]
> .NET Core 是一个实现了 [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) 规范的开源跨平台 .NET 平台。

需要有语音服务订阅密钥才能完成此快速入门。 你可以免费获得一个。 有关详细信息，请参阅[免费试用语音服务](get-started.md)。

## <a name="prerequisites"></a>先决条件

本快速入门需要：

* [.NET Core SDK](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 或更高版本
* 语音服务的 Azure 订阅密钥。 [免费获得一个](get-started.md)。

## <a name="create-a-visual-studio-project"></a>创建 Visual Studio 项目

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>添加示例代码

1. 打开 `Program.cs` 并将其中的所有代码替换为以下内容。

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.CognitiveServices.Speech;
    
    namespace helloworld
    {
        class Program
        {
            public static async Task SynthesisToSpeakerAsync()
            {
                // Creates an instance of a speech config with specified subscription key and service region.
                // Replace with your own subscription key and service region (e.g., "chinaeast2").
                var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    
                // Creates a speech synthesizer using the default speaker as audio output.
                using (var synthesizer = new SpeechSynthesizer(config))
                {
                    // Receive a text from console input and synthesize it to speaker.
                    Console.WriteLine("Type some text that you want to speak...");
                    Console.Write("> ");
                    string text = Console.ReadLine();
    
                    using (var result = await synthesizer.SpeakTextAsync(text))
                    {
                        if (result.Reason == ResultReason.SynthesizingAudioCompleted)
                        {
                            Console.WriteLine($"Speech synthesized to speaker for text [{text}]");
                        }
                        else if (result.Reason == ResultReason.Canceled)
                        {
                            var cancellation = SpeechSynthesisCancellationDetails.FromResult(result);
                            Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");
    
                            if (cancellation.Reason == CancellationReason.Error)
                            {
                                Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                                Console.WriteLine($"CANCELED: ErrorDetails=[{cancellation.ErrorDetails}]");
                                Console.WriteLine($"CANCELED: Did you update the subscription info?");
                            }
                        }
                    }
                }
            }
    
            static void Main()
            {
                SynthesisToSpeakerAsync().Wait();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }
        }
    }
    ```
1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 另请将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](regions.md)。

1. 保存对项目的更改。

## <a name="build-and-run-the-app"></a>生成并运行应用

1. 构建应用程序。 从菜单栏中，选择“构建” > “构建解决方案”   。 编译代码时应不会出错。

    ![Visual Studio 应用程序的屏幕截图，其中突出显示了“生成解决方案”选项](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "成功生成")

1. 启动应用程序。 在菜单栏中，选择“调试” > “开始调试”，或按 F5    。

    ![Visual Studio 应用程序的屏幕截图，其中突出显示了“启动调试”选项](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "启动应用进入调试")

1. 此时会显示控制台窗口，提示你键入一些文本。 键入几个单词或一个句子。 键入的文本将传输到语音服务，并合成为语音，在扬声器上播放。

    ![成功合成后的控制台输出的屏幕截图](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "成功合成后的控制台输出")

## <a name="next-steps"></a>后续步骤

GitHub 上提供了其他示例，例如如何将语音合成到音频文件。

> [!div class="nextstepaction"]
> [浏览 GitHub 上的 C# 示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="see-also"></a>另请参阅

- [自定义语音字体](how-to-custom-voice-create-voice.md)
- [录制语音示例](record-custom-voice-samples.md)
