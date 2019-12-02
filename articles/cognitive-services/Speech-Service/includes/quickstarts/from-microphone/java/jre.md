---
author: erhopf
ms.service: cognitive-services
ms.topic: include
origin.date: 08/06/2019
ms.date: 11/25/2019
ms.author: v-tawe
ms.openlocfilehash: 87842a4545ea73bb2dedc059f11a6b0589ee7e4c
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74390059"
---
## <a name="prerequisites"></a>先决条件

> [!div class="checklist"]
> * [创建 Azure 语音资源](../../../../get-started.md)
> * [设置开发环境](../../../../quickstarts/setup-platform.md?tabs=jre)
> * [创建空示例项目](../../../../quickstarts/create-project.md?tabs=jre)

## <a name="add-sample-code"></a>添加示例代码

1. 若要向 Java 项目添加新的空类，请选择“文件” > “新建” > “类”。   

1. 在“新建 Java 类”窗口中，在“包”字段内输入 **speechsdk.quickstart**，在“名称”字段内输入 **Main**。   

   ![“新建 Java 类”窗口的屏幕截图](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-06-create-main-java.png)

1. 将 `Main.java` 中的所有代码替换为以下代码片段：

    ```java
    package speechsdk.quickstart;
    
    import java.util.concurrent.Future;
    import com.microsoft.cognitiveservices.speech.*;
    
    /**
     * Quickstart: recognize speech using the Speech SDK for Java.
     */
    public class Main {
    
        /**
         * @param args Arguments are ignored in this sample.
         */
        public static void main(String[] args) {
            try {
                // Replace below with your own subscription key
                String speechSubscriptionKey = "YourSubscriptionKey";
                // Replace below with your own service region (e.g., "westus").
                String serviceRegion = "YourServiceRegion";
    
                int exitCode = 1;
                SpeechConfig config = SpeechConfig.fromSubscription(speechSubscriptionKey, serviceRegion);
                assert(config != null);
    
                SpeechRecognizer reco = new SpeechRecognizer(config);
                assert(reco != null);
    
                System.out.println("Say something...");
    
                Future<SpeechRecognitionResult> task = reco.recognizeOnceAsync();
                assert(task != null);
    
                SpeechRecognitionResult result = task.get();
                assert(result != null);
    
                if (result.getReason() == ResultReason.RecognizedSpeech) {
                    System.out.println("We recognized: " + result.getText());
                    exitCode = 0;
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
    
                reco.close();
    
                System.exit(exitCode);
            } catch (Exception ex) {
                System.out.println("Unexpected exception: " + ex.getMessage());
    
                assert(false);
                System.exit(1);
            }
        }
    }
    ```

1. 将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

1. 保存对项目的更改。

## <a name="build-and-run-the-app"></a>生成并运行应用

按 F11，或选择“运行” > “调试”。  
接下来的 15 秒，通过麦克风提供的语音输入将被识别并记录到控制台窗口中。

![成功识别后的控制台输出的屏幕截图](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [footer](./footer.md)]
