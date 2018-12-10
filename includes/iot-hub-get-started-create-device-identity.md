## <a name="create-a-device-identity"></a>创建设备标识

在本部分中，将使用 Azure CLI 为本教程创建设备标识。 Azure CLI 预先安装在 [Azure Cloud Shell](https://docs.microsoft.com/zure/cloud-shell/overview) 中，也可以[在本地安装](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。 设备 ID 区分大小写。

1. 在使用 Azure CLI 安装 IoT 扩展的命令行环境中运行以下命令：

    ```cmd/sh
    az extension add --name azure-cli-iot-ext
    ```

1. 如果要在本地运行 Azure CLI，请使用以下命令登录 Azure 帐户（如果使用的是 Cloud Shell，则表示你已自动登录，并且无需运行此命令）：

    ```cmd/sh
    az login
    ```

1. 最后，使用以下命令创建一个名为 `myDeviceId` 的新设备标识并检索设备连接字符串：

    ```cmd/sh
    az iot hub device-identity create --device-id myDeviceId --hub-name {Your IoT Hub name}
    az iot hub device-identity show-connection-string --device-id myDeviceId --hub-name {Your IoT Hub name} -o table
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

记下结果中的设备连接字符串。 设备应用使用此设备连接字符串以设备身份连接到 IoT 中心。

<!-- images and links -->
[img-identity]: ./media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/quickstart-send-telemetry-dotnet.md
