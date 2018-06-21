---
title: 使用 Azure IoT 中心设备孪生属性 (Node) | Azure
description: 如何使用 Azure IoT 中心设备孪生配置设备。 使用 Azure IoT SDK for Node.js 实现模拟设备应用，并实现可使用设备孪生更改设备配置的服务应用。
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: ''
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 09/07/2017
ms.date: 10/16/2017
ms.author: v-yiso
ms.openlocfilehash: b1c6527a72ad9dddbedf164b4a0336769ac4d995
ms.sourcegitcommit: 9d3011bb050f232095f24e34f290730b33dff5e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2017
ms.locfileid: "22338817"
---
# <a name="use-desired-properties-to-configure-devices-node"></a>使用所需属性配置设备 (Node)

[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

在本教程结束时，将拥有两个 Node.js 控制台应用：

* **SimulateDeviceConfiguration.js**，一个模拟设备应用，它等待所需配置更新并报告模拟配置更新过程的状态。
* **SetDesiredConfigurationAndQuery.js**（Node.js 后端应用），用于在设备上设置所需配置并查询配置更新过程。

> [!NOTE]
> [Azure IoT SDK][lnk-hub-sdks] 文章介绍了可用于构建设备和后端应用的 Azure IoT SDK。
> 
> 

若要完成本教程，需要满足以下条件：

+ Node.js 版本 4.0.x 或更高版本。

+ 有效的 Azure 帐户。 如果没有帐户，可以创建一个[试用帐户][lnk-free-trial]，只需几分钟即可完成。

如果已按照[设备孪生入门][lnk-twin-tutorial]教程执行了操作，则已经有一个 IoT 中心和一个名为 **myDeviceId** 的设备标识；可以跳到[创建模拟设备应用][lnk-how-to-configure-createapp]部分。

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-simulated-device-app"></a>创建模拟设备应用
在此部分，用户需创建一个 Node.js 控制台应用，该应用可作为 **myDeviceId**连接到中心并等待所需配置更新，并针对模拟配置更新过程报告更新。

1. 新建名为 **simulatedeviceconfiguration**的空文件夹。 在命令提示符下的 **simulatedeviceconfiguration** 文件夹中，使用以下命令创建新的 package.json 文件。 接受所有默认值：

    ```
    npm init
    ```
2. 在 **simulatedeviceconfiguration** 文件夹的命令提示符下，运行以下命令安装 **azure-iot-device** 和 **azure-iot-device-mqtt** 包：
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. 使用文本编辑器，在 **simulatedeviceconfiguration** 文件夹中创建新的 **SimulateDeviceConfiguration.js** 文件。
4. 将以下代码添加到 **SimulateDeviceConfiguration.js** 文件，并将 **{device connection string}** 占位符替换为创建 **myDeviceId** 设备标识时复制的设备连接字符串：
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    **客户端**对象公开从设备与设备孪生进行交互所需的所有方法。 上面的代码在初始化 **Client** 对象后会检索 **myDeviceId** 的设备孪生，并在所需属性上附加用于更新的处理程序。 该处理程序通过比较 configId 来验证是否存在实际配置更改请求，并调用启动配置更改的方法。
   
    请注意，为简单起见，上一代码对初始配置使用硬编码默认值。 实际的应用可能会从本地存储加载该配置。
   
   > [!IMPORTANT]
   > 所需属性更改事件始终在设备连接时发出一次，请确保在执行任何操作之前检查所需属性中是否存在实际更改。
   > 
   > 
5. 在 `client.open()` 调用前添加以下方法：
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    **initConfigChange** 方法使用配置更新请求更新本地设备孪生对象的报告属性，并将状态设置为“Pending”，然后更新服务的设备孪生。 成功更新设备孪生后，它会模拟在执行 **completeConfigChange** 期间终止的长时间运行的进程。 此方法会更新本地设备孪生的报告属性，将状态设置为 **Success** 并删除 **pendingConfig** 对象。 然后，它会更新服务的设备孪生。

    请注意，为了节省带宽，仅通过指定要修改的属性（在上述代码中名为 **patch**）而不是替换整个文档来更新报告属性。
   
   > [!NOTE]
   > 本教程不模拟并发配置更新的任何行为。 某些配置更新过程可能无法在更新运行期间适应目标配置的更改，另外一些过程可能必须对更改进行排队，还有一些过程可能会拒绝更改并显示错误条件。 请务必考虑特定配置过程的所需行为，并在启动配置更改之前添加相应的逻辑。
   > 
   > 
6. 运行设备应用：
   
        node SimulateDeviceConfiguration.js
   
    此时会显示消息 `retrieved device twin`。 使应用保持运行状态。

## <a name="create-the-service-app"></a>创建服务应用
在此部分，会创建一个 Node.js 控制台应用，它会使用新的遥测配置对象更新与 *myDeviceId* 关联的设备孪生的 **所需属性** 。 该应用随后会查询存储在 IoT 中心的设备孪生，并显示设备的所需配置与报告配置之间的差异。

1. 新建名为 **setdesiredandqueryapp**的空文件夹。 在命令提示符下，使用以下命令在 **setdesiredandqueryapp** 文件夹中创建新的 package.json 文件。 接受所有默认值：
   
    ```
    npm init
    ```
2. 在 **setdesiredandqueryapp** 文件夹的命令提示符下，运行以下命令以安装 **azure-iothub** 包：
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. 使用文本编辑器，在 setdesiredandqueryapp 文件夹中新建 SetDesiredAndQuery.js 文件。
4. 将以下代码添加到 **SetDesiredAndQuery.js** 文件，并将 **{iot hub connection string}** 占位符替换为创建中心时复制的 IoT 中心连接字符串：
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    **Registry** 对象公开从服务与设备孪生进行交互所需的所有方法。 前面的代码在初始化 **Registry** 对象后检索 **myDeviceId** 的设备孪生，并使用新的遥测配置对象更新其所需属性。 在此之后，它调用 **queryTwins** 函数事件 10 秒。

    > [!IMPORTANT]
    > 为进行说明，此应用程序每 10 秒查询 IoT 中心一次。 使用查询跨多个设备生成面向用户的报表，而不检测更改。 如果解决方案需要设备事件的实时通知，请使用[孪生通知][lnk-twin-notifications]。
    > 
    >

1. 恰好在 `registry.getDeviceTwin()` 调用前添加以下代码以实现 **queryTwins** 函数：
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    上述代码查询 IoT 中心内存储的设备孪生，并打印所需的遥测配置和报告遥测配置。 请参阅 [IoT 中心查询语言][lnk-query] 以了解如何跨所有设备生成丰富的报告。
2. 在 **SimulateDeviceConfiguration.js** 运行期间，使用以下内容运行应用程序：
   
        node SetDesiredAndQuery.js 5m
   
    应该看到报告的配置从 **Success** 更改为 **Pending**，再更改回 **Success**，此时新的活动发送频率已变为五分钟而不是 24 小时。

   > [!IMPORTANT]
   > 设备报告操作与查询结果之间最多存在一分钟的延迟。 这是为了使查询基础结构可以采用非常大的规模来工作。 若要检索单个设备孪生的一致视图，请使用 **Registry** 类中的 **getDeviceTwin** 方法。
   > 
   > 

## <a name="next-steps"></a>后续步骤
在本教程中，从后端应用将所需配置设置为所需属性，并编写一个模拟设备应用来检测该更改及模拟多步骤更新过程（将其状态作为报告属性报告给设备孪生）。

充分利用以下资源：

- 通过 [Get started with IoT Hub][lnk-iothub-getstarted] （IoT 中心入门）教程学习如何从设备发送遥测；
- 关于对大型设备集进行计划或执行操作，请参阅 [计划和广播作业][lnk-schedule-jobs] 教程。
- 通过[使用直接方法][lnk-methods-tutorial]教程学习如何以交互方式控制设备（例如从用户控制的应用打开风扇）。

<!-- links -->
[lnk-hub-sdks]: ./iot-hub-devguide-sdks.md
[lnk-free-trial]: https://www.azure.cn/pricing/1rmb-trial/

[lnk-devguide-jobs]: ./iot-hub-devguide-jobs.md
[lnk-query]: ./iot-hub-devguide-query-language.md
[lnk-twin-notifications]: ./iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: ./iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ./iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ./iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: ./iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://www.azure.cn/develop/iot/
[lnk-device-management]: ./iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ./iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: ./iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: ./iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: ./iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app


<!--Update_Description:update meta properties and wording-->