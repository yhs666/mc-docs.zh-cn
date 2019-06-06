---
title: 将 Java WAR 部署到 Azure Stack 中的虚拟机 | Microsoft Docs
description: 将 Java WAR 部署到 Azure Stack 中的虚拟机。
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: overview
origin.date: 04/24/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 11d0fc7a0904018af0369bb4550594025b70c15a
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249146"
---
# <a name="how-to-deploy-a-java-web-app-to-a-vm-in-azure-stack"></a>如何将 Java Web 应用部署到 Azure Stack 中的 VM

可以创建一个 VM 来托管 Azure Stack 中的 Python Web 应用。 本文介绍在设置服务器、配置服务器以托管 Python Web 应用以及随后部署应用时要遵循的步骤。

Java 是一种并发性的、基于类的、面向对象的通用计算机编程语言，旨在尽量减少实现依赖项。 它可以让应用程序开发人员做到“编写一次，随处运行”，即，编译的 Java 代码可在支持 Java 的所有平台上运行，而无需重新编译。 若要了解 Java 编程语言并查找适用于 Java 的其他资源，请参阅 [Java.com](https://www.java.com)。

本文逐步讲解如何在 Azure Stack 中的 Linux VM 上安装和配置 Apache Tomcat 服务器，然后将一个 Java Web 应用程序资源 (WAR) 文件载入该服务器。 WAR 文件用于分发共同构成 Web 应用程序的 JAR 文件、JavaServer 页、Java Servlet、Java 类、XML 文件、标记库、静态网页（HTML 和相关文件）和其他资源的集合。

Apache Tomcat（通常称作 Tomcat Server）是 Apache Software Foundation 开发的开源 Java Servlet 容器。 Tomcat 实施多种 Java EE 规范（包括 Java Servlet、JavaServer 页、Java EL 和 WebSocket），并提供一个“纯 Java 式”的 HTTP Web 服务器环境用于运行 Java 代码。

## <a name="create-a-vm"></a>创建 VM

1. 在 Azure Stack 中设置 VM。 遵循[部署 Linux VM 以在 Azure Stack 中托管 Web 应用](azure-stack-dev-start-howto-deploy-linux.md)中的步骤操作。

2. 在“VM 网络”边栏选项卡中，确保可访问以下端口：

    | 端口 | 协议 | 说明 |
    | --- | --- | --- |
    | 80 | HTTP | 超文本传输协议 (HTTP) 是一种适用于分布式协作型超媒体信息系统的应用程序协议。 客户端将使用 VM 的公共 IP 或 DNS 名称连接到你的 Web 应用。 |
    | 443 | HTTPS | 安全超文本传输协议 (HTTPS) 是超文本传输协议 (HTTP) 的扩展。 它用于通过计算机网络进行安全通信。 客户端将使用 VM 的公共 IP 或 DNS 名称连接到你的 Web 应用。 |
    | 22 | SSH | 安全外壳 (SSH) 是一种加密网络协议，用于在不安全的网络上安全地运行网络服务。 你将在 SSH 客户端上使用此连接来配置 VM 并部署应用。 |
    | 3389 | RDP | 可选。 远程桌面协议允许远程桌面连接使用计算机的图形用户界面。   |
    | 8080 | “自定义” | Apache Tomcat 服务的默认端口为 8080。 对于生产服务器，需要通过 80 和 443 路由流量。 |

## <a name="install-java"></a>安装 Java

1. 使用 SSH 客户端连接到 VM。 有关说明，请参阅[使用 PuTTy 通过 SSH 进行连接](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)。
2. 在 VM 上的 bash 提示符下，键入以下命令：

    ```bash  
        sudo apt-get install default-jdk
    ```

3. 验证安装。 如果仍在 SSH 会话中与 VM 保持连接，请键入以下命令：

    ```bash  
        java -version
    ```

## <a name="install-and-configure-tomcat"></a>安装并配置 Tomcat

1. 使用 SSH 客户端连接到 VM。 有关说明，请参阅[使用 PuTTy 通过 SSH 进行连接](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)。

2. 创建 Tomcat 用户。
    - 首先创建新的 Tomcat 组：
        ```bash  
            sudo groupadd tomcat
        ```
     
    - 然后创建新的 Tomcat 用户，并将此用户指定为 tomcat 组的成员，该组的主目录为 `/opt/tomcat`（将在其中安装 Tomcat），shell 为 `/bin/false`（因此没有任何人可以登录到该帐户）：
        ```bash  
            sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
        ```

3. 安装 Tomcat。
    - 首先从 Tomcat 8 下载页 ([http://tomcat.apache.org/download-80.cgi](http://tomcat.apache.org/download-80.cgi)) 获取最新版 Tomcat 8 的 tar 的 URL。
    - 然后使用 curl 通过该链接下载最新版本。

        ```bash  
            cd /tmp 
            curl -O <URL for the tar for the latest version of Tomcat 8>
        ```

    - 最后将 Tomcat 安装到 `/opt/tomcat` 目录。 创建目录，然后使用以下命令解压缩存档：

        ```bash  
            sudo mkdir /opt/tomcat
            sudo tar xzvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
            sudo chown -R tomcat webapps/ work/ temp/ logs/
        ```

4. 更新对 Tomcat 的权限。

    ```bash  
        sudo chgrp -R tomcat /opt/tomcat
        sudo chmod -R g+r conf
        sudo chmod g+x conf
    ```

5. 创建 `systemd` 服务文件， 以便可将 Tomcat 作为服务运行。

    - Tomcat 需要知道 Java 的安装位置。 此路径通常称为 **JAVA_HOME**。 运行以下命令找到该位置：

        ```bash  
            sudo update-java-alternatives -l
        ```

        这会生成如下所示的信息：

        ```Text  
            Output
            java-1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
        ```

        可以通过提取输出中的路径并添加 `/jre` 来构造 **JAVA_HOME** 变量值。 沿用上面的示例：`/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre`

    - 使用服务器中的值创建 systemd 服务文件。

        ```bash  
            sudo nano /etc/systemd/system/tomcat.service
        ```

    - 将以下内容粘贴到该服务文件中。 根据需要修改 **JAVA_HOME** 的值，使之与系统上的值匹配。 还可以修改 CATALINA_OPTS 中指定的内存分配设置：

        ```Text  
            [Unit]
            Description=Apache Tomcat Web Application Container
            After=network.target
            
            [Service]
            Type=forking
            
            Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre
            Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
            Environment=CATALINA_HOME=/opt/tomcat
            Environment=CATALINA_BASE=/opt/tomcat
            Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
            Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'
            
            ExecStart=/opt/tomcat/bin/startup.sh
            ExecStop=/opt/tomcat/bin/shutdown.sh
        
            User=tomcat
            Group=tomcat
            UMask=0007
            RestartSec=10
            Restart=always
            
            [Install]
            WantedBy=multi-user.target
        ```

    - 保存并关闭该文件。
    - 重新加载 systemd 守护程序，使其能够识别上述服务文件：

        ```bash  
            sudo systemctl daemon-reload
        ```

    - 启动 Tomcat 服务 

        ```bash  
            sudo systemctl start tomcat
        ```

    - 键入以下命令，验证该服务是否已启动且未出错：

        ```bash  
            ssudo systemctl status tomcat
        ```

6. 验证 Tomcat 服务器。 Tomcat 使用端口 8080 来接受传统的请求。 运行以下命令，允许流量流向该端口：

    ```bash  
        sudo ufw allow 8080
    ```

    如果尚未为 Azure Stack VM 添加**入站端口规则**，现在请添加这些规则。 有关详细信息，请参阅[创建 VM](#create-a-vm)。

7. 在 Azure Stack 所在的同一网络中打开浏览器，然后打开服务器 `yourmachine.local.cloudapp.azurestack.external:8080`。

    ![Azure Stack VM 上的 Apache Tomcat](media/azure-stack-dev-start-howto-vm-java/apache-tomcat.png)

    此时会加载服务器上的 Apache Tomcat 页。 接下来，将服务器配置为允许你访问服务器状态、管理器应用和主机管理器。

8. 启用服务文件，以便在重新启动服务器时自动启动 Tomcat：

    ```bash  
        sudo systemctl enable tomcat
    ```

9. 将 Tomcat 服务器配置为允许你访问 Web 管理界面。 编辑 `tomcat-users.xml` 并定义一个角色和用户，以便能够登录。 将用户定义为可访问 `manager-gui` 和 `admin-gui`。

    ```bash  
        sudo nano /opt/tomcat/conf/tomcat-users.xml
    ```

    - 在 `<tomcat-users>` 节中添加以下元素：

    ```XML  
        <role rolename="tomcat"/>
        <user username="<username>" password="<password>" roles="tomcat,manager-gui,admin-gui"/>
    ```

    - 例如，最终的文件可能如下所示：

    ```XML  
        <tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
         <role rolename="tomcat"/>
        <user username="tomcatuser" password="changemepassword" roles="tomcat,manager-gui,admin-gui"/>
        </tomcat-users>
    ```

    - 保存并关闭该文件。


10. Tomcat 会将“管理器”和“主机管理器”应用的访问权限限制为来自服务器的连接。   由于你要在 Azure Stack 中的 VM 上安装 Tomcat，因此需要解除此限制。 通过编辑相应的 `context.xml` 文件来更改对这些应用的 IP 地址限制。

    - 更新 `context.xml` 管理器应用：

        ```bash  
            sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
        ```

    - 注释掉 IP 地址限制以允许从任何位置进行连接，或添加用于连接 Tomcat 的计算机的 IP 地址。

        ```XML  
        <Context antiResourceLocking="false" privileged="true" >
          <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
                 allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
        </Context>
        ```

    - 保存并关闭该文件。

    - 使用类似的更新命令更新 `context.xml` 主机管理器应用：

        ```bash  
            sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
        ```

    - 保存并关闭该文件。

11. 重启 Tomcat 服务以使用所做的更改更新服务器：

    ```bash  
        sudo systemctl restart tomcat
    ```

12. 在 Azure Stack 所在的同一网络中打开浏览器，然后打开服务器 `yourmachine.local.cloudapp.azurestack.external:8080`。

    - 选择“服务器状态”以查看 Tomcat 服务器的状态，并验证你是否拥有访问权限。
    - 使用 Tomcat 凭据登录。

![Azure Stack VM 上的 Apache Tomcat](media/azure-stack-dev-start-howto-vm-java/apache-tomcat-management-app.png)

## <a name="create-an-app"></a>创建应用

需要创建一个要部署到 Tomcat 的 WAR。 如果你只是想要检查环境，可以在 Apache TomCat 站点上找到一个示例 War：[示例应用](https://tomcat.apache.org/tomcat-6.0-doc/appdev/sample/)。

有关在 Azure 中开发 Java 应用的指导，请参阅[在 Azure 中生成和部署 Java 应用](https://azure.microsoft.com/develop/java/)。

## <a name="deploy-and-run-the-app"></a>部署和运行应用

1. 使用 SSH 客户端连接到 VM。 有关说明，请参阅[使用 PuTTy 通过 SSH 进行连接](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)。
1. 停止 Tomcat 服务，以使用应用包更新服务器：

    ```bash  
        sudo systemctl stop tomcat
    ```

2.  将 FTP 用户添加到 tomcat 组，以便可以写入 webapps 文件夹。 该 FTP 用户是在 Azure Stack 中创建 VM 时定义的用户。

    ```bash  
        sudo usermod -a -G tomcat <VM-user>
    ```

3. 使用 FileZilla 连接到 VM 以清除 webapps 文件夹，然后加载新的或已更新的 WAR。 有关使用 FileZila 的说明，请参阅[使用 FileZilla 通过 SFTP 进行连接](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-sftp-with-filezilla)。
    - 清除 `TOMCAT_HOME/webapps`。
    - 将 WAR 添加到 ` TOMCAT_HOME/webapps`，例如 `/opt/tomcat/webapps/`。

4.  Tomcat 会自动扩展并部署应用程序。 可以使用先前创建的 DNS 名称查看该应用程序。 例如：

    ```HTTP  
       http://yourmachine.local.cloudapp.azurestack.external:8080/sample

## Next steps

- Learn more about how to [Develop for Azure Stack](azure-stack-dev-start.md)
- Learn about [common deployments for Azure Stack as IaaS](azure-stack-dev-start-deploy-app.md).