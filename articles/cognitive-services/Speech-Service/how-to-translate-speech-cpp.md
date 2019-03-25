---
title: 使用适用于 C++ 的语音 SDK 转换语音
titleSuffix: Azure Cognitive Services
description: 本文包含在 C++ 环境中使用语音 SDK 翻译语音的示例代码。
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
ms.openlocfilehash: f68520b6d35a5b67a9e5047df605fc1aa2df65e7
ms.sourcegitcommit: edce097f471b6e9427718f0641ee2b421e3c0ed2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348476"
---
# <a name="translate-speech-with-the-cognitive-services-speech-sdk-for-c"></a>使用适用于 C++ 的认知服务语音 SDK 翻译语音

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-translate-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-translate-speech-intro.md)]

[!INCLUDE [Introduction to top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

```C++
#include <string>
#include <vector>
#include <speechapi_cxx.h>

using namespace std;
using namespace Microsoft::CognitiveServices::Speech;
using namespace Microsoft::CognitiveServices::Speech::Translation;
```

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-translate-speech-microphone.md)]

```C++
// Creates an instance of a speech translation config with specified subscription key and service region.
// Replace with your own subscription key and service region.
auto config = SpeechTranslationConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Sets source and target languages
// Replace with the languages of your choice.
auto fromLanguage = "en-US";
config->SetSpeechRecognitionLanguage(fromLanguage);
config->AddTargetLanguage("de");
config->AddTargetLanguage("fr");

// Creates a translation recognizer using microphone as audio input.
auto recognizer = TranslationRecognizer::FromConfig(config);
cout << "Say something...\n";

// Starts translation, and returns after a single utterance is recognized. The end of a
// single utterance is determined by listening for silence at the end or until a maximum of 15
// seconds of audio is processed.  The task returns the recognition text as result. 
// Note: Since RecognizeOnceAsync() returns only a single utterance, it is suitable only for single
// shot recognition like command or query. 
// For long-running multi-utterance recognition, use StartContinuousRecognitionAsync() instead.
auto result = recognizer->RecognizeOnceAsync().get();

// Checks result.
if (result->Reason == ResultReason::TranslatedSpeech)
{
    cout << "RECOGNIZED: Text=" << result->Text << std::endl
         << "  Language=" << fromLanguage << std::endl;

    for (const auto& it : result->Translations)
    {
        cout << "TRANSLATED into '" << it.first.c_str() << "': " << it.second.c_str() << std::endl;
    }
}
else if (result->Reason == ResultReason::RecognizedSpeech)
{
    cout << "RECOGNIZED: Text=" << result->Text << " (text could not be translated)" << std::endl;
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

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>后续步骤

- [如何识别语音](how-to-recognize-speech-cpp.md)
