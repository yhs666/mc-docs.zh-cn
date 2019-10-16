---
title: 快速入门：PowerShell 和 REST API - Azure 搜索
description: 使用 PowerShell 的 Invoke-RestMethod 与 Azure 搜索 REST API 创建、加载和查询索引。
origin.date: 05/16/2019
ms.date: 09/26/2019
author: heidisteen
manager: cgronlun
ms.author: v-tawe
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 4690fe05a430f297a158faab30ae27ba066387a7
ms.sourcegitcommit: a5a43ed8b9ab870f30b94ab613663af5f24ae6e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71674242"
---
# <a name="quickstart-create-an-azure-search-index-using-powershell"></a>快速入门：使用 PowerShell 创建 Azure 搜索索引
> [!div class="op_single_selector"]
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-fiddler.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-create-index-portal.md)
> 

本文将引导你完成使用 PowerShell 与 [Azure 搜索服务 REST API](https://docs.microsoft.com/rest/api/searchservice/) 创建、加载和查询 Azure 搜索[索引](search-what-is-an-index.md)的过程。 索引定义和可搜索内容作为正确格式的 JSON 内容在请求正文中提供。

如果没有 Azure 订阅，请在开始之前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)，然后[注册 Azure 搜索](search-create-service-portal.md)。

## <a name="prerequisites"></a>先决条件

本快速入门使用以下服务和工具。 

