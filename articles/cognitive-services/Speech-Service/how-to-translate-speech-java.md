---
title: 使用适用于 Java 的语音 SDK 转换语音
titleSuffix: Azure Cognitive Services
description: 本文包含在 Java 环境中使用语音 SDK 翻译语音的示例代码。
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
ms.openlocfilehash: 975180bfb653ccc5c3810a0ae9405a73478a4527
ms.sourcegitcommit: edce097f471b6e9427718f0641ee2b421e3c0ed2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348251"
---
# <a name="translate-speech-with-the-speech-sdk-for-java"></a>使用适用于 Java 的语音 SDK 转换语音

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-translate-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-translate-speech-intro.md)]

[!INCLUDE [Introduction to top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

```Java
import com.microsoft.cognitiveservices.speech.*;
import com.microsoft.cognitiveservices.speech.audio.*;
import com.microsoft.cognitiveservices.speech.translation.*;
```

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-translate-speech-microphone.md)]

```Java
// Creates an instance of a speech translation config with specified
// subscription key and service region. Replace with your own subscription key
// and service region.
SpeechTranslationConfig config = SpeechTranslationConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Sets source and target language(s).
String fromLanguage = "en-US";
config.setSpeechRecognitionLanguage(fromLanguage);
config.addTargetLanguage("de");

// Sets voice name of synthesis output.
String GermanVoice = "Microsoft Server Speech Text to Speech Voice (de-DE, Hedda)";
config.setVoiceName(GermanVoice);

// Creates a translation recognizer using microphone as audio input.
TranslationRecognizer recognizer = new TranslationRecognizer(config);
{
    // Subscribes to events.
    recognizer.recognizing.addEventListener((s, e) -> {
        System.out.println("RECOGNIZING in '" + fromLanguage + "': Text=" + e.getResult().getText());

        Map<String, String> map = e.getResult().getTranslations();
        for(String element : map.keySet()) {
            System.out.println("    TRANSLATING into '" + element + "'': " + map.get(element));
        }
    });

    recognizer.recognized.addEventListener((s, e) -> {
        if (e.getResult().getReason() == ResultReason.TranslatedSpeech) {
            System.out.println("RECOGNIZED in '" + fromLanguage + "': Text=" + e.getResult().getText());

            Map<String, String> map = e.getResult().getTranslations();
            for(String element : map.keySet()) {
                System.out.println("    TRANSLATED into '" + element + "'': " + map.get(element));
            }
        }
        if (e.getResult().getReason() == ResultReason.RecognizedSpeech) {
            System.out.println("RECOGNIZED: Text=" + e.getResult().getText());
            System.out.println("    Speech not translated.");
        }
        else if (e.getResult().getReason() == ResultReason.NoMatch) {
            System.out.println("NOMATCH: Speech could not be recognized.");
        }
    });

    recognizer.synthesizing.addEventListener((s, e) -> {
        byte[] data = e.getResult().getAudio();

        System.out.println("Synthesis result received. Size of audio data: " + data.length);

        // Play the TTS data of we got more than the wav header.
        if (data != null && data.length > 44) {
            try {
                ByteArrayInputStream arrayInputStream = new ByteArrayInputStream(data);
                AudioInputStream audioInputStream = AudioSystem.getAudioInputStream(arrayInputStream);
                AudioFormat audioFormat = audioInputStream.getFormat();
                DataLine.Info info = new DataLine.Info(Clip.class, audioFormat);
                Clip clip = (Clip) AudioSystem.getLine(info);

                clip.open(audioInputStream);
                clip.start();
            } catch (LineUnavailableException e1) {
                e1.printStackTrace();
            } catch (UnsupportedAudioFileException e1) {
                e1.printStackTrace();
            } catch (IOException e1) {
                e1.printStackTrace();
            }
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
        System.out.println("\nSession started event.");
    });

    recognizer.sessionStopped.addEventListener((s, e) -> {
        System.out.println("\nSession stopped event.");
    });

    // Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
    System.out.println("Say something...");
    recognizer.startContinuousRecognitionAsync().get();

    System.out.println("Press any key to stop");
    new Scanner(System.in).nextLine();

    recognizer.stopContinuousRecognitionAsync().get();
}
```

[!INCLUDE [Introduction to using a file](../../../includes/cognitive-services-speech-service-how-to-translate-speech-file.md)]

```Java
private static Semaphore stopTranslationWithFileSemaphore;

public static void translationWithFileAsync() throws InterruptedException, ExecutionException
{
    stopTranslationWithFileSemaphore = new Semaphore(0);

    // Creates an instance of a speech translation config with specified
    // subscription key and service region. Replace with your own subscription key
    // and service region.
    SpeechTranslationConfig config = SpeechTranslationConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");

    // Sets source and target languages
    String fromLanguage = "en-US";
    config.setSpeechRecognitionLanguage(fromLanguage);
    config.addTargetLanguage("de");
    config.addTargetLanguage("fr");

    // Creates a translation recognizer using file as audio input.
    // Replace with your own audio file name.
    AudioConfig audioInput = AudioConfig.fromWavFileInput("YourAudioFile.wav");
    TranslationRecognizer recognizer = new TranslationRecognizer(config, audioInput);
    {
        // Subscribes to events.
        recognizer.recognizing.addEventListener((s, e) -> {
            System.out.println("RECOGNIZING in '" + fromLanguage + "': Text=" + e.getResult().getText());

            Map<String, String> map = e.getResult().getTranslations();
            for(String element : map.keySet()) {
                System.out.println("    TRANSLATING into '" + element + "'': " + map.get(element));
            }
        });

        recognizer.recognized.addEventListener((s, e) -> {
            if (e.getResult().getReason() == ResultReason.TranslatedSpeech) {
                System.out.println("RECOGNIZED in '" + fromLanguage + "': Text=" + e.getResult().getText());

                Map<String, String> map = e.getResult().getTranslations();
                for(String element : map.keySet()) {
                    System.out.println("    TRANSLATED into '" + element + "'': " + map.get(element));
                }
            }
            if (e.getResult().getReason() == ResultReason.RecognizedSpeech) {
                System.out.println("RECOGNIZED: Text=" + e.getResult().getText());
                System.out.println("    Speech not translated.");
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

            stopTranslationWithFileSemaphore.release();;
        });

        recognizer.sessionStarted.addEventListener((s, e) -> {
            System.out.println("\nSession started event.");
        });

        recognizer.sessionStopped.addEventListener((s, e) -> {
            System.out.println("\nSession stopped event.");

            // Stops translation when session stop is detected.
            System.out.println("\nStop translation.");
            stopTranslationWithFileSemaphore.release();;
        });

        // Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
        System.out.println("Start translation...");
        recognizer.startContinuousRecognitionAsync().get();

        // Waits for completion.
        stopTranslationWithFileSemaphore.acquire();;

        // Stops translation.
        recognizer.stopContinuousRecognitionAsync().get();
    }
}
```

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>后续步骤

- [如何识别语音](how-to-recognize-speech-java.md)
