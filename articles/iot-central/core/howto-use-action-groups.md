---
title: 从 Azure IoT Central 规则运行多个操作 | Microsoft Docs
description: 从单个 IoT Central 规则运行多个操作，并创建可从多个规则运行的可重用操作组。
services: iot-central
author: dominicbetts
ms.author: v-yiso
origin.date: 07/10/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
manager: philmea
ms.openlocfilehash: 402d3feacf2c9910ae26eb509f5c87eaae822e1a
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883273"
---
# <a name="group-multiple-actions-to-run-from-one-or-more-rules"></a>将多个操作分组以从一个或多个规则运行

*本文面向构建人员和管理员。*

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

在 Azure IoT Central 中可以创建规则，以便在满足条件时运行操作。 规则基于设备遥测数据或事件。 例如，当设备温度超过阈值时，可以通知操作员。 本文介绍如何使用 [Azure Monitor](../../azure-monitor/overview.md) 操作组将多个操作附加到 IoT Central 规则。  可将一个操作组附加到多个规则。 [操作组](../../azure-monitor/platform/action-groups.md)是由 Azure 订阅的所有者定义的通知首选项的集合。

## <a name="prerequisites"></a>先决条件

- 即用即付应用程序
- 用于创建和管理 Azure Monitor 操作组的 Azure 帐户与订阅

## <a name="create-action-groups"></a>创建操作组

可以 [在 Azure 门户中创建和管理操作组](../../azure-monitor/platform/action-groups.md)，也可以使用 [Azure 资源管理器模板](../../azure-monitor/platform/action-groups-create-resource-manager-template.md)执行这些操作。

操作组可以：

- 发送电子邮件、短信等通知，或拨打语音电话。
- 运行某个操作，例如调用 Webhook。

以下屏幕截图显示了一个可以发送电子邮件和短信通知以及调用 Webhook 的操作组：

![操作组](media/howto-use-action-groups/actiongroup.png)

若要在 IoT Central 规则中使用某个操作组，该操作组必须与 IoT Central 应用程序位于同一 Azure 订阅中。

## <a name="use-an-action-group"></a>使用操作组

若要在 IoT Central 应用程序中使用操作组，请先创建一个遥测或事件规则。 将操作添加到规则时，请选择“Azure Monitor 操作组”： 

![选择操作](media/howto-use-action-groups/chooseaction.png)

从 Azure 订阅中选择操作组：

![选择操作组](media/howto-use-action-groups/chooseactiongroup.png)

选择“保存”  。 该操作组随即显示在触发规则时要运行的操作列表中：

![保存的操作组](media/howto-use-action-groups/savedactiongroup.png)

下表汇总了发送到受支持操作类型的信息：

| 操作类型 | 输出格式 |
| ----------- | -------------- |
| Email       | 标准 IoT Central 电子邮件模板 |
| SMS         | Azure IoT Central alert: ${applicationName} - "${ruleName}" triggered on "${deviceName}" at ${triggerDate} ${triggerTime} |
| 语音       | Azure I.O.T Central alert: rule "${ruleName}" triggered on device "${deviceName}" at ${triggerDate} ${triggerTime}, in application ${applicationName} |
| Webhook     | { "schemaId" :"AzureIoTCentralRuleWebhook", "data": {[regular webhook payload](#payload)} } |

以下文本是来自操作组的示例短信：

`iotcentral: Azure IoT Central alert: Sample Contoso 22xu4spxjve - "Low pressure alert" triggered on "Refrigerator 2" at March 20, 2019 10:12 UTC`

<a id="payload"></a> 以下 JSON 显示了一个示例 Webhook 操作有效负载：

```json
{
  "schemaId":"AzureIoTCentralRuleWebhook",
  "data":{
    "id":"97ae27c4-17c5-4e13-9248-65c7a2c57a1b",
    "timestamp":"2019-03-20T10:53:17.059Z",
    "rule":{
      "id":"031b660e-528d-47bb-b33d-f1158d7e31bf",
      "name":"Low pressure alert",
      "enabled":true,
      "deviceTemplate":{
        "id":"c318d580-39fc-4aca-b995-843719821049",
        "version":"1.0.0"
      }
    },
    "device":{
      "id":"2383d8ba-c98c-403a-b4d5-8963859643bb",
      "name":"Refrigerator 2",
      "simulated":true,
      "deviceId":"2383d8ba-c98c-403a-b4d5-8963859643bb",
      "deviceTemplate":{
        "id":"c318d580-39fc-4aca-b995-843719821049",
        "version":"1.0.0"
      },
      "measurements":{
        "telemetry":{
           "pressure":343.269190673549
        }
      }
    },
    "application":{
      "id":"8e70742b-0d5c-4a1d-84f1-4dfd42e61c7b",
      "name":"Sample Contoso",
      "subdomain":"sample-contoso"
    }
  }
}
```

## <a name="next-steps"></a>后续步骤

了解如何在规则中使用操作组后，建议接下来了解如何[管理设备](howto-manage-devices.md)。
