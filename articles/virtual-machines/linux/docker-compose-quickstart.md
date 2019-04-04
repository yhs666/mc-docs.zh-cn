---
title: 在 Azure 中的 Linux VM 上使用 Docker Compose | Azure
description: 如何通过 Azure CLI 在 Linux 虚拟机上安装和使用 Docker 和 Compose
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
origin.date: 02/14/2019
ms.date: 04/01/2019
ms.author: v-yeche
ms.openlocfilehash: 27087ed7e8570da322b36e354392e4b4dcdaabac
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58627165"
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>使用 Docker 和 Compose 在 Azure 中定义和运行多容器应用程序入门
借助 [Compose](https://github.com/docker/compose)，可以使用简单的文本文件定义由多个 Docker 容器组成的应用程序。 然后，使用单个命令启动应用程序，该命令会执行部署定义环境的所有操作。 作为示例，本文说明如何在 Ubuntu VM 上使用后端 MariaDB SQL 数据库快速设置 WordPress 博客。 也可以使用 Compose 设置更复杂的应用程序。

本文最后一次使用 [Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest) 版本 2.0.58 于 2019 年 2 月 14 日进行了测试。

<!--Not Available on [Azure local Shell](https://shell.azure.com/bash)-->

## <a name="create-docker-host-with-azure-cli"></a>使用 Azure CLI 创建 Docker 主机
安装最新的 [Azure CLI](https://docs.azure.cn/zh-cn/cli/install-az-cli2?view=azure-cli-latest) 并使用 [az login](https://docs.azure.cn/zh-cn/cli/reference-index?view=azure-cli-latest#az-login) 登录到 Azure 帐户。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

首先，使用 [az group create](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-create) 为 Docker 环境创建资源组。 以下示例在“chinaeast”位置创建名为“myResourceGroup”的资源组：

```azurecli
az group create --name myDockerGroup --location chinaeast
```

创建名为“cloud-init.txt”的文件并粘贴以下配置。 输入 `sensible-editor cloud-init.txt` 以创建文件并查看可用编辑器的列表。 

```yaml
#include https://get.docker.com
```

现在，请使用 [az vm create](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#az-vm-create) 创建 VM。 使用 `--custom-data` 参数传入 cloud-init 配置文件。 如果未将 cloud-init.txt 配置文件保存在现有工作目录中，请提供该文件的完整路径。 下面的示例创建了名为 myDockerVM 的 VM 并打开了 Web 流量端口 80。

```azurecli
az vm create \
    --resource-group myDockerGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
az vm open-port --port 80 \
    --resource-group myDockerGroup \
    --name myDockerVM
```

创建 VM、安装程序包和启动应用需耗时几分钟。 在 Azure CLI 返回提示之后，仍然存在继续运行的后台任务。 创建 VM 后，请记下 Azure CLI 显示的 `publicIpAddress`。 

## <a name="install-compose"></a>安装 Compose

通过 SSH 连接到新的 Docker 主机 VM。 提供自己的 IP 地址。

```bash
ssh azureuser@10.10.111.11
```

在 VM 上安装 Compose。

```bash
sudo apt install docker-compose
```

将看到类似于 *docker-compose 1.6.2, build 4d72027* 的输出。

> [!TIP]
> 如果使用另一种方法创建 Docker 主机，而且需要自行安装 Compose，请参阅 [Compose 文档](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md)。

<!-- URL is Correct to append .md posfix -->

## <a name="create-a-docker-composeyml-configuration-file"></a>创建 docker-compose.yml 配置文件
接下来，创建 `docker-compose.yml` 文件，它只是一个文本配置文件，用于定义要在 VM 上运行的 Docker 容器。 该文件指定要在每个容器中运行的映像（或者它可能从 Dockerfile 生成）、必要的环境变量和依赖关系、端口以及容器之间的链接。 有关 yml 文件语法的详细信息，请参阅 [Compose 文件参考](https://docs.docker.com/compose/compose-file/)。

创建 docker-compose.yml 文件。 使用最喜欢的文本编辑器向文件添加一些数据。 以下示例创建带有 `sensible-editor` 提示的文件，以便选择要使用的编辑器：

```bash
sensible-editor docker-compose.yml
```

将以下示例粘贴到 Docker Compose 文件中。 此配置将使用 [DockerHub 注册表](https://registry.hub.docker.com/_/wordpress/)中的映像安装 WordPress（开源博客和内容管理系统）和链接的后端 MariaDB SQL 数据库。 输入自己的 *MYSQL_ROOT_PASSWORD*，如下所示：

```sh
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="start-the-containers-with-compose"></a>使用 Compose 启动容器
在 *docker-compose.yml* 文件所在的目录中，运行以下命令（可能需要使用 `sudo` 运行 `docker-compose`，具体取决于环境）：

```bash
docker-compose up -d
```

此命令启动 *docker-compose.yml* 中指定的 Docker 容器。 此步骤需要一到两分钟才能完成。 会看到类似如下示例的输出：

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> 启动时请务必使用 **-d** 选项，使容器在后台继续运行。

若要验证容器是否已启动，请键入 `docker-compose ps`。 应看到类似如下的内容：

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

现在可以在 VM 的端口 80 上直接连接到 WordPress。 打开 Web 浏览器并输入 VM 的 DNS 名称（如 `http://mypublicdns.chinaeast.cloudapp.chinacloudapi.cn`）。 现在，应看到 WordPress 开始屏幕，可以在其中完成安装并开始使用应用程序。

<!-- URL is correct on http://mypublicdns.chinaeast.cloudapp.chinacloudapi.cn -->

![WordPress 开始屏幕][wordpress_start]

## <a name="next-steps"></a>后续步骤
* 转到 [Docker VM 扩展用户指南](https://github.com/Azure/azure-docker-extension/blob/master/README.md) ，了解用于在 Docker VM 中配置 Docker 和 Compose 的更多选项。 例如，其中一个选项是将 Compose yml 文件（已转换为 JSON）直接放在 Docker VM 扩展的配置中。
* 有关构建和部署多容器应用的更多示例，请查阅 [Compose 命令行参考](http://docs.docker.com/compose/reference/)和[用户指南](http://docs.docker.com/compose/)。
* 使用 Azure Resource Manager 模板（自己的或[社区](https://github.com/Azure/azure-quickstart-templates/)提供的），通过 Docker 部署 Azure VM 和使用 Compose 设置的应用程序。 例如， [使用 Docker 部署 WordPress 博客](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) 模板使用 Docker 和 Compose 通过 Ubuntu VM 上的 MySQL 后端快速部署 WordPress。
* 尝试将 Docker Compose 与 Docker Swarm 群集集成。 相关方案请参阅 [Using Compose with Swarm](https://docs.docker.com/compose/swarm/)（将 Compose 与 Swarm 配合使用）。

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png

<!--Update_Description: update meta properties -->