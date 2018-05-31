---
title: 从 HTTP URL 创建 Azure 媒体服务作业输入 | Azure
description: 本主题介绍如何从 HTTP URL 创建作业输入。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
origin.date: 03/19/2018
ms.date: 05/28/2018
ms.author: v-nany
ms.openlocfilehash: e8e93c7e5ae01d7c317a08acdb30854b6d91d60b
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475113"
---
# <a name="create-a-job-input-from-an-https-url"></a>从 HTTP URL 创建作业输入

在媒体服务 v3 中提交作业来处理视频时，必须告知媒体服务查找输入视频的位置。 其中一个选项是指定 HTTP URL 作为作业输入（如本例中所示）。 有关完整示例，请参阅此 [github 示例](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs)。

## <a name="net-sample"></a>.Net 示例

以下代码说明了如何使用 HTTPS URL 输入创建作业。

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

