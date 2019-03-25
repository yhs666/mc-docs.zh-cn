---
title: 使用适用于 C# 的语音 SDK 转换语音
titleSuffix: Azure Cognitive Services
description: 本文包含在 C# 环境中使用语音 SDK 翻译语音的示例代码。
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 12/06/2018
ms.date: 04/01/2019
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: adf03ad4d67cedd183be623c217e8c79ba3fbf67
ms.sourcegitcommit: edce097f471b6e9427718f0641ee2b421e3c0ed2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348324"
---
# <a name="translate-speech-with-the-cognitive-services-speech-sdk-for-c"></a>使用适用于 C# 的认知服务语音 SDK 翻译语音

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-translate-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-translate-speech-intro.md)]

[!INCLUDE [Introduction to top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

```C#
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Translation;
#if NET461
using System.Media;
#endif
```

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-translate-speech-microphone.md)]

```C#
// Translation source language.
// Replace with a language of your choice.
string fromLanguage = "en-US";

// Voice name of synthesis output.
const string GermanVoice = "Microsoft Server Speech Text to Speech Voice (de-DE, Hedda)";

// Creates an instance of a speech translation config with specified subscription key and service region.
// Replace with your own subscription key and service region.
var config = SpeechTranslationConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
config.SpeechRecognitionLanguage = fromLanguage;
config.VoiceName = GermanVoice;

// Translation target language(s).
// Replace with language(s) of your choice.
config.AddTargetLanguage("de");

// Creates a translation recognizer using microphone as audio input.
using (var recognizer = new TranslationRecognizer(config))
{
    // Subscribes to events.
    recognizer.Recognizing += (s, e) =>
    {
        Console.WriteLine($"RECOGNIZING in '{fromLanguage}': Text={e.Result.Text}");
        foreach (var element in e.Result.Translations)
        {
            Console.WriteLine($"    TRANSLATING into '{element.Key}': {element.Value}");
        }
    };

    recognizer.Recognized += (s, e) =>
    {
        if (e.Result.Reason == ResultReason.TranslatedSpeech)
        {
            Console.WriteLine($"RECOGNIZED in '{fromLanguage}': Text={e.Result.Text}");
            foreach (var element in e.Result.Translations)
            {
                Console.WriteLine($"    TRANSLATED into '{element.Key}': {element.Value}");
            }
        }
        else if (e.Result.Reason == ResultReason.RecognizedSpeech)
        {
            Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}");
            Console.WriteLine($"    Speech not translated.");
        }
        else if (e.Result.Reason == ResultReason.NoMatch)
        {
            Console.WriteLine($"NOMATCH: Speech could not be recognized.");
        }
    };

    recognizer.Synthesizing += (s, e) =>
    {
        var audio = e.Result.GetAudio();
        Console.WriteLine(audio.Length != 0
            ? $"AudioSize: {audio.Length}"
            : $"AudioSize: {audio.Length} (end of synthesis data)");

        if (audio.Length > 0)
        {
            #if NET461
            using (var m = new MemoryStream(audio))
            {
                SoundPlayer simpleSound = new SoundPlayer(m);
                simpleSound.PlaySync();
            }
            #endif
        }
    };

    recognizer.Canceled += (s, e) =>
    {
        Console.WriteLine($"CANCELED: Reason={e.Reason}");

        if (e.Reason == CancellationReason.Error)
        {
            Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
            Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
            Console.WriteLine($"CANCELED: Did you update the subscription info?");
        }
    };

    recognizer.SessionStarted += (s, e) =>
    {
        Console.WriteLine("\nSession started event.");
    };

    recognizer.SessionStopped += (s, e) =>
    {
        Console.WriteLine("\nSession stopped event.");
    };

    // Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
    Console.WriteLine("Say something...");
    await recognizer.StartContinuousRecognitionAsync().ConfigureAwait(false);

    do
    {
        Console.WriteLine("Press Enter to stop");
    } while (Console.ReadKey().Key != ConsoleKey.Enter);

    // Stops continuous recognition.
    await recognizer.StopContinuousRecognitionAsync().ConfigureAwait(false);
}
```

[!INCLUDE [Introduction to using a file](../../../includes/cognitive-services-speech-service-how-to-translate-speech-file.md)]

```C#
// Translation source language.
// Replace with a language of your choice.
string fromLanguage = "en-US";

// Creates an instance of a speech translation config with specified subscription key and service region.
// Replace with your own subscription key and service region.
var config = SpeechTranslationConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
config.SpeechRecognitionLanguage = fromLanguage;

// Translation target language(s).
// Replace with language(s) of your choice.
config.AddTargetLanguage("de");
config.AddTargetLanguage("fr");

var stopTranslation = new TaskCompletionSource<int>();

// Creates a translation recognizer using file as audio input.
// Replace with your own audio file name.
using (var audioInput = AudioConfig.FromWavFileInput(@"whatstheweatherlike.wav"))
{
    using (var recognizer = new TranslationRecognizer(config, audioInput))
    {
        // Subscribes to events.
        recognizer.Recognizing += (s, e) =>
        {
            Console.WriteLine($"RECOGNIZING in '{fromLanguage}': Text={e.Result.Text}");
            foreach (var element in e.Result.Translations)
            {
                Console.WriteLine($"    TRANSLATING into '{element.Key}': {element.Value}");
            }
        };

        recognizer.Recognized += (s, e) => {
            if (e.Result.Reason == ResultReason.TranslatedSpeech)
            {
                Console.WriteLine($"RECOGNIZED in '{fromLanguage}': Text={e.Result.Text}");
                foreach (var element in e.Result.Translations)
                {
                    Console.WriteLine($"    TRANSLATED into '{element.Key}': {element.Value}");
                }
            }
            else if (e.Result.Reason == ResultReason.RecognizedSpeech)
            {
                Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}");
                Console.WriteLine($"    Speech not translated.");
            }
            else if (e.Result.Reason == ResultReason.NoMatch)
            {
                Console.WriteLine($"NOMATCH: Speech could not be recognized.");
            }
        };

        recognizer.Canceled += (s, e) =>
        {
            Console.WriteLine($"CANCELED: Reason={e.Reason}");

            if (e.Reason == CancellationReason.Error)
            {
                Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
                Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
                Console.WriteLine($"CANCELED: Did you update the subscription info?");
            }

            stopTranslation.TrySetResult(0);
        };

        recognizer.SpeechStartDetected += (s, e) => {
            Console.WriteLine("\nSpeech start detected event.");
        };

        recognizer.SpeechEndDetected += (s, e) => {
            Console.WriteLine("\nSpeech end detected event.");
        };

        recognizer.SessionStarted += (s, e) => {
            Console.WriteLine("\nSession started event.");
        };

        recognizer.SessionStopped += (s, e) => {
            Console.WriteLine("\nSession stopped event.");
            Console.WriteLine($"\nStop translation.");
            stopTranslation.TrySetResult(0);
        };

        // Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
        Console.WriteLine("Start translation...");
        await recognizer.StartContinuousRecognitionAsync().ConfigureAwait(false);

        // Waits for completion.
        // Use Task.WaitAny to keep the task rooted.
        Task.WaitAny(new[] { stopTranslation.Task });

        // Stops translation.
        await recognizer.StopContinuousRecognitionAsync().ConfigureAwait(false);
    }
}
```

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>后续步骤

- [如何识别语音](how-to-recognize-speech-csharp.md)

