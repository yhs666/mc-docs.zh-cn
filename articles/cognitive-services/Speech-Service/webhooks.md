---
title: Webhook - 语音服务
titleSuffix: Azure Cognitive Services
description: Webhook 是一种 HTTP 回调，在处理长时间运行的进程（例如导入、自适应、准确度测试，或长时间运行的文件听录）时，它非常适用于优化解决方案。
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 09/19/2019
ms.author: v-tawe
ms.openlocfilehash: 5b7881b4824363577e4c30ac90b2693653e15e6a
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267122"
---
# <a name="webhooks-for-speech-services"></a>适用于语音服务的 Webhook

Webhook 类似于 HTTP 回调，在语音服务中的数据可用时，应用程序可以通过 Webhook 接受这些数据。 使用 Webhook 可以优化 REST API 的使用，而无需持续轮询响应。 下面的几个部分将会介绍如何在语音服务中使用 Webhook。

## <a name="supported-operations"></a>支持的操作

语音服务支持对所有长时间运行的操作使用 Webhook。 下面列出的每个操作都可以在完成后触发 HTTP 回调。

* DataImportCompletion
* ModelAdaptationCompletion
* AccuracyTestCompletion
* TranscriptionCompletion
* EndpointDeploymentCompletion
* EndpointDataCollectionCompletion

接下来，让我们创建一个 Webhook。

## <a name="create-a-webhook"></a>创建 Webhook

让我们创建一个用于脱机听录的 Webhook。 场景：用户希望使用批量听录 API 以异步方式听录一个长时间运行的音频文件。

可以通过向 https://\<区域\>.cris.ai/api/speechtotext/v2.1/transcriptions/hooks 发出 POST 请求来创建 Webhook。

请求的配置参数作为 JSON 提供：

```json
{
  "configuration": {
    "url": "https://your.callback.url/goes/here",
    "secret": "<my_secret>"
  },
  "events": [
    "TranscriptionCompletion"
  ],
  "active": true,
  "name": "TranscriptionCompletionWebHook",
  "description": "This is a Webhook created to trigger an HTTP POST request when my audio file transcription is completed.",
  "properties": {
      "Active" : "True"
  }

}
```
对批量听录 API 发出的所有 POST 请求都需要一个 `name`。 `description` 和 `properties` 参数是可选的。

可以使用 `Active` 属性来打开和关闭 URL 回调，而无需删除再重新创建 Webhook 注册。 如果你只需在完成该过程后回调一次，请删除 Webhook 并将 `Active` 属性切换为 false。

事件类型 `TranscriptionCompletion` 在事件数组中提供。 当听录进入最终状态（`Succeeded` 或 `Failed`）时，请求将回调终结点。 回调注册的 URL 时，请求将包含一个 `X-MicrosoftSpeechServices-Event` 标头，该标头包含已注册的事件类型之一。 每个已注册的事件类型有一个请求。

有一个无法订阅的事件类型： `Ping` 事件类型。 使用 ping URL 创建完 Webhook 后，此类型的请求将发送到 URL（参阅下文）。  

在配置中，`url` 属性是必需的。 POST 请求将发送到此 URL。 `secret` 用于创建有效负载的 SHA256 哈希，其机密为 HMAC 密钥。 回调已注册的 URL 时，该哈希将设置为 `X-MicrosoftSpeechServices-Signature` 标头。 此标头已经过 Base64 编码。

此示例演示如何使用 C# 验证有效负载：

```csharp

private const string EventTypeHeaderName = "X-MicrosoftSpeechServices-Event";
private const string SignatureHeaderName = "X-MicrosoftSpeechServices-Signature";

[HttpPost]
public async Task<IActionResult> PostAsync([FromHeader(Name = EventTypeHeaderName)]WebHookEventType eventTypeHeader, [FromHeader(Name = SignatureHeaderName)]string signature)
{
    string body = string.Empty;
    using (var streamReader = new StreamReader(this.Request.Body))
    {
        body = await streamReader.ReadToEndAsync().ConfigureAwait(false);
        var secretBytes = Encoding.UTF8.GetBytes("my_secret");
        using (var hmacsha256 = new HMACSHA256(secretBytes))
        {
            var contentBytes = Encoding.UTF8.GetBytes(body);
            var contentHash = hmacsha256.ComputeHash(contentBytes);
            var storedHash = Convert.FromBase64String(signature);
            var validated = contentHash.SequenceEqual(storedHash);
        }
    }

    switch (eventTypeHeader)
    {
        case WebHookEventType.Ping:
            // Do your ping event related stuff here (or ignore this event)
            break;
        case WebHookEventType.TranscriptionCompletion:
            // Do your subscription related stuff here.
            break;
        default:
            break;
    }

    return this.Ok();
}

```
在此代码片段中，`secret` 已解码并验证。 你还会注意到，Webhook 事件类型已切换。 目前，每个已完成的听录都有一个事件。 代码将针对每个事件重试 5 次（延迟一秒），然后放弃。

