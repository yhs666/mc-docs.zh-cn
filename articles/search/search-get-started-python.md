---
title: 快速入门：Python 和 REST API - Azure 搜索
description: 使用 Python、Jupyter Notebooks 和 Azure 搜索 REST API 创建、加载和查询索引。
origin.date: 05/15/2019
ms.date: 06/03/2019
author: heidisteen
manager: cgronlun
ms.author: v-biyu
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 19ddb53daa2963eddab7798631f5c7a4ceb9f1e4
ms.sourcegitcommit: bf4afcef846cc82005f06e6dfe8dd3b00f9d49f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66004739"
---
# <a name="quickstart-create-an-azure-search-index-using-jupyter-python-notebooks"></a>快速入门：使用 Jupyter Python 笔记本创建 Azure 搜索索引
> [!div class="op_single_selector"]
> * [Python (REST)](search-get-started-python.md)
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-fiddler.md)
> * [Portal](search-create-index-portal.md)
> 

使用 Python 和 [Azure 搜索服务 REST API](https://docs.microsoft.com/rest/api/searchservice/) 生成可创建、加载和查询 Azure 搜索[索引](search-what-is-an-index.md)的 Jupyter 笔记本。 本文逐步介绍如何生成你自己的笔记本。 或者，你可以运行一个已完成的笔记本。 若要下载副本，请转到 [Azure-Search-python-samples 存储库](https://github.com/Azure-Samples/azure-search-python-samples)。

如果没有 Azure 订阅，请在开始之前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)，然后[注册 Azure 搜索](search-create-service-portal.md)。

## <a name="prerequisites"></a>先决条件

本快速入门使用以下服务和工具。 

+ [创建 Azure 搜索服务](search-create-service-portal.md)或在当前订阅下[查找现有服务](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)。 可以使用本快速入门的免费服务。 

