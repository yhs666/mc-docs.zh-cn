---
title: 示例：向 PersonGroup 添加人脸 - 人脸 API
titleSuffix: Azure Cognitive Services
description: 使用人脸 API 添加图像中的人脸。
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
origin.date: 04/10/2019
ms.date: 05/14/2019
ms.author: v-junlch
ms.openlocfilehash: 4f65f5599cb49f9595c809273083fedae26c422d
ms.sourcegitcommit: 71172ca8af82d93d3da548222fbc82ed596d6256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65668967"
---
# <a name="how-to-add-faces-to-a-persongroup"></a>如何向 PersonGroup 添加人脸

本指南介绍了将大量人员和人脸添加到 PersonGroup 对象的最佳做法。 相同的策略也适用于 LargePersonGroup、FaceList 和 LargeFaceList。 此示例是使用人脸 API .NET 客户端库以 C# 语言编写的。

## <a name="step-1-initialization"></a>步骤 1：初始化

以下代码声明多个变量并实现帮助程序函数，以便对人脸添加请求进行计划。

- `PersonCount` 是人员总数。
- `CallLimitPerSecond` 是与订阅层相关的每秒最大调用次数。
- `_timeStampQueue` 是一个队列，用于记录请求时间戳。
- `await WaitCallLimitPerSecondAsync()` 将会等到能够有效地将发送下一个请求。

```csharp
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

## <a name="step-2-authorize-the-api-call"></a>步骤 2：授权 API 调用

使用客户端库时，必须将订阅密钥传递给 FaceServiceClient 类的构造函数。 例如：

```csharp
FaceServiceClient faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

在 [Azure 门户](https://portal.azure.cn)中创建认知服务后，可以获取订阅密钥。

## <a name="step-3-create-the-persongroup"></a>步骤 3：创建 PersonGroup

创建用于保存人员的 PersonGroup，命名为“MyPersonGroup”。
为了确保整体验证，请求时间排入 `_timeStampQueue` 队列。

```csharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step-4-create-the-persons-to-the-persongroup"></a>步骤 4：创建添加到 PersonGroup 中的人员

以并发方式创建人员，另外还要应用 `await WaitCallLimitPerSecondAsync()` 以避免超出调用限制。

```csharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceServiceClient.CreatePersonAsync(personGroupId, personName);
});
```

## <a name="step-5-add-faces-to-the-persons"></a>步骤 5：向人员添加人脸

虽然可同时处理向不同人员添加人脸，但对于某个人来说，处理则是依序的。
同样，为了确保请求频率在限制范围内，还会调用 `await WaitCallLimitPerSecondAsync()`。

```csharp
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

本指南介绍了如何创建包含大量人员和人脸的 PersonGroup。 请注意以下几点：

- 此策略也适用于 FaceList 和 LargePersonGroup。
- 可同时处理在 LargePersonGroup 中添加/删除不同 FaceList 或 Person 的人脸。
- 应依序完成对 LargePersonGroup 中某个 FaceList 或 Person 执行的一些操作。
- 为简单起见，本指南省略了潜在异常处理。 如果想要增强可靠性，应该应用适当的重试策略。

下面快速提示了之前介绍和展示的功能：

- 使用 [PersonGroup - 创建](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API 创建 PersonGroup
- 使用 [PersonGroup 人员 - 创建](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API 创建人员
- 使用 [PersonGroup 人员 - 添加人脸](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API 向人员添加人脸

## <a name="related-topics"></a>相关主题

- [如何识别图像中的人脸](HowtoIdentifyFacesinImage.md)
- [如何检测图像中的人脸](HowtoDetectFacesinImage.md)
- [如何使用大规模功能](how-to-use-large-scale.md)

<!-- Update_Description: wording update -->