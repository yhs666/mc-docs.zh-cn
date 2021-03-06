---
title: 导出 Azure IoT Central 数据 | Microsoft Docs
description: 如何将数据从 Azure IoT Central 应用程序导出到 Azure 事件中心、Azure 服务总线和 Azure Blob 存储
services: iot-central
author: viv-liu
ms.author: v-yiso
origin.date: 10/15/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
manager: corywink
ms.openlocfilehash: 7c4294256202fa3ec1d2d965ac1d1591b03f36a7
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335916"
---
# <a name="export-your-azure-iot-central-data-preview-features"></a>导出 Azure IoT Central 数据（预览版功能）

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

*本主题适用于管理员。*

本文介绍如何使用 Azure IoT Central 中的连续数据导出功能将数据导出到 **Azure 事件中心**、**Azure 服务总线**或 **Azure Blob 存储**实例。 数据以 JSON 格式导出，可以包含遥测数据、设备信息和设备模板信息。 使用导出的数据可以获取：

- 热路径见解和分析。 此选项包括在 Azure 流分析中触发自定义规则、在 Azure 逻辑应用中触发自定义工作流，或者通过 Azure Functions 传递数据以进行转换。
- 冷路径分析，例如 Azure 机器学习中的训练模型或 Microsoft Power BI 中的长期趋势分析。

> [!Note]
> 启用连续数据导出时，只能获得从那时之后的数据。 目前，关闭连续数据导出后将暂时无法检索数据。 若要保留更多的历史数据，请及早启用连续数据导出。

## <a name="prerequisites"></a>先决条件

你必须是 IoT Central 应用程序中的管理员

## <a name="set-up-export-destination"></a>设置导出目标

在配置连续数据导出之前，导出目标必须存在。

### <a name="create-event-hubs-namespace"></a>创建事件中心命名空间

如果当前没有可导出到的事件中心命名空间，请执行以下步骤：

