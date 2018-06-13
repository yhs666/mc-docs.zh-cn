---
title: 使用 Azure 媒体服务对自定义转换进行编码 | Azure
description: 本主题演示如何使用 Azure 媒体服务对自定义转换进行编码。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
origin.date: 05/17/2018
ms.date: 05/28/2018
ms.author: v-nany
ms.openlocfilehash: 99f024f8c6e543325f7d9d894ca5c78570f8b01d
ms.sourcegitcommit: d4176361d9c6da60729c06cc93a496cb4702d4c2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2018
ms.locfileid: "35324226"
---
# <a name="customize-encoder-presets"></a>自定义编码器预设

使用 Azure 媒体服务进行编码时，可以使用[流文件](stream-files-tutorial-with-api.md)教程中所示的内置 EncoderNamedPreset 或使用自定义预设。 本文中介绍的示例演示了如何创建自定义编解码器和层输出设置。

> [!Note]
>  所有比特率都以位/秒为单位。  

## <a name="download-the-sample"></a>下载示例

使用以下命令将包含完整 .NET Core 示例的 GitHub 存储库克隆到计算机：  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
可以在[此文件](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs)中找到本主题中讨论的代码。

## <a name="create-a-transform-with-a-custom-preset"></a>使用自定义预设创建转换 

创建新实例时，需要指定希望生成的输出内容[转换](https://docs.microsoft.com/rest/api/media/transforms)。 所需参数是 TransformOutput 对象，如以下代码所示。 每个 TransformOutput 包含一个预设。 预设介绍了视频和/或音频处理操作的分步说明，这些操作将用于生成所需的 TransformOutput。 以下 **TransformOutput** 创建自定义编解码器和层输出设置。

在创建时[转换](https://docs.microsoft.com/rest/api/media/transforms)，首先应检查是否其中一个已存在使用**获取**方法，如下面的代码中所示。  在媒体服务 v3 中，如果实体不存在（对名称进行不区分大小写检查），实体上的 **Get** 方法将返回 **null**。

```C#
private static Transform EnsureTransformExists(IAzureMediaServicesClient client, string resourceGroupName, string accountName, string transformName)
{
    // Does a Transform already exist with the desired name? Assume that an existing Transform with the desired name
    // also uses the same recipe or Preset for processing content.
    Transform transform = client.Transforms.Get(resourceGroupName, accountName, transformName);

    if (transform == null)
    {
        // Create a new Transform Outputs array - this defines the set of outputs for the Transform
        TransformOutput[] outputs = new TransformOutput[]
        {
            // Create a new TransformOutput with a custom Standard Encoder Preset
            // This demonstrates how to create custom codec and layer output settings

          new TransformOutput(
                new StandardEncoderPreset(
                    codecs: new Codec[]
                    {
                        // Add an AAC Audio layer for the audio encoding
                        new AacAudio(
                            channels: 2,
                            samplingRate: 48000,
                            bitrate: 128000,
                            profile: AacAudioProfile.AacLc
                        ),
                        // Next, add a H264Video for the video encoding
                       new H264Video (
                            // Set the GOP interval to 2 seconds for both H264Layers
                            keyFrameInterval:TimeSpan.FromSeconds(2),
                             // Add H264Layers, one at HD and the other at SD. Assign a label that you can use for the output filename
                            layers:  new H264Layer[]
                            {
                                new H264Layer (
                                    bitrate: 1000000, // Note that the units is in bits per second
                                    width: "1280",
                                    height: "720",
                                    label: "HD" // This label is used to modify the file name in the output formats
                                ),
                                new H264Layer (
                                    bitrate: 600000, 
                                    width: "640",
                                    height: "480",
                                    label: "SD"
                                )
                            }
                        ),
                        // Also generate a set of PNG thumbnails
                        new PngImage(
                            start: "25%",
                            step: "25%",
                            range: "80%",
                            layers: new PngLayer[]{
                                new PngLayer(
                                    width: "50%", 
                                    height: "50%"
                                )
                            }
                        )
                    },
                    // Specify the format for the output files - one for video+audio, and another for the thumbnails
                    formats: new Format[]
                    {
                        // Mux the H.264 video and AAC audio into MP4 files, using basename, label, bitrate and extension macros
                        // Note that since you have multiple H264Layers defined above, you have to use a macro that produces unique names per H264Layer
                        // Either {Label} or {Bitrate} should suffice
                         
                        new Mp4Format(
                            filenamePattern:"Video-{Basename}-{Label}-{Bitrate}{Extension}"
                        ),
                        new PngFormat(
                            filenamePattern:"Thumbnail-{Basename}-{Index}{Extension}"
                        )
                    }
                ),
                onError: OnErrorType.StopProcessingJob,
                relativePriority: Priority.Normal
            )
        };

        string description = "A simple custom encoding transform with 2 MP4 bitrates";
        // Create the custom Transform with the outputs defined above
        transform = client.Transforms.CreateOrUpdate(resourceGroupName, accountName, transformName, outputs, description);
    }

    return transform;
}
```
