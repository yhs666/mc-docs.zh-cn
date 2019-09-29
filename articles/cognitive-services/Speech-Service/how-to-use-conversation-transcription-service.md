---
title: 使用语音 SDK 听录多参与者对话 - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何通过语音 SDK 使用对话听录。 适用于 C++、C# 和 Java。
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 09/23/2019
ms.author: v-tawe
ms.openlocfilehash: 15baecb7ae8057d487a76f8959e22a363525c1ed
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267101"
---
# <a name="transcribe-multi-participant-conversations-with-the-speech-sdk"></a>使用语音 SDK 听录多参与者对话

语音 SDK 的**对话听录器** API 使你能够听录会议/会话，并能够通过使用 `PullStream` 或 `PushStream` 将音频流式传输到语音服务来添加、删除和识别参与者。

## <a name="limitations"></a>限制

* Windows、Linux 和 Android 上的 C++、C# 和 Java 支持对话听录器。
* ROOBO DevKit 是用于创建对话听录的受支持硬件环境，因为它提供了可以有效地用于说话人识别的环形多麦克风阵列。 [有关详细信息，请参阅“语音设备 SDK”](speech-devices-sdk.md)。
* 语音 SDK 对对话听录的支持仅限于具有 8 个声道的 16 位 16 kHz PCM 音频的音频拉取和推送模式流。 目前，仅支持使用以下工具包进行 8 声道音频捕获：
   * [ROOBO Smart Audio Circular 7-Mic DK](https://ddk.roobo.com/)
* 对话听录目前以“en-US”和“zh-CN”语言提供。

## <a name="prerequisites"></a>先决条件

* [获取语音试用订阅。](https://www.azure.cn/home/features/cognitive-services/)
* 需要语音 SDK 版本 1.5.1 或更高版本。

## <a name="create-voice-signatures-for-participants"></a>为参与者创建语音签名

第一步是为对话参与者创建语音签名。 创建语音签名是有效识别说话人所必需的。

### <a name="requirements-for-input-wave-file"></a>输入波形文件的要求

* 用于创建语音签名的输入音频波形文件应采用 16 位样本、16 kHz 采样率和单声道 (Mono) 格式。
* 每个音频样本的建议长度为 30 秒到 2 分钟。

以下示例展示了如何[使用 C# 的 REST API](https://signature.centralus.cts.speech.microsoft.com/UI/index.html) 通过两种不同方式创建语音签名：

```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static void Main(string[] args)
    {
        CreateVoiceSignatureByUsingFormData().Wait();
        CreateVoiceSignatureByUsingBody().Wait();
    }
}
```

## <a name="transcribing-conversations"></a>听录对话

若要听录与多个参与者的对话，请创建与为对话会话创建的 `AudioConfig` 对象关联的 `ConversationTranscriber` 对象，并使用 `PullAudioInputStream` 或 `PushAudioInputStream` 流式传输音频。

假设你有一个名为 `MyConversationTranscriber` 的 ConversationTranscriber 类。 你的代码可能如下所示：

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Conversation;

public class MyConversationTranscriber
{
    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own subscription key and region.
        var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
        var stopTranscription = new TaskCompletionSource<int>();

        // Create an audio stream from a wav file.
        // Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
        using (var audioInput = Helper.OpenWavFile(@"8channelsOfRecordedPCMAudio.wav"))
        {
            // Creates a conversation transcriber using audio stream input.
            using (var transcriber = new ConversationTranscriber(config, audioInput))
            {
                // Subscribes to events.
                transcriber.Recognizing += (s, e) =>
                {
                    Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
                };

                transcriber.Recognized += (s, e) =>
                {
                    if (e.Result.Reason == ResultReason.RecognizedSpeech)
                    {
                        Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}, UserID={e.Result.UserId}");
                    }
                    else if (e.Result.Reason == ResultReason.NoMatch)
                    {
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                    }
                };

                transcriber.Canceled += (s, e) =>
                {
                    Console.WriteLine($"CANCELED: Reason={e.Reason}");

                    if (e.Reason == CancellationReason.Error)
                    {
                        Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
                        Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
                        Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        stopTranscription.TrySetResult(0);
                    }
                };

                transcriber.SessionStarted += (s, e) =>
                {
                    Console.WriteLine("\nSession started event.");
                };

                transcriber.SessionStopped += (s, e) =>
                {
                    Console.WriteLine("\nSession stopped event.");
                    Console.WriteLine("\nStop recognition.");
                    stopTranscription.TrySetResult(0);
                };

                // Sets a conversation Id.
                transcriber.ConversationId = "AConversationFromTeams";

                // Add participants to the conversation.
                // Create voice signatures using REST API described in the earlier section in this document.
                // Voice signature needs to be in the following format:
                // { "Version": <Numeric value>, "Tag": "string", "Data": "string" }

                var speakerA = Participant.From("Speaker_A", "en-us", signatureA);
                var speakerB = Participant.From("Speaker_B", "en-us", signatureB);
                var speakerC = Participant.From("SPeaker_C", "en-us", signatureC);
                transcriber.AddParticipant(speakerA);
                transcriber.AddParticipant(speakerB);
                transcriber.AddParticipant(speakerC);

                // Starts transcribing of the conversation. Uses StopTranscribingAsync() to stop transcribing when all participants leave.
                await transcriber.StartTranscribingAsync().ConfigureAwait(false);

                // Waits for completion.
                // Use Task.WaitAny to keep the task rooted.
                Task.WaitAny(new[] { stopTranscription.Task });

                // Stop transcribing the conversation.
                await transcriber.StopTranscribingAsync().ConfigureAwait(false);

                // Ends the conversation.
                await transcriber.EndConversationAsync().ConfigureAwait(false);
            }
        }
    }
}
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [浏览 GitHub 上的示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
