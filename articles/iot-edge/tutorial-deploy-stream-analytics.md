---
title: 教程：将 Azure 流分析作业部署到设备 - Azure IoT Edge
description: 在本教程中，请将 Azure 流分析作为模块部署到 IoT Edge 设备
author: kgremban
manager: timlt
ms.author: v-yiso
origin.date: 09/21/2018
ms.date: 04/08/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1fb8d1e2931c1c7c5ea34aae57d2ef06fac4e663
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58625795"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module"></a>教程：将 Azure 流分析作为 IoT Edge 模块进行部署

许多 IoT 解决方案使用分析服务来了解从 IoT 设备到达云的数据。 使用 Azure IoT Edge 时，可以获取 [Azure 流分析](/stream-analytics/)逻辑，将其转到设备中。 在边缘处理遥测流可以减少上传数据量，缩短对可操作见解进行响应的时间。

Azure IoT Edge 和 Azure 流分析已集成，因此可以在 Azure 门户中创建一项 Azure 流分析作业，然后将其作为 IoT Edge 模块进行部署，不需额外的代码。  

Azure 流分析提供一种丰富结构化的查询语法，可用于在云和 IoT Edge 设备上进行数据分析。 有关 IoT Edge 上的 Azure 流分析的详细信息，请参阅 [Azure 流分析文档](../stream-analytics/stream-analytics-edge.md)。

本教程中的流分析模块在一个滚动的 30 秒时段内计算平均温度。 当平均温度达到 70 时，模块会发送一个警报，以便设备采取操作。 在这种情况下，该操作是重置模拟温度传感器。 在生产环境中，当温度达到危险级别时，可以使用此功能关闭机器或采取预防措施。 

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 在 Edge 上创建 Azure 流分析作业用于处理数据。
> * 将新的 Azure 流分析作业与其他 IoT Edge 模块相连接。
> * 将 Azure 流分析作业从 Azure 门户部署到 IoT Edge 设备。

<center>

<img src="./media/tutorial-deploy-stream-analytics/ASATutorialDiagram.png" alt="Tutorial architecture diagram"/>

</center>


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件

Azure IoT Edge 设备：

* 可以按照适用于 [Linux](quickstart-linux.md) 或 [Windows 设备](quickstart.md)的快速入门中的步骤，将开发计算机或虚拟机用作 Edge 设备。

云资源：

* Azure 中的免费或标准层 [IoT 中心](../iot-hub/iot-hub-create-through-portal.md)。 


## <a name="create-an-azure-stream-analytics-job"></a>创建 Azure 流分析作业

本部分的步骤将创建一个 Azure 流分析作业，用于从 IoT 中心提取数据、查询从设备发送的遥测数据，然后将结果转发到 Azure Blob 存储容器。 

### <a name="create-a-storage-account"></a>创建存储帐户

创建可以在 IoT Edge 设备上运行的 Azure 流分析作业时，需采用特定的存储方式，以便能够从设备进行调用。 可以使用现有 Azure 存储帐户，也可以创建新的 Azure 存储帐户。 

1. 在 Azure 门户中，访问“创建资源” > “存储” > “存储帐户 - blob、文件、表、队列”。 

1. 提供以下值，以便创建存储帐户：

   | 字段 | 值 |
   | ----- | ----- |
   | 名称 | 为存储帐户提供唯一的名称。 | 
   | 位置 | 选择靠近你的位置。 |
   | 订阅 | 选择与 IoT 中心相同的订阅。 |
   | 资源组 | 建议对在 IoT Edge 快速入门和教程中创建的所有测试资源使用同一资源组。 例如，**IoTEdgeResources**。 |

1. 其他字段保留默认值，然后选择“创建”。 

### <a name="create-a-new-job"></a>创建新的作业

1. 在 Azure 门户中，转到“创建资源” > “物联网” > “流分析作业”。

