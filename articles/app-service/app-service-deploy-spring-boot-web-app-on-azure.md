---
title: "将 Spring Boot 应用程序部署到 Azure 应用服务 | Azure"
description: "本教程引导开发人员完成将 Spring Boot 入门 Web 应用部署到 Azure 应用服务的步骤。"
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
origin.date: 08/04/2017
ms.date: 10/30/2017
ms.author: v-yiso
ms.openlocfilehash: 4fd31d746b5948b395d1d8b8b992350c100d7759
ms.sourcegitcommit: 6ef36b2aa8da8a7f249b31fb15a0fb4cc49b2a1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2017
---
# <a name="deploy-a-spring-boot-application-to-the-azure-app-service"></a>将 Spring Boot 应用程序部署到 Azure 应用服务

**[Spring Framework]** 是可以帮助 Java 开发人员创建企业级应用程序的开源解决方案。构建在该平台基础之上的较热门项目之一是 [Spring Boot]，它提供一种简化的方法来创建独立的 Java 应用程序。

本教程将逐步讲解如何创建示例 Spring Boot 入门 Web 应用，并将其部署到 [Azure 应用服务]。

### <a name="prerequisites"></a>先决条件

若要完成本教程中的步骤，需要满足以下先决条件：

* 一个 Azure 订阅；如果你没有 Azure 订阅，可以注册[试用 Azure 帐户]。
* 最新的 [Java 开发人员工具包 (JDK)]。
* Apache 的 [Maven] 生成工具（版本 3）。
* 一个 [Git] 客户端。

## <a name="create-the-spring-boot-getting-started-web-app"></a>创建 Spring Boot 入门 Web 应用

以下步骤将引导你创建一个简单的 Spring Boot Web 应用程序并在本地对其进行测试。

1. 打开命令提示符并创建一个用于保存应用程序的本地目录，然后切换到该目录；例如：
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- 或 --
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. 将 [Spring Boot 入门]示例项目克隆到刚刚创建的目录；例如：
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. 将目录切换到已完成项目所在的目录；例如：
   ```
   cd gs-spring-boot
   cd complete
   ```

1. 使用 Maven 生成 JAR 文件；例如：
   ```
   mvn package
   ```

1. 创建 Web 应用后，请将目录切换到 JAR 文件所在的目录并启动该 Web 应用；例如：
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. 使用 Web 浏览器浏览到 http://localhost:8080 以测试该 Web 应用；如果有可用的 curl，请使用类似下面的示例语法：
   ```
   curl http://localhost:8080
   ```

1. 此时应显示以下消息：“来自 Spring Boot 的问候!”

   ![浏览示例应用][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a>创建用于 Java 的 Azure Web 应用

以下步骤将引导你创建 Azure Web 应用、配置 Java 所需的设置，然后配置 FTP 凭据。

1. 浏览到 [Azure 门户]并登录。

1. 在 Azure 门户中登录到帐户后，请单击“应用服务”对应的菜单图标：

   ![Azure 门户][AZ01]

1. 显示“应用服务”页后，请单击“+ 添加”创建新的应用服务。

   ![创建应用服务][AZ02]

1. 显示 Web 应用模板列表后，请单击“基本 Microsoft Web 应用”对应的链接。

   ![Web 应用模板][AZ03]

1. 显示 Web 应用模板的信息页后，请单击“创建”。

   ![创建 Web 应用][AZ04]

1. 提供 Web 应用的唯一名称并指定其他任何设置，然后单击“创建”。

   ![创建 Web 应用设置][AZ05]

1. 创建 Web 应用后，请单击“应用服务”对应的菜单图标，然后单击新建的 Web 应用：

   ![列出 Web 应用][AZ06]

1. 显示你的 Web 应用后，请使用以下步骤指定 Java 版本：

   a. 单击“应用程序设置”菜单项。

   b. 为 Java 版本选择“Java 8”。

   c. 为次要 Java 版本选择“最新”。

   d.单击“下一步”。 为 Web 容器选择“最新的 Tomcat 8.5”。 （实际上并不使用此容器；Azure 将使用 Spring Boot 应用程序中的容器。）

   e. 单击“保存” 。

   ![应用程序设置][AZ07]

1. 使用以下步骤指定 FTP 部署凭据：

   a. 单击“部署凭据”菜单项。

   b. 指定用户名和密码。

   c. 单击“保存” 。

   ![指定部署凭据][AZ08]

1. 使用以下步骤检索 FTP 连接信息：

   a. 单击“部署凭据”菜单项。

   b. 复制并保存完整的 FTP 用户名和 URL，以便在本教程的下一部分使用。

   ![FTP URL 和凭据][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a>将 Spring Boot Web 应用部署到 Azure

以下步骤引导你将 Spring Boot Web 应用部署到 Azure。

1. 打开文本编辑器（例如 Windows 记事本）并将以下文本粘贴到新文档中，然后将文件保存为 *web.config*：
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. 在系统中保存 *web.config* 文件后，请使用在本教程前一部分中获取的 URL、用户名和密码通过 FTP 连接到 Web 应用。 例如：
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.chinacloudapi.cn
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. 将远程目录切换到 Web 应用的根文件夹（路径为 */site/wwwroot*），然后复制 Spring Boot 应用程序中的 JAR 文件，以及前面创建的 *web.config*。 例如：
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. 将 JAR 和 *web.config* 文件部署到 Web 应用后，需要使用 Azure 门户重启 Web 应用：

   ![][AZ10]

1. 使用 Web 浏览器浏览到 Web 应用的 URL 以测试该 Web 应用；如果有可用的 curl，请使用类似下面的示例语法：
   ```
   curl http://wingtiptoys-springboot.chinacloudsites.cn/
   ```

1. 此时应显示以下消息：“来自 Spring Boot 的问候!”

   ![浏览示例应用][SB02]

## <a name="next-steps"></a>后续步骤

有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心]和[用于 Visual Studio Team Services 的 Java 工具]。

有关使用 FTP 将 Web 应用部署到 Azure 的更多信息，请参阅[使用 FTP/S 将应用部署到 Azure 应用服务]。

有关 Spring Boot 示例项目的更多详细信息，请参阅 [Spring Boot 入门]。

有关创建自己的 Spring Boot 应用程序的入门帮助，请参阅 https://start.spring.io/ 上的 **Spring Initializr**。

有关为 Web 应用配置其他设置的详细信息，请参阅[在 Azure 应用服务中配置 Web 应用]。

<!-- URL List -->

[Azure 应用服务]: https://www.azure.cn/home/features/app-service/
[Azure Java 开发人员中心]: /develop/java/
[Azure 门户]: https://portal.azure.cn/
[在 Azure 应用服务中配置 Web 应用]: /app-service-web/web-sites-configure
[使用 FTP/S 将应用部署到 Azure 应用服务]: /app-service-web/app-service-deploy-ftp
[试用 Azure 帐户]: https://www.azure.cn/pricing/1rmb-trial/
[Git]: https://github.com/
[Java 开发人员工具包 (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png