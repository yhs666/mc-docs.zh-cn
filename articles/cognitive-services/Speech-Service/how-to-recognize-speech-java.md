---
title: 使用适用于 Java 的语音 SDK 识别语音
titleSuffix: Azure Cognitive Services
description: 了解如何使用适用于 Java 的语音 SDK 识别语音（从文件、从麦克风、使用自定义模型、连续或单次）。
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 09/24/2018
ms.date: 04/01/2019
ms.author: v-biyu
ms.openlocfilehash: 559b35101c06da57579d5b05304743380b0a7d4a
ms.sourcegitcommit: edce097f471b6e9427718f0641ee2b421e3c0ed2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348452"
---
# <a name="recognize-speech-by-using-the-speech-sdk-for-java"></a>使用适用于 Java 的语音 SDK 识别语音

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-intro.md)]

[!INCLUDE [Introduction for top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

```Java
import com.microsoft.cognitiveservices.speech.*;
import com.microsoft.cognitiveservices.speech.audio.*;
```

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-microphone.md)]

```Java
// Creates an instance of a speech config with specified
// subscription key and service region. Replace with your own subscription key
// and service region.
// The default language is "en-us".
SpeechConfig config = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Creates a speech recognizer using microphone as audio input.
SpeechRecognizer recognizer = new SpeechRecognizer(config);
{
    // Starts recognizing.
    System.out.println("Say something...");

    // Starts recognition. It returns when the first utterance has been recognized.
    SpeechRecognitionResult result = recognizer.recognizeOnceAsync().get();

    // Checks result.
    if (result.getReason() == ResultReason.RecognizedSpeech) {
        System.out.println("RECOGNIZED: Text=" + result.getText());
    }
    else if (result.getReason() == ResultReason.NoMatch) {
        System.out.println("NOMATCH: Speech could not be recognized.");
    }
    else if (result.getReason() == ResultReason.Canceled) {
        CancellationDetails cancellation = CancellationDetails.fromResult(result);
        System.out.println("CANCELED: Reason=" + cancellation.getReason());

        if (cancellation.getReason() == CancellationReason.Error) {
            System.out.println("CANCELED: ErrorCode=" + cancellation.getErrorCode());
            System.out.println("CANCELED: ErrorDetails=" + cancellation.getErrorDetails());
            System.out.println("CANCELED: Did you update the subscription info?");
        }
    }
}
recognizer.close();
```

[!INCLUDE [Introduction to using customized recognition](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-customized.md)]

```Java
// Creates an instance of a speech config with specified
// subscription key and service region. Replace with your own subscription key
// and service region.
SpeechConfig config = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
// Replace with the CRIS endpoint id of your customized model.
config.setEndpointId("YourEndpointId");

// Creates a speech recognizer using microphone as audio input.
SpeechRecognizer recognizer = new SpeechRecognizer(config);
{
    // Starts recognizing.
    System.out.println("Say something...");

    // Starts recognition. It returns when the first utterance has been recognized.
    SpeechRecognitionResult result = recognizer.recognizeOnceAsync().get();

    // Checks result.
    if (result.getReason() == ResultReason.RecognizedSpeech) {
        System.out.println("RECOGNIZED: Text=" + result.getText());
    }
    else if (result.getReason() == ResultReason.NoMatch) {
        System.out.println("NOMATCH: Speech could not be recognized.");
    }
    else if (result.getReason() == ResultReason.Canceled) {
        CancellationDetails cancellation = CancellationDetails.fromResult(result);
        System.out.println("CANCELED: Reason=" + cancellation.getReason());

        if (cancellation.getReason() == CancellationReason.Error) {
            System.out.println("CANCELED: ErrorCode=" + cancellation.getErrorCode());
            System.out.println("CANCELED: ErrorDetails=" + cancellation.getErrorDetails());
            System.out.println("CANCELED: Did you update the subscription info?");
        }
    }
}

recognizer.close();
```

[!INCLUDE [Introduction to using a continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-continuous.md)]

```Java
// Creates an instance of a speech config with specified
// subscription key and service region. Replace with your own subscription key
// and service region .
SpeechConfig config = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Creates a speech recognizer using file as audio input.
// Replace with your own audio file name.
AudioConfig audioInput = AudioConfig.fromWavFileInput("YourAudioFile.wav");
SpeechRecognizer recognizer = new SpeechRecognizer(config, audioInput);
{
    // Subscribes to events.
    recognizer.recognizing.addEventListener((s, e) -> {
        System.out.println("RECOGNIZING: Text=" + e.getResult().getText());
    });

    recognizer.recognized.addEventListener((s, e) -> {
        if (e.getResult().getReason() == ResultReason.RecognizedSpeech) {
            System.out.println("RECOGNIZED: Text=" + e.getResult().getText());
        }
        else if (e.getResult().getReason() == ResultReason.NoMatch) {
            System.out.println("NOMATCH: Speech could not be recognized.");
        }
    });

    recognizer.canceled.addEventListener((s, e) -> {
        System.out.println("CANCELED: Reason=" + e.getReason());

        if (e.getReason() == CancellationReason.Error) {
            System.out.println("CANCELED: ErrorCode=" + e.getErrorCode());
            System.out.println("CANCELED: ErrorDetails=" + e.getErrorDetails());
            System.out.println("CANCELED: Did you update the subscription info?");
        }
    });

    recognizer.sessionStarted.addEventListener((s, e) -> {
        System.out.println("\n    Session started event.");
    });

    recognizer.sessionStopped.addEventListener((s, e) -> {
        System.out.println("\n    Session stopped event.");
    });

    // Starts continuous recognition. Uses stopContinuousRecognitionAsync() to stop recognition.
    System.out.println("Say something...");
    recognizer.startContinuousRecognitionAsync().get();

    System.out.println("Press any key to stop");
    new Scanner(System.in).nextLine();

    recognizer.stopContinuousRecognitionAsync().get();
}
```

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]


## <a name="next-steps"></a>后续步骤

- [如何转换语音](how-to-translate-speech-java.md)

