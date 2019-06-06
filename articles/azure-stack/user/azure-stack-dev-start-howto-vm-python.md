---
title: 如何将 Python Web 应用部署到 Azure Stack 中的虚拟机 | Microsoft Docs
description: 将 Python Web 应用部署到 Azure Stack 中的虚拟机。
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: overview
origin.date: 04/24/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: c97d019ed430b7f124d40b4246bcce811d53a8e5
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249144"
---
# <a name="how-to-deploy-a-python-web-app-to-a-vm-in-azure-stack"></a>如何将 Python Web 应用部署到 Azure Stack 中的 VM

可以创建一个 VM 来托管 Azure Stack 中的 Java Web 应用。 本文介绍在设置服务器、配置服务器以托管 Python Web 应用以及随后部署应用时要遵循的步骤。

Python 是一种经过解释的高级通用编程语言。 Python 由 Guido van Rossum 创造，在 1991 年首次发布，其设计宗旨是强调代码可读性，注重使用空格。 它提供的构造可以进行清晰的编程，不管是小规模还是大规模。 若要了解 Python 编程语言并找到适用于 Python 的其他资源，请参阅 [Python.org](https://www.python.org)。

本文将使用 Python 3.x，在 Ngnix 服务器的虚拟环境中运行 Flask。

## <a name="create-a-vm"></a>创建 VM

1. 在 Azure Stack 中设置 VM。 遵循[部署 Linux VM 以在 Azure Stack 中托管 Web 应用](azure-stack-dev-start-howto-deploy-linux.md)中的步骤操作。

2. 在“VM 网络”边栏选项卡中，确保可访问以下端口：

    | 端口 | 协议 | 说明 |
    | --- | --- | --- |
    | 80 | HTTP | 超文本传输协议 (HTTP) 是一种适用于分布式协作型超媒体信息系统的应用程序协议。 客户端将使用 VM 的公共 IP 或 DNS 名称连接到你的 Web 应用。 |
    | 443 | HTTPS | 安全超文本传输协议 (HTTPS) 是超文本传输协议 (HTTP) 的扩展。 它用于通过计算机网络进行安全通信。 客户端将使用 VM 的公共 IP 或 DNS 名称连接到你的 Web 应用。 |
    | 22 | SSH | 安全外壳 (SSH) 是一种加密网络协议，用于在不安全的网络上安全地运行网络服务。 你将在 SSH 客户端上使用此连接来配置 VM 并部署应用。 |
    | 3389 | RDP | 可选。 远程桌面协议允许远程桌面连接使用计算机的图形用户界面。   |
    | 5000、8000 | “自定义” | 开发中的 Flask Web 框架使用端口 5000 和 8000。 对于生产服务器，需要通过 80 和 443 路由流量。 |

## <a name="install-python"></a>安装 Python

1. 使用 SSH 客户端连接到 VM。 有关说明，请参阅[使用 PuTTy 通过 SSH 进行连接](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)。
2. 在 VM 上的 bash 提示符下，键入以下命令：

    ```bash  
    sudo apt-get -y install python3 python3-venv python3-dev
    ```

3. 验证安装。 如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash  
        python -version
    ```


3. 安装 Nginx。 [Nginx](https://www.nginx.com/resources/wiki/) 是一个 Web 服务器，也可用作反向代理、负载均衡器、邮件代理以及 HTTP 缓存。 如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash  
       sudo apt-get -y install nginx git
    ```

4. 安装 Git。 [Git](https://git-scm.com) 是一个广泛分布的修订控制和源代码管理 (SCM) 系统。 如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="deploy-and-run-the-app"></a>部署和运行应用

1. 在 VM 上设置 Git 存储库。 如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash  
       git clone https://github.com/mattbriggs/flask-hello-world.git
    
       cd flask-hello-world
    ```

2. 创建一个虚拟环境，在其中填充所有包依赖项。  如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash  
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    
    export FLASK_APP=application.py
    # export FLASK_DEBUG=1 
    flask run -h 0.0.0.0
    ```

3.  现在，导航到新服务器就会看到正在运行的 Web 应用程序。

    ```HTTP  
       http://yourhostname.chinacloudapp.cn:5000
    ```

## <a name="update-your-server"></a>更新服务器

1. 在 SSH 会话中连接到 VM。 通过键入 `CTRL+C` 来停止服务器。
2. 键入以下命令：

    ```bash  
    deactivate
    open the git repo
    git pull
    ```

3. 激活虚拟环境并启动应用

    ```bash  
    source venv/bin/activate
    export FLASK_APP=application.py
    flask run -h 0.0.0.0
    ```

## <a name="next-steps"></a>后续步骤

- 详细了解如何[针对 Azure Stack 进行开发](azure-stack-dev-start.md)
- 了解[用作 IaaS 的 Azure Stack 的常见部署](azure-stack-dev-start-deploy-app.md)。
