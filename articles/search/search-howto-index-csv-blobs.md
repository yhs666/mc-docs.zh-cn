---
title: 使用 Azure 搜索 Blob 索引器为 CSV Blob 编制索引 - Azure 搜索
description: 使用 Azure 搜索索引在 Azure Blob 存储中抓取 CSV Blob 以进行全文搜索。 索引器可自动为所选数据源（如 Azure Blob 存储）引入数据。
origin.date: 05/02/2019
ms.date: 06/03/2019
author: mgottein
manager: cgronlun
ms.author: v-biyu
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 11a3d4047cf614676fd2d1200c0c1d2fb3af081b
ms.sourcegitcommit: bf4afcef846cc82005f06e6dfe8dd3b00f9d49f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66004375"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>使用 Azure 搜索 Blob 索引器对 CSV blob 编制索引

> [!Note]
> delimitedText 分析模式目前为预览版，不适合在生产环境中使用。 [REST API 版本 2019-05-06-Preview](search-api-preview.md) 提供了此功能。 目前不支持 .NET SDK。
>

默认情况下，[Azure 搜索 Blob 索引器](search-howto-indexing-azure-blob-storage.md)会将分隔的文本 blob 分析为单个文本块。 但在 blob 含有 CSV 数据的情况下，通常希望将 blob 中的每一行视为一个单独文档。 例如，给定以下带分隔符的文本，可能要将其分析为两个文档，每个文档包含“id”、“datePublished”和“tags”字段： 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

本文介绍如何设置 `delimitedText` 分析模式，以便使用 Azure 搜索 Blob 索引器分析 CSV Blob。 

> [!NOTE]
> 请遵循[一对多索引](search-howto-index-one-to-many-blobs.md)中的索引器配置建议从一个 Azure Blob 输出多个搜索文档。

## <a name="setting-up-csv-indexing"></a>设置 CSV 索引
若要对 CSV blob 编制索引，请使用 `delimitedText` 分析模式根据[创建索引器](https://docs.microsoft.com/rest/api/searchservice/create-indexer)请求创建或更新索引器定义：

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

`firstLineContainsHeaders` 指示每个 blob 的第一行（非空）包含标头。
如果 blob 未包含初始标头行，则应在索引器配置中指定标头： 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

可以使用 `delimitedTextDelimiter` 配置设置来自定义分隔符字符。 例如：

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }


## <a name="request-examples"></a>请求示例
汇总后，以下是完整的有效负载示例。 

数据源： 

    POST https://[service name].search.chinacloudapi.cn/datasources?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

索引器：

    POST https://[service name].search.chinacloudapi.cn/indexers?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