+ [创建 Azure 搜索服务](search-create-service-portal.md)或在当前订阅下[查找现有服务](https://portal.azure.cn/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)。 可以使用本快速入门的免费服务。 

+ [PowerShell 5.1 或更高版本](https://github.com/PowerShell/PowerShell)，使用 [Invoke-RestMethod](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Invoke-RestMethod) 按序完成交互式步骤。

## <a name="get-a-key-and-url"></a>获取密钥和 URL

REST 调用需要在每个请求中使用服务 URL 和访问密钥。 搜索服务是使用这二者创建的，因此，如果向订阅添加了 Azure 搜索，则请按以下步骤获取必需信息：

1. [登录到 Azure 门户](https://portal.azure.cn/)，在搜索服务的“概述”页中获取 URL。  示例终结点可能类似于 `https://mydemo.search.chinacloudapi.cn`。

2. 在“设置” > “密钥”中，获取有关该服务的完全权限的管理员密钥   。 有两个可交换的管理员密钥，为保证业务连续性而提供，以防需要滚动一个密钥。 可以在请求中使用主要或辅助密钥来添加、修改和删除对象。

![获取 HTTP 终结点和访问密钥](media/search-fiddler/get-url-key.png "Get an HTTP endpoint and access key")

所有请求对发送到服务的每个请求都需要 API 密钥。 具有有效的密钥可以在发送请求的应用程序与处理请求的服务之间建立信任关系，这种信任关系以每个请求为基础。

## <a name="connect-to-azure-search"></a>连接到 Azure 搜索

1. 在 PowerShell 中，创建一个 **$headers** 对象用于存储内容类型和 API 密钥。 将管理 API 密钥 (YOUR-ADMIN-API-KEY) 替换为对搜索服务有效的值。 在整个会话持续时间内，只需设置此标头一次，但需要将它添加到每个请求。 

    ```powershell
    $headers = @{
    'api-key' = '<YOUR-ADMIN-API-KEY>'
    'Content-Type' = 'application/json' 
    'Accept' = 'application/json' }
    ```

2. 创建一个 **$url** 对象，用于指定服务的索引集合。 将服务名称 (YOUR-SEARCH-SERVICE-NAME) 替换为有效的搜索服务。

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE-NAME>.search.chinacloudapi.cn/indexes?api-version=2019-05-06"
    ```

3. 运行 **Invoke-RestMethod** 将 GET 请求发送到服务，并验证连接。 添加 **ConvertTo-Json**，以便可以查看服务发回的响应。

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

   如果服务为空且没有索引，则结果将类似于以下示例。 否则，会看到索引定义的 JSON 表示形式。

    ```
    {
        "@odata.context":  "https://mydemo.search.chinacloudapi.cn/$metadata#indexes",
        "value":  [

                ]
    }
    ```

## <a name="1---create-an-index"></a>1 - 创建索引

除非使用门户，否则在加载数据之前，服务中必须存在一个索引。 此步骤定义索引并将其推送到服务。 此步骤使用[创建索引 REST API](https://docs.microsoft.com/rest/api/searchservice/create-index)。

索引的所需元素包括名称和字段集合。 字段集合定义文档的结构。  每个字段具有一个确定其用法的名称、类型和属性（例如，该字段在搜索结果是否可全文搜索、可筛选或可检索）。 在索引中，必须将一个 `Edm.String` 类型的字段指定为文档标识的键。 

此索引名为“hotels-powershell”，使用下面所示的字段定义。 它是其他演练中使用的一个更大 [Hotels 索引](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON)的子集。 为简明起见，本快速入门已对其进行修整。

1. 将此示例粘贴到 PowerShell，以创建包含索引架构的 **$body** 对象。

    ```powershell
    $body = @"
    {
        "name": "hotels-powershell",  
        "fields": [
            {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
            {"name": "baseRate", "type": "Edm.Double"},
            {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
            {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
            {"name": "hotelName", "type": "Edm.String", "facetable": false},
            {"name": "category", "type": "Edm.String"},
            {"name": "tags", "type": "Collection(Edm.String)"},
            {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
            {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
            {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
            {"name": "rating", "type": "Edm.Int32"},
            {"name": "location", "type": "Edm.GeographyPoint"}
        ]
    }
    "@
    ```

2. 设置服务中的索引集合以及 *hotels-powershell* 索引的 URI。

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.chinacloudapi.cn/indexes/hotels-powershell?api-version=2019-05-06"
    ```

3. 结合 **$url**、 **$headers** 和 **$body** 运行该命令，以在服务中创建索引。 

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Put -Body $body | ConvertTo-Json
    ```

    结果应如下所示（为简明起见，已截断前两个字段后面的内容）：

    ```
    {
        "@odata.context":  "https://mydemo.search.chinacloudapi.cn/$metadata#indexes/$entity",
        "@odata.etag":  "\"0x8D6A99E2DED96B0\"",
        "name":  "hotels-powershell",
        "defaultScoringProfile":  null,
        "fields":  [
                    {
                        "name":  "hotelId",
                        "type":  "Edm.String",
                        "searchable":  false,
                        "filterable":  true,
                        "retrievable":  true,
                        "sortable":  false,
                        "facetable":  false,
                        "key":  true,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
                    {
                        "name":  "baseRate",
                        "type":  "Edm.Double",
                        "searchable":  false,
                        "filterable":  true,
                        "retrievable":  true,
                        "sortable":  true,
                        "facetable":  true,
                        "key":  false,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
    . . .
    ```

> [!Tip]
> 为了进行验证，还可以在门户中检查“索引”列表，或重新运行用于验证服务连接的命令，以查看“索引”集合中列出的 *hotels-powershell* 索引。

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - 加载文档

若要推送文档，请向索引的 URL 终结点发出 HTTP POST 请求。 此任务的 REST API 为 [添加、更新或删除文档](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)。

1. 将此示例粘贴到 PowerShell，以创建包含所要上传的文档的 **$body** 对象。 

    此请求包含两条完整记录和一条不完整的记录。 不完整的记录演示可以上传不完整的文档。 `@search.action` 参数指定如何编制索引。 有效值包括 upload、merge、mergeOrUpload 和 delete。 mergeOrUpload 行为将创建 hotelId 为 3 的新文档，如果该文档已存在，则更新内容。

    ```powershell
    $body = @"
    {
        "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
                "hotelName": "Fancy Stay",
                "category": "Luxury",
                "tags": ["pool", "view", "wifi", "concierge"],
                "parkingIncluded": false,
                "smokingAllowed": false,
                "lastRenovationDate": "2010-06-27T00:00:00Z",
                "rating": 5,
                "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
            },
            {
                "@search.action": "upload",
                "hotelId": "2",
                "baseRate": 79.99,
                "description": "Cheapest hotel in town",
                "hotelName": "Roach Motel",
                "category": "Budget",
                "tags": ["motel", "budget"],
                "parkingIncluded": true,
                "smokingAllowed": true,
                "lastRenovationDate": "1982-04-28T00:00:00Z",
                "rating": 1,
                "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
            },
            {
                "@search.action": "mergeOrUpload",
                "hotelId": "3",
                "baseRate": 129.99,
                "description": "Close to town hall and the river"
            }
        ]
    }
    "@
    ```

1. 将终结点设置为 *hotels-powershell* 文档集合，并包含索引操作 (indexes/hotels-powershell/docs/index)。

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.chinacloudapi.cn/indexes/hotels-powershell/docs/index?api-version=2019-05-06"
    ```

1. 结合 **$url**、 **$headers** 和 **$body** 运行该命令，以将文档载入 hotels-powershell 索引。

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Post -Body $body | ConvertTo-Json
    ```
    结果应类似于以下示例。 应会看到状态代码 201。 有关所有状态代码的说明，请参阅 [HTTP 状态代码（Azure 搜索）](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes)。

    ```
    {
        "@odata.context":  "https://mydemo.search.chinacloudapi.cn/indexes/hotels-powershell/$metadata#Collection(Microsoft.Azure.Search.V2017_11_11.IndexResult)",
        "value":  [
                    {
                        "key":  "1",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "2",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "3",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    }
                ]
    }
    ```

## <a name="3---search-an-index"></a>3 - 搜索索引

此步骤说明如何使用[搜索文档 API](https://docs.microsoft.com/rest/api/searchservice/search-documents) 查询索引。

1. 将终结点设置为 *hotels-powershell* 文档集合，并添加一个 **search** 参数以包含查询字符串。 此字符串是一个空搜索，返回所有文档的未排名列表。

    ```powershell
    $url = 'https://<YOUR-SEARCH-SERVICE>.search.chinacloudapi.cn/indexes/hotels-powershell/docs?api-version=2019-05-06&search=*'
    ```

1. 运行该命令以将 **$url** 发送到服务。

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

    结果应类似于以下输出。

    ```
    {
        "@odata.context":  "https://mydemo.search.chinacloudapi.cn/indexes/hotels-powershell/$metadata#docs(*)",
        "value":  [
                    {
                        "@search.score":  1.0,
                        "hotelId":  "1",
                        "baseRate":  199.0,
                        "description":  "Best hotel in town",
                        "description_fr":  null,
                        "hotelName":  "Fancy Stay",
                        "category":  "Luxury",
                        "tags":  "pool view wifi concierge",
                        "parkingIncluded":  false,
                        "smokingAllowed":  false,
                        "lastRenovationDate":  "2010-06-27T00:00:00Z",
                        "rating":  5,
                        "location":  "@{type=Point; coordinates=System.Object[]; crs=}"
                    },
                    {
                        "@search.score":  1.0,
                        "hotelId":  "2",
                        "baseRate":  79.99,
                        "description":  "Cheapest hotel in town",
                        "description_fr":  null,
                        "hotelName":  "Roach Motel",
                        "category":  "Budget",
                        "tags":  "motel budget",
                        "parkingIncluded":  true,
                        "smokingAllowed":  true,
                        "lastRenovationDate":  "1982-04-28T00:00:00Z",
                        "rating":  1,
                        "location":  "@{type=Point; coordinates=System.Object[]; crs=}"
                    },
                    {
                        "@search.score":  1.0,
                        "hotelId":  "3",
                        "baseRate":  129.99,
                        "description":  "Close to town hall and the river",
                        "description_fr":  null,
                        "hotelName":  null,
                        "category":  null,
                        "tags":  "",
                        "parkingIncluded":  null,
                        "smokingAllowed":  null,
                        "lastRenovationDate":  null,
                        "rating":  null,
                        "location":  null
                    }
                ]
    }
    ```

请尝试其他几个查询示例，以大致了解语法。 可以执行字符串搜索、原义 $filter 查询、限制结果集、将搜索范围限定为特定的字段，等待。

```powershell
# Query example 1
# Search the entire index for the term 'budget'
# Return only the `hotelName` field, "Roach hotel"
$url = 'https://<YOUR-SEARCH-SERVICE>.search.chinacloudapi.cn/indexes/hotels-powershell/docs?api-version=2019-05-06&search=budget&$select=hotelName'

# Query example 2 
# Apply a filter to the index to find hotels cheaper than $150 per night
# Returns the `hotelId` and `description`. Two documents match.
$url = 'https://<YOUR-SEARCH-SERVICE>.search.chinacloudapi.cn/indexes/hotels-powershell/docs?api-version=2019-05-06&search=*&$filter=baseRate lt 150&$select=hotelId,description'

# Query example 3
# Search the entire index, order by a specific field (`lastRenovationDate`) in descending order
# Take the top two results, and show only `hotelName` and `lastRenovationDate`
$url = 'https://<YOUR-SEARCH-SERVICE>.search.chinacloudapi.cn/indexes/hotels-powershell/docs?api-version=2019-05-06&search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate'
```
## <a name="clean-up"></a>清理 

应该删除不再需要的索引。 在免费服务中最多只能创建三个索引。 建议删除所有未真正使用的索引，以便能够顺利完成其他教程中的每个步骤。

```powershell
# Set the URI to the hotel index
$url = 'https://mydemo.search.chinacloudapi.cn/indexes/hotels-powershell?api-version=2019-05-06'

# Delete the index
Invoke-RestMethod -Uri $url -Headers $headers -Method Delete
```

## <a name="next-steps"></a>后续步骤

尝试将法语说明添加到索引。 以下示例包含法语字符串，演示其他搜索操作。 使用 mergeOrUpload 创建字段或者在现有字段中添加内容。 以下字符串需采用 UTF-8 编码。

```json
{
    "value": [
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "1",
            "description_fr": "Meilleur hôtel en ville"
        },
        {
            "@search.action": "merge",
            "hotelId": "2",
            "description_fr": "Hôtel le moins cher en ville"
        }
    ]
}
```
