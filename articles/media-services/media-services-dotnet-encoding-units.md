---
title: "通过添加编码单元缩放媒体处理 - Azure |  Microsoft Docs"
description: "了解如何使用 .NET 添加编码单位"
services: media-services
documentationcenter: 
author: hayley244
manager: digimobile
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/09/2017
ms.date: 09/04/2017
ms.author: v-haiqya
ms.openlocfilehash: 3ff01c3f643d2100d48a2f8993058eb56271dc64
ms.sourcegitcommit: 20f589947fbfbe791debd71674f3e4649762b70d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2017
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
> 请确保查看[概述](media-services-scale-media-processing-overview.md)主题，以获取有关缩放媒体处理主题的详细信息。
> 
> 

若要使用 .NET SDK 更改保留单位类型和编码保留单位数目，请执行以下操作：

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

<!--Update_Description: remove support ticket related content-->