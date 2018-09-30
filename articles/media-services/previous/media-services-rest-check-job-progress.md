---
title: 如何使用 REST API 检查作业进度 | Microsoft Docs
description: 了解如何跟踪作业进度。
services: media-services
documentationcenter: ''
author: hayley244
manager: digimobile
editor: ''
ms.assetid: a1a1f956-c035-448a-af9c-5ac15fcce9dd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 12/07/2017
ms.date: 05/07/2018
ms.author: v-haiqya
ms.openlocfilehash: df95363ff596580d6175d9d1b4896a8243793309
ms.sourcegitcommit: 04071a6ddf4e969464d815214d6fdd9813c5c5a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "47426128"
---
# <a name="how-to-check-job-progress"></a>如何：检查作业进度
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-check-job-progress.md)
> * [.NET](media-services-check-job-progress.md)
> * [REST](media-services-rest-check-job-progress.md)
> 
> 

运行作业时，通常需要采用某种方式跟踪作业进度。 可以使用作业的 State 属性来查看该作业的状态。 有关 State 属性的详细信息，请参阅 [作业实体属性](https://docs.microsoft.com/rest/api/media/operations/job#job_entity_properties)。

## <a name="connect-to-media-services"></a>连接到媒体服务

若要了解如何连接到 AMS API，请参阅[通过 Azure AD 身份验证访问 Azure 媒体服务 API](media-services-use-aad-auth-to-access-ams-api.md)。 


## <a name="check-job-progress"></a>检查作业进度

请求：

    GET https://media.chinacloudapi.cn/api/Jobs()?$filter=Id%20eq%20'nb%3Ajid%3AUUID%3Af3c43f94-327f-2347-90bb-3bf79f8559f1'&$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    Host: media.chinacloudapi.cn



响应：

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 450
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: d9b83c57-e26c-4d10-a20b-2be634c4b6a8
    request-id: 91d2be35-20ed-4e1c-a147-e82cd000c193
    x-ms-request-id: 91d2be35-20ed-4e1c-a147-e82cd000c193
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains

    {"odata.metadata":"https://media.chinacloudapi.cn/api/$metadata#Jobs","value":[{"Id":"nb:jid:UUID:f3c43f94-327f-2347-90bb-3bf79f8559f1","Name":"Encoding BigBuckBunny into to H264 Adaptive Bitrate MP4 Set 720p","Created":"2015-02-11T01:46:08.897","LastModified":"2015-02-11T01:46:08.897","EndTime":null,"Priority":0,"RunningDuration":0.0,"StartTime":"2015-02-11T01:46:16.58","State":2,"TemplateId":null,"JobNotificationSubscriptions":[]}]} 


## <a name="see-also"></a>另请参阅

[媒体服务操作 REST API 概述](media-services-rest-how-to-use.md)
<!--Update_Description: update to use AAD token to connect media service-->