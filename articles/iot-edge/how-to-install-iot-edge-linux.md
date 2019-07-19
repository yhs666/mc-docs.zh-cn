---
title: 在 Linux 上安装 Azure IoT Edge
description: 在运行 Ubuntu 的 Linux AMD64 设备上安装 Azure IoT Edge 的相关说明
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
origin.date: 06/27/2019
ms.date: 07/22/2019
ms.author: v-yiso
ms.openlocfilehash: 580d3b5d1a980e2a35ab9a1ee2e64cd785103130
ms.sourcegitcommit: f4351979a313ac7b5700deab684d1153ae51d725
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67845235"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>在 Linux 上安装 Azure IoT Edge 运行时 (x64)

使用 Azure IoT Edge 运行时可将设备转变为 IoT Edge 设备。 该运行时可以部署在像 Raspberry Pi 一样小的设备上，也可以部署在像工业服务器一样大的设备上。 使用 IoT Edge 运行时配置设备后，即可开始从云中部署业务逻辑。 

若要了解详细信息，请参阅[了解 Azure IoT Edge 运行时及其体系结构](iot-edge-runtime.md)。

本文列出了在 Ubuntu Linux x64 (Intel/AMD) IoT Edge 设备上安装 Azure IoT Edge 运行时的步骤。 有关支持的 AMD64 操作系统的列表，请参阅 [Azure IoT Edge 支持的系统](support.md#operating-systems)。

>[!NOTE]
>Linux 软件存储库中的包受到每个包中的许可条款限制 (/usr/share/doc/*package-name*)。 使用程序包之前请阅读许可条款。 安装和使用程序包即表示接受这些条款。 如果不同意许可条款，则不要使用程序包。

## <a name="install-the-latest-version"></a>安装最新版本

根据以下部分的说明，将最新版 Azure IoT Edge 服务安装到设备上。 

### <a name="register-microsoft-key-and-software-repository-feed"></a>注册 Microsoft 密钥和软件存储库源

准备设备，以便安装 IoT Edge 运行时。


安装存储库配置。 根据你的 Ubuntu 版本选择合适的 **16.04** 或 **18.04** 代码片段：

> [!NOTE]
> 确保根据你的 Ubuntu 版本从正确的代码框中选择代码片段。

* 对于 **Ubuntu 16.04**：
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   ```

* 对于 **Ubuntu 18.04**：
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
   ```
   
复制生成的列表。

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

安装 Microsoft GPG 公钥

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-the-container-runtime"></a>安装容器运行时

Azure IoT Edge 依赖于 [OCI 兼容的](https://www.opencontainers.org/)容器运行时。 对于生产方案，建议使用下面提供的[基于 Moby](https://mobyproject.org/) 的引擎。 这是官方唯一支持用于 Azure IoT Edge 的容器引擎。 Docker CE/EE 容器映像与 Moby 运行时兼容。

执行 apt 更新。

```bash
sudo apt-get update
```

安装 Moby 引擎。 

```bash
sudo apt-get install moby-engine
```

安装 Moby 命令行接口 (CLI)。 CLI 对开发非常有用，但对生产部署来说是可选的。

```bash
sudo apt-get install moby-cli
```

#### <a name="verify-your-linux-kernel-for-moby-compatibility"></a>验证 Linux 内核的 Moby 兼容性

许多嵌入式设备制造商寄送的设备映像包含的自定义 Linux 内核可能缺少确保容器运行时兼容所需的功能。 如果在安装建议的 [Moby](https://github.com/moby/moby) 容器运行时时遇到问题，则可在设备上运行以下命令，使用在官方 [Moby Github 存储库](https://github.com/moby/moby)中提供的 [check-config](https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh) 脚本排查 Linux 内核配置问题。

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

这样就会提供一个详细的输出，其中包含 Moby 运行时使用的内核功能的状态。 需确保启用 `Generally Necessary` 和 `Network Drivers` 下的所有项，这样才能确保内核完全兼容 Moby 运行时。  如果确定有缺失的功能，则可启用它们，方法是：通过源重新构建内核，然后选择关联的模块，将其包括在相应的内核 .config 中。同样，如果使用内核配置生成器（例如 defconfig 或 menuconfig），则需找到并启用相应的功能，然后以相应方式重新构建内核。  部署新修改的内核以后，请再次运行 check-config 脚本，验证是否已成功启用所确定的功能。

### <a name="install-the-azure-iot-edge-security-daemon"></a>安装 Azure IoT Edge 安全守护程序

**IoT Edge 安全守护程序**提供和维护 IoT Edge 设备上的安全标准。 守护程序在每次开机时启动，并通过启动 IoT Edge 运行时的其余部分来启动设备。

安装命令还会安装标准版本的 **libiothsm**（如果尚未存在）。

执行 apt 更新。

```bash
sudo apt-get update
```

安装安全守护程序。 该包安装在 `/etc/iotedge/`。

```bash
sudo apt-get install iotedge
```

IoT Edge 成功安装以后，输出会提示你更新配置文件。 执行[配置 Azure IoT Edge 安全守护程序](#configure-the-azure-iot-edge-security-daemon)部分的步骤，完成设备预配。 

## <a name="install-a-specific-version"></a>安装特定版本

若要安装特定版本的 Azure IoT Edge，可以直接将 IoT Edge GitHub 存储库中的组件文件作为目标。 使用以下步骤将所有 IoT Edge 组件安装到设备上：首先是 Moby 引擎和 CLI，然后是 libiothsm，最后是 IoT Edge 安全守护程序。

1. 导航到 [Azure IoT Edge 版本](https://github.com/Azure/azure-iotedge/releases)，找到需要将其作为目标的发行版。 

2. 展开该版本的“资产”部分。 

3. 任何给定版本中的 Moby 引擎不一定有更新。 如果看到以 **moby-engine** 和 **moby-cli** 开头的文件，请使用以下命令更新这些组件。 如果没有看到任何 Moby 文件，并且尚未在设备上安装 Moby，则请回到更低版资产中查找它们。 

   1. 找到与 IoT Edge 设备的体系结构匹配的 **moby-engine** 文件。 右键单击文件链接并复制链接地址。

   2. 将复制的链接用在以下命令中，安装该版本的 Moby 引擎： 

      ```bash
      curl -L <moby-engine link> -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb
      ```

   3. 找到与 IoT Edge 设备的体系结构匹配的 **moby-cli** 文件。 Moby CLI 是一个可选组件，但在开发过程中可能很有用。 右键单击文件链接并复制链接地址。 

   4. 将复制的链接用在以下命令中，安装该版本的 Moby CLI： 

      ```bash
      curl -L <moby-cli link> -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb
      ```

4. 每个版本应该都有用于 IoT Edge 安全守护程序和 hsmlib 的新文件。 使用以下命令更新这些组件。 

   1. 找到与 IoT Edge 设备的体系结构匹配的 **libiothsm-std** 文件。 右键单击文件链接并复制链接地址。 

   2. 将复制的链接用在以下命令中，安装该版本的 hsmlib：

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```
   
   3. 找到与 IoT Edge 设备的体系结构匹配的 **iotedge** 文件。 右键单击文件链接并复制链接地址。 

   4. 将复制的链接用在以下命令中，安装该版本的 IoT Edge 安全守护程序： 

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

IoT Edge 成功安装以后，输出会提示你更新配置文件。 按照下一部分中的步骤完成设备预配。 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>配置 Azure IoT Edge 安全守护程序

配置 IoT Edge 运行时以将物理设备与 Azure IoT 中心中存在的设备标识相链接。 

可以使用 `/etc/iotedge/config.yaml` 处的配置文件配置守护程序。 默认情况下，该文件有写保护，你可能需要提升权限才能对其进行编辑。

可以使用 IoT 中心提供的设备连接字符串手动预配单个 IoT Edge 设备。 或者，可以使用设备预配服务自动预配设备，当需要预配多个设备时这会非常有用。 根据预配选项，选择合适的安装脚本。 

### <a name="option-1-manual-provisioning"></a>选项 1：手动预配

若要手动预配设备，需要为其提供[设备连接字符串](how-to-register-device-portal.md)，可以通过在 IoT 中心注册新设备来创建该设备连接字符串。


打开配置文件。 

```bash
sudo nano /etc/iotedge/config.yaml
```

找到文件的 provisioning 节。 取消注释 **manual** 预配模式，并确保注释掉 dps 预配模式。使用 IoT Edge 设备的连接字符串更新 **device_connection_string** 的值。

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning: 
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

保存并关闭该文件。 

   `CTRL + X`、`Y`、`Enter`

在配置文件中输入预配信息后，重启守护程序：

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>选项 2：自动预配

若要自动预配设备，请[设置设备预配服务并检索设备注册 ID](how-to-auto-provision-simulated-device-linux.md)。 自动预配仅适用于具有受信任的平台模块 (TPM) 芯片的设备。 例如，默认情况下，Raspberry Pi 设备未附带 TPM。

打开配置文件。 

```bash
sudo nano /etc/iotedge/config.yaml
```

找到文件的 provisioning 节。 取消注释 **dps** 预配模式，并注释掉 manual 节。 使用 IoT 中心设备预配服务中的值更新 **scope_id** 和 **registration_id** 的值，并使用 TPM 更新 IoT Edge 设备。

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning: 
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

保存并关闭该文件。 

   `CTRL + X`、`Y`、`Enter`

在配置文件中输入预配信息后，重启守护程序：

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>验证是否成功安装

如果使用了上一部分中的**手动配置**步骤，则应在设备上成功预配并运行 IoT Edge 运行时。 如果使用了**自动配置**步骤，则需要完成一些额外的步骤，以便运行时可以代表你向 IoT 中心注册你的设备。 有关后续步骤，请参阅[在 Linux 虚拟机上创建和预配模拟 TPM IoT Edge 设备](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm)。

使用以下命令检查 IoT Edge 守护程序的状态：

```bash
systemctl status iotedge
```

使用以下命令查看守护程序日志：

```bash
journalctl -u iotedge --no-pager --no-full
```

此外，还可使用以下命令列出正在运行的模块：

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>提示和建议

需要提升的权限才能运行 `iotedge` 命令。 安装运行时后，请从计算机中注销并重新登录以自动更新权限。 在此之前，在任何 `iotedge` 命令前都要使用 **sudo**。

在资源受限的设备上，强烈建议按照[故障排除指南](troubleshoot.md)中的说明将 *OptimizeForPerformance* 环境变量设置为 *false*。

如果网络具有代理服务器，请按照[配置 IoT Edge 设备以通过代理服务器进行通信](how-to-configure-proxy-support.md)中的步骤进行操作。

## <a name="uninstall-iot-edge"></a>卸载 IoT Edge

如果要从 Linux 设备中删除 IoT Edge 安装，请从命令行使用以下命令。

删除 IoT Edge 运行时。

```bash
sudo apt-get remove --purge iotedge
```

删除 IoT Edge 运行时以后，已创建的容器会被停止，但仍存在于设备上。 查看所有容器以了解哪些容器仍然存在。

```bash
sudo docker ps -a
```

从设备中删除容器，包括两个运行时容器。

```bash
sudo docker rm -f <container name>
```

最后，从设备中删除容器运行时。

```bash
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>后续步骤

预配了安装运行时的 IoT Edge 设备后，现在可以[部署 IoT Edge 模块](how-to-deploy-modules-portal.md)。

如果无法正确安装 IoT Edge 运行时，请参阅[故障排除](troubleshoot.md)页。

若要将现有安装更新到最新版本的 IoT Edge，请参阅[更新 IoT Edge 安全守护程序和运行时](how-to-update-iot-edge.md)。
