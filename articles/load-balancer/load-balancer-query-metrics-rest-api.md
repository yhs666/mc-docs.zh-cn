---
title: 使用 REST API 检索指标
titlesuffix: Azure Load Balancer
description: 在给定的时间和日期范围内，使用 Azure REST API 收集负载均衡器的运行状况和利用率指标。
services: sql-database
author: WenJason
ms.reviewer: routlaw
manager: digimobile
ms.service: load-balancer
ms.custom: REST, seodec18
ms.topic: article
origin.date: 06/06/2017
ms.date: 03/25/2019
ms.author: v-jay
ms.openlocfilehash: 721706df82e1cc5b58f875a9d84075c39eb05d6e
ms.sourcegitcommit: 41a1c699c77a9643db56c5acd84d0758143c8c2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348523"
---
# <a name="get-load-balancer-utilization-metrics-using-the-rest-api"></a>使用 REST API 获取负载均衡器利用率指标

本操作指南演示如何使用 [Azure REST API](https://docs.microsoft.com/rest/api/azure/) 收集[标准负载均衡器](/load-balancer/load-balancer-standard-overview)在一段时间内处理的字节数。

有关 REST API 的完整参考文档和其他示例，请查看 [Azure Monitor REST reference](https://docs.microsoft.com/rest/api/monitor)（Azure Monitor REST 参考）。 

## <a name="build-the-request"></a>生成请求

使用以下 GET 请求从标准负载均衡器收集 ByteCount 指标。 

```http
GET https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=ByteCount&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
```

### <a name="request-headers"></a>请求标头

以下标头是必需的： 

|请求标头|说明|  
|--------------------|-----------------|  
|Content-Type：|必需。 设置为 `application/json`。|  
|Authorization：|必需。 设置为有效的 `Bearer` [访问令牌](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients)。 |  

### <a name="uri-parameters"></a>URI 参数

| Name | 说明 |
| :--- | :---------- |
| subscriptionId | 用于标识 Azure 订阅的订阅 ID。 如果拥有多个订阅，请参阅[使用多个订阅](/cli/manage-azure-subscriptions-azure-cli?view=azure-cli-latest)。 |
| resourceGroupName | 包含该资源的资源组名称。 可以从 Azure 资源管理器 API、CLI 或门户获取此值。 |
| loadBalancerName | Azure 负载均衡器的名称。 |
| metricnames | 包含有效负载均衡器指标的逗号分隔的列表。 |
| api-version | 用于请求的 API 版本。<br /><br /> 本文档介绍上述 URL 中包括的 api-version `2018-01-01`。  |
| timespan | 查询的时间跨度。 它是具有格式 `startDateTime_ISO/endDateTime_ISO` 的字符串。 设置此可选参数是为了在示例中返回一天的时间。 |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>请求正文

此操作不需请求正文。

## <a name="handle-the-response"></a>处理响应

成功返回指标值列表以后，会返回状态代码 200。 [参考文档](https://docs.microsoft.com/rest/api/monitor/metrics/list#errorresponse)中提供错误代码的完整列表。

## <a name="example-response"></a>示例响应 

```json
{
    "cost": 0,
    "timespan": "2018-06-05T03:00:00Z/2018-06-07T03:00:00Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/Microsoft.Insights/metrics/ByteCount",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "ByteCount",
                "localizedValue": "Byte Count"
            },
            "unit": "Count",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-06T17:24:00Z",
                            "total": 1067921034.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:25:00Z",
                            "total": 0.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:26:00Z",
                            "total": 3781344.0
                        },
                    ]
                }
            ]
        }
    ],
    "namespace": "Microsoft.Network/loadBalancers",
    "resourceregion": "chinaeast"
}
```