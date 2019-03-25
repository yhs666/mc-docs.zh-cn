---
title: 使用适用于 C++ 的语音 SDK 识别语音
titleSuffix: Azure Cognitive Services
description: 了解如何使用适用于 C++ 的语音 SDK 识别语音（从文件、从麦克风、使用自定义模型、连续或单次）。
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 09/24/2018
ms.date: 04/01/2019
ms.author: v-biyu
ms.openlocfilehash: d4a4164997339204c04fa27097c319b5258d440d
ms.sourcegitcommit: edce097f471b6e9427718f0641ee2b421e3c0ed2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348239"
---
# <a name="recognize-speech-by-using-the-speech-sdk-for-c"></a>使用适用于 C++ 的语音 SDK 识别语音

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-intro.md)]

[!INCLUDE [Introduction for top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

```C++
#include <speechapi_cxx.h>
#include <fstream>
#include "wav_file_reader.h"

using namespace std;
using namespace Microsoft::CognitiveServices::Speech;
using namespace Microsoft::CognitiveServices::Speech::Audio;
```

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-microphone.md)]

```C++
// Creates an instance of a speech config with specified subscription key and service region.
// Replace with your own subscription key and service region.
auto config = SpeechConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Creates a speech recognizer using microphone as audio input. The default language is "en-us".
auto recognizer = SpeechRecognizer::FromConfig(config);
cout << "Say something...\n";

// Starts speech recognition, and returns after a single utterance is recognized. The end of a
// single utterance is determined by listening for silence at the end or until a maximum of 15
// seconds of audio is processed.  The task returns the recognition text as result.
// Note: Since RecognizeOnceAsync() returns only a single utterance, it is suitable only for single
// shot recognition like command or query.
// For long-running multi-utterance recognition, use StartContinuousRecognitionAsync() instead.
auto result = recognizer->RecognizeOnceAsync().get();

// Checks result.
if (result->Reason == ResultReason::RecognizedSpeech)
{
    cout << "RECOGNIZED: Text=" << result->Text << std::endl;
}
else if (result->Reason == ResultReason::NoMatch)
{
    cout << "NOMATCH: Speech could not be recognized." << std::endl;
}
else if (result->Reason == ResultReason::Canceled)
{
    auto cancellation = CancellationDetails::FromResult(result);
    cout << "CANCELED: Reason=" << (int)cancellation->Reason << std::endl;

    if (cancellation->Reason == CancellationReason::Error)
    {
        cout << "CANCELED: ErrorCode=" << (int)cancellation->ErrorCode << std::endl;
        cout << "CANCELED: ErrorDetails=" << cancellation->ErrorDetails << std::endl;
        cout << "CANCELED: Did you update the subscription info?" << std::endl;
    }
}
```

[!INCLUDE [Introduction to using customized recognition](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-customized.md)]

```C++
// Creates an instance of a speech config with specified subscription key and service region.
// Replace with your own subscription key and service region.
auto config = SpeechConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");
// Set the endpoint ID of your customized model
// Replace with your own CRIS endpoint ID.
config->SetEndpointId("YourEndpointId");

// Creates a speech recognizer using microphone as audio input.
auto recognizer = SpeechRecognizer::FromConfig(config);

cout << "Say something...\n";

// Starts speech recognition, and returns after a single utterance is recognized. The end of a
// single utterance is determined by listening for silence at the end or until a maximum of 15
// seconds of audio is processed.  The task returns the recognition text as result. 
// Note: Since RecognizeOnceAsync() returns only a single utterance, it is suitable only for single
// shot recognition like command or query. 
// For long-running multi-utterance recognition, use StartContinuousRecognitionAsync() instead.
auto result = recognizer->RecognizeOnceAsync().get();

// Checks result.
if (result->Reason == ResultReason::RecognizedSpeech)
{
    cout << "RECOGNIZED: Text=" << result->Text << std::endl;
}
else if (result->Reason == ResultReason::NoMatch)
{
    cout << "NOMATCH: Speech could not be recognized." << std::endl;
}
else if (result->Reason == ResultReason::Canceled)
{
    auto cancellation = CancellationDetails::FromResult(result);
    cout << "CANCELED: Reason=" << (int)cancellation->Reason << std::endl;

    if (cancellation->Reason == CancellationReason::Error)
    {
        cout << "CANCELED: ErrorCode=" << (int)cancellation->ErrorCode << std::endl;
        cout << "CANCELED: ErrorDetails=" << cancellation->ErrorDetails << std::endl;
        cout << "CANCELED: Did you update the subscription info?" << std::endl;
    }
}
```

[!INCLUDE [Introduction to using a continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-continuous.md)]

```C++
// Creates an instance of a speech config with specified subscription key and service region.
// Replace with your own subscription key and service region.
auto config = SpeechConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Creates a speech recognizer using file as audio input.
// Replace with your own audio file name.
auto audioInput = AudioConfig::FromWavFileInput("whatstheweatherlike.wav");
auto recognizer = SpeechRecognizer::FromConfig(config, audioInput);

// promise for synchronization of recognition end.
promise<void> recognitionEnd;

// Subscribes to events.
recognizer->Recognizing.Connect([] (const SpeechRecognitionEventArgs& e)
{
    cout << "Recognizing:" << e.Result->Text << std::endl;
});

recognizer->Recognized.Connect([] (const SpeechRecognitionEventArgs& e)
{
    if (e.Result->Reason == ResultReason::RecognizedSpeech)
    {
        cout << "RECOGNIZED: Text=" << e.Result->Text << "\n"
             << "  Offset=" << e.Result->Offset() << "\n"
             << "  Duration=" << e.Result->Duration() << std::endl;
    }
    else if (e.Result->Reason == ResultReason::NoMatch)
    {
        cout << "NOMATCH: Speech could not be recognized." << std::endl;
    }
});

recognizer->Canceled.Connect([&recognitionEnd](const SpeechRecognitionCanceledEventArgs& e)
{
    cout << "CANCELED: Reason=" << (int)e.Reason << std::endl;

    if (e.Reason == CancellationReason::Error)
    {
        cout << "CANCELED: ErrorCode=" << (int)e.ErrorCode << "\n"
             << "CANCELED: ErrorDetails=" << e.ErrorDetails << "\n"
             << "CANCELED: Did you update the subscription info?" << std::endl;
    }
    recognitionEnd.set_value(); // Notify to stop recognition.
});

recognizer->SessionStopped.Connect([&recognitionEnd](const SessionEventArgs& e)
{
    cout << "Session stopped.";
    recognitionEnd.set_value(); // Notify to stop recognition.
});

// Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
recognizer->StartContinuousRecognitionAsync().get();

// Waits for recognition end.
recognitionEnd.get_future().get();

// Stops recognition.
recognizer->StopContinuousRecognitionAsync().get();
```

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]


## <a name="next-steps"></a>后续步骤

- [如何转换语音](how-to-translate-speech-cpp.md)