1. 提供以下值，以便创建作业：

   | 字段 | 值 |
   | ----- | ----- |
   | 作业名称 | 为作业提供一个名称。 例如，**IoTEdgeJob** | 
   | 订阅 | 选择与 IoT 中心相同的订阅。 |
   | 资源组 | 建议对在 IoT Edge 快速入门和教程中创建的所有测试资源使用同一资源组。 例如，**IoTEdgeResources**。 |
   | 位置 | 选择靠近你的位置。 | 
   | 宿主环境 | 选择“边缘”。 |
 
1. 选择**创建**。

### <a name="configure-your-job"></a>配置作业

在 Azure 门户中创建了流分析作业后，可以为该作业配置输入、输出以及要对传递的数据运行的查询。 

本部分介绍如何使用输入、输出和查询这三个元素创建从 IoT Edge 设备接收温度数据的作业。 它将分析 30 秒的滚动窗口中的数据。 如果该窗口中的平均温度超过 70 度，则会将警报发送到 IoT Edge 设备。 在下一部分中部署作业时将会确切指定数据的来源和数据要发送到的目标位置。  

1. 在 Azure 门户中导航到流分析作业。 

1. 在“作业拓扑”下，选择“输入”，然后选择“添加流输入”。

   ![Azure 流分析添加输入](./media/tutorial-deploy-stream-analytics/asa_input.png)

1. 从下拉列表中选择“Edge 中心”。

1. 在“新建输入”窗格中，输入 **temperature** 作为输入别名。 

1. 其他字段保留默认值，然后选择“保存”。

1. 在“作业拓扑”下，打开“输出”，然后选择“添加”。

   ![Azure 流分析添加输出](./media/tutorial-deploy-stream-analytics/asa_output.png)

1. 从下拉列表中选择“Edge 中心”。

1. 在“新建输出”窗格中，输入 **alert** 作为输出别名。 

1. 其他字段保留默认值，然后选择“保存”。

1. 在“作业拓扑”下选择“查询”。

