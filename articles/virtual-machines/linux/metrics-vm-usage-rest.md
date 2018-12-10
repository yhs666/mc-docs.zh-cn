---
title: 使用 REST API 获取 Azure 虚拟机使用情况数据 | Azure
description: 使用 Azure REST API 收集虚拟机的使用情况指标。
services: virtual-machines
author: rockboyfor
ms.reviewer: routlaw
manager: digimobile
ms.service: load-balancer
ms.custom: REST
ms.topic: article
origin.date: 06/13/2018
ms.date: 09/24/2018
ms.author: v-yeche
ms.openlocfilehash: 0989ac14bbcb54e2003b08f83a1c8b1ac1a44399
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52643605"
---
# <a name="get-virtual-machine-usage-metrics-using-the-rest-api"></a>使用 REST API 获取虚拟机使用情况指标

本示例演示如何使用 [Azure REST API](https://docs.microsoft.com/rest/api/azure/) 检索 [Linux 虚拟机](/virtual-machines/linux/monitor)的 CPU 使用情况。

有关 REST API 的完整参考文档和其他示例，请查看 [Azure Monitor REST reference](https://docs.microsoft.com/rest/api/monitor)（Azure Monitor REST 参考）。 

## <a name="build-the-request"></a>生成请求

使用以下 GET 请求从虚拟机收集[百分比 CPU 指标](/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftcomputevirtualmachines)

```http
GET https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmname}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=Percentage%20CPU&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
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
| subscriptionId | 用于标识 Azure 订阅的订阅 ID。 如果拥有多个订阅，请参阅[使用多个订阅](https://docs.azure.cn/zh-cn/cli/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions)。 |
| resourceGroupName | 与资源关联的 Azure 资源组的名称。 可从 Azure 资源管理器 API、CLI 或门户获取此值。 |
| vmname | Azure 虚拟机的名称。 |
| metricnames | 包含有效[负载均衡器指标](/load-balancer/load-balancer-standard-diagnostics)的逗号分隔的列表。 |
| api-version | 用于请求的 API 版本。<br /><br /> 本文档介绍上述 URL 中包括的 api-version `2018-01-01`。  |
| timespan | 具有以下 `startDateTime_ISO/endDateTime_ISO` 格式的字符串，用于定义已返回指标的时间范围。 设置此可选参数是为了在示例中返回一天的时间。 |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>请求正文

此操作不需请求正文。

## <a name="handle-the-response"></a>处理响应

成功返回指标值列表以后，会返回状态代码 200。 [参考文档](https://docs.microsoft.com/rest/api/monitor/metrics/list#errorresponse)中提供错误代码的完整列表。

## <a name="example-response"></a>示例响应 

```json
{
    "cost": 0,
    "timespan": "2018-06-08T23:48:10Z/2018-06-09T00:48:10Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmname}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=Percentage%20CPU",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "Percentage CPU",
                "localizedValue": "Percentage CPU"
            },
            "unit": "Percent",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-08T23:48:00Z",
                            "average": 0.44
                        },
                        {
                            "timeStamp": "2018-06-08T23:49:00Z",
                            "average": 0.31
                        },
                        {
                            "timeStamp": "2018-06-08T23:50:00Z",
                            "average": 0.29
                        },
                        {
                            "timeStamp": "2018-06-08T23:51:00Z",
                            "average": 0.29
                        },
                        {
                            "timeStamp": "2018-06-08T23:52:00Z",
                            "average": 0.285
                        } ]
                } ]
        } ]
}
```
<!-- Update_Description: update link -->

