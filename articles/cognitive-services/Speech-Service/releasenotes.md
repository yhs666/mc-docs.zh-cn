---
title: 发行说明 - 语音服务
titleSuffix: Azure Cognitive Services
description: 参阅语音服务的功能发布、改进、bug 修复和已知问题的运行日志。
services: cognitive-services
author: BrianMouncer
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 11/05/2019
ms.date: 11/25/2019
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: b6f122e398ed6b7b7e486ab90086df3175fcd907
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389531"
---
# <a name="release-notes"></a>发行说明

## <a name="speech-sdk-180-2019-november-release"></a>语音 SDK 1.8.0：2019-November 版本

**新功能**

- 添加了远程对话 Java API，用于以异步批的方式进行对话听录。

**重大更改**

- 对话听录器功能已移到 Microsoft.CognitiveServices.Speech.Transcription 命名空间下。
- 部分对话听录器方法已移到新的对话类。
- 放弃了对 32 位（ARMv7 和 x86）iOS 的支持


**示例**

- 用于自动源语言检测的 C++ 和 Java 示例。

## <a name="speech-sdk-170-2019-september-release"></a>语音 SDK 1.7.0：2019-September 版本

**新功能**

- 添加了对通用 Windows 平台 (UWP)、Android 和 iOS 上的 Xamarin 的支持
- 添加了对 Unity 的 iOS 支持
- 添加了对 Android、iOS 和 Linux 上的 ALaw、Mulaw、FLAC 的压缩输入支持
- 在 Connection 类中添加了 SendMessageAsync，用于向服务发送消息
- 在 Connection 类中添加了 SetMessageProperty，用于设置消息的属性

**Bug 修复**

- 修复了 Unity 2019 for Android 上的 IL2CPP 生成问题
- 修复了 wav 文件输入中格式错误的标头被错误处理的问题
- 修复了 UUID 在某些连接属性中不唯一的问题
- 修复了一些有关 Swift 绑定中存在为 Null 性说明符的警告（可能需要小的代码更改）
- 修复了一个 Bug，该 Bug 导致 websocket 连接在网络负载下被意外关闭
- 修复了 Android 上的一个问题，该问题有时候导致 DialogServiceConnector 使用的印象 ID 重复
- 改进了进行多轮交互时连接的稳定性，以及它们发生在 DialogServiceConnector 上时（通过取消的事件）对故障进行的报告
- 现在，DialogServiceConnector 会话开始时会正确提供事件，包括在活动 StartKeywordRecognitionAsync() 期间调用 ListenOnceAsync() 的时候
- 解决了与收到的 DialogServiceConnector 活动相关联的崩溃

**示例**

- Xamarin 的快速入门
- 使用 Linux ARM64 信息更新了 CPP 快速入门
- 使用 iOS 信息更新了 Unity 快速入门

## <a name="speech-sdk-160-2019-june-release"></a>语音 SDK 1.6.0：2019 年 6 月发布

**示例**

- iOS 上的 Swift 快速入门示例
- 更新了 DialogServiceConnector 的快速入门示例

**改进 / 更改**

- 对话命名空间：
  - SpeechBotConnector 已重命名为 DialogServiceConnector
  - BotConfig 已重命名为 DialogServiceConfig
  - BotConfig::FromChannelSecret() 已重新映射到 DialogServiceConfig::FromBotSecret()
- 改写了传递无效区域时出现的错误消息
- Swift/Objective-C：
  - 改进了错误报告：可能导致出错的方法现在有两个版本：一个版本公开用于错误处理的 `NSError` 对象，另一个版本引发异常。 前者向 Swift 公开。 此项更改需要对现有的 Swift 代码进行改编。
  - 改进了事件处理

**Bug 修复**

- 修复了 C# 中的封送字符串，以支持完整语言
- 修复了示例中的 .NET Core 应用问题，以使用 net461 目标框架加载核心库
- 修复了示例中的偶发性问题，以将本机库部署到输出文件夹
- 修复了 Web 套接字可靠关闭的问题
- 修复了在 Linux 负载极高的情况下打开连接时可能发生崩溃的问题
- 修复了 macOS 框架捆绑包中缺少元数据的问题
- 修复了 Windows 上的 `pip install --user` 问题

