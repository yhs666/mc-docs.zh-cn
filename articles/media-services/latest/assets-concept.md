---
title: Azure 媒体服务中的资产 | Azure
description: 本文介绍何为资产以及 Azure 媒体服务如何使用这些资产。
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
ms.openlocfilehash: 2d28052547464e0e5422413f416fcbdab49a70b2
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475135"
---
# <a name="assets"></a>资产

**资产**包含数字文件（包括视频、音频、图像、缩略图集合、文本轨道和隐藏式字幕文件）以及这些文件的相关元数据。 数字文件在上传到资产中后，即可用于媒体服务编码和流式处理工作流。

资产会被映射到 [Azure 存储帐户](storage-account-concept.md)中的 blob 容器，而资产中的文件会作为块 blob 存储在该容器中。 可以使用存储 SDK 客户端与容器中的资产文件进行交互。

当帐户使用常规用途 v2 (GPv2) 存储时，Azure 媒体服务支持 Blob 层。 借助 GPv2，可以将文件移动到冷存储。 冷存储适合存储不再需要的源文件（例如，在它们已编码后）。

在媒体服务 v3 中，可以从资产或 HTTP URL 创建作业输入。 若要创建可用作作业输入的资产，请参阅[从本地文件创建作业输入](job-input-from-local-file-how-to.md)。

此外，请参阅[媒体服务中的存储帐户](storage-account-concept.md)和[转换和作业](transform-concept.md)。

## <a name="asset-definition"></a>资产定义

下表显示了资产的属性并提供了其定义。

|名称|类型|说明|
|---|---|---|
|ID|字符串|资源的完全限定资源 ID。|
|name|字符串|资源的名称。|
|properties.alternateId |字符串|资产的备用 ID。|
|properties.assetId |字符串|资产 ID。|
|properties.container |字符串|资产 Blob 容器的名称。|
|properties.created |字符串|资产的创建日期。|
|properties.description |字符串|资产说明。|
|properties.lastModified |字符串|资产的上次修改日期。|
|properties.storageAccountName |字符串|存储帐户的名称。|
|properties.storageEncryptionFormat |AssetStorageEncryptionFormat |资产加密格式。 None 和 MediaStorageEncryption 之一。|
|type|字符串|资源的类型。|

有关完整定义，请参阅[资产](https://docs.microsoft.com/rest/api/media/assets)。

## <a name="filtering-ordering-and-paging-support"></a>筛选、排序和分页支持

媒体服务支持针对资产的以下 OData 查询选项： 

* $filter 
* $orderby 
* $top 
* $skiptoken 

### <a name="filteringordering"></a>筛选/排序

下表展示了可以如何将这些选项应用于资产属性： 

|名称|筛选器|顺序|
|---|---|---|
|Id|支持：<br/>等于<br/>大于<br/>小于|支持：<br/>升序<br/>降序|
|name|||
|properties.alternateId |支持：<br/>等于||
|properties.assetId |支持：<br/>等于||
|properties.container |||
|properties.created|支持：<br/>等于<br/>大于<br/>小于|支持：<br/>升序<br/>降序|
|properties.description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|type|||

下面的 C# 示例根据创建日期进行筛选：

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

### <a name="pagination"></a>分页

四种已启用的排序顺序每种都支持分页。 

如果查询响应包含许多项（目前为超过 1000），则服务将返回“@odata.nextLink”属性以获取下一页的结果。 这可以用来逐页浏览整个结果集。 用户无法配置页面大小。 

如果在逐页浏览集合的过程中创建或删除了资产，则更改将反映在返回的结果中（如果那些更改位于集合中尚未下载的部分中）。 

下面的 C# 示例展示了如何枚举帐户中的所有资产。

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

有关 REST 示例，请参阅[资产 - 列表](https://docs.microsoft.com/rest/api/media/assets/list)

