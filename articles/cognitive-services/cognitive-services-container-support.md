---
title: 容器支持
titleSuffix: Azure Cognitive Services
description: 了解 Docker 容器如何使认知服务深入了解你的数据。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
origin.date: 08/21/2019
ms.date: 10/11/2019
ms.author: v-tawe
ms.openlocfilehash: 0e035e0ac9b9b1c709bf54e85b608ebae1d6da60
ms.sourcegitcommit: c21b37e8a5e7f833b374d8260b11e2fb2f451782
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72583699"
---
# <a name="container-support-in-azure-cognitive-services"></a>Azure 认知服务中的容器支持

Azure 认知服务中的容器支持让开发人员能够使用与 Azure 中可用的 API 一样丰富的 API，并能够灵活地选择部署和托管随附 [Docker 容器](https://www.docker.com/what-container)的服务的位置。 目前已在 Azure 认知服务的子集中发布容器支持预览版，这些服务包括：

<!-- * [Computer Vision][cv-containers] -->
<!-- * [Face][fa-containers] -->

* [语言理解 (LUIS)][lu-containers]
* [语音服务][sp-containers]
* [文本分析][ta-containers]

容器化是一种软件分发方法，其中应用程序或服务（包括其依赖关系和配置）被一起打包为容器映像。 如果几乎不进行修改，可将容器映像部署在容器主机上。 容器彼此隔离并与基础操作系统隔离，内存占用小于虚拟机。 容器可以从容器映像中实例化以用于短期任务，并在不再需要时将其删除。

认知服务资源可在 [Azure](https://www.azure.cn) 上获得。 登录到 [Azure 门户](https://portal.azure.cn/)，创建和浏览适用于这些服务的 Azure 资源。

## <a name="features-and-benefits"></a>功能和优势

- **对数据的控制**：允许客户选择认知服务在何处处理其数据。 对于不能将数据发送到云，但需要访问认知服务技术的客户，此功能非常重要。 支持混合环境中的一致性 - 跨数据、管理、标识和安全性。
- **对模型更新的控制**：为客户提供其解决方案中部署的模型的版本控制和更新方面的灵活性。
- **可移植的体系结构**：支持创建可在 Azure、本地和边缘部署的可移植应用程序体系结构。 可直接将容器部署到 [Azure Kubernetes 服务](../aks/index.yml)或部署到 [Azure Stack](/azure-stack/operator) 的 [Kubernetes](https://kubernetes.io/) 群集。
- **高吞吐量/低延迟**：通过使以物理方式运行的认知服务更深入了解其应用程序逻辑和数据，为客户提供缩放功能，以满足高吞吐量和低延迟扩展要求。 容器不限制每秒综合事务数 (TPS)，如果提供了必要的硬件资源，它还可进行纵向或横向扩展，来应对需求。 

## <a name="containers-in-azure-cognitive-services"></a>Azure 认知服务中的容器

Azure 认知服务容器提供以下一组 Docker 容器，其中每个容器都包含 Azure 认知服务中的服务的功能子集：

| 服务 | 支持的定价层 | 容器 | 说明 |
|---------|----------|----------|-------------|
|[LUIS][lu-containers] |F0、S0|**LUIS**（[映像](https://hub.docker.com/_/microsoft-azure-cognitive-services-luis)）|可将已训练或已发布的语言理解模型（也称为 LUIS 应用）加载到 docker 容器中并提供对容器的 API 终结点中的查询预测的访问权限。 可以从容器中收集查询日志并将这些日志上传回 [LUIS 门户](https://luis.azure.cn)以提高应用的预测准确性。|
|[语音服务 API][sp-containers] |F0、S0|**文本转语音** |将文本转换为自然发音的语音。|
|[文本分析][ta-containers] |F0、S|关键短语提取（[映像](https://hub.docker.com/_/microsoft-azure-cognitive-services-keyphrase)）  |提取关键短语，以标识要点。 例如，针对输入文本“The food was delicious and there were wonderful staff”，该 API 会返回谈话要点：“food”和“wonderful staff”。 |
|[文本分析][ta-containers]|F0、S|语言检测（[映像](https://hub.docker.com/_/microsoft-azure-cognitive-services-language)）  |针对多达 120 种语言，检测输入文本是使用哪种语言编写的，并报告请求中提交的每个文档的单个语言代码。 语言代码与表示评分强度的评分相搭配。 |
|[文本分析][ta-containers]|F0、S|情绪分析（[映像](https://hub.docker.com/_/microsoft-azure-cognitive-services-sentiment)）  |分析原始文本，获取正面或负面情绪的线索。 此 API 针对每个文档返回介于 0 和 1 之间的情绪评分，1 是最积极的评分。 分析模型已使用 Microsoft 提供的大量文本正文和自然语言技术进行预先训练。 对于[选定的语言](./text-analytics/language-support.md)，该 API 可以分析和评分提供的任何原始文本，并直接将结果返回给调用方应用程序。 |

<!--
|[Personalizer](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409) |F0, S0|**Personalizer** ([image](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409))|Azure Personalizer is a cloud-based API service that allows you to choose the best experience to show to your users, learning from their real-time behavior.|
-->

此外，认知服务[**一体化产品/服务**](https://portal.azure.cn/#create/Microsoft.CognitiveServices)资源密钥支持某些容器。 可以为以下服务创建单个认知服务一体化资源，并在支持的服务之间使用相同的计费密钥：

* 计算机视觉
* 人脸
* LUIS
* 文本分析

## <a name="container-availability-in-azure-cognitive-services"></a>Azure 认知服务中的容器可用性

Azure 认知服务容器通过 Azure 订阅公开发布，并可以从 Microsoft 容器注册表或 Docker 中心拉取 Docker 容器映像。 可以使用 [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) 命令从相应注册表下载容器映像。

> [!IMPORTANT]
> 目前，你必须完成注册过程才能访问以下容器，在此过程中，你需要填写并提交一份调查问卷，其中包含有关你、你的公司以及你要实施这些容器的用例的问题。 一旦被授予访问权限并提供了凭据，你就可以从 Azure 容器注册表托管的专用容器注册表中拉取容器映像。
> * [文本转语音](Speech-Service/speech-container-howto.md#request-access-to-the-container-registry)

[!INCLUDE [Container repositories and images](containers/includes/cognitive-services-container-images.md)]

## <a name="prerequisites"></a>先决条件

使用 Azure 认知服务容器之前，必须先满足以下先决条件：

**Docker 引擎**：必须在本地安装 Docker 引擎。 Docker 提供用于在 [macOS](https://docs.docker.com/docker-for-mac/)、[Linux](https://docs.docker.com/engine/installation/#supported-platforms) 和 [Windows](https://docs.docker.com/docker-for-windows/) 上配置 Docker 环境的包。 在 Windows 上，必须将 Docker 配置为支持 Linux 容器。 还可将 Docker 容器直接部署到 [Azure Kubernetes 服务](../aks/index.yml)。

必须将 Docker 配置为允许容器连接 Azure 并向其发送账单数据。

**熟悉 Microsoft 容器注册表和 Docker**：应对 Microsoft 容器注册表和 Docker 概念有基本的了解，例如注册表、存储库、容器和容器映像，以及基本的 `docker` 命令的知识。

有关 Docker 和容器的基础知识，请参阅 [Docker 概述](https://docs.docker.com/engine/docker-overview/)。

各容器还可以有其自己的要求，包括服务器和内存分配要求。

[!INCLUDE [Discoverability of more container information](../../includes/cognitive-services-containers-discoverability.md)]

## <a name="next-steps"></a>后续步骤

了解可以用于认知服务的[容器配方](containers/container-reuse-recipe.md)。

安装和浏览 Azure 认知服务中的容器提供的功能：

* [语言理解 (LUIS) 容器][lu-containers]
* [语音服务容器][sp-containers]
* [文本分析容器][ta-containers]

<!--* [Personalizer containers](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409)
-->

[lu-containers]: luis/luis-container-howto.md
[sp-containers]: speech-service/speech-container-howto.md
[ta-containers]: text-analytics/how-tos/text-analytics-how-to-install-containers.md

<!-- Update_Description: content update -->