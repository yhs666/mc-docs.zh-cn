---
title: "Azure Stack 快速入门 - 创建 VM 门户"
description: "Azure Stack 快速入门 - 使用门户创建 Linux VM"
services: azure-stack
cloud: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
origin.date: 12/11/2017
ms.date: 03/08/2018
ms.author: v-junlch
ms.reviewer: 
ms.custom: mvc
ms.openlocfilehash: 946e26d6a373f582009836386969cce5bd9e1cb6
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-linux-virtual-machine-with-the-azure-stack-portal"></a>使用 Azure Stack 门户创建 Linux 虚拟机

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可以通过 Azure Stack 门户创建 Azure Stack 虚拟机。 此方法提供基于浏览器的用户界面用于创建和配置虚拟机及所有相关的资源。 本快速入门介绍如何快速创建 Linux 虚拟机并在其上安装 Web 服务器。

## <a name="prerequisites"></a>先决条件

- **Azure Stack Marketplace 中的 Linux 映像**

   默认情况下，Azure Stack Marketplace 不包含 Linux 映像。 因此，必须先使用[将 Marketplace 项从 Azure 下载到 Azure Stack](../azure-stack-download-azure-marketplace-item.md) 主题所述的步骤确保 Azure Stack 运营商已下载 **Ubuntu Server 16.04 LTS** 映像，然后才能创建 Linux 虚拟机。

- **可以访问 SSH 客户端**

   如果使用 Azure Stack 开发工具包 (ASDK)，可能无法访问环境中的 SSH 客户端。 如果是这样，可以从包含 SSH 客户端的多个包中进行选择。 例如，可以安装包含 SSH 客户端和 SSH 密钥生成器 (puttygen.exe) 的 PuTTY。 有关潜在可用选项的详细信息，请参阅以下相关 Azure 文章：[如何在 Azure 上的 Windows 中使用 SSH 密钥](/virtual-machines/linux/ssh-from-windows#windows-packages-and-ssh-clients)。

   本快速入门使用 PuTTY 生成 SSH 密钥并连接到 Linux 虚拟机。 若要下载并安装 PuTTY，请转到 [http://www.putty.org/](http://www.putty.org)。

## <a name="create-an-ssh-key-pair"></a>创建 SSH 密钥对

需要创建 SSH 密钥对才能完成本快速入门中的操作。 如果有现成的 SSH 密钥对，则可跳过此步骤。

1. 导航到 PuTTY 安装文件夹（默认位置为 ```C:\Program Files\PuTTY```）并运行 ```puttygen.exe```。
2. 在“PuTTY 密钥生成器”窗口中，确保“要生成的密钥类型”已设置为 **RSA**，并且“所生成密钥中的位数”已设置为 **2048**。 准备就绪时，单击“生成”。

   ![puttygen.exe](./media/azure-stack-quick-linux-portal/Putty01.PNG)

3. 若要完成密钥生成过程，请将鼠标光标移到“PuTTY 密钥生成器”窗口中。
4. 完成密钥生成过程后，单击“保存公钥”和“保存私钥”，将公钥和私钥保存到文件中。

   ![PuTTY 密钥](./media/azure-stack-quick-linux-portal/Putty02.PNG)



## <a name="sign-in-to-the-azure-stack-portal"></a>登录到 Azure Stack 门户

登录到 Azure Stack 门户。 Azure Stack 门户的地址取决于连接到的 Azure Stack 产品：

- 对于 Azure Stack 开发工具包 (ASDK)，请转到：https://portal.local.azurestack.external。
- 对于 Azure Stack 集成系统，请转到 Azure Stack 运营商提供的 URL。

## <a name="create-the-virtual-machine"></a>创建虚拟机

1. 在 Azure Stack 门户的左上角单击“创建资源”。

2. 依次选择“计算”、“Ubuntu Server 16.04 LTS”。
3. 单击“创建”。

4. 键入虚拟机信息。 对于“身份验证类型”，请选择“SSH 公钥”。 粘贴 SSH 公钥（前面已保存到文件中）时，请慎重删除任何前导或尾随空格。 完成后，单击“确定”。

   ![虚拟机基本信息](./media/azure-stack-quick-linux-portal/linux-01.PNG)

5. 为虚拟机选择“D1_V2”。

   ![虚拟机大小](./media/azure-stack-quick-linux-portal/linux-02.PNG)

6. 在“设置”页上保留默认值，然后单击“确定”。

7. 在“摘要”页上，单击“确定”开始部署虚拟机。


## <a name="connect-to-the-virtual-machine"></a>连接到虚拟机

1. 在虚拟机页上，单击“连接”。 随后会显示可用于连接到虚拟机的 SSH 连接字符串。

   ![连接虚拟机](./media/azure-stack-quick-linux-portal/linux-03.PNG)

2. 打开 PuTTY。
3. 在“PuTTY 配置”屏幕的“类别”下面，展开“SSH”，然后单击“身份验证”。单击“浏览”，选择前面保存的私钥文件。

   ![PuTTY 私钥](./media/azure-stack-quick-linux-portal/Putty03.PNG)
4. 在“类别”下面向上滚动，单击“会话”。
5. 在“主机名(或 IP 地址)”框中，粘贴前面在 Azure Stack 门户中看到的连接字符串。 在本示例中，该字符串为 ```asadmin@192.168.102.34```。
 
   ![PuTTY 会话](./media/azure-stack-quick-linux-portal/Putty04.PNG)
6. 单击“打开”打开与虚拟机之间的会话。

   ![Linus 会话](./media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-nginx"></a>安装 NGINX

使用以下 bash 脚本在虚拟机上更新包源并安装最新的 NGINX 包。 

```bash 
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

完成后，退出 SSH 会话，并返回到 Azure Stack 门户中的虚拟机“概述”页。


## <a name="open-port-80-for-web-traffic"></a>为 Web 流量打开端口 80 

网络安全组 (NSG) 保护入站和出站流量的安全。 在 Azure Stack 门户中创建虚拟机后，将在进行 SSH 连接的端口 22 上创建入站规则。 由于此虚拟机托管 webserver，因此需要为端口 80 创建 NSG 规则。

1. 在虚拟机的“概述”页上，单击**资源组**的名称。
2. 选择虚拟机的**网络安全组**。 可以通过“类型”列来标识 NSG。 
3. 在左侧菜单的“设置”下，单击“入站安全规则”。
4. 单击“添加” 。
5. 在“名称”中，键入“http”。 请确保将“端口范围”设置为 80，将“操作”设置为“允许”。 
6. 单击 **“确定”**。


## <a name="view-the-nginx-welcome-page"></a>查看 NGINX 欢迎页

在虚拟机上安装 NGINX 并打开端口 80 后，可通过虚拟机的公共 IP 地址访问 Web 服务器。 可在 Azure Stack 门户中的虚拟机“概述”页上找到该公共 IP 地址。

打开 Web 浏览器，并浏览到 ```http://<public IP address>```。

![NGINX 默认站点](./media/azure-stack-quick-linux-portal/linux-04.PNG)


## <a name="clean-up-resources"></a>清理资源

不再需要资源组、虚拟机和所有相关的资源时，可将其删除。 为此，请从虚拟机页中选择该资源组，并单击“删除”。

## <a name="next-steps"></a>后续步骤

在本快速入门中，我们部署了一个简单的 Linux 虚拟机、一条网络安全组规则，并安装了一个 Web 服务器。 有关 Azure Stack 虚拟机的详细信息，请转到 [Azure Stack 中虚拟机的注意事项](azure-stack-vm-considerations.md)。


