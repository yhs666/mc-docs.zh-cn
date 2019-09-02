---
title: 快速入门：识别语音，Java (Android) - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 Android 上使用语音 SDK 通过 Java 识别语音
services: cognitive-services
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 07/05/2019
ms.date: 09/02/2019
ms.author: v-biyu
ms.openlocfilehash: fc5f7b92dd26aa4a7cdc3c28e119944ad37aa9e9
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70104087"
---
# <a name="quickstart-recognize-speech-in-java-on-android-by-using-the-speech-sdk"></a>快速入门：使用语音 SDK 在 Android 上的 Java 中识别语音

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

本文介绍如何使用认知服务语音 SDK 将语音转为文本，进而开发适用于 Android 的 Java 应用程序。
该应用程序基于 Microsoft 认知服务语音 SDK Maven 包（版本 1.3.1）和 Android Studio 3.1。
语音 SDK 目前与具有 32/64 位 ARM 和 Intel x86/x64 兼容处理器的 Android 设备兼容。

> [!NOTE]
> 对于语音设备 SDK 和 Roobo 设备，请参阅[语音设备 SDK](speech-devices-sdk.md)。

## <a name="prerequisites"></a>先决条件

需要具有语音服务订阅密钥才能完成此快速入门。 你可以免费获得一个。 有关详细信息，请参阅[试用语音服务](get-started.md)。

## <a name="create-and-configure-a-project"></a>创建并配置项目

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-android-create-proj.md)]

## <a name="create-user-interface"></a>创建用户界面

我们将创建应用程序的基本用户界面。 编辑主活动的布局 `activity_main.xml` 。 一开始的布局包含一个带应用程序名称的标题栏，以及包含“Hello World!”文本的 TextView。

* 单击 TextView 元素。 在右上角将其 ID 属性更改为 `hello`。

* 从 `activity_main.xml` 窗口左上角的调色板中，将“按钮”拖到文本上方的空白区域。

* 在右侧的按钮属性中，在 `onClick` 属性的值中输入 `onSpeechButtonClicked`。 我们将使用此名称编写一个方法来处理按钮事件。  在右上角将其 ID 属性更改为 `button`。

* 若要推断布局约束，请使用设计器顶部的魔术棒图标。

  ![魔术棒图标的屏幕截图](media/sdk/qs-java-android-10-infer-layout-constraints.png)

现在，UI 的文本和图形表示形式应如下所示：

<table>
<tr>
<td valign="top">
<img src="media/sdk/qs-java-android-11-gui.png" alt=""/>
</td>
<td valign="top">
<!-- Can not find reference ~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml -->
</td>
</tr>
</table>

## <a name="add-sample-code"></a>添加示例代码

1. 打开源文件 `MainActivity.java`。 将此文件中的所有代码替换为以下内容。

```Java
package com.microsoft.cognitiveservices.speech.samples.quickstart;

import android.support.v4.app.ActivityCompat;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.TextView;

import com.microsoft.cognitiveservices.speech.ResultReason;
import com.microsoft.cognitiveservices.speech.SpeechConfig;
import com.microsoft.cognitiveservices.speech.SpeechRecognitionResult;
import com.microsoft.cognitiveservices.speech.SpeechRecognizer;

import java.util.concurrent.Future;

import static android.Manifest.permission.*;

public class MainActivity extends AppCompatActivity {

    // Replace below with your own subscription key
    private static String speechSubscriptionKey = "YourSubscriptionKey";
    // Replace below with your own service region .
    private static String serviceRegion = "YourServiceRegion";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Note: we need to request the permissions
        int requestCode = 5; // unique code for the permission request
        ActivityCompat.requestPermissions(MainActivity.this, new String[]{RECORD_AUDIO, INTERNET}, requestCode);
    }

    public void onSpeechButtonClicked(View v) {
        TextView txt = (TextView) this.findViewById(R.id.hello); // 'hello' is the ID of your text view

        try {
            SpeechConfig config = SpeechConfig.fromSubscription(speechSubscriptionKey, serviceRegion);
            assert(config != null);

            SpeechRecognizer reco = new SpeechRecognizer(config);
            assert(reco != null);

            Future<SpeechRecognitionResult> task = reco.recognizeOnceAsync();
            assert(task != null);

            // Note: this will block the UI thread, so eventually, you want to
            //        register for the event (see full samples)
            SpeechRecognitionResult result = task.get();
            assert(result != null);

            if (result.getReason() == ResultReason.RecognizedSpeech) {
                txt.setText(result.toString());
            }
            else {
                txt.setText("Error recognizing. Did you update the subscription info?" + System.lineSeparator() + result.toString());
            }

            reco.close();
        } catch (Exception ex) {
            Log.e("SpeechSDKDemo", "unexpected " + ex.getMessage());
            assert(false);
        }
    }
}
```

   * `onCreate` 方法包含的代码可请求麦克风和 Internet 权限并初始化本机平台绑定。 只需配置本机平台绑定一次。 这应该尽快在应用程序初始化期间完成。

   * 如前所述，`onSpeechButtonClicked` 方法是按钮单击处理程序。 按下按钮会触发语音到文本的听录。

1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 另请将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](regions.md)。

## <a name="build-and-run-the-app"></a>生成并运行应用

1. 将 Android 设备连接到开发电脑。 确保已在设备上启用[开发模式和 USB 调试](https://developer.android.com/studio/debug/dev-options)。

1. 若要生成应用程序，请按 Ctrl+F9，或者从菜单栏中选择“生成” > “生成项目”   。

1. 若要启动应用程序，请按 Shift+F10 或选择“运行” > “运行‘应用’”   。

1. 在出现的部署目标窗口中，选择 Android 设备。

   ![“选择部署目标”窗口的屏幕截图](media/sdk/qs-java-android-12-deploy.png)

按应用程序中的按钮，启动语音识别部分。 随后的 15 秒英语语音将会发送到语音服务进行转录。 结果显示在 Android 应用程序中，以及 Android Studio 的 logcat 窗口中。

![Android 应用程序的屏幕截图](media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [浏览 GitHub 上的 Java 示例](https://aka.ms/csspeech/samples)
