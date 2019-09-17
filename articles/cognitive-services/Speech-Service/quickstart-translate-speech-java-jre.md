---
title: 快速入门：翻译语音，Java（Windows、Linux）- 语音服务
titleSuffix: Azure Cognitive Services
description: 在本快速入门中，你将创建一个简单的 Java 应用程序来捕获用户语音，将其翻译为另一种语言，并将文本输出到命令行。 本指南适用于 Windows 和 Linux 用户。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 8/20/2019
ms.date: 07/05/2019
ms.author: v-biyu
ms.openlocfilehash: fd1ad359b65451cd03649a4b36e8afe67f515298
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70103752"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-java"></a>快速入门：使用适用于 Java 的语音 SDK 转换语音

针对[语音转文本](quickstart-java-jre.md)也提供了快速入门。

在本快速入门中，你将创建一个简单的 Java 应用程序，该应用程序从计算机的麦克风中捕获用户语音，翻译语音，并将翻译后的文本实时转录到命令行。 此应用程序需在 64 位 Windows 或 64 位 Linux（Ubuntu 16.04、Ubuntu 18.04、Debian 9）或者 macOS 10.13 或更高版本上运行。 它是使用语音 SDK Maven 包和 Eclipse Java IDE 生成的。

有关可用于语音翻译的语言的完整列表，请参阅[语言支持](language-support.md)。

## <a name="prerequisites"></a>先决条件

本快速入门需要：

* 操作系统：64 位 Windows、64 位 Linux（Ubuntu 16.04、Ubuntu 18.04、Debian 9）或 macOS 10.13 或更高版本
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) 或 [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* 语音服务的 Azure 订阅密钥。 [获取一个试用版](get-started.md)。

如果运行 Linux，请确保在启动 Eclipse 之前安装这些依赖项。

 * 在 Ubuntu 上：

   ```sh
   sudo apt-get update
   sudo apt-get install libssl1.0.0 libasound2
   ```

 * 在 Debian 9 上：

   ```sh
   sudo apt-get update
   sudo apt-get install libssl1.0.2 libasound2
   ```

> [!NOTE]
> 对于语音设备 SDK 和 Roobo 设备，请参阅[语音设备 SDK](speech-devices-sdk.md)。

## <a name="create-and-configure-project"></a>创建并配置项目

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>添加示例代码

1. 若要向 Java 项目添加新的空类，请选择“文件” > “新建” > “类”。   

1. 在“新建 Java 类”窗口中，在“包”字段内输入 **speechsdk.quickstart**，在“名称”字段内输入 **Main**。   

   ![“新建 Java 类”窗口的屏幕截图](media/sdk/qs-java-jre-06-create-main-java.png)

1. 将 `Main.java` 中的所有代码替换为以下代码片段：

```Java
package speechsdk.quickstart;

import java.io.IOException;
import java.util.Map;
import java.util.Scanner;
import java.util.concurrent.ExecutionException;
import com.microsoft.cognitiveservices.speech.*;
import com.microsoft.cognitiveservices.speech.translation.*;

public class Main {

    public static void translationWithMicrophoneAsync() throws InterruptedException, ExecutionException, IOException
    {
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
                    System.out.println("    TRANSLATING into '" + element + "': " + map.get(element));
                }
            });

            recognizer.recognized.addEventListener((s, e) -> {
                if (e.getResult().getReason() == ResultReason.TranslatedSpeech) {
                    System.out.println("RECOGNIZED in '" + fromLanguage + "': Text=" + e.getResult().getText());

                    Map<String, String> map = e.getResult().getTranslations();
                    for(String element : map.keySet()) {
                        System.out.println("    TRANSLATED into '" + element + "': " + map.get(element));
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
                System.out.println("Synthesis result received. Size of audio data: " + e.getResult().getAudio().length);
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
    }

    public static void main(String[] args) {
        try {
            translationWithMicrophoneAsync();
        } catch (Exception ex) {
            System.out.println("Unexpected exception: " + ex.getMessage());
            assert(false);
            System.exit(1);
        }
    }
}
```

1. 将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](regions.md)。

1. 保存对项目的更改。

## <a name="build-and-run-the-app"></a>生成并运行应用

按 F11，或选择“运行” > “调试”。  

通过麦克风提供的语音输入将转译为德语，并记录到控制台窗口中。 按“Enter”停止捕获语音。

![成功识别后的控制台输出的屏幕截图](media/sdk/qs-translate-java-jre-output.png)

## <a name="next-steps"></a>后续步骤

GitHub 上提供了其他示例，例如如何从音频文件中读取语音，以及将翻译后的文本作为合成语音输出。

> [!div class="nextstepaction"]
> [浏览 GitHub 上的 Java 示例](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>另请参阅

- [快速入门：识别语音，Java（Windows、Linux）](quickstart-java-jre.md)
