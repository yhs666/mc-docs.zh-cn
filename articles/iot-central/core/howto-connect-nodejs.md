---
title: 将通用 Node.js 客户端应用连接到 Azure IoT Central | Microsoft Docs
description: 如何以设备开发人员的身份将通用 Node.js 设备连接到 Azure IoT Central 应用程序。
author: dominicbetts
ms.author: v-yiso
origin.date: 09/12/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 7697c4f1af9fdb8e2e7a12a7e532f50b879149c9
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883722"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>将泛型客户端应用程序连接到 Azure IoT Central 应用程序 (Node.js)

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

本文介绍如何以设备开发人员的身份将代表真实设备的泛型 Node.js 应用程序连接到 Microsoft Azure IoT Central 应用程序。

## <a name="before-you-begin"></a>准备阶段

若要完成本文中的步骤，需要做好以下准备：

- Azure IoT Central 应用程序。 有关详细信息，请参阅[创建应用程序快速入门](quick-deploy-iot-central.md)。
- 安装了 [Node.js](https://nodejs.org/) 4.0.0 或更高版本的开发计算机。 若要检查版本，可以在命令行中运行 `node --version`。 Node.js 适用于各种操作系统。

## <a name="create-a-device-template"></a>创建设备模板

在 Azure IoT Central 应用程序中，需要一个包含以下度量、设备属性、设置和命令的设备模板。

有关有效属性名称的详细信息，请参阅[标记和属性格式](../../iot-hub/iot-hub-devguide-device-twins.md#tags-and-properties-format)。

### <a name="telemetry-measurements"></a>遥测数据度量

在“度量”页上添加以下遥测项： 

| 显示名称 | 字段名称  | Units | Min | Max | 小数位数 |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| 温度  | 温度 | F     | 60  | 110 | 0              |
| 湿度     | 湿度    | %     | 0   | 100 | 0              |
| 压力     | 压力    | kPa   | 80  | 110 | 0              |

> [!NOTE]
> 遥测度量的数据类型是一个浮点数。

将表中所示字段名称准确输入设备模板中。 如果字段名称与对应的设备代码中的属性名称不匹配，则遥测项无法显示在应用程序中。

### <a name="state-measurements"></a>状态度量

在“度量”页上添加以下状态： 

| 显示名称 | 字段名称  | 值 1 | 显示名称 | 值 2 | 显示名称 |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| 风扇模式     | fanmode     | 1       | 正在运行      | 0       | 已停止      |

> [!NOTE]
> 状态度量的数据类型为字符串。

将表中所示字段名称准确输入设备模板中。 如果字段名称与对应的设备代码中的属性名称不匹配，则状态无法显示在应用程序中。

### <a name="event-measurements"></a>事件度量

在“度量”页上添加以下事件： 

| 显示名称 | 字段名称  | severity |
| ------------ | ----------- | -------- |
| 过热  | 过热    | 错误    |

> [!NOTE]
> 事件度量的数据类型为字符串。

### <a name="location-measurements"></a>位置度量

在“度量”页上添加以下位置度量： 

| 显示名称 | 字段名称  |
| ------------ | ----------- |
| 位置     | location    |

位置度量数据类型由表示经度和纬度的两个浮点数，以及表示高度的一个可选浮点数构成。

将表中所示字段名称准确输入设备模板中。 如果字段名称与对应的设备代码中的属性名称不匹配，则位置无法显示在应用程序中。

### <a name="device-properties"></a>设备属性

在“属性”页上添加以下设备属性： 

| 显示名称        | 字段名称        | 数据类型 |
| ------------------- | ----------------- | --------- |
| 序列号       | serialNumber      | text      |
| 设备制造商 | 制造商      | text      |

将表中所示字段名称准确输入设备模板中。 如果字段名称与对应的设备代码中的属性名称不匹配，则属性无法显示在应用程序中。

### <a name="settings"></a>设置

在“设置”页上添加以下**数字**设置： 

| 显示名称    | 字段名称     | Units | 小数 | Min | Max  | 初始 |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| 风扇速度       | fanSpeed       | rpm   | 0        | 0   | 3000 | 0       |
| 设置温度 | setTemperature | F     | 0        | 20 个  | 200  | 80      |

将表中所示字段名称准确输入设备模板中。 如果字段名称与对应的设备代码中的属性名称不匹配，则设备无法接收设置值。

### <a name="commands"></a>命令

在“命令”页上添加以下命令： 

| 显示名称    | 字段名称     | 默认超时 | 数据类型 |
| --------------- | -------------- | --------------- | --------- |
| Countdown       | countdown      | 30              | number    |

将以下输入字段添加到 Countdown 命令：

| 显示名称    | 字段名称     | 数据类型 | Value |
| --------------- | -------------- | --------- | ----- |
| 倒计数起始值      | countFrom      | number    | 10 个    |

将表中所示字段名称准确输入设备模板中。 如果字段名称与对应的设备代码中的属性名称不匹配，则设备无法处理命令。

## <a name="add-a-real-device"></a>添加真实设备

在 Azure IoT Central 应用程序中，将真实设备添加到在上一部分创建的设备模板。

记下“设备连接”页上的设备连接信息：  “范围 ID”、“设备 ID”和“主密钥”。    稍后在本操作指南中需要将这些值添加到设备代码中：

![设备连接信息](./media/howto-connect-nodejs/device-connection.png)

### <a name="create-a-nodejs-application"></a>创建 Node.js 应用程序

以下步骤演示如何创建一个客户端应用程序，以便实现已添加到应用程序的真实设备。 此处，Node.js 应用程序代表真实设备。

1. 在计算机上创建名为 `connected-air-conditioner-adv` 的文件夹。 导航到命令行环境中的该文件夹。

1. 若要初始化 Node.js 项目，请运行以下命令：

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt azure-iot-provisioning-device-mqtt azure-iot-security-symmetric-key --save
    ```

1. 在 `connected-air-conditioner-adv` 文件夹中创建名为 **connectedAirConditionerAdv.js** 的文件。

1. 在 **connectedAirConditionerAdv.js** 文件的开头添加以下 `require` 语句：

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var iotHubTransport = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var Message = require('azure-iot-device').Message;
    var ProvisioningTransport = require('azure-iot-provisioning-device-mqtt').Mqtt;
    var SymmetricKeySecurityClient = require('azure-iot-security-symmetric-key').SymmetricKeySecurityClient;
    var ProvisioningDeviceClient = require('azure-iot-provisioning-device').ProvisioningDeviceClient;
    ```

1. 将以下变量声明添加到该文件：

    ```javascript
    var provisioningHost = 'global.azure-devices-provisioning.cn';
    var idScope = '{your Scope ID}';
    var registrationId = '{your Device ID}';
    var symmetricKey = '{your Primary Key}';
    var provisioningSecurityClient = new SymmetricKeySecurityClient(registrationId, symmetricKey);
    var provisioningClient = ProvisioningDeviceClient.create(provisioningHost, idScope, new ProvisioningTransport(), provisioningSecurityClient);
    var hubClient;

    var targetTemperature = 0;
    var locLong = -122.1215;
    var locLat = 47.6740;
    ```

    将 `{your Scope ID}`、`{your Device ID}` 和 `{your Primary Key}` 占位符更新为前面记下的值。 此示例将 `targetTemperature` 初始化为零。你可以使用设备中的当前读数，也可以从设备孪生获取一个值。

1. 若要向 Azure IoT Central 应用程序发送遥测数据、状态、事件和位置度量，请将以下函数添加到文件：

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var humidity = 70 + (Math.random() * 10);
      var pressure = 90 + (Math.random() * 5);
      var fanmode = 0;
      var locationLong = locLong - (Math.random() / 100);
      var locationLat = locLat - (Math.random() / 100);
      var data = JSON.stringify({
        temperature: temperature,
        humidity: humidity,
        pressure: pressure,
        fanmode: (temperature > 25) ? "1" : "0",
        overheat: (temperature > 35) ? "ER123" : undefined,
        location: {
            lon: locationLong,
            lat: locationLat }
        });
      var message = new Message(data);
      hubClient.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

1. 若要向 Azure IoT Central 应用程序发送设备属性，请将以下函数添加到文件：

    ```javascript
    // Send device reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. 若要定义供设备响应的设置，请添加以下定义：

    ```javascript
    // Add any settings your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'fanSpeed': (newValue, callback) => {
          // Simulate it taking 1 second to set the fan speed.
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. 若要处理 Azure IoT Central 应用程序提供的已更新设置，请向文件添加以下代码：

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

1. 添加以下代码，以处理从 IoT Central 应用程序发送的 countdown 命令：

    ```javascript
    // Handle countdown command
    function onCountdown(request, response) {
      console.log('Received call to countdown');
    
      var countFrom = (typeof(request.payload.countFrom) === 'number' && request.payload.countFrom < 100) ? request.payload.countFrom : 10;
    
      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          hubClient.getTwin((err, twin) => {
            function doCountdown(){
              if ( countFrom >= 0 ) {
                var patch = {
                  countdown:{
                    value: countFrom
                  }
                };
                sendDeviceProperties(twin, patch);
                countFrom--;
                setTimeout(doCountdown, 2000 );
              }
            }
    
            doCountdown();
          });
        }
      });
    }
    ```

1. 添加以下代码以完成与 Azure IoT Central 的连接，并在客户端代码中挂接函数：

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Create handler for countdown command
        hubClient.onDeviceMethod('countdown', onCountdown);

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        hubClient.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            var properties = {
              serialNumber: '123-ABC',
              manufacturer: 'Contoso'
            };
            sendDeviceProperties(twin, properties);

            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    // Start the device (register and connect to Azure IoT Central).
    provisioningClient.register((err, result) => {
      if (err) {
        console.log('Error registering device: ' + err);
      } else {
        console.log('Registration succeeded');
        console.log('Assigned hub=' + result.assignedHub);
        console.log('DeviceId=' + result.deviceId);
        var connectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';SharedAccessKey=' + symmetricKey;
        hubClient = Client.fromConnectionString(connectionString, iotHubTransport);

        hubClient.open(connectCallback);
      }
    });
    ```

## <a name="run-your-nodejs-application"></a>运行 Node.js 应用程序

在命令行环境中运行以下命令：

```cmd/sh
node connectedAirConditionerAdv.js
```

作为 Azure IoT Central 应用程序中的操作员，你可以对真实设备执行以下操作：

* 在“度量”页中查看遥测： 

    ![查看遥测数据](media/howto-connect-nodejs/viewtelemetry.png)

* 在“度量”页上查看位置： 

    ![查看位置度量](media/howto-connect-nodejs/viewlocation.png)

* 在“属性”页上查看从设备发送的设备属性值。  设备连接时，设备属性磁贴将会更新：

    ![查看设备属性](media/howto-connect-nodejs/viewproperties.png)

* 在“设置”页中设置风扇速度和目标温度： 

    ![设置风扇速度](media/howto-connect-nodejs/setfanspeed.png)

* 从“命令”页调用 countdown 命令： 

    ![调用 countdown 命令](media/howto-connect-nodejs/callcountdown.png)

## <a name="next-steps"></a>后续步骤

了解如何将通用 Node.js 客户端连接到 Azure IoT Central 应用程序后，建议接下来了解如何为自己的 IoT 设备[设置自定义设备模板](howto-set-up-template.md)。
