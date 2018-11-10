---
title: 使用 REST API 创建 Azure 负载均衡器 | Microsoft Docs
description: 了解如何使用 REST API 创建 Azure 负载均衡器。
services: load-balancer
documentationcenter: na
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: load-balancer
origin.date: 06/06/2018
ms.date: 11/05/2018
ms.author: v-jay
ms.openlocfilehash: 2734915f4a93de911e08b8d11657a530f3a412a3
ms.sourcegitcommit: 9be84d4dc546d66a0d9d1d2be67dd79c84b2c210
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50408862"
---
# <a name="create-an-azure-basic-load-balancer-using-rest-api"></a>使用 REST API 创建 Azure 基本负载均衡器

Azure 负载均衡器根据规则和运行状况探测，将抵达负载均衡器前端的新入站流量分配到后端池实例。 负载均衡器以两种 SKU 提供：“基本”和“标准”。 若要了解两种 SKU 版本的差异，请参阅[负载均衡器 SKU 比较](load-balancer-overview.md#skus)。
 
本操作方法指南介绍如何使用 [Azure REST API](https://docs.microsoft.com/rest/api/azure/) 创建 Azure 基本负载均衡器，以便对 Azure 虚拟网络中跨多个 VM 的传入请求进行负载均衡。 [Azure 负载均衡器 REST 参考](https://docs.microsoft.com/rest/api/load-balancer/)中提供了完整的参考文档和其他示例。
 
## <a name="build-the-request"></a>生成请求
使用以下 HTTP PUT 请求创建新的 Azure 基本负载均衡器。
 ```HTTP
  PUT https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}?api-version=2018-02-01
  ```
### <a name="uri-parameters"></a>URI 参数

|Name  |In  |必须 |类型 |说明 |
|---------|---------|---------|---------|--------|
|subscriptionId   |  path       |  True       |   字符串      |  可以唯一标识 Azure 订阅的订阅凭据。 此订阅 ID 是每个服务调用的 URI 的一部分。      |
|resourceGroupName     |     path    | True        |  字符串       |   资源组的名称。     |
|loadBalancerName     |  path       |      True   |    字符串     |    负载均衡器的名称。    |
|api-version    |   查询     |  True       |     字符串    |  客户端 API 版本。      |



### <a name="request-body"></a>请求正文

唯一必需的参数为 `location`。 如果不定义 *SKU* 版本，则会默认创建基本负载均衡器。  请使用[可选参数](https://docs.microsoft.com/rest/api/load-balancer/loadbalancers/createorupdate#request-body)来自定义负载均衡器。

| Name | 类型 | 说明 |
| :--- | :--- | :---------- |
| location | 字符串 | 资源位置。 使用[列出位置](https://docs.microsoft.com/rest/api/resources/subscriptions/listlocations)操作获取位置的当前列表。 |


## <a name="example-create-and-update-a-basic-load-balancer"></a>示例：创建和更新基本负载均衡器

### <a name="create-a-basic-load-balancer"></a>创建基本负载均衡器
在此步骤中，请在“中国东部”位置的 *rg1* 资源组中创建名为 *lb* 的基本负载均衡器。
#### <a name="sample-request"></a>示例请求

  ```HTTP    
  PUT https://management.chinacloudapi.cn/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb?api-version=2018-02-01
  ```
#### <a name="request-body"></a>请求正文

  ```JSON
   {
    "location": "chinaeast",
   }
  ```
