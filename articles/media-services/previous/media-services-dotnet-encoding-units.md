---
title: 通过添加编码单元缩放媒体处理 - Azure |  Microsoft Docs
description: 了解如何使用 .NET 添加编码单元
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/18/2019
ms.date: 09/23/2019
ms.author: v-jay
ms.reviewer: milangada
ms.openlocfilehash: 78e20bec903f42c707b64e88f353c6a80c7a0f0c
ms.sourcegitcommit: 8248259e4c3947aa0658ad6c28f54988a8aeebf8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124389"
---
# <a name="how-to-scale-encoding-with-net-sdk"></a>如何使用 .NET SDK 缩放编码
> [!div class="op_single_selector"]
> * [门户](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>概述
> [!IMPORTANT]
> 请确保查看[概述](media-services-scale-media-processing-overview.md)主题，获取有关调整媒体处理规模的详细信息。
> 
> 

若要使用 .NET SDK 更改预留单位类型和编码预留单位数目，请执行以下操作：

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="media-services-learning-paths"></a>媒体服务学习路径
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

