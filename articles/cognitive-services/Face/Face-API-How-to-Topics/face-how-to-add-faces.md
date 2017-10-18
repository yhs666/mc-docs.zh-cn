---
title: "使用人脸 API 添加人脸 | Microsoft Docs"
description: "使用认知服务中的人脸 API 在图像中添加人脸。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: face
ms.topic: article
origin.date: 05/02/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: e6c5e32cd1b0b2a7893d57faad294e22f3e0cee6
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="how-to-add-faces"></a>如何添加人脸

本指南演示有关在人员组（也适用于人脸列表）中添加大量人员和人脸的最佳做法。 这些示例是使用人脸 API 客户端库以 C# 语言编写的。

## <a name="step1"></a>步骤 1：初始化

声明多个变量并实现帮助器函数来计划请求。

- `PersonCount` 是人员总数。
- `CallLimitPerSecond` 是与订阅层相关的每秒最大调用次数。
- `_timeStampQueue` 是一个队列，用于记录请求时间戳。
- `await WaitCallLimitPerSecondAsync()` 将会等到能够有效地将发送下一个请求。

```CSharp
const int PersonCount = 10000;
const int CallLimitPerSecond = 10;
static Queue<DateTime> _timeStampQueue = new Queue<DateTime>(CallLimitPerSecond);

static async Task WaitCallLimitPerSecondAsync()
{
    Monitor.Enter(_timeStampQueue);
    try
    {
        if (_timeStampQueue.Count >= CallLimitPerSecond)
        {
            TimeSpan timeInterval = DateTime.UtcNow - _timeStampQueue.Peek();
            if (timeInterval < TimeSpan.FromSeconds(1))
            {
                await Task.Delay(TimeSpan.FromSeconds(1) - timeInterval);
            }
            _timeStampQueue.Dequeue();
        }
        _timeStampQueue.Enqueue(DateTime.UtcNow);
    }
    finally
    {
        Monitor.Exit(_timeStampQueue);
    }
}
```

## <a name="step2"></a>步骤 2：授权 API 调用

使用客户端库时，订阅密钥通过 FaceServiceClient 类的构造函数传入。 例如：

```CSharp
FaceServiceClient faceServiceClient = new FaceServiceClient("Your subscription key");
```

可以从 Azure 门户的“Marketplace”页获取订阅密钥。 请参阅[订阅](https://www.microsoft.com/cognitive-services/en-us/sign-up)。

## <a name="step3"></a>步骤 3：创建人员组

创建名为“MyPersonGroup”的人员组用于保存人员。
还要将请求时间排队到 `_timeStampQueue`，确保执行整体验证。

```CSharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step4"></a>步骤 4：在人员组中创建人员

以并发方式创建人员，另外还要应用 `await WaitCallLimitPerSecondAsync()` 以避免超出调用限制。

```CSharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceServiceClient.CreatePersonAsync(personGroupId, personName);
});
```

## <a name="step5"></a>步骤 5：将人脸添加到人员

将人脸添加到不同人员的操作将以并发方式进行处理，不过，我们建议按顺序将人脸添加到一个特定的人员。
同样，应调用 `await WaitCallLimitPerSecondAsync()` 来确保请求频率始终在限制范围内。

```CSharp
Parallel.For(0, PersonCount, async i =>
{
    Guid personId = persons[i].PersonId;
    string personImageDir = @"/path/to/person/i/images";

    foreach (string imagePath in Directory.GetFiles(personImageDir, "*.jpg"))
    {
        await WaitCallLimitPerSecondAsync();

        using (Stream stream = File.OpenRead(imagePath))
        {
            await faceServiceClient.AddPersonFaceAsync(personGroupId, personId, stream);
        }
    }
});
```

## <a name="summary"></a>摘要

本指南介绍了创建一个包含大量人员和人脸的人员组的过程。 几条提醒事项：

- 此策略也适用于将人脸添加到人脸列表。 可以并发方式处理在不同人脸列表中添加/删除人脸的操作，对一个特定的人脸列表执行相同的操作应按顺序进行。
- 为了保持简洁，本指南省略了潜在异常的处理方法。 如果想要增强可靠性，应该应用适当的重试策略。

下面是前面解释和演示的功能的简要提醒：

- 使用[人员组 - 创建人员组](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API 创建人员组
- 使用[人员 - 创建人员](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API 创建人员
- 使用[人员 - 添加人员人脸](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API 将人脸添加到人员

## <a name="related"></a> 后续步骤
- [如何识别图像中的人脸](HowtoIdentifyFacesinImage.md)
- [如何检测图像中的人脸](HowtoDetectFacesinImage.md)

