---
title: " 使用 Azure 门户将文件上传到媒体服务帐户 | Azure"
description: "本教程指导完成利用 Azure 门户将文件上传到媒体服务帐户中的步骤"
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
oringin.date: 02/13/2017
ms.date: 08/07/2017
ms.author: v-haiqya
ms.openlocfilehash: f02a4135d20c630b207f10b17fbd11aa0dbba687
ms.sourcegitcommit: dc2d05f1b67f4988ef28a0931e6e38712f4492af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2017
---
# <a name="upload-files-into-a-media-services-account-using-the-azure-portal"></a>使用 Azure 门户将文件上传到媒体服务帐户
> [!div class="op_single_selector"]
> * [门户](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 
> [!NOTE]
> 要完成本教程，需要一个 Azure 帐户。 有关详细信息，请参阅 [Azure 试用版](https://www.azure.cn/pricing/1rmb-trial/)。 
> 

在媒体服务中，可以将数字文件上传到资产中。 资产可包含视频、音频、图像、缩略图集合、文本轨道和隐藏式字幕文件（以及这些文件的相关元数据）。） 上传文件完成后，相关内容即安全地存储在云中供后续处理和流式处理。

## <a name="upload-files"></a>上传文件

>[!NOTE]
>在媒体服务中进行处理时，系统支持的最大文件大小存在限制。 有关文件大小限制的详细信息，请参阅[此主题](media-services-quotas-and-limitations.md)。
>

1. 在 [Azure 门户](https://portal.azure.cn/)中，选择 Azure 媒体服务帐户。
2. 在“设置”边栏选项卡中，单击“资产”。

    ![上传文件](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. 单击“上传”按钮  。

    此时会显示“上传视频资产”窗口。

   > [!NOTE]
   > 没有文件大小限制。
   > 
   > 
4. 浏览到计算机中所需视频的位置，选中该视频，并点击“确定”。  

    上传开始，可以在文件名下看到进度。  

上传完成后，会看到“资产”窗口中列出新的资产。 

## <a name="next-steps"></a>后续步骤
现即可编码已上传的资产。 有关详细信息，请参阅[对资产进行编码](media-services-portal-encode.md)。

也可使用 Azure Functions 根据到达已配置容器的文件触发编码作业。 有关详细信息，请参阅[此示例](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ )。

<!--Update_Description: new file-->