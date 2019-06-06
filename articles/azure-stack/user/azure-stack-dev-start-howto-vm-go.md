---
title: 如何将 Go Web 应用部署到 Azure Stack 中的虚拟机 | Microsoft Docs
description: 如何将 GO Web 应用部署到 Azure Stack 中的 VM
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: overview
origin.date: 04/24/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 9c106dd28a90f6a5c4a8153db8eef85f8fa5dd48
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249132"
---
# <a name="how-to-deploy-a-go-web-app-to-a-vm-in-azure-stack"></a>如何将 GO Web 应用部署到 Azure Stack 中的 VM

可以创建一个 VM 来托管 Azure Stack 中的 Go Web 应用。 本文介绍在设置服务器、配置服务器以托管 GO Web 应用以及随后部署应用时将遵循的步骤。

Go 富有表达力、简洁、干净、高效。 它的并发机制使得编写能够充分利用多核和网络计算机的程序变得容易，而它的类型系统支持灵活的模块化程序构造。 若要了解 Go 编程语言并找到适用于 GO 的其他资源，请参阅 [Golang.org](https://golang.org)。

## <a name="create-a-vm"></a>创建 VM

1. 在 Azure Stack 中设置 VM。 按照[部署 Linux VM 以在 Azure Stack 中托管 Web 应用](azure-stack-dev-start-howto-deploy-linux.md)中的步骤操作。

2. 在“VM 网络”边栏选项卡中，确保以下端口可访问：

    | 端口 | 协议 | 说明 |
    | --- | --- | --- |
    | 80 | HTTP | 超文本传输协议 (HTTP) 是一种适用于分布式协作型超媒体信息系统的应用程序协议。 客户端将使用 VM 的公共 IP 或 DNS 名称连接到你的 Web 应用。 |
    | 443 | HTTPS | 安全超文本传输协议 (HTTPS) 是超文本传输协议 (HTTP) 的扩展。 它用于通过计算机网络进行安全通信。 客户端将使用 VM 的公共 IP 或 DNS 名称连接到你的 Web 应用。 |
    | 22 | SSH | 安全外壳 (SSH) 是一种加密网络协议，用于在不安全的网络上安全地运行网络服务。 你将在 SSH 客户端上使用此连接来配置 VM 并部署应用。 |
    | 3389 | RDP | 可选。 远程桌面协议允许远程桌面连接使用计算机的图形用户界面。   |
    | 3000 | “自定义” | 开发中的 Go Web 框架使用端口 3000。 对于生产服务器，你需要通过 80 和 443 路由流量。 |

## <a name="install-go"></a>安装 GO

1. 使用 SSH 客户端连接到 VM。 有关说明，请参阅[使用 PuTTy 通过 SSH 连接](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)。
1. 在 VM 上的 bash 提示符下，键入以下命令：

    ```bash  
    wget https://dl.google.com/go/go1.10.linux-amd64.tar.gz
    sudo tar -xvf go1.10.linux-amd64.tar.gz
    sudo mv go /usr/local
    ```

2. 在 VM 上设置 GO 环境。 仍在 SSH 会话中连接到 VM，键入以下命令：

    ```bash  
    export GOROOT=/usr/local/go
    export GOPATH=$HOME/Projects/ADMFactory/Golang
    export PATH=$GOPATH/bin:$GOROOT/bin:$PATH

    vi ~/.profile
    ```

3. 验证你的安装。 仍在 SSH 会话中连接到 VM，键入以下命令：

    ```bash  
        go version
    ```

3. 安装 Git。 [Git](https://git-scm.com) 是一个广泛分布的修订控制和源代码管理 (SCM) 系统。 仍在 SSH 会话中连接到 VM，键入以下命令：

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="deploy-and-run-the-app"></a>部署和运行应用

1. 在 VM 上设置 Git 存储库。 仍在 SSH 会话中连接到 VM，键入以下命令：

    ```bash  
       git clone https://github.com/appleboy/go-hello
    
       cd go-hello
       go get -d
    ```

2. 启动应用。 仍在 SSH 会话中连接到 VM，键入以下命令：

    ```bash  
       go run hello-world.go
    ```

3.  现在导航到新服务器，应看到正在运行的 Web 应用程序。

    ```HTTP  
       http://yourhostname.chinacloudapp.cn:3000
    ```

## <a name="next-steps"></a>后续步骤

- 详细了解如何[针对 Azure Stack 进行开发](azure-stack-dev-start.md)
- 了解 [Azure Stack 作为 IaaS 的常见部署](azure-stack-dev-start-deploy-app.md)。