1. [在 Azure 门户中创建新的事件中心命名空间](https://ms.portal.azure.cn/#create/Microsoft.EventHub)。 可以在 [Azure 事件中心文档](../../event-hubs/event-hubs-create.md)中进行详细的了解。

2. 选择订阅。 可将数据导出到其他订阅，这些订阅与即用即付 IoT Central 应用程序不在同一个订阅中。 在这种情况下，需使用连接字符串进行连接。

3. 在事件中心命名空间中创建事件中心。 转到命名空间，选择顶部的“+ 事件中心”，以便创建事件中心实例。 

### <a name="create-service-bus-namespace"></a>创建服务总线命名空间

如果当前没有可导出到的服务总线命名空间，请执行以下步骤：

1. [在 Azure 门户中创建新的服务总线命名空间](https://ms.portal.azure.cn/#create/Microsoft.ServiceBus.1.0.5)。 可以在 [Azure 服务总线文档](../../service-bus-messaging/service-bus-create-namespace-portal.md)中进行详细的了解。
2. 选择订阅。 可将数据导出到其他订阅，这些订阅与即用即付 IoT Central 应用程序不在同一个订阅中。 在这种情况下，需使用连接字符串进行连接。

3. 转到服务总线命名空间，选择顶部的“+ 队列”或“+ 主题”，以便创建要向其导出内容的队列或主题。  

选择服务总线作为导出目标时，队列和主题不得启用“会话”或“重复检测”。 如果启用了其中任一选项，则某些消息不会到达队列或主题中。

### <a name="create-storage-account"></a>创建存储帐户

如果当前没有可导出到的 Azure 存储帐户，请执行以下步骤：

1. [在 Azure 门户中创建新的存储帐户](https://ms.portal.azure.cn/#create/Microsoft.StorageAccount-ARM)。 可以详细了解如何创建新的 [Azure Blob 存储帐户](https://aka.ms/blobdocscreatestorageaccount)或 [Azure Data Lake Storage v2 存储帐户](../../storage/blobs/data-lake-storage-quickstart-create-account.md)。

    - 如果选择将数据导出到 Azure Data Lake Storage v2 存储帐户，则必须选择“Blob 存储”作为“帐户类型”。  
    - 可将数据导出到不同于即用即付 IoT Central 应用程序的订阅中的存储帐户。 在此示例中，将使用连接字符串进行连接。

2. 在存储帐户中创建容器。 转到存储帐户。 在“Blob 服务”下选择“浏览 Blob”   。 选择顶部的“+ 容器”以创建新容器。 

## <a name="set-up-continuous-data-export"></a>设置连续数据导出

现在已经有了要将数据导出到的目标，接下来请遵循以下步骤设置连续数据导出。

1. 登录到 IoT Central 应用程序。

2. 在左侧窗格中选择“数据导出”。 

    > [!Note]
    > 如果在左侧窗格中看不到“数据导出”，则说明你不是应用中的管理员。 请与管理员联系以设置数据导出。

3. 选择右上角的“+ 新建”按钮。  选择某个 **Azure 事件中心**、**Azure 服务总线**或 **Azure Blob 存储**作为导出目标。 每个应用程序的最大导出数目是 5。

    ![创建新的连续数据导出](media/howto-export-data/export-new2.png)

4. 在下拉列表框中，选择自己的**事件中心命名空间**、**服务总线命名空间**、**存储帐户命名空间**，或**输入连接字符串**。

    - 只会在 IoT Central 应用程序所在的同一个订阅中看到存储帐户、事件中心命名空间和服务总线命名空间。 若要导出到此订阅外部的某个目标，请选择“输入连接字符串”，然后参阅步骤 5。 
    - 对于试用七天的应用，只能通过连接字符串配置连续数据导出。 试用七天的应用没有关联的 Azure 订阅。

    ![创建新的事件中心](media/howto-export-data/export-eh.png)

5. （可选）如果选中了“输入连接字符串”，则会出现一个用于粘贴连接字符串的新框。  若要获取连接字符串，请执行以下操作：
    - 对于事件中心或服务总线，请转到 Azure 门户中的命名空间。
        - 在“设置”下，选择“共享访问策略”  
        - 选择默认的 **RootManageSharedAccessKey**，或者创建一个新的
        - 复制主连接字符串或辅助连接字符串
    - 对于存储帐户的连接字符串，请在 Azure 门户中转到“存储帐户”：
        - 在“设置”下，选择“访问密钥”  
        - 复制密钥 1 连接字符串或密钥 2 连接字符串

6. 从下拉列表框中选择一个事件中心、队列、主题或容器。

7. 在“要导出的数据”下，通过将相应类型设置为“打开”来指定要导出的数据类型。  

8. 若要启用连续数据导出，请确保“数据导出”  开关为“打开”  。 选择“保存”  。

9. 几分钟后，数据便会出现在所选目标中。

## <a name="export-contents-and-format"></a>导出内容和格式

导出的遥测数据包含设备发送到 IoT Central 的整个消息，而不仅仅是遥测值本身。 导出的设备数据包含对所有设备的属性和元数据的更改，而导出的设备模板则包含对所有设备模板的更改。

对于事件中心和服务总线，数据将以近实时的速度导出。 数据位于 body 属性中，采用 JSON 格式（请参阅下面的示例）。

对于 Blob 存储，数据将每分钟导出一次，其中的每个文件包含自上次导出文件以来发生的更改批。 导出的数据以 JSON 格式放置在三个文件夹中。 存储帐户中的默认路径是：

- 遥测数据： _{container}/{app-id}/telemetry/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_
- 设备： _{container}/{app-id}/devices/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_
- 设备模板： _{container}/{app-id}/deviceTemplates/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_

若要浏览导出的文件，可以在 Azure 门户中导航到相应的文件，并选择“编辑 Blob”选项卡。 


## <a name="telemetry"></a>遥测

对于事件中心和服务总线，在 IoT Central 从设备收到消息后，将快速导出一条新消息，导出的每条消息包含 body 属性中以 JSON 格式发送的整个消息。

对于 Blob 存储，消息将会分批并每分钟导出一次。 导出的文件所用格式与通过 [IoT 中心消息路由](../../iot-hub/iot-hub-csharp-csharp-process-d2c.md)导出到 Blob 存储中的消息文件格式相同。 

> [!NOTE]
> 对于 Blob 存储，请确保设备发送包含 `contentType: application/JSON` 和 `contentEncoding:utf-8`（或 `utf-16`、`utf-32`）的消息。 有关示例，请参阅 [IoT 中心文档](../../iot-hub/iot-hub-devguide-routing-query-syntax.md#message-routing-query-based-on-message-body)。

发送遥测数据的设备由设备 ID 表示（请参阅以下部分）。 若要获取设备的名称，请导出设备数据并使用与设备消息的 **deviceId** 匹配的 **connectionDeviceId** 来关联每条消息。

这是在事件中心、服务总线队列或主题中收到的示例消息。

```json
{
  "body":{
    "temp":67.96099945281145,
    "humid":58.51139305465015,
    "pm25":36.91162432340187
  },
  "annotations":{
    "iothub-connection-device-id":"<deviceId>",
    "iothub-connection-auth-method":"{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id":"<generationId>",
    "iothub-enqueuedtime":1539381029965,
    "iothub-message-source":"Telemetry",
    "x-opt-sequence-number":25325,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539381030200
  },
  "sequenceNumber":25325,
  "enqueuedTimeUtc":"2018-10-12T21:50:30.200Z",
  "offset":"<offset>",
  "properties":{
    "content_type":"application/json",
    "content_encoding":"utf-8"
  }
}
```

这是导出到 Blob 存储的示例记录：

```json
{
  "EnqueuedTimeUtc":"2019-09-26T17:46:09.8870000Z",
  "Properties":{

  },
  "SystemProperties":{
    "connectionDeviceId":"<deviceid>",
    "connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "connectionDeviceGenerationId":"637051167384630591",
    "contentType":"application/json",
    "contentEncoding":"utf-8",
    "enqueuedTime":"2019-09-26T17:46:09.8870000Z"
  },
  "Body":{
    "temp":49.91322758395974,
    "humid":49.61214852573155,
    "pm25":25.87332214661367
  }
}
```

## <a name="devices"></a>设备

快照中的每条消息或记录表示自上次导出消息以来对设备所做的一项或多项更改。 这包括：

- IoT Central 中设备的 `@id`
- 设备的 `name`
- [设备预配服务](../core/howto-connect-nodejs.md?toc=/iot-central/preview/toc.json&bc=/iot-central/preview/breadcrumb/toc.json)中的 `deviceId`
- 设备模板信息
- 属性值

每台设备所属的设备模板由 `instanceOf` 表示。 若要获取设备模板的名称和其他信息，请务必也导出设备模板数据。

删除的设备不会导出。 目前，在导出的消息中没有针对已删除设备的指示器。

对于事件中心和服务总线，包含设备数据的消息将以近实时的速度发送到事件中心、服务总线队列或主题，就像在 IoT Central 中一样。 

对于 Blob 存储，包含自上次写入以来所做的全部更改的新快照将每分钟导出一次。

这是有关事件中心、服务总线队列或主题中的设备和属性数据的示例消息：

```json
{
  "body":{
    "@id":"<id>",
    "@type":"Device",
    "displayName":"Airbox - 266d30aedn5",
    "data":{
      "$cloudProperties":{
        "Color":"blue"
      },
      "EnvironmentalSensor":{
        "thsensormodel":{
          "reported":{
            "value":"A1",
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        },
        "pm25sensormodel":{
          "reported":{
            "value":"P1",
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        }
      },
      "urn_azureiot_DeviceManagement_DeviceInformation":{
        "totalStorage":{
          "reported":{
            "value":3088.1959855710156,
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        },
        "totalMemory":{
          "reported":{
            "value":16005.703586477555,
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        }
      }
    },
    "instanceOf":"<templateId>",
    "deviceId":"<deviceId>",
    "simulated":true
  },
  "annotations":{
    "iotcentral-message-source":"devices",
    "x-opt-partition-key":"<partitionKey>",
    "x-opt-sequence-number":39740,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539274959654
  },
  "partitionKey":"<partitionKey>",
  "sequenceNumber":39740,
  "enqueuedTimeUtc":"2019-10-02T18:14:49.3820326Z",
  "offset":"<offset>"
}
```

这是包含 Blob 存储中设备和属性数据的示例快照。 导出的文件为每条记录包含一行。

```json
{
  "@id":"<id-value>",
  "@type":"Device",
  "displayName":"Airbox",
  "data":{
    "$cloudProperties":{
        "Color":"blue"
    },
    "EnvironmentalSensor":{
      "thsensormodel":{
        "reported":{
          "value":"Neque quia et voluptatem veritatis assumenda consequuntur quod.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "pm25sensormodel":{
        "reported":{
          "value":"Aut alias odio.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    },
    "urn_azureiot_DeviceManagement_DeviceInformation":{
      "totalStorage":{
        "reported":{
          "value":27900.9730905171,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "totalMemory":{
        "reported":{
          "value":4667.82916715811,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    }
  },
  "instanceOf":"<template-id>",
  "deviceId":"<device-id>",
  "simulated":true
}
```

## <a name="device-templates"></a>设备模板

每条消息或快照记录表示自上次导出消息以来对设备模板所做的一项或多项更改。 在每条消息或记录中发送的信息包括：

- 设备模板的 `@id`，与上面的设备流的 `instanceOf` 相匹配
- 设备模板的 `name`
- 设备模板的 `version`
- 设备的 `capabilityModel`，包括其 `interfaces`、遥测数据、属性和命令定义
- `cloudProperties` 定义
- 重写和初始值，与 `capabilityModel` 内联在一起

删除的设备模板不会导出。 目前，在导出的消息中没有针对已删除设备模板的指示器。

对于事件中心和服务总线，包含设备模板数据的消息将以近实时的速度发送到事件中心、服务总线队列或主题，就像在 IoT Central 中一样。 

对于 Blob 存储，包含自上次写入以来所做的全部更改的新快照将每分钟导出一次。

这是有关事件中心、服务总线队列或主题中的设备模板数据的示例消息：

```json
{
  "body":{
    "@id":"<template-id>",
    "@type":"DeviceModelDefinition",
    "displayName":"Airbox",
    "capabilityModel":{
      "@id":"<id>",
      "@type":"CapabilityModel",
      "implements":[
        {
          "@id":"<id>",
          "@type":"InterfaceInstance",
          "name":"EnvironmentalSensor",
          "schema":{
            "@id":"<id>",
            "@type":"Interface",
            "comment":"Requires temperature and humidity sensors.",
            "description":"Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
            "displayName":"Environmental Sensor",
            "contents":[
              {
                "@id":"<id>",
                "@type":"Telemetry",
                "description":"Current temperature on the device",
                "displayName":"Temperature",
                "name":"temp",
                "schema":"double",
                "unit":"Units/Temperature/celsius",
                "valueDetail":{
                  "@id":"<id>",
                  "@type":"ValueDetail/NumberValueDetail",
                  "minValue":{
                    "@value":"50"
                  }
                },
                "visualizationDetail":{
                  "@id":"<id>",
                  "@type":"VisualizationDetail"
                }
              },
              {
                "@id":"<id>",
                "@type":"Telemetry",
                "description":"Current humidity on the device",
                "displayName":"Humidity",
                "name":"humid",
                "schema":"integer"
              },
              {
                "@id":"<id>",
                "@type":"Telemetry",
                "description":"Current PM2.5 on the device",
                "displayName":"PM2.5",
                "name":"pm25",
                "schema":"integer"
              },
              {
                "@id":"<id>",
                "@type":"Property",
                "description":"T&H Sensor Model Name",
                "displayName":"T&H Sensor Model",
                "name":"thsensormodel",
                "schema":"string"
              },
              {
                "@id":"<id>",
                "@type":"Property",
                "description":"PM2.5 Sensor Model Name",
                "displayName":"PM2.5 Sensor Model",
                "name":"pm25sensormodel",
                "schema":"string"
              }
            ]
          }
        },
        {
          "@id":"<id>",
          "@type":"InterfaceInstance",
          "name":"urn_azureiot_DeviceManagement_DeviceInformation",
          "schema":{
            "@id":"<id>",
            "@type":"Interface",
            "displayName":"Device information",
            "contents":[
              {
                "@id":"<id>",
                "@type":"Property",
                "comment":"Total available storage on the device in kilobytes. Ex. 20480000 kilobytes.",
                "displayName":"Total storage",
                "name":"totalStorage",
                "displayUnit":"kilobytes",
                "schema":"long"
              },
              {
                "@id":"<id>",
                "@type":"Property",
                "comment":"Total available memory on the device in kilobytes. Ex. 256000 kilobytes.",
                "displayName":"Total memory",
                "name":"totalMemory",
                "displayUnit":"kilobytes",
                "schema":"long"
              }
            ]
          }
        }
      ],
      "displayName":"AAEONAirbox52"
    },
    "solutionModel":{
      "@id":"<id>",
      "@type":"SolutionModel",
      "cloudProperties":[
        {
          "@id":"<id>",
          "@type":"CloudProperty",
          "displayName":"Color",
          "name":"Color",
          "schema":"string",
          "valueDetail":{
            "@id":"<id>",
            "@type":"ValueDetail/StringValueDetail"
          },
          "visualizationDetail":{
            "@id":"<id>",
            "@type":"VisualizationDetail"
          }
        }
      ]
    },
    "annotations":{
      "iotcentral-message-source":"deviceTemplates",
      "x-opt-partition-key":"<partitionKey>",
      "x-opt-sequence-number":25315,
      "x-opt-offset":"<offset>",
      "x-opt-enqueued-time":1539274985085
    },
    "partitionKey":"<partitionKey>",
    "sequenceNumber":25315,
    "enqueuedTimeUtc":"2019-10-02T16:23:05.085Z",
    "offset":"<offset>"
  }
}
```

这是包含 Blob 存储中设备和属性数据的示例快照。 导出的文件为每条记录包含一行。

```json
{
  "@id":"<template-id>",
  "@type":"DeviceModelDefinition",
  "displayName":"Airbox",
  "capabilityModel":{
    "@id":"<id>",
    "@type":"CapabilityModel",
    "implements":[
      {
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"EnvironmentalSensor",
        "schema":{
          "@id":"<id>",
          "@type":"Interface",
          "comment":"Requires temperature and humidity sensors.",
          "description":"Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
          "displayName":"Environmental Sensor",
          "contents":[
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current temperature on the device",
              "displayName":"Temperature",
              "name":"temp",
              "schema":"double",
              "unit":"Units/Temperature/celsius",
              "valueDetail":{
                "@id":"<id>",
                "@type":"ValueDetail/NumberValueDetail",
                "minValue":{
                  "@value":"50"
                }
              },
              "visualizationDetail":{
                "@id":"<id>",
                "@type":"VisualizationDetail"
              }
            },
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current humidity on the device",
              "displayName":"Humidity",
              "name":"humid",
              "schema":"integer"
            },
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current PM2.5 on the device",
              "displayName":"PM2.5",
              "name":"pm25",
              "schema":"integer"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "description":"T&H Sensor Model Name",
              "displayName":"T&H Sensor Model",
              "name":"thsensormodel",
              "schema":"string"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "description":"PM2.5 Sensor Model Name",
              "displayName":"PM2.5 Sensor Model",
              "name":"pm25sensormodel",
              "schema":"string"
            }
          ]
        }
      },
      {
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"urn_azureiot_DeviceManagement_DeviceInformation",
        "schema":{
          "@id":"<id>",
          "@type":"Interface",
          "displayName":"Device information",
          "contents":[
            {
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available storage on the device in kilobytes. Ex. 20480000 kilobytes.",
              "displayName":"Total storage",
              "name":"totalStorage",
              "displayUnit":"kilobytes",
              "schema":"long"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available memory on the device in kilobytes. Ex. 256000 kilobytes.",
              "displayName":"Total memory",
              "name":"totalMemory",
              "displayUnit":"kilobytes",
              "schema":"long"
            }
          ]
        }
      }
    ],
    "displayName":"AAEONAirbox52"
  },
  "solutionModel":{
    "@id":"<id>",
    "@type":"SolutionModel",
    "cloudProperties":[
      {
        "@id":"<id>",
        "@type":"CloudProperty",
        "displayName":"Color",
        "name":"Color",
        "schema":"string",
        "valueDetail":{
          "@id":"<id>",
          "@type":"ValueDetail/StringValueDetail"
        },
        "visualizationDetail":{
          "@id":"<id>",
          "@type":"VisualizationDetail"
        }
      }
    ]
  }
}
```

## <a name="next-steps"></a>后续步骤

了解如何将数据导出到 Azure 事件中心、Azure 服务总线和 Azure Blob 存储后，接下来请继续学习：

> [!div class="nextstepaction"]
> [如何触发 Azure Functions](../core/howto-trigger-azure-functions.md?toc=/iot-central/preview/toc.json&bc=/iot-central/preview/breadcrumb/toc.json)
