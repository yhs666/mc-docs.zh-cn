---
title: 为 Windows 设备开发模块 - Azure IoT Edge | Microsoft Docs
description: 本教程逐步介绍如何设置开发计算机和云资源，以使用适用于 Windows 设备的 Windows 容器开发 IoT Edge 模块
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/20/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 6c4e84c716bb6e2b75646e3c35c304ea5e9e91c7
ms.sourcegitcommit: 99ef971eb118e3c86a6c5299c7b4020e215409b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2019
ms.locfileid: "65830251"
---
# <a name="tutorial-develop-iot-edge-modules-for-windows-devices"></a>教程：开发适用于 Windows 设备的 IoT Edge 模块

使用 Visual Studio 2017 可以开发代码并将其部署到运行 IoT Edge 的 Windows 设备。

在快速入门中，你已使用 Windows 虚拟机创建了一个 IoT Edge 设备，并通过 Azure 市场部署了一个预生成的模块。 本教程将逐步介绍如何开发你自己的代码并将其部署到 IoT Edge 设备。 本教程是更详细地介绍特定编程语言或 Azure 服务的其他所有教程的有用先决条件。 

本教程使用了有关**将 C 模块部署到 Windows 设备**的示例。 之所以选择此示例，是因为它比较简单，无论是否安装了适当的库，它都可以让你从中了解开发工具。 了解开发概念之后，可以选择首选的语言或 Azure 服务深入到详细信息。 

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 设置开发计算机。
> * 使用适用于 Visual Studio 2017 的 IoT Edge Tools 创建新项目。
> * 将项目生成为容器，并将其存储在 Azure 容器注册表中。
> * 将代码部署到 IoT Edge 设备。 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="key-concepts"></a>关键概念

本教程将逐步讲解 IoT Edge 模块的开发。 IoT Edge 模块（有时简称为“模块”）是包含可执行代码的容器。 可将一个或多个模块部署到 IoT Edge 设备。 模块执行特定的任务，例如，从传感器引入数据、执行数据分析或数据清理操作，或者将消息发送到 IoT 中心。 有关详细信息，请参阅[了解 Azure IoT Edge 模块](iot-edge-modules.md)。

开发 IoT Edge 模块时，必须了解开发计算机与该模块最终要部署到的目标 IoT Edge 设备之间的差异。 生成的用于保存模块代码的容器必须与目标设备的操作系统 (OS) 相匹配。 对于 Windows 容器开发，此概念更为简单，因为 Windows 容器仅在 Windows 操作系统上运行。 但是，举例而言，可以使用 Windows 开发计算机来生成适用于 Linux IoT Edge 设备的模块。 在这种情况下，必须确保开发计算机运行的是 Linux 容器。 在学习本教程的过程中，请注意开发计算机 OS 与容器 OS 之间的差异。

本教程面向运行 IoT Edge 的 Windows 设备。 Windows IoT Edge 设备使用 Windows 容器。 我们建议使用 Visual Studio 2017 对 Windows 设备进行开发，而本教程也正是使用此工具。 也可以使用 Visual Studio Code，不过，这两个工具之间存在支持方面的差异。

下表列出了 Visual Studio Code 和 Visual Studio 2017 支持的 **Windows 容器**开发方案。