1. 将默认文本替换为以下查询。 如果 30 秒的窗口中的平均机器温度达到了 70 度，SQL 代码会将重置命令发送到 alert 输出。 重置命令已作为可执行的操作预先编程到传感器中。 

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```

1. 选择“保存”。

### <a name="configure-iot-edge-settings"></a>配置 IoT Edge 设置

若要准备要部署到 IoT Edge 设备上的流分析作业，需要将该作业与存储帐户中的容器关联。 部署作业时，作业定义会导出到该存储容器。 

1. 在“配置”下，选择“存储帐户设置”。

1. 选择“添加存储帐户”。 

1. 从下拉菜单中选择自己的**存储帐户**。

1. 对于“容器”字段，请选择“新建”，然后为存储容器提供一个名称。 

1. 选择“保存”。 

## <a name="deploy-the-job"></a>部署作业

现已准备好在 IoT Edge 设备上部署 Azure 流分析作业。 

在本部分，请使用 Azure 门户中的**设置模块**向导来创建部署清单。 部署清单是一个 JSON 文件，它描述将要部署到设备的所有模块、存储模块映像的容器注册表、应如何管理模块以及模块如何相互通信。 IoT Edge 设备从 IoT 中心检索其部署清单，然后使用该清单中的信息来部署和配置其所有已分配的模块。 

对于本教程中，需要部署两个模块。 第一个是 **tempSensor**，这是一个用于模拟温度和湿度传感器的模块。 第二个是流分析作业。 传感器模块提供的数据流是作业查询将要分析的。 

1. 在 Azure 门户的 IoT 中心转到“IoT Edge”，然后打开 IoT Edge 设备的详细信息页。

1. 选择“设置模块”。  

1. 如果以前在此设备上部署了 tempSensor 模块，则它可以自动填充数据。 否则，请通过以下步骤来添加模块：

   1. 单击“添加”，然后选择“IoT Edge 模块”。
   1. 键入 **tempSensor** 作为名称。
   1. 输入 **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0** 作为映像 URI。 
   1. 将其他设置保留不变，然后选择“保存”。

1. 通过以下步骤添加 Azure 流分析 Edge 作业：

   1. 单击“添加”，然后选择“Azure 流分析模块”。
   1. 选择创建的订阅和 Azure 流分析 Edge 作业。 
   1. 选择“保存”。

1. 等到流分析作业部署到已创建的存储容器以后，请单击模块名称，查看流分析模块的构造方式。 

   映像 URI 指向一个标准 Azure 流分析映像。 这是用于部署到 IoT Edge 设备的每一作业的同一映像。 

   模块孪生是通过一个名为 **ASAJobInfo** 的所需属性配置的。 该属性的值指向存储容器中的作业定义。 此属性是使用特定作业信息配置流分析映像的方式。 

1. 关闭模块页。

1. 请记下流分析模块的名称，因为需要在下一步使用它，然后选择“下一步”以继续操作。

1. 将“Routes”中的默认值替换为以下代码。 将 _{moduleName}_ 的所有三个实例更新为 Azure 流分析模块的名称。 

    ```json
    {
        "routes": {
            "telemetryToCloud": "FROM /messages/modules/tempSensor/* INTO $upstream",
            "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream",
            "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")",
            "telemetryToAsa": "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")"
        }
    }
    ```

   此处声明的路由定义通过 IoT Edge 设备的数据流。 来自 tempSensor 的遥测数据将发送到 IoT 中心和在流分析作业中配置的 **temperature** 输入。 “警报”输出消息先发送到 IoT 中心，然后发送到 tempSensor 模块以触发重置命令。 

1. 选择“**下一步**”。

1. 在“复查部署”步骤中，选择“提交”。

1. 返回到“设备详细信息”页，并选择“刷新”。  

    应会看到新的流分析模块与 IoT Edge 代理模块和 IoT Edge 中心在同时运行。

    ![设备报告的 tempSensor 和 ASA 模块](./media/tutorial-deploy-stream-analytics/module_output2.png)

## <a name="view-data"></a>查看数据

现在可以转到 IoT Edge 设备，检查 Azure 流分析模块与 tempSensor 模块之间的交互。

1. 检查所有模块是否在 Docker 中运行：

   ```cmd/sh
   iotedge list  
   ```
   <!--
   ![Docker output](./media/tutorial-deploy-stream-analytics/docker_output.png)
   -->
2. 查看所有系统日志和指标数据。 使用流分析模块名称：

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

应该可以看到，机器温度逐渐升高，在 30 秒内达到了 70 度。 然后，流分析模块触发重置，机器温度下降到 21 度。 

   ![将命令输出重置为模块日志](./media/tutorial-deploy-stream-analytics/docker_log.png)

## <a name="clean-up-resources"></a>清理资源 

如果打算继续学习下一篇建议的文章，可以保留已创建的资源和配置，以便重复使用。 还可以继续使用相同的 IoT Edge 设备作为测试设备。 

否则，可以删除本文中创建的本地配置和 Azure 资源，以避免收费。 
 
[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]



<!-- Images. -->
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output2.png
[8]: ./media/tutorial-deploy-stream-analytics/docker_output.png
[9]: ./media/tutorial-deploy-stream-analytics/docker_log.png
[10]: ./media/tutorial-deploy-stream-analytics/storage_settings.png
[11]: ./media/tutorial-deploy-stream-analytics/temp_module.png


<!-- Links -->
[lnk-what-is-iot-edge]: what-is-iot-edge.md
[lnk-module-dev]: module-development.md
[iot-hub-get-started-create-hub]: ../../includes/iot-hub-get-started-create-hub.md
[azure-iot]: https://docs.microsoft.com/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/azure/storage/
[azure-stream]: https://docs.microsoft.com/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md