## <a name="speech-sdk-151"></a>语音 SDK 1.5.1

这是一个 Bug 修复版本，只影响本机/托管 SDK。 它不影响 SDK 的 JavaScript 版本。

**Bug 修复**

- 修复了 FromSubscription 与对话听录一起使用时出现的问题。

## <a name="speech-sdk-150-2019-may-release"></a>语音 SDK 1.5.0：2019 年 5 月发布

**新功能**

- 短语提示功能通过 SDK 提供。 有关详细信息，请参阅[此文](how-to-phrase-lists.md)。

**示例**

- 添加了 SDK 支持的新功能或新服务的示例。

**改进 / 更改**

- 添加了各种识别器属性，以调整服务行为或服务结果（例如屏蔽猥亵内容等）。
- 现在，即使你创建了识别器 `FromEndpoint`，也能通过标准配置属性来配置识别器。
- Objective-C：已将 `OutputFormat` 属性添加到 SPXSpeechConfiguration。
- SDK 现在支持将 Debian 9 用作 Linux 分发版。


## <a name="speech-sdk-142"></a>语音 SDK 1.4.2

这是一个 Bug 修复版本，只影响本机/托管 SDK。 它不影响 SDK 的 JavaScript 版本。

## <a name="speech-sdk-141"></a>语音 SDK 1.4.1

这是一个仅限 JavaScript 的版本。 未增加任何功能。 进行了以下修复：

- 阻止 Web 包加载 https-proxy-agent。

## <a name="speech-sdk-140-2019-april-release"></a>语音 SDK 1.4.0：2019 年 4 月发布

**新功能**

- SDK 现在支持将 MP3 和 Opus/OGG 音频文件用作流输入文件。 此功能只能通过 C++ 和 C# 在 Linux 上使用，目前为 beta 版（更多详细信息请参见[此处](how-to-use-codec-compressed-audio-input-streams.md)）。
- 适用于 Java、.NET Core C++和 Objective-C 的语音 SDK 已获得 macOS 支持。 macOS 的 Objective-C 支持目前以 beta 版提供。
- iOS：适用于 iOS (Objective-C) 的语音 SDK 现在也已作为 CocoaPod 发布。
- JavaScript：支持将非默认麦克风用作输入设备。
- JavaScript：Node.js 的代理支持。

**示例**

- 添加了有关在 macOS 上的 C++ 和 Objective-C 中使用语音 SDK 的示例。

**改进 / 更改**

- Python:现在会通过 `properties` 属性公开识别结果的附加属性。
- 若要获得更多开发和调试支持，可将 SDK 日志记录和诊断信息重定向到日志文件中（更多详细信息请参见[此处](how-to-use-logging.md)）。
- JavaScript：提高了音频处理性能。

**Bug 修复**

- Mac/iOS：修复了未能与语音服务建立连接时导致长时间等待的 bug。
- Python：改进了 Python 回调中的参数的错误处理。
- JavaScript：修复了 RequestSession 中结束的语音的错误状态报告。

## <a name="speech-sdk-131-2019-february-refresh"></a>语音 SDK 1.3.1：2019 年 2 月刷新

这是一个 Bug 修复版本，只影响本机/托管 SDK。 它不影响 SDK 的 JavaScript 版本。

**Bug 修复**

- 修复了使用麦克风输入时出现的内存泄漏问题。 基于流的输入或文件输入不受影响。

## <a name="speech-sdk-130-2019-february-release"></a>语音 SDK 1.3.0：2019 年 2 月版本

**新功能**

