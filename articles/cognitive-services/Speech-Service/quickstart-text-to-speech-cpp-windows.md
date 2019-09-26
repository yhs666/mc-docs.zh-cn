---
title: 快速入门：合成语音，C++ (Windows) - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 Windows 桌面上使用语音 SDK 通过 C++ 合成语音
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 08/24/2019
ms.date: 09/23/2019
ms.author: v-tawe
ms.openlocfilehash: 44fba30b5a82438fd45b766967243ba4a498a207
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267113"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>快速入门：使用语音 SDK 在 Windows 上的 C++ 中合成语音

在本文中，请创建适用于 Windows 的 C++ 控制台应用程序。 使用认知服务[语音 SDK](speech-sdk.md) 实时从文本合成语音，并在电脑的扬声器上播放语音。 该应用程序是使用[语音 SDK NuGet 包](https://aka.ms/csspeech/nuget)和 Microsoft Visual Studio 2019（任何版本）构建的。

有关可用于语音合成的语言/语音的完整列表，请参阅[语言支持](language-support.md#text-to-speech)。

## <a name="prerequisites"></a>先决条件

需要有语音服务订阅密钥才能完成此快速入门。 你可以免费获得一个。 有关详细信息，请参阅[免费试用语音服务](get-started.md)。

## <a name="create-a-visual-studio-project"></a>创建 Visual Studio 项目

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>添加示例代码

1. 打开源文件 **helloworld.cpp**。

1. 将所有代码替换为以下片段：

    ```cpp
    using UnityEngine;
    using UnityEngine.UI;
    using Microsoft.CognitiveServices.Speech;

    public class HelloWorld : MonoBehaviour
    {
        // Hook up the three properties below with a Text, InputField and Button object in your UI.
        public Text outputText;
        public InputField inputField;
        public Button speakButton;
        public AudioSource audioSource;
    
        private object threadLocker = new object();
        private bool waitingForSpeak;
        private string message;
    
        public void ButtonClick()
        {
            // Creates an instance of a speech config with specified subscription key and service region.
            // Replace with your own subscription key and service region (e.g., "westus").
            var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    
            // Creates a speech synthesizer.
            // Make sure to dispose the synthesizer after use!
            using (var synthsizer = new SpeechSynthesizer(config, null))
            {
                lock (threadLocker)
                {
                    waitingForSpeak = true;
                }
    
                // Starts speech synthesis, and returns after a single utterance is synthesized.
                var result = synthsizer.SpeakTextAsync(inputField.text).Result;
    
                // Checks result.
                string newMessage = string.Empty;
                if (result.Reason == ResultReason.SynthesizingAudioCompleted)
                {
                    // Since native playback is not yet supported on Unity yet (currently only supported on Windows/Linux Desktop),
                    // use the Unity API to play audio here as a short term solution.
                    // Native playback support will be added in the future release.
                    var sampleCount = result.AudioData.Length / 2;
                    var audioData = new float[sampleCount];
                    for (var i = 0; i < sampleCount; ++i)
                    {
                        audioData[i] = (short)(result.AudioData[i * 2 + 1] << 8 | result.AudioData[i * 2]) / 32768.0F;
                    }
    
                    // The default output audio format is 16K 16bit mono
                    var audioClip = AudioClip.Create("SynthesizedAudio", sampleCount, 1, 16000, false);
                    audioClip.SetData(audioData, 0);
                    audioSource.clip = audioClip;
                    audioSource.Play();
    
                    newMessage = "Speech synthesis succeeded!";
                }
                else if (result.Reason == ResultReason.Canceled)
                {
                    var cancellation = SpeechSynthesisCancellationDetails.FromResult(result);
                    newMessage = $"CANCELED:\nReason=[{cancellation.Reason}]\nErrorDetails=[{cancellation.ErrorDetails}]\nDid you update the subscription info?";
                }
    
                lock (threadLocker)
                {
                    message = newMessage;
                    waitingForSpeak = false;
                }
            }
        }
    
        void Start()
        {
            if (outputText == null)
            {
                UnityEngine.Debug.LogError("outputText property is null! Assign a UI Text element to it.");
            }
            else if (inputField == null)
            {
                message = "inputField property is null! Assign a UI InputField element to it.";
                UnityEngine.Debug.LogError(message);
            }
            else if (speakButton == null)
            {
                message = "speakButton property is null! Assign a UI Button to it.";
                UnityEngine.Debug.LogError(message);
            }
            else
            {
                // Continue with normal initialization, Text, InputField and Button objects are present.
                inputField.text = "Enter text you wish spoken here.";
                message = "Click button to synthesize speech";
                speakButton.onClick.AddListener(ButtonClick);
            }
        }
    
        void Update()
        {
            lock (threadLocker)
            {
                if (speakButton != null)
                {
                    speakButton.interactable = !waitingForSpeak;
                }
    
                if (outputText != null)
                {
                    outputText.text = message;
                }
            }
        }
    }
    ```

1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](regions.md)。

1. 在菜单栏中，选择“文件”   > “全部保存”  。

## <a name="build-and-run-the-application"></a>生成并运行应用程序

1. 从菜单栏中，选择“构建”   > “构建解决方案”  以构建应用程序。 现在，编译代码时应不会提示错误。

1. 选择“调试”   > “开始调试”  （或按 F5  ）以启动 helloworld  应用程序。

1. 键入一个英语短语或句子。 应用程序将你的文本传输到语音服务，该服务会将合成的语音发送到应用程序以在你的扬声器上播放。

   ![成功语音合成后的控制台输出](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>后续步骤

GitHub 上提供了其他示例，例如如何将语音保存到音频文件。

> [!div class="nextstepaction"]
> [浏览 GitHub 上的 C++ 示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="see-also"></a>另请参阅

- [创建自定义语音](how-to-custom-voice-create-voice.md)
- [录制自定义语音示例](record-custom-voice-samples.md)