+ [Anaconda 3.x](https://www.anaconda.com/distribution/#download-section)，提供 Python 3.x 和 Jupyter Notebooks。

## <a name="get-a-key-and-url"></a>获取密钥和 URL

REST 调用需要在每个请求中使用服务 URL 和访问密钥。 搜索服务是使用这二者创建的，因此，如果向订阅添加了 Azure 搜索，则请按以下步骤获取必需信息：

1. [登录到 Azure 门户](https://portal.azure.cn/)，在搜索服务的“概述”页中获取 URL。  示例终结点可能类似于 `https://mydemo.search.chinacloudapi.cn`。

1. 在“设置” > “密钥”中，获取有关该服务的完全权限的管理员密钥   。 有两个可交换的管理员密钥，为保证业务连续性而提供，以防需要滚动一个密钥。 可以在请求中使用主要或辅助密钥来添加、修改和删除对象。

![获取 HTTP 终结点和访问密钥](media/search-fiddler/get-url-key.png "Get an HTTP endpoint and access key")

所有请求对发送到服务的每个请求都需要 API 密钥。 具有有效的密钥可以在发送请求的应用程序与处理请求的服务之间建立信任关系，这种信任关系以每个请求为基础。

## <a name="connect-to-azure-search"></a>连接到 Azure 搜索

打开一个 Jupyter 笔记本，并通过在服务中请求一个索引列表来验证从本地工作站建立的连接。 在装有 Anaconda3 的 Windows 上，可以使用 Anaconda Navigator 来启动笔记本。

1. 创建新的 Python3 笔记本。

1. 在第一个单元格中，加载用于处理 JSON 和构建 HTTP 请求的库。

   ```python
   import json
   import requests
   from pprint import pprint
   ```

1. 在第二个单元格中，输入在每个请求时都用作常量的请求元素。 将搜索服务名称 (YOUR-SEARCH-SERVICE-NAME) 和管理 API 密钥 (YOUR-ADMIN-API-KEY) 替换为有效的值。 

   ```python
    endpoint = 'https://<YOUR-SEARCH-SERVICE-NAME>.search.chinacloudapi.cn/'
    api_version = '?api-version=2019-05-06'
    headers = {'Content-Type': 'application/json',
           'api-key': '<YOUR-ADMIN-API-KEY>' }
   ```

1. 在第三个单元格中构建请求。 此 GET 请求针对搜索服务的索引集合，并选择名称属性。

   ```python
   url = endpoint + "indexes" + api_version + "&$select=name"
   response  = requests.get(url, headers=headers)
   index_list = response.json()
   pprint(index_list)
   ```

1. 运行每个步骤。 如果索引存在，则响应将包含一个索引列表。 在以下屏幕截图中，服务包含 azureblob-index 和 realestate-us-sample 索引。

   ![Jupyter 笔记本中的 Python 脚本，该脚本包含对 Azure 搜索发出的 HTTP 请求](media/search-get-started-python/connect-azure-search.png "Jupyter 笔记本中的 Python 脚本，该脚本包含对 Azure 搜索发出的 HTTP 请求")

   空索引集合返回以下响应：`{'@odata.context': 'https://mydemo.search.chinacloudapi.cn/$metadata#indexes(name)', 'value': []}`

> [!Tip]
> 在免费服务中，最多只能创建三个索引、索引器和数据源。 本快速入门将创建一个索引、索引器和数据源。 在进一步学习之前，请确保有足够的空间可用于创建新对象。

## <a name="1---create-an-index"></a>1 - 创建索引

除非使用门户，否则在加载数据之前，服务中必须存在一个索引。 此步骤使用[创建索引 REST API](https://docs.microsoft.com/rest/api/searchservice/create-index) 将索引架构推送到服务

字段集合定义文档的结构。  索引的所需元素包括名称和字段集合。 每个字段具有一个确定其用法的名称、类型和属性（例如，该字段在搜索结果是否可全文搜索、可筛选或可检索）。 在索引中，必须将一个 `Edm.String` 类型的字段指定为文档标识的键。 

此索引名为“hotels-py”，使用下面所示的字段定义。 它是其他演练中使用的一个更大 [Hotels 索引](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON)的子集。 为简明起见，本快速入门已对其进行修整。


1. 在下一个单元格中，将以下示例粘贴到某个单元格以提供架构。 

    ```python
    index_schema = {
       "name": "hotels-py",  
       "fields": [
         {"name": "HotelId", "type": "Edm.String", "key": "true", "filterable": "true"},
         {"name": "HotelName", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "true", "facetable": "false"},
         {"name": "Description", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "false", "facetable": "false", "analyzer": "en.lucene"},
         {"name": "Description_fr", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "false", "facetable": "false", "analyzer": "fr.lucene"},
         {"name": "Category", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Tags", "type": "Collection(Edm.String)", "searchable": "true", "filterable": "true", "sortable": "false", "facetable": "true"},
         {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Rating", "type": "Edm.Double", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Address", "type": "Edm.ComplexType", 
         "fields": [
         {"name": "StreetAddress", "type": "Edm.String", "filterable": "false", "sortable": "false", "facetable": "false", "searchable": "true"},
         {"name": "City", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "StateProvince", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "PostalCode", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Country", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"}
        ]
       }
      ]
    }
    ```

2. 在另一个单元格中构建请求。 此 PUT 请求针对搜索服务的索引集合，根据在上一步骤中提供的索引架构创建索引。

   ```python
   url = endpoint + "indexes" + api_version
   response  = requests.post(url, headers=headers, json=index_schema)
   index = response.json()
   pprint(index)
   ```

3. 运行每个步骤。

   响应包含架构的 JSON 表示形式。 以下屏幕截图修整了索引架构的某些部分，以便可以看到更多的响应。

    ![请求创建索引](media/search-get-started-python/create-index.png "请求创建索引")

> [!Tip]
> 为了进行验证，还可以在门户中检查“索引”列表，或重新运行服务连接请求以查看“索引”集合中列出的 *hotels-py* 索引。

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - 加载文档

若要推送文档，请向索引的 URL 终结点发出 HTTP POST 请求。 REST API 为[添加、更新或删除文档](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)。 文档源自于 GitHub 上的 [HotelsData](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/HotelsData_toAzureSearch.JSON)。

1. 在新单元格中，提供符合索引架构的三个文档。 指定每个文档的上传操作。

    ```python
    documents = {
        "value": [
        {
        "@search.action": "upload",
        "HotelId": "1",
        "HotelName": "Secret Point Motel",
        "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
        "Description_fr": "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
        "Category": "Boutique",
        "Tags": [ "pool", "air conditioning", "concierge" ],
        "ParkingIncluded": "false",
        "LastRenovationDate": "1970-01-18T00:00:00Z",
        "Rating": 3.60,
        "Address": {
            "StreetAddress": "677 5th Ave",
            "City": "New York",
            "StateProvince": "NY",
            "PostalCode": "10022",
            "Country": "USA"
            }
        },
        {
        "@search.action": "upload",
        "HotelId": "2",
        "HotelName": "Twin Dome Motel",
        "Description": "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
        "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
        "Category": "Boutique",
        "Tags": [ "pool", "free wifi", "concierge" ],
        "ParkingIncluded": "false",
        "LastRenovationDate": "1979-02-18T00:00:00Z",
        "Rating": 3.60,
        "Address": {
            "StreetAddress": "140 University Town Center Dr",
            "City": "Sarasota",
            "StateProvince": "FL",
            "PostalCode": "34243",
            "Country": "USA"
            }
        },
        {
        "@search.action": "upload",
        "HotelId": "3",
        "HotelName": "Triple Landscape Hotel",
        "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
        "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
        "Category": "Resort and Spa",
        "Tags": [ "air conditioning", "bar", "continental breakfast" ],
        "ParkingIncluded": "true",
        "LastRenovationDate": "2015-09-20T00:00:00Z",
        "Rating": 4.80,
        "Address": {
            "StreetAddress": "3393 Peachtree Rd",
            "City": "Atlanta",
            "StateProvince": "GA",
            "PostalCode": "30326",
            "Country": "USA"
        }
      }
     ]
    }
    ```

2. 在另一个单元格中构建请求。 此 POST 请求针对 hotels-py 索引的文档集合，将推送在上一步骤中提供的文档。

   ```python
   url = endpoint + "indexes/hotels-py/docs/index" + api_version
   response  = requests.post(url, headers=headers, json=documents)
   index_content = response.json()
   pprint(index_content)
   ```

3. 运行每个步骤，将文档推送到搜索服务中的索引。 结果应类似于以下示例。 

   ```
   {'@odata.context': "https://mydemo.search.chinacloudapi.cn/indexes('hotels-py')/$metadata#Collection(Microsoft.Azure.Search.V2019_05_06.IndexResult)",
    'value': [{'errorMessage': None,
            'key': '1',
            'status': True,
            'statusCode': 201},
           {'errorMessage': None,
            'key': '2',
            'status': True,
            'statusCode': 201},
           {'errorMessage': None,
            'key': '3',
            'status': True,
            'statusCode': 201}]}
     ```


## <a name="3---search-an-index"></a>3 - 搜索索引

此步骤说明如何使用[搜索文档 REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents) 查询索引。


1. 在新单元格中提供一个查询表达式。 以下示例根据字词“hotels”和“wifi”执行搜索。 它还会返回匹配文档的计数，并选择要包含在搜索结果中的字段。  

   ```python
   searchstring = '&search=hotels wifi&$count=true&$select=HotelId,HotelName'
   ```

2. 构建请求。 此 GET 请求针对 hotels-py 索引的文档集合，将附加在上一步骤中指定的查询。

   ```python
   url = endpoint + "indexes/hotels-py/docs" + api_version + searchstring
   response  = requests.get(url, headers=headers, json=searchstring)
   query = response.json()
   pprint(query)
   ```

   结果应类似于以下输出。

   ```
   {'@odata.context': "https://mydemo.search.chinacloudapi.cn/indexes('hotels-py')/$metadata#docs(*)",
    '@odata.count': 3,
    'value': [{'@search.score': 1.0,
               'HotelId': '1',
               'HotelName': 'Secret Point Motel'},
              {'@search.score': 1.0,
               'HotelId': '2',
               'HotelName': 'Twin Dome Motel'},
              {'@search.score': 1.0,
               'HotelId': '3',
               'HotelName': 'Triple Landscape Hotel'}]}
   ```

3. 请尝试其他几个查询示例，以大致了解语法。 可以应用筛选器、提取排名靠前的两个结果、按特定字段排序，或 

   + `searchstring = '&search=*&$filter=Rating gt 4&$select=HotelId,HotelName,Description'`

   + `searchstring = '&search=hotel&$top=2&$select=HotelId,HotelName,Description'`

   + `searchstring = '&search=pool&$orderby=Address/City&$select=HotelId, HotelName, Address/City, Address/StateProvince'`

## <a name="clean-up"></a>清理 

应该删除不再需要的索引。 在免费服务中最多只能创建三个索引。 对于并未真正使用的任何索引，建议将其删除，以便为其他教程留出空间。

   ```python
  url = endpoint + "indexes/hotels-py" + api_version
  response  = requests.delete(url, headers=headers)
   ```

可以通过返回现有索引的列表来验证索引是否已删除。 如果 hotels-py 已消失，则表示请求成功。

```python
url = endpoint + "indexes" + api_version + "&$select=name"

response  = requests.get(url, headers=headers)
index_list = response.json()
pprint(index_list)
```

## <a name="next-steps"></a>后续步骤

详细了解查询语法和方案。

> [!div class="nextstepaction"]
> [创建基本查询](search-query-overview.md)
