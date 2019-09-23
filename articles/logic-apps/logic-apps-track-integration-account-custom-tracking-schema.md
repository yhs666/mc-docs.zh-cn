---
title: 用于 B2B 消息的自定义跟踪架构 - Azure 逻辑应用 | Microsoft Docs
description: 为带有 Enterprise Integration Pack 的 Azure 逻辑应用创建用于监视集成帐户中的 B2B 消息的自定义跟踪架构
services: logic-apps
documentationcenter: ''
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/27/2017
ms.author: v-yiso
ms.date: 10/15/2018
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e3788cf69f300c8c87d28585d69ddc8131f61aa9
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52651973"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-apps"></a>创建用于监视 Azure 逻辑应用中的端到端工作流的自定义跟踪架构

可以为企业到企业工作流的不同部分启用内置跟踪，例如跟踪 AS2 或 X12 消息。 当创建的工作流包含逻辑应用、BizTalk Server、SQL Server 或任何其他层时，用户可以启用自定义跟踪，以便从头至尾记录工作流的事件。 

本文提供的自定义代码可以用于逻辑应用外部的层。 

## <a name="custom-tracking-schema"></a>自定义跟踪架构

```json
{
   "sourceType": "",
   "source": {
      "workflow": {
         "systemId": ""
      },
      "runInstance": {
         "runId": ""
      },
      "operation": {
         "operationName": "",
         "repeatItemScopeName": "",
         "repeatItemIndex": "",
         "trackingId": "",
         "correlationId": "",
         "clientRequestId": ""
      }
   },
   "events": [
      {
         "eventLevel": "",
         "eventTime": "",
         "recordType": "",
         "record": {                
         }
      }
   ]
}
```

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| sourceType |   | 运行的源的类型。 允许的值为“Microsoft.Logic/workflows”  和“custom”  。 （必需） |
| 源 |   | 如果源类型为 **Microsoft.Logic/workflows**，则源信息必须遵循此架构。 如果源类型为“自定义”  ；架构为 JToken。 （必需） |
| systemId | String | 逻辑应用系统 ID。 （必需） |
| runId | String | 逻辑应用运行 ID。 （必需） |
| operationName | String | 操作（例如操作或触发）的名称。 （必需） |
| repeatItemScopeName | String | 如果操作位于 `foreach`/`until` 循环内，请重复项名称。 （必需） |
| repeatItemIndex | Integer | 操作是否位于 `foreach`/`until` 循环内。 指示重复的项索引。 （必需） |
| trackingId | String | 跟踪 ID，用于关联消息。 (可选) |
| correlationId | String | 相关性 ID，用于关联消息。 (可选) |
| ClientRequestId | String | 客户端可以填充它以关联消息。 (可选) |
| EventLevel |   | 事件的级别。 （必需） |
| EventTime |   | 事件的时间（UTC 格式 YYYY-MM-DDTHH:MM:SS.00000Z）。 （必需） |
| recordType |   | 跟踪记录的类型。 允许的值为“自定义”  。 （必需） |
| record |   | 自定义记录类型。 允许的格式为 JToken。 （必需） |
||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B 协议跟踪架构
有关 B2B 协议跟踪架构的信息，请参阅：
* [AS2 跟踪架构](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [X12 跟踪架构](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>后续步骤
* 了解有关[监视 B2B 消息](logic-apps-monitor-b2b-message.md)的详细信息。   
* 了解有关 [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) 的详细信息。