- 语音 SDK 支持通过 AudioConfig 类来选择输入麦克风。 这样，便可以将音频数据从非默认麦克风流式传输到语音服务。 有关详细信息，请参阅介绍[音频输入设备选择](how-to-select-audio-input-devices.md)的文档。 此功能在 JavaScript 中尚不可用。
- 语音 SDK 目前在 beta 版本中支持 Unity。 请通过 [GitHub 示例存储库](https://github.com/Azure-Samples/cognitive-services-speech-sdk)中的问题部分来提供反馈。 此版本支持在 Windows x86 和 x64（桌面或通用 Windows 平台应用程序）以及 Android（ARM32/64，x86）上使用 Unity。 [Unity 快速入门](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=unity)中提供了更多信息。
- 不再需要 `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` 文件（在以前的版本中提供）。 此功能现在集成到核心 SDK 中。

**示例**

[示例存储库](https://github.com/Azure-Samples/cognitive-services-speech-sdk)中提供了以下新内容：

- 有关 AudioConfig.FromMicrophoneInput 的更多示例。
- 有关在 iOS 中使用连接对象的更多示例。
- 有关使用[批量听录 REST API](batch-transcription.md) 的新示例。

**改进 / 更改**

- Python
  - 改进了 SpeechConfig 中的参数验证和错误消息。
  - 增加了对连接对象的支持。
  - 支持 Windows 上的 32 位 Python (x86)。
  - 适用于 Python 的语音 SDK 已完成 beta 版本。
- iOS
  - SDK 现在是基于 iOS SDK 版本 12.1 构建的。
  - SDK 现在支持 iOS 版本 9.2 及更高版本。
  - 改进了参考文档并修复了多个属性名称。
- Javascript
  - 增加了对连接对象的支持。
  - 添加了捆绑的 JavaScript 的类型定义文件
  - 首次支持并实现了短语提示。
  - 随服务 JSON 返回属性集合以用于识别
- Windows DLL 现在包含一个版本资源。
- 如果创建识别器 `FromEndpoint`，则可将参数直接添加到终结点 URL。 使用 `FromEndpoint` 时，无法通过标准的配置属性来配置识别器。

**Bug 修复**

- 过去无法正确处理空的代理用户名和代理密码。 在此版本中，如果将代理用户名和代理密码设置为空字符串，则在连接到代理时不会提交它们。
- 对于某些语言&nbsp;/ 环境，由 SDK 创建的 SessionId 并非总是真正随机的。 已添加了随机生成器初始化来修复此问题。
- 改进了对授权令牌的处理。 如果希望使用授权令牌，请在 SpeechConfig 中进行指定并将订阅密钥保留为空。 然后，像往常一样创建识别器。
- 过去，在某些情况下，连接对象不能正确释放。 现在已修复此问题。

## <a name="speech-sdk-121"></a>语音 SDK 1.2.1

这是一个仅限 JavaScript 的版本。 未增加任何功能。 进行了以下修复：

- 在 turn.end 处触发流结束，在 speech.end 处不触发。
- 修复了音频泵中在当前发送失败时不安排下一次发送的 bug。
- 修复了使用身份验证令牌进行的连续识别。
- 对不同识别器 / 终结点的 bug 修复。
- 文档改进。

## <a name="speech-sdk-120-2018-december-release"></a>语音 SDK 1.2.0：2018 年 12 月版本

**新功能**

- Python
  - 此版本支持 Python 的 Beta 版本（3.5 及更高版本）。 有关详细信息，请参阅此文](quickstart-python.md)。
- Javascript
  - 适用于 JavaScript 的语音 SDK 已开放了源代码。 [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js) 上提供了源代码。
  - 我们现在支持 Node.js，可以在[此处](quickstart-js-node.md)找到详细信息。
  - 已删除了对音频会话的长度限制，将自动在后台进行重新连接。
- 连接对象
  - 可以从识别器中访问连接对象。 此对象允许你显式启动服务连接并订阅连接事件和断开连接事件。
    （此功能在 JavaScript 和 Python 中尚不可用。）
- 支持 Ubuntu 18.04。
- Android
  - 在生成 APK 期间启用了 ProGuard 支持。

**改进**

- 改进了内部线程的使用，减少了线程、锁和互斥的数量。
- 改进了错误报告 / 信息。 在某些情况下，错误消息没有完全传播出去。
- 更新了 JavaScript 中的开发依赖项来使用最新模块。

**Bug 修复**

- 修复了由于 RecognizeAsync 中的类型不匹配导致的内存泄漏。
- 在某些情况下，异常会被泄露。
- 修复了翻译事件参数中的内存泄漏。
- 修复了长时间运行的会话中与重新连接相关的锁定问题。
- 修复了可能会导致失败的翻译缺少最终结果的问题。
- C#：如果在主线程中没有等待异步操作，则可能会在异步任务完成之前释放识别器。
- Java:修复了导致 Java VM 故障的一个问题。
- Objective-C：修复了枚举映射；之前返回 RecognizedIntent 而非 RecognizingIntent。
- JavaScript：在 SpeechConfig 中将默认输出格式设置为“simple”。
- JavaScript：删除了 JavaScript 和其他语言中配置对象中的属性之间的不一致。

**示例**

- 在[示例存储库](https://github.com/Azure-Samples/cognitive-services-speech-sdk)中添加了 Node.js 示例。

## <a name="speech-sdk-110"></a>语音 SDK 1.1.0

**新功能**

- 对 Android x86/x64 的支持。
- 代理支持：在 SpeechConfig 对象中，现在可以调用某个函数来设置代理信息（主机名、端口、用户名和密码）。 此功能在 iOS 上尚不可用。
- 改进了错误代码和消息。 如果识别返回了错误，这在过去会将 `Reason`（在已取消事件中）或 `CancellationDetails`（在识别结果中）设置为 `Error`。 取消的事件现在包含两个附加的成员：`ErrorCode` 和 `ErrorDetails`。 如果服务器随所报告的错误返回了附加的错误信息，则现在将在新成员中提供该信息。

**改进**

- 在识别器配置中添加了附加的验证并添加了附加的错误消息。
- 改进了对音频文件中间的长时间静默的处理。
- NuGet 包：对于 .NET Framework 项目，它阻止使用 AnyCPU 配置进行构建。

**Bug 修复**

- 修复了在识别器中发现的几处异常。 此外，还会捕获异常并将其转换为已取消事件。
- 修复了属性管理中的内存泄漏。
- 修复了音频输入文件可能会导致识别器发生故障的 bug。
- 修复了在会话停止事件后无法检索事件的 bug。
- 修复了线程中的一些争用条件。
- 修复了可能会导致故障的 iOS 兼容性问题。
- 改进了对 Android 麦克风的支持的稳定性。
- 修复了 JavaScript 中的识别器将忽略识别语言的 bug。
- 修复了阻止在 JavaScript 中设置 EndpointId（在某些情况下）的 bug。
- 更改了 JavaScript 中的 AddIntent 中的参数顺序，并添加了缺少的 AddIntent JavaScript 签名。

**示例**

- 在[示例存储库](https://github.com/Azure-Samples/cognitive-services-speech-sdk)中添加了拉取和推送流用法的 C++ 和 C# 示例。

## <a name="speech-sdk-101"></a>语音 SDK 1.0.1

可靠性改进和 bug 修复：

- 修复了处理识别器时由于争用条件而导致的潜在严重错误
- 修复了未设置属性情况下的潜在严重错误。
- 添加了其他错误检查和参数检查。
- Objective-C：修复了 NSString 中名称替代而引起的潜在严重错误。
- Objective-C：调整了 API 的可见性
- JavaScript：针对事件及其有效负载进行了修复。
- 文档改进。

在[示例存储库](https://github.com/Azure-Samples/cognitive-services-speech-sdk)中已添加了适用于 JavaScript 的新示例。

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>认知服务语音 SDK 1.0.0：2018 年 9 月版本

**新功能**

- 支持 iOS 中的 Objective-C。 请查看[适用于 iOS 的 Objective-C 快速入门](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone-langs/objectivec-ios.md)。
- 支持浏览器中的 JavaScript。 请查看 [JavaScript 快速入门](quickstart-js-browser.md)。

**重大更改**

- 该版本中推出了大量重大更改。
  有关详细信息，请查看[此页](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/docs/breaking_changes_1_0_0.md)。

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>认知服务语音 SDK 0.6.0：2018 年 8 月版本

**新功能**

- 使用语音 SDK 生成的 UWP 应用现在可以通过 Windows 应用认证工具包 (WACK)。
  请查看 [UWP 快速入门](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp&tabs=uwp)。
- 在 Linux (Ubuntu 16.04 x64) 上支持 .NET Standard 2.0。
- 试验：在 Windows (64-bit) 和 Linux (Ubuntu 16.04 x64) 上支持 Java 8。
  请查看 [Java 运行时环境快速入门](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=jre)。

**功能性更改**

- 公开了关于连接错误的更多错误详细信息。

**重大更改**

- 在 Java (Android) 中，`SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` 函数不再需要 path 参数。 现在，在所有受支持的平台上都会自动检测路径。
- 在 Java 和 C# 中，属性 `EndpointUrl` 的 get 访问器已被删除。

**Bug 修复**

- 在 Java 中，目前在翻译识别器上实现了音频合成结果。
- 修复了一个 bug，该 bug 可能会导致非活动线程和更多的已打开且未使用的套接字。
- 修复了一个问题，该问题导致在传输过程中无法终止长时间运行的识别。
- 修复了识别器关闭过程中的一个争用条件。

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>认知服务语音 SDK 0.5.0：2018 年 7 月版本

**新功能**

- 支持 Android 平台（API 23：Android 6.0 Marshmallow 或更高版本）。 查看 [Android 快速入门](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=android)。
- 在 Windows 上支持 .NET Standard 2.0。 查看 [.NET Core 快速入门](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore)。
- 试验：在 Windows 上支持 UWP（版本 1709 或更高版本）。
  - 请查看 [UWP 快速入门](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp)。
  - 注意：使用语音 SDK 生成的 UWP 应用尚未通过 Windows 应用认证工具包 (WACK)。
- 通过自动重新连接支持识别功能长时间运行。

**功能性更改**

- `StartContinuousRecognitionAsync()` 支持识别功能长时间运行。
- 识别结果包含更多字段。 这些字段是识别文本的音频开始和持续时间（时钟周期数）的偏移量和表示识别状态的其他值（例如 `InitialSilenceTimeout`、`InitialBabbleTimeout`）。
- 支持 AuthorizationToken 用于创建工厂实例。

**重大更改**

- 识别事件：NoMatch 事件类型已合并到 Error 事件中。
- C# 中的 SpeechOutputFormat 已重命名为 OutputFormat 以与 C++ 保持一致。
- `AudioInputStream` 接口的某些方法的返回类型略有更改：
  - 在 Java 中，`read` 方法现返回 `long` 而不是 `int`。
  - 在 C# 中，`Read` 方法现返回 `uint` 而不是 `int`。
  - 在 C++ 中，`Read` 和 `GetFormat` 方法现返回 `size_t` 而不是 `int`。
- C++：音频输入流的实例现在只能作为 `shared_ptr` 传递。

**Bug 修复**

- 修复了 `RecognizeAsync()` 超时时结果中的错误返回值。
- 删除了 Windows 上媒体基础库的依赖项。 SDK 现在使用 Core Audio API。
- 文档修复：添加了一个[区域](regions.md)页来描述支持的区域。

**已知问题**

- 适用于 Android 的语音 SDK 不报告用于翻译的语音合成结果。 此问题将在下一版本中修复。

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>认知服务语音 SDK 0.4.0：2018 年 6 月版本

**功能性更改**

- AudioInputStream

  一种现可将流用作音频源的识别器。 有关详细信息，请参阅相关[操作说明指南](how-to-use-audio-input-streams.md)。

- 详细输出格式

  创建 `SpeechRecognizer` 时，可请求 `Detailed` 或 `Simple` 输出格式。 `DetailedSpeechRecognitionResult` 包含置信度分数、识别的文本、原始词法形式、标准化形式和已屏蔽不当字词的标准化形式。

**重大更改**

- 将 C# 中的 `SpeechRecognitionResult.RecognizedText` 更改为 `SpeechRecognitionResult.Text`。

**Bug 修复**

- 修复了关闭期间 USP 层中可能出现的回叫问题。

- 如果识别器使用了音频输入文件，则它在文件句柄上停留的时间将超过必要时间。

- 删除了消息泵和识别器之间的多个死锁。

- 在服务的响应超时后触发 `NoMatch` 结果。

- Windows 上的媒体基础库为延迟加载。 此库仅用于麦克风输入。

- 音频数据的上传速度约限制为原始音频速度的两倍。

- 在 Windows 上，C# .NET 程序集现在为强命名。

- 文档修复：`Region` 是创建识别器所必需的信息。

已添加更多示例，还将持续更新。 有关最新的示例集，请参阅[语音 SDK 示例 GitHub 存储库](https://github.com/Azure-Samples/cognitive-services-speech-sdk)。

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>认知服务语音 0.2.12733：2018 年 5 月版本

此版本是认知服务语音 SDK 的第一个公共预览版本。