|   | Visual Studio Code | Visual Studio 2017 |
| - | ------------------ | ------------------ |
| **Azure 服务** | Azure Functions <br> Azure 流分析 |   |
| **语言** | C#（不支持调试） | C <br> C# |
| **详细信息** | [适用于 Visual Studio Code 的 Azure IoT Edge](https://marketplace.visualstudio.com/itemdetails?itemName=vsciot-vscode.azure-iot-edge) | [适用于 Visual Studio 2017 的 Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) |

本教程将会讲解 Visual Studio 2017 的开发步骤。 如果你偏向于使用 Visual Studio Code，请参阅[使用 Visual Studio Code 开发和调试 Azure IoT Edge 的模块](how-to-vs-code-develop-module.md)中的说明。

## <a name="prerequisites"></a>先决条件

一台开发计算机：

* 装有 1809 更新或更高版本的 Windows 10。
* 可以根据开发偏好，使用自己的计算机或虚拟机。
* 安装 [Git](https://git-scm.com/)。 
* 通过 vcpkg 安装 Azure IoT C SDK for Windows x64：

   ```powershell
   git clone https://github.com/Microsoft/vcpkg
   cd vcpkg
   .\bootstrap-vcpkg.bat
   .\vcpkg install azure-iot-sdk-c:x64-windows
   .\vcpkg --triplet x64-windows integrate install
   ```

<!--vcpkg only required for C development-->

Windows 上的一个 Azure IoT Edge 设备：

* 我们建议不要在开发计算机上运行 IoT Edge，而是使用独立的设备。 分开使用开发计算机和 IoT Edge 设备可以更准确地反映真实的部署方案，并且有助于直接体现不同的概念。
* 如果没有另一个可用的设备，请参考快速入门文章使用 [Windows 虚拟机](quickstart.md)在 Azure 中创建一个 IoT Edge 设备。

云资源：

* Azure 中的免费或标准层 [IoT 中心](../iot-hub/iot-hub-create-through-portal.md)。 

## <a name="install-container-engine"></a>安装容器引擎

IoT Edge 模块打包为容器，因此，需要在开发计算机上安装一个容器引擎用于生成和管理容器。 我们建议使用 Docker Desktop 进行开发，由于它包含许多的功能，是非常流行的容器引擎。 在 Windows 计算机上使用 Docker Desktop 可以在 Linux 容器与 Windows 容器之间切换，以便轻松地为不同类型的 IoT Edge 设备开发模块。 

参考 Docker 文档在开发计算机上安装： 

* [安装适用于 Windows 的 Docker Desktop](https://docs.docker.com/docker-for-windows/install/)

  * 安装适用于 Windows 的 Docker Desktop 时，系统会询问你是要使用 Linux 还是 Windows 容器。 本教程使用 **Windows 容器**。 有关详细信息，请参阅[在 Windows 与 Linux 容器之间切换](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)。


## <a name="set-up-visual-studio-and-tools"></a>安装 Visual Studio 和工具

使用适用于 Visual Studio 2017 的 IoT 扩展来开发 IoT Edge 模块。 这些扩展提供项目模板、自动创建部署清单，并可让你监视和管理 IoT Edge 设备。 在本部分，你将安装 Visual Studio 和 IoT Edge 扩展，然后设置 Azure 帐户以从 Visual Studio 内部管理 IoT 中心资源。 

1. 如果尚未在开发计算机上安装 Visual Studio，请[安装包含以下工作负荷的 Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio?view=vs-2017)： 

   * Azure 开发
   * 使用 C++ 的桌面开发
   * .NET Core 跨平台开发

1. 如果已在开发计算机上安装了 Visual Studio 2017，请确保其版本为 15.7 或更高。 遵循[修改 Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) 中的步骤添加所需的工作负荷（如果尚未添加）。

2. 下载并安装适用于 Visual Studio 2017 的 [Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) 扩展。 

3. 安装完成后，打开 Visual Studio。

4. 选择“视图” > “Cloud Explorer”。 

5. 在 Cloud Explorer 中选择个人资料图标并登录到 Azure 帐户（如果尚未登录）。 

6. 登录后，系统会列出你的 Azure 订阅。 选择要通过 Cloud Explorer 访问的订阅，然后选择“应用”。 

7. 展开订阅，然后依次选择“IoT 中心”和你的 IoT 中心。 应会看到 IoT 设备的列表，并可以使用 Cloud Explorer 对其进行管理。 

   ![在 Cloud Explorer 中访问 IoT 中心资源](./media/tutorial-develop-for-windows/cloud-explorer-view-hub.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>创建新的模块项目

Azure IoT Edge Tools 扩展为 Visual Studio 2017 中支持的所有 IoT Edge 模块语言提供项目模板。 这些模板包含将工作模块部署到测试 IoT Edge 所需的所有文件和代码，或者提供一个起点让你使用自己的业务逻辑自定义模板。 

1. 以管理员身份运行 Visual Studio。

2. 选择“文件” > “新建” > “项目”。 

3. 在“新建项目”窗口中选择“Azure IoT”项目类型，然后选择“Azure IoT Edge”项目。 重命名项目和解决方案，或接受默认值 **AzureIoTEdgeApp1**。 选择“确定”创建该项目。 

   ![创建新的 Azure IoT Edge 项目](./media/tutorial-develop-for-windows/new-project.png)

4. 在 IoT Edge 应用程序和模块窗口中，使用以下值配置项目： 

   | 字段 | Value |
   | ----- | ----- |
   | 应用程序平台 | 取消选中“Linux Amd64”，选中“WindowsAmd64”。 |
   | 选择模板 | 选择“C 模块”。 | 
   | 模块项目名称 | 接受默认值 **IoTEdgeModule1**。 | 
   | Docker 映像存储库 | 映像存储库包含容器注册表的名称和容器映像的名称。 系统已基于模块项目名称值预先填充容器映像。 将 **localhost:5000** 替换为 Azure 容器注册表中的登录服务器值。 可以在 Azure 门户的容器注册表的“概览”页中检索登录服务器。 <br><br> 最终的映像存储库看起来类似于 \<registry name\>.azurecr.io/iotedgemodule1。 |

   ![配置目标设备、模块类型和容器注册表的项目](./media/tutorial-develop-for-windows/add-application-and-module.png)

5. 选择“确定”以应用更改。 

新项目载入到 Visual Studio 窗口中后，请花费片刻时间来熟悉它所创建的文件： 

* 名为 **AzureIoTEdgeApp1.Windows.Amd64** 的 IoT Edge 项目。
    * **Modules** 文件夹包含指向项目中模块的指针。 在本例中，该模块应该只是 IoTEdgeModule1。 
    * **deployment.template.json** 文件帮助你创建部署清单的模板。 部署清单文件确切地定义要在设备上部署的模块、模块的配置方式，以及它们如何相互通信以及与云通信。 
* 名为 **IoTEdgeModule1** 的 IoT Edge 模块项目。
    * **main.c** 文件包含项目模板附带的默认 C 模块代码。 默认模块从源中接收输入，并将其传递给 IoT 中心。 
    * **module.json** 文件包含有关模块的详细信息，包括完整的映像存储库、映像版本，以及每个受支持平台使用的 Dockerfile。

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>为 IoT Edge 代理提供注册表凭据

IoT Edge 运行时需要使用注册表凭据将容器映像提取到 IoT Edge 设备中。 请将这些凭据添加到部署模板中。 

1. 打开 **deployment.template.json** 文件。

2. 在 $edgeAgent 所需属性中找到 **registryCredentials** 属性。 

3. 使用你的凭据用以下格式更新该属性： 

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "<username>",
       "password": "<password>",
       "address": "<registry name>.azurecr.io"
     }
   }

4. Save the deployment.template.json file. 

### Review the sample code

The solution template that you created includes sample code for an IoT Edge module. This sample module simply receives messages and then passes them on. The pipeline functionality demonstrates an important concept in IoT Edge, which is how modules communicate with each other.

Each module can have multiple *input* and *output* queues declared in their code. The IoT Edge hub running on the device routes messages from the output of one module into the input of one or more modules. The specific language for declaring inputs and outputs varies between languages, but the concept is the same across all modules. For more information about routing between modules, see [Declare routes](module-composition.md#declare-routes).

1. In the **main.c** file, find the **SetupCallbacksForModule** function.

2. This function sets up an input queue to receive incoming messages. It calls the C SDK module client function [SetInputMessageCallback](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-setinputmessagecallback). Review this function and see that it initializes an input queue called **input1**. 

   ![Find the input name in the SetInputMessageCallback constructor](./media/tutorial-develop-for-windows/declare-input-queue.png)

3. Next, find the **InputQueue1Callback** function.

4. This function processes received messages and sets up an output queue to pass them along. It calls the C SDK module client function [SendEventToOutputAsync](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-sendeventtooutputasync). Review this function and see that it initializes an output queue called **output1**. 

   ![Find the output name in the SendEventToOutputAsync constructor](./media/tutorial-develop-for-windows/declare-output-queue.png)

5. Open the **deployment.template.json** file.

6. Find the **modules** property of the $edgeAgent desired properties. 

   There should be two modules listed here. The first is **tempSensor**, which is included in all the templates by default to provide simulated temperature data that you can use to test your modules. The second is the **IotEdgeModule1** module that you created as part of this project.

   This modules property declares which modules should be included in the deployment to your device or devices. 

7. Find the **routes** property of the $edgeHub desired properties. 

   One of the functions if the IoT Edge hub module is to route messages between all the modules in a deployment. Review the values in the routes property. The first route, **IotEdgeModule1ToIoTHub**, uses a wildcard character (**\***) to include any message coming from any output queue in the IoTEdgeModule1 module. These messages go into *$upstream*, which is a reserved name that indicates IoT Hub. The second route, **sensorToIotEdgeModule1**, takes messages coming from the tempSensor module and routes them to the *input1* input queue of the IotEdgeModule1 module. 

   ![Review routes in deployment.template.json](./media/tutorial-develop-for-windows/deployment-routes.png)


## Build and push your solution

You've reviewed the module code and the deployment template to understand some key deployment concepts. Now, you're ready to build the IotEdgeModule1 container image and push it to your container registry. With the IoT tools extension for Visual Studio, this step also generates the deployment manifest based on the information in the template file and the module information from the solution files. 

### Sign in to Docker

Provide your container registry credentials to Docker on your development machine so that it can push your container image to be stored in the registry. 

1. Open PowerShell or a command prompt.

2. Sign in to Docker with the Azure container registry credentials that you saved after creating the registry. 

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   可能会出现一条安全警告，其中建议使用 `--password-stdin`。 这条最佳做法是针对生产场景建议的，这超出了本教程的范畴。 有关详细信息，请参阅 [docker login](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) 参考。

### <a name="build-and-push"></a>生成并推送

现在，开发计算机以及 IoT Edge 设备都可以访问你的容器注册表。 接下来，可将项目代码转换为容器映像。 

1. 右键单击“AzureIotEdgeApp1.Windows.Amd64”项目文件夹，然后选择“生成并推送 IoT Edge 模块”。 

   ![生成并推送 IoT Edge 模块](./media/tutorial-develop-for-windows/build-and-push-modules.png)

   “生成并推送”命令会启动三项操作。 首先，它在解决方案中创建名为 **config** 的新文件夹，用于保存基于部署模板和其他解决方案文件中的信息生成的完整部署清单。 其次，它会运行 `docker build`，以基于目标体系结构的相应 dockerfile 生成容器映像。 然后，它会运行 `docker push`，以将映像存储库推送到容器注册表。 

   此过程在首次运行时可能需要花费几分钟时间，但下一次运行这些命令时可以更快地完成。 

2. 打开新建的 config 文件夹中的 **deployment.windows-amd64.json** 文件。 （config 文件夹可能未显示在 Visual Studio 中的解决方案资源管理器中。 如果是这样，请在解决方案资源管理器任务栏中选择“显示所有文件”图标。）

3. 找到 IotEdgeModule1 节的 **image** 参数。 请注意，该映像包含完整的映像存储库，其中附带了来自 module.json 文件的名称、版本和体系结构标记。

4. 打开 IotEdgeModule1 文件夹中的 **module.json** 文件。 

5. 更改模块映像的版本号。 （是 version 而不是 $schema-version。）例如，将修补程序版本号递增为 **0.0.2**，就如同我们在模块代码中做了细微的修复一样。 

   >[!TIP]
   >模块版本启用版本控制，并可让你在将更新部署到生产环境之前，在少量的设备上测试更改。 如果在生成和推送之前不递增模块版本，则会覆盖容器注册表中的存储库。 

6. 保存对 module.json 文件所做的更改。

7. 再次右键单击“AzureIotEdgeApp1.Windows.Amd64”项目文件夹，然后选择“生成并推送 IoT Edge 模块”。 

8. 再次打开 **deployment.windows-amd64.json** 文件。 请注意，再次运行“生成并推送”命令时未创建新文件， 而是更新了同一文件以反映更改。 IotEdgeModule1 映像现在指向容器的版本 0.0.2。 在部署清单中做出此项更改的目的是告知 IoT Edge 设备有新的模块版本可供提取。 

9. 若要进一步验证“生成并推送”命令执行了哪些操作，请转到 [Azure 门户](https://portal.azure.com)并导航到你的容器注册表。 

10. 在该容器注册表中，依次选择“存储库”、“iotedgemodule1”。 验证映像的两个版本是否已推送到注册表。

    ![查看容器注册表中的两个映像版本](./media/tutorial-develop-for-windows/view-repository-versions.png)

### <a name="troubleshoot"></a>故障排除

如果在生成和推送模块映像时遇到错误，这些错误往往与开发计算机上的 Docker 配置相关。 使用以下提问来检查配置： 

* 是否使用从容器注册表复制的凭据运行了 `docker login` 命令？ 这些凭据不同于用来登录到 Azure 的凭据。 
* 你的容器存储库是否正确？ 存储库中是否包含正确的容器注册表名称和模块名称？ 打开 IotEdgeModule1 文件夹中的 **module.json** 文件进行检查。 存储库值应类似于 **\<注册表名称\>.azurecr.io/iotedgemodule1**。 
* 如果为模块使用的名称不是 **IotEdgeModule1**，使用的名称是否在整个解决方案中一致？
* 计算机运行的容器是否与正在生成的容器的类型相同？ 本教程适用于 Windows IoT Edge 设备，因此，Visual Studio 文件应包含 **windows-amd64** 扩展，Docker Desktop 应该运行 Windows 容器。 

## <a name="deploy-modules-to-device"></a>将模块部署到设备

确认生成的容器映像已存储在容器注册表中之后，现在可以将其部署到设备。 请确保 IoT Edge 设备已启动并正在运行。 

1. 在 Visual Studio 中打开 Cloud Explorer，并展开 IoT 中心的详细信息。 

2. 选择要部署到的设备的名称。 在“操作”列表中，选择“创建部署”。

   ![为单个设备创建部署](./media/tutorial-develop-for-windows/create-deployment.png)


3. 在文件资源管理器，导航到项目的 config 文件夹，然后选择“deployment.windows-amd64.json”文件。 此文件通常位于 `C:\Users\<username>\source\repos\AzureIotEdgeApp1\AzureIotEdgeApp1.Windows.Amd64\config\deployment.windows-amd64.json`

   不要使用 deployment.template.json 文件，因为其中不包含完整模块映像的值。 

4. 在 Cloud Explorer 中展开 IoT Edge 设备的详细信息，以查看设备上的模块。

5. 使用“刷新”按钮更新设备状态，以查看是否为设备部署了 tempSensor 和 IotEdgeModule1 模块。 


   ![查看 IoT Edge 设备上运行的模块](./media/tutorial-develop-for-windows/view-running-modules.png)

## <a name="view-messages-from-device"></a>查看来自设备的消息

IotEdgeModule1 代码通过其输入队列接收消息，然后通过其输出队列传递消息。 部署清单声明了从 tempSensor 向 IotEdgeModule1 传递消息，然后从 IotEdgeModule1 向 IoT 中心转发消息的路由。 使用适用于 Visual Studio 的 Azure IoT Edge Tools 可以查看单个设备发出的已抵达 IoT 中心的消息。 

1. 在 Visual Studio Cloud Explorer 中，选择已部署到的 IoT Edge 设备的名称。 

2. 在“操作”菜单中，选择“开始监视 D2C 消息”。

3. 观察 Visual Studio 中的“输出”部分，以查看抵达 IoT 中心的消息。 

   启动这两个模块可能需要几分钟时间。 IoT Edge 运行时需要接收其新部署清单、从容器运行时提取模块映像，然后启动每个新模块。 如果你 

   ![查看传入的设备到云消息](./media/tutorial-develop-for-windows/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>查看设备上的更改

若要查看设备本身上发生的情况，请使用本部分所述的命令来检查设备上运行的 IoT Edge 运行时和模块。 

本部分所述的命令适用于 IoT Edge 设备，而不适用于开发计算机。 如果对 IoT Edge 设备使用了虚拟机，现在请连接到该虚拟机。 在 Azure 中，转到该虚拟机的概述页，并选择“连接”以访问远程桌面连接。 在设备上打开命令提示符或 PowerShell 窗口，以运行 `iotedge` 命令。

* 查看已部署到设备的所有模块，并检查其状态：

   ```cmd
   iotedge list
   ```

   应会看到四个模块：两个 IoT Edge 运行时模块、tempSensor 和 IotEdgeModule1。 所有四个模块应列为“正在运行”。

* 检查特定模块的日志：

   ```cmd
   iotedge logs <module name>
   ```

   IoT Edge 模块区分大小写。 

   tempSensor 和 IotEdgeModule1 日志应显示它们正在处理的消息。 edgeAgent 模块负责启动其他模块，因此，其日志将包含有关实现部署清单的信息。 如有任一模块未列出或未运行，edgeAgent 日志可能会包含错误。 edgeHub 模块负责模块与 IoT 中心之间的通信。 如果模块已启动并正在运行，但消息未抵达 IoT 中心，edgeHub 日志可能会包含错误。 

## <a name="next-steps"></a>后续步骤

在本教程中，你已在开发计算机上安装了 Visual Studio 2017 并从中部署了第一个 IoT Edge 模块。 了解基本概念后，接下来请尝试将功能添加到模块，使它可以分析其中传递的数据。 选择首选的语言： 

> [!div class="nextstepaction"] 
> [C](tutorial-c-module-windows.md)
> [C#](tutorial-csharp-module-windows.md)