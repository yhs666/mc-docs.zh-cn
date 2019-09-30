---
title: 安装语音容器 - 语音服务
titleSuffix: Azure Cognitive Services
description: 安装和运行语音容器。 文本转语音可将输入文本转换为类似人类的合成语音。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 06/19/2019
ms.date: 09/23/2019
ms.author: v-tawe
ms.openlocfilehash: f2fba09d533aee5e50c0ee8c4777cacda7a8e7c5
ms.sourcegitcommit: 73a8bff422741faeb19093467e0a2a608cb896e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2019
ms.locfileid: "71267095"
---
# <a name="install-and-run-speech-service-containers"></a>安装和运行语音服务容器

语音容器使客户能够构建一个经过优化的语音应用程序体系结构，以利用强大的云功能和边缘位置。 

语音容器为“文本转语音”。  

|函数|功能|最新版本|
|-|-|--|
|文本转语音| <li>将文本转换为自然发音的语音。 使用纯文本输入或语音合成标记语言 (SSML)。 |1.2.0|

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial-full/?form-type=identityauth)。

## <a name="prerequisites"></a>先决条件

使用语音容器之前，必须满足以下先决条件：

|必须|目的|
|--|--|
|Docker 引擎| 需要在[主计算机](#the-host-computer)上安装 Docker 引擎。 Docker 提供用于在 [macOS](https://docs.docker.com/docker-for-mac/)、[Windows](https://docs.docker.com/docker-for-windows/) 和 [Linux](https://docs.docker.com/engine/installation/#supported-platforms) 上配置 Docker 环境的包。 有关 Docker 和容器的基础知识，请参阅 [Docker 概述](https://docs.docker.com/engine/docker-overview/)。<br><br> 必须将 Docker 配置为允许容器连接 Azure 并向其发送账单数据。 <br><br>  在 Windows 上，还必须将 Docker 配置为支持 Linux 容器。<br><br>|
|熟悉 Docker | 应对 Docker 概念有基本的了解，例如注册表、存储库、容器和容器映像，以及基本的 `docker` 命令的知识。| 
|语音资源 |若要使用这些容器，必须具有：<br><br>一个用于获取关联 API 密钥和终结点 URI 的 Azure 语音资源。  可在 Azure 门户的**语音**“概述”和“密钥”页上获取两个值。 必须获取这两个值才能启动容器。<br><br>**{API_KEY}** ：“密钥”页上提供的两个可用资源密钥中的一个 <br><br>**{ENDPOINT_URI}** ：“概述”页上提供的终结点 |

## <a name="request-access-to-the-container-registry"></a>请求访问容器注册表

在请求访问容器之前，必须先填写并提交[认知服务语音容器请求表单](https://aka.ms/speechcontainerspreview/)。 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>主计算机

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>高级矢量扩展支持

 主机是运行 docker 容器的计算机。 主机必须支持[高级矢量扩展](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) (AVX2)。 可使用以下命令检查 Linux 主机是否提供此项支持： 

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```

### <a name="container-requirements-and-recommendations"></a>容器要求和建议

下表显示了为每个语音容器分配的最小和建议的 CPU 核心数和内存。

| 容器 | 最小值 | 建议 |
|-----------|---------|-------------|
|cognitive-services-text-to-speech | 单核，0.5-GB 内存| 双核，1-GB 内存 |

* 每个核心必须至少为 2.6 千兆赫 (GHz) 或更快。

核心和内存对应于 `--cpus` 和 `--memory` 设置，用作 `docker run` 命令的一部分。

**注意**：最小和建议值基于 Docker 限制，而不是基于主机资源。  例如，大型语言模型的文本转语音容器内存映射部分；建议将整个文件装入内存，这需要额外的 4-6 GB。  另外，由于模型要在内存中分页，因此首次运行任一容器可能需要更长的时间。

## <a name="get-the-container-image-with-docker-pull"></a>使用 `docker pull` 获取容器映像

可以使用适用于语音的容器映像。

| 容器 | 存储库 |
|-----------|------------|
| cognitive-services-text-to-speech | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="language-locale-is-in-container-tag"></a>语言区域设置位于容器标记中

`latest` 标记提取 `en-us` 区域设置和 `jessarus` 语音。

<!-- speech-to-text -->

#### <a name="text-to-speech-locales"></a>文本转语音区域设置

除 `latest` 以外的所有标记均采用以下格式，其中，`<culture>` 表示区域设置，`<voice>` 表示容器的语音：

```
<major>.<minor>.<patch>-<platform>-<culture>-<voice>-<prerelease>
```

以下标记是格式示例：

```
1.2.0-amd64-en-us-jessarus-preview
```

下表列出了 1.2.0 版容器中的**文本转语音**支持的区域设置：

|语言区域设置|Tags|支持的语音|
|--|--|--|
|中文|`zh-cn`|huihuirus<br>kangkang-apollo<br>yaoyao-apollo|
|英语 |`en-au`|catherine<br>hayleyrus|
|英语 |`en-gb`|george-apollo<br>hazelrus<br>susan-apollo|
|英语 |`en-in`|heera-apollo<br>priyarus<br>ravi-apollo<br>|
|英语 |`en-us`|jessarus<br>benjaminrus<br>jessa24krus<br>zirarus<br>guy24krus|
|法语|`fr-ca`|caroline<br>harmonierus|
|法语|`fr-fr`|hortenserus<br>julie-apollo<br>paul-apollo|
|德语|`de-de`|hedda<br>heddarus<br>stefan-apollo|
|意大利语|`it-it`|cosimo-apollo<br>luciarus|
|日语|`ja-jp`|ayumi-apollo<br>harukarus<br>ichiro-apollo|
|韩语|`ko-kr`|heamirus|
|葡萄牙语|`pt-br`|daniel-apollo<br>heloisarus|
|西班牙语|`es-es`|elenarus<br>laura-apollo<br>pablo-apollo<br>|
|西班牙语|`es-mx`|hildarus<br>raul-apollo|

### <a name="docker-pull-for-the-speech-containers"></a>语音容器的 Docker 提取

<!-- speech-to-text -->

#### <a name="text-to-speech"></a>文本转语音

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

## <a name="how-to-use-the-container"></a>如何使用容器

一旦容器位于[主计算机](#the-host-computer)上，请通过以下过程使用容器。

1. 使用所需的计费设置[运行容器](#run-the-container-with-docker-run)。 提供 `docker run` 命令的多个[示例](speech-container-configuration.md#example-docker-run-commands)。
1. [查询容器的预测终结点](#query-the-containers-prediction-endpoint)。

## <a name="run-the-container-with-docker-run"></a>通过 `docker run` 运行容器

使用 [docker run](https://docs.docker.com/engine/reference/commandline/run/) 命令运行三个容器中的任意一个。 该命令使用以下参数：

**在预览期**，计费设置必须有效才能启动容器，但不会向你收取使用费。

| 占位符 | Value |
|-------------|-------|
|{API_KEY} | 此密钥用于启动容器，可以在 Azure 门户的“语音密钥”页上找到它。  |
|{ENDPOINT_URI} | 可以在 Azure 门户的“语音概述”页上获取计费终结点 URI 值。|

在以下示例 `docker run` 命令中，请将这些参数替换为自己的值。

### <a name="text-to-speech"></a>文本转语音

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

<!-- speech-to-text -->

## <a name="query-the-containers-prediction-endpoint"></a>查询容器的预测终结点

|容器|终结点|
|--|--|
|文本转语音|http://localhost:5000/speech/synthesize/cognitiveservices/v1|

<!-- speech-to-text -->

#### <a name="for-c"></a>对于 C#

请从使用此 Azure 云初始化调用：

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

更改为使用容器终结点发出此调用：

```csharp
var config = SpeechConfig.FromEndpoint(
    new Uri("ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1"),
    "YourSubscriptionKey");
```

#### <a name="for-python"></a>对于 Python

请从使用此 Azure 云初始化调用

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

更改为使用容器终结点发出此调用：

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, endpoint="ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1")
```

### <a name="text-to-speech"></a>文本转语音

该容器提供 REST 终结点 API，可在[此处](rest-text-to-speech.md)找到这些 API，可在[此处](https://azure.microsoft.com/resources/samples/cognitive-speech-tts/)找到示例。

[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>停止容器

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>故障排除

运行该容器时，该容器将使用 **stdout** 和 **stderr** 来输出信息，这些信息有助于排查启动或运行容器时发生的问题。

## <a name="billing"></a>计费

语音容器使用 Azure 帐户中的“语音”资源向 Azure 发送计费信息。 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

有关这些选项的详细信息，请参阅[配置容器](speech-container-configuration.md)。

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>摘要

本文已介绍语音容器的概念，及其下载、安装和运行工作流。 综上所述：

* 语音为 Docker 提供两个 Linux 容器用于封装文本转语音服务。
* 可从 Azure 中的专用容器注册表下载容器映像。
* 容器映像在 Docker 中运行。
* 可以使用 REST API 或 SDK 通过指定容器的主机 URI 来调用语音容器中的操作。
* 实例化容器时，必须提供计费信息。

> [!IMPORTANT]
>  如果未连接到 Azure 进行计量，则无法授权并运行认知服务容器。 客户需要始终让容器向计量服务传送账单信息。 认知服务容器不会将客户数据（例如，正在分析的图像或文本）发送给 Microsoft。

## <a name="next-steps"></a>后续步骤

* 查看[配置容器](speech-container-configuration.md)了解配置设置
* 使用更多[认知服务容器](../cognitive-services-container-support.md)
