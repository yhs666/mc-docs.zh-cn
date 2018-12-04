---
title: 使用 REST API 获取 Azure 资源运行状况事件 | Microsoft Docs
description: 使用 Azure REST API 获取 Azure 资源的运行状况事件。
services: Resource health
author: rloutlaw
ms.reviewer: routlaw
manager: angerobe
ms.service: service-health
ms.custom: REST
ms.topic: article
origin.date: 06/06/2017
ms.date: 06/23/2018
ms.author: v-yiso
ms.openlocfilehash: 083b5d16131f1cde9a8c02a3f300770b18c02d7f
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52663959"
---
# <a name="get-resource-health-using-the-rest-api"></a>使用 REST API 获取资源运行状况 

本示例文章介绍如何使用 [Azure REST API](https://docs.microsoft.com/en-us/rest/api/azure/) 检索订阅中 Azure 资源的运行状况事件的列表。

有关 REST API 的完整参考文档和其他示例，请查看 [Azure Monitor REST reference](https://docs.microsoft.com/en-us/rest/api/monitor/)（Azure Monitor REST 参考）。 

## <a name="build-the-request"></a>生成请求

使用以下 `GET` HTTP 请求列出 `2018-05-16` 到 `2018-06-20` 这个时间范围内订阅的运行状况事件。

```http
https://management.azure.cn/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&%24filter=eventTimestamp%20ge%20'2018-05-16T04%3A36%3A37.6407898Z'%20and%20eventTimestamp%20le%20'2018-06-20T04%3A36%3A37.6407898Z'
```

### <a name="request-headers"></a>请求标头

以下标头是必需的： 

|请求标头|说明|  
|--------------------|-----------------|  
|Content-Type：|必需。 设置为 `application/json`。|  
|Authorization：|必需。 设置为有效的 `Bearer` [访问令牌](https://docs.microsoft.com/en-us/rest/api/azure/#authorization-code-grant-interactive-clients)。 |  

### <a name="uri-parameters"></a>URI 参数

| Name | 说明 |
| :--- | :---------- |
| subscriptionId | 用于标识 Azure 订阅的订阅 ID。 如果拥有多个订阅，请参阅[使用多个订阅](https://docs.azure.cn/cli/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions)。 |
| api-version | 用于请求的 API 版本。<br /><br /> 本文档介绍上述 URL 中包括的 api-version `2015-04-01`。  |
| $filter | 一个筛选选项，用于缩减返回结果集。 [活动日志操作参考](https://docs.microsoft.com/en-us/rest/api/monitor/activitylogs/list#uri-parameters)中提供了此参数的允许模式。 所示示例捕获了 2018-05-16 到 2018-06-20 这个时间范围内的所有事件 |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>请求正文

此操作不需请求正文。

## <a name="handle-the-response"></a>处理响应

返回状态代码 200 和一个对应于筛选器参数的运行状况事件值的列表，并返回一个 `nextlink` URI，用于检索下一页的结果。

## <a name="example-response"></a>示例响应 

```json
{
  "value": [
    {
      "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
      "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
      },
      "id": "/subscriptions/{subscription-id}/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
      "resourceGroupName": "MSSupportGroup",
      "resourceProviderName": {
        "value": "microsoft.support",
        "localizedValue": "microsoft.support"
      },
      "operationName": {
        "value": "microsoft.support/supporttickets/write",
        "localizedValue": "microsoft.support/supporttickets/write"
      },
      "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
      },
      "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
      "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
      "level": "Informational"
    }
  ],
  "nextLink": "https://management.azure.cn/########-####-####-####-############$skiptoken=######"
}
```