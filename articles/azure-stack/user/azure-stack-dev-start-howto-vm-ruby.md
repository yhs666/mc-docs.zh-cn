---
title: 将 Ruby 应用部署到 Azure Stack 中的虚拟机 | Microsoft Docs
description: 将 Ruby 应用部署到 Azure Stack 中的虚拟机。
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: overview
origin.date: 04/24/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 8281eb4654fbea6e5f7b137cb469c2f65dcc2e5e
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249133"
---
# <a name="how-to-deploy-a-ruby-web-app-to-a-vm-in-azure-stack"></a>如何将 Ruby Web 应用部署到 Azure Stack 中的 VM

可以创建一个 VM 来托管 Azure Stack 中的 Ruby Web 应用。 本文介绍如何执行特定的步骤，以便设置服务器、配置用于托管 Ruby Web 应用的服务器，然后部署应用。

Ruby 是一种对各方面进行了谨慎平衡的语言。 其创建者 Yukihiro "Matz" Matsumoto 在融合了自己最喜欢的语言（Perl、Smalltalk、Eiffel、Ada 和 Lisp）的基础上创造了一种新的语言，对函数编程和命令式编程进行了平衡。 若要了解 Ruby 编程语言并找到适用于 Python 的其他资源，请参阅 [Ruby-lang.org](https://www.ruby-lang.org)。

本文将使用 Ruby 以及 Ruby on Rails Web 框架。

## <a name="create-a-vm"></a>创建 VM

1. 在 Azure Stack 中设置 VM。 遵循[部署 Linux VM 以在 Azure Stack 中托管 Web 应用](azure-stack-dev-start-howto-deploy-linux.md)中的步骤操作。

2. 在“VM 网络”边栏选项卡中，确保可访问以下端口：

    | 端口 | 协议 | 说明 |
    | --- | --- | --- |
    | 80 | HTTP | 超文本传输协议 (HTTP) 是一种适用于分布式协作型超媒体信息系统的应用程序协议。 客户端将使用 VM 的公共 IP 或 DNS 名称连接到你的 Web 应用。 |
    | 443 | HTTPS | 安全超文本传输协议 (HTTPS) 是超文本传输协议 (HTTP) 的扩展。 它用于通过计算机网络进行安全通信。 客户端将使用 VM 的公共 IP 或 DNS 名称连接到你的 Web 应用。 |
    | 22 | SSH | 安全外壳 (SSH) 是一种加密网络协议，用于在不安全的网络上安全地运行网络服务。 你将在 SSH 客户端上使用此连接来配置 VM 并部署应用。 |
    | 3389 | RDP | 可选。 远程桌面协议允许远程桌面连接使用计算机的图形用户界面。   |
    | 3000 | “自定义” | 开发中的 Ruby-on-rails Web 框架使用端口 3000。 对于生产服务器，需要通过 80 和 443 路由流量。 |

## <a name="install-ruby"></a>安装 Ruby

1. 使用 SSH 客户端连接到 VM。 有关说明，请参阅[使用 PuTTy 通过 SSH 进行连接](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)。
1. 安装 PPA 存储库。 在 VM 上的 bash 提示符下，键入以下命令：

    ```bash  
    sudo apt -y install software-properties-common
    sudo apt-add-repository ppa:brightbox/ruby-ng

    sudo apt update
    ```

2. 在 VM 上安装 Ruby 和 Ruby on Rails。 如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash  
    sudo apt install ruby
    gem install rails -v 4.2.6
    ```

3. 安装 Ruby on Rails 依赖项。 如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash  
    sudo apt-get install make
    sudo apt-get install gcc
    sudo apt-get install sqlite3
    sudo apt-get install nodejs
    sudo gem install sqlite
    sudo gem install bundler
    ```

    > [!Note]  
    > 在安装过程中，可能需要反复运行 `sudo gem install bundler`。 如果尝试安装依赖项但失败，请查看错误日志并解决问题。

4. 验证安装。 如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash  
        ruby -v
    ```

3. 安装 Git。 [Git](https://git-scm.com) 是一个广泛分布的修订控制和源代码管理 (SCM) 系统。 如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="create-and-run-an-app"></a>创建并运行应用

1. 如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash
        rails new myapp
        cd myapp
        rails server -b 0.0.0.0 -p 3000
    ```

2.  现在，导航到新服务器就会看到正在运行的 Web 应用程序。

    ```HTTP  
       http://yourhostname.chinacloudapp.cn:3000
    ```

## <a name="next-steps"></a>后续步骤

- 详细了解如何[针对 Azure Stack 进行开发](azure-stack-dev-start.md)
- 了解[用作 IaaS 的 Azure Stack 的常见部署](azure-stack-dev-start-deploy-app.md)。