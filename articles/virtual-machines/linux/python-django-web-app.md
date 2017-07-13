---
title: "在 Linux 上使用 Django 的 Python Web 应用 | Azure"
description: "了解如何在 Azure 中使用 Linux 虚拟机托管基于 Django 的 Web 应用程序。"
services: virtual-machines-linux
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-resource-manager
ms.assetid: 00ad4c2c-4316-4f9a-913f-f7f49b158db7
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: python
ms.topic: article
origin.date: 05/31/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: b202ccad64f8b877977dabfc7e22e3458d70eaf8
ms.sourcegitcommit: b3e981fc35408835936113e2e22a0102a2028ca0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# Linux VM 上的 Django Hello World Web 应用程序
<a id="django-hello-world-web-application-on-a-linux-vm" class="xliff"></a>
> [!div class="op_single_selector"]
> * [Windows](../windows/classic/python-django-web-app.md?toc=%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [Mac/Linux](../windows/classic/python-django-web-app.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

本教程介绍如何在 Azure 中使用 Linux 虚拟机托管基于 Django 的网站。 本教程假定你之前未使用过 Azure。 完成本指南之后，可获得在云中启动和运行的基于 Django 的应用程序。

你将了解如何执行以下操作：

* 设置 Azure 虚拟机以托管 Django。 虽然本教程介绍如何在 Linux 下实现此目的，但也可以使用托管在 Azure 中的 Windows Server VM 实现相同目的。 
* 从 Linux 创建新的 Django 应用程序。

通过按照本教程中的说明进行操作，你将构建一个简单的 Hello World Web 应用程序。 该应用程序将托管在 Azure 虚拟机中。

以下是已完成应用程序的屏幕快照：

![显示 Azure 上的 hello world 页面的浏览器窗口](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## 创建并配置 Azure 虚拟机以托管 Django
<a id="creating-and-configuring-an-azure-virtual-machine-to-host-django" class="xliff"></a>
1. 按照[此处](quick-create-portal.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)提供的说明创建 Ubuntu Server 14.04 LTS 分发的 Azure 虚拟机。  如果需要，可以选择密码身份验证而不是 SSH 公钥。
2. 使用[此处](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md)的说明编辑网络安全组，以允许将 http 流量传入到端口 80。
3. 默认情况下，新的虚拟机没有完全限定的域名 (FQDN)。  可以按照[此处](../windows/portal-create-fqdn.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)的说明创建一个。  完成本教程时，此步骤是可选的。

## <a id="setup"></a>设置开发环境
注意：如果需要安装 Python 或希望使用客户端库，请参阅 [Python 安装指南](../../python-how-to-install.md)。

Ubuntu Linux VM 已预安装了 Python 2.7，但它没有安装 Apache 或 Django。  按照以下步骤可连接到 VM 并安装 Apache 和 Django。

1. 启动新的终端窗口。
2. 输入以下命令连接到 Azure VM。  如果未创建 FQDN，可使用 Azure 门户的虚拟机摘要中显示的公共 IP 地址进行连接。

       $ ssh yourusername@yourVmUrl
3. 输入以下命令来安装 Django：

       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. 输入以下命令，安装带有 mod-wsgi 的 Apache：

       $ sudo apt-get install apache2 libapache2-mod-wsgi

## 创建新的 Django 应用程序
<a id="creating-a-new-django-application" class="xliff"></a>
1. 打开上一节中使用的终端窗口，通过 ssh 进入 VM。
2. 输入以下命令来创建新的 Django 项目：

       $ cd /var/www
       $ sudo django-admin.py startproject helloworld

   django-admin.py 脚本生成基于 Django 的网站的基本结构：

   * helloworld/manage.py 可帮助用户启动托管和停止托管基于 Django 的网站
   * helloworld/helloworld/settings.py 包含应用程序的 Django 设置。
   * helloworld/helloworld/urls.py 包含每个 url 及其视图之间的映射代码。
3. 在 /var/www/helloworld/helloworld 目录中创建一个名为 views.py 的新文件。 这会包含呈现“hello world”页面的视图。 启动编辑器并输入以下代码：

       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. 现在，将 urls.py 文件的内容替换为以下代码：

       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## 设置 Apache
<a id="setting-up-apache" class="xliff"></a>
1. 创建 Apache 虚拟主机配置文件 /etc/apache2/sites-available/helloworld.conf。 将内容设置为以下项，并将 yourVmName 替换为所使用的计算机的实际名称（例如 pyubuntu）。

       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. 使用以下命令启用该站点：

       $ sudo a2ensite helloworld
3. 使用以下命令重启 Apache：

       $ sudo service apache2 reload
4. 最后，在浏览器中加载网页：

   ![显示 Azure 上的 hello world 页面的浏览器窗口](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

## 关闭你的 Azure 虚拟机
<a id="shutting-down-your-azure-virtual-machine" class="xliff"></a>
完成本教程后，关闭并/或删除新创建的 Azure 虚拟机，以便为其他教程释放资源并避免产生 Azure 使用费。