### <a name="other-webhook-operations"></a>其他 Webhook 操作

获取所有已注册的 Webhook：GET https://chinaeast.cris.ai/api/speechtotext/v2.1/transcriptions/hooks

获取一个特定的 Webhook：GET https://chinaeast.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

删除一个特定的 Webhook：DELETE https://chinaeast.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

> [!Note]
> 在以上示例中，区域为“chinaeast”。 应将此区域替换为在 Azure 门户中创建了语音服务资源的区域。

POST https://chinaeast.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/ping 正文：空

将 POST 请求发送到已注册的 URL。 该请求包含值为 ping 的 `X-MicrosoftSpeechServices-Event` 标头。 如果使用机密注册了 Webhook，则该请求将包含 `X-MicrosoftSpeechServices-Signature` 标头以及有效负载的 SHA256 哈希，其机密为 HMAC 密钥。 该哈希已经过 Base64 编码。

POST https://chinaeast.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/test 正文：空

如果已订阅事件类型（听录）的实体在系统中存在并处于适当的状态，则将 POST 请求发送到已注册的 URL。 将从已调用 Webhook 的最后一个实体生成有效负载。 如果不存在实体，则 POST 将会响应 204。 如果可以发出测试请求，则会响应 200。 请求正文的形状与 Webhook 已订阅的特定实体（例如听录）的 GET 请求的形状相同。 请求包含如上所述的 `X-MicrosoftSpeechServices-Event` 和 `X-MicrosoftSpeechServices-Signature` 标头。

### <a name="run-a-test"></a>运行测试

可以使用网站 https://bin.webhookrelay.com 来完成快速测试。 在该网站中，可以获取作为参数传递给 HTTP POST 的、用于创建本文档前面所述 Webhook 的回调 URL。

单击“创建桶”，并按照屏幕上的说明获取 hook。 然后，使用此页面中提供的信息将 hook 注册到语音服务。 完成听录后响应的中继消息的有效负载如下所示：

```json
{
    "results": [],
    "recordingsUrls": [
        "my recording URL"
    ],
    "models": [
        {
            "modelKind": "AcousticAndLanguage",
            "datasets": [],
            "id": "a09c8c8b-1090-443c-895c-3b1cf442dec4",
            "createdDateTime": "2019-03-26T12:48:46Z",
            "lastActionDateTime": "2019-03-26T14:04:47Z",
            "status": "Succeeded",
            "locale": "en-US",
            "name": "v4.13 Unified",
            "description": "Unified",
            "properties": {
                "Purpose": "OnlineTranscription,BatchTranscription,LanguageAdaptation",
                "ModelClass": "unified-v4"
            }
        }
    ],
    "statusMessage": "None.",
    "id": "d41615e1-a60e-444b-b063-129649810b3a",
    "createdDateTime": "2019-04-16T09:35:51Z",
    "lastActionDateTime": "2019-04-16T09:38:09Z",
    "status": "Succeeded",
    "locale": "en-US",
    "name": "Simple transcription",
    "description": "Simple transcription description",
    "properties": {
        "PunctuationMode": "DictatedAndAutomatic",
        "ProfanityFilterMode": "Masked",
        "AddWordLevelTimestamps": "True",
        "AddSentiment": "True",
        "Duration": "00:00:02"
    }
}
```
该消息包含用于听录该录制内容的录制 URL 和模型。

## <a name="next-steps"></a>后续步骤

* [获取语音试用订阅](https://www.azure.cn/home/features/cognitive-services/speech-service/)
