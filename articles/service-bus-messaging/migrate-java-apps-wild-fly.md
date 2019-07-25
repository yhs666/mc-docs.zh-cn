---
title: 将 Java Enterprise Edition 应用迁移到 Azure | Microsoft Docs
description: 了解将 Java Enterprise Edition (EE) 应用迁移到 Azure 的方法之一
services: service-bus-messaging
documentationcenter: ''
author: selvasingh
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2019
ms.author: asirveda
ms.openlocfilehash: c0aa19058814219e591302b5ff58141cb9c7cea7
ms.sourcegitcommit: 68f7c41974143a8f7bd9b7a54acf41c09893e587
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68332698"
---
# <a name="migrate-java-enterprise-edition-ee-apps-to-azure"></a>将 Java Enterprise Edition (EE) 应用迁移到 Azure
本文引导你完成将现有 Java EE 工作负荷迁移到 Azure 的过程：
 
- 将 Java 企业应用（消息驱动的企业 bean）迁移到 Linux 上 Azure 应用服务
- 将应用的消息传送子系统迁移到 Azure 服务总线
- 对交互式 Java 工作负荷使用 WebSocket 

## <a name="in-this-article"></a>本文内容

* [要迁移到云的内容](#what-you-will-migrate-to-cloud)
* [先决条件](#prerequisites)
* [入门](#get-started)
    * [克隆 Git 存储库并做好准备](#clone-and-prepare-the-git-repository)
* [生成示例存档](#build-the-sample-archive)
* [生成控制台应用 - 使用 Java 消息服务 (JMS) 发送和接收服务总线消息](#build-and-run-console-app-to-send-and-receive-messages)
    * [创建并配置 Azure 服务总线](#create-and-configure-azure-service-bus)
    * [生成并运行控制台应用](#build-and-run-the-console-app)
* [将消息驱动的企业 bean 迁移到 Azure](#migrate-a-message-driven-enterprise-bean-to-azure)
    * [准备环境](#prepare-environment)
    * [将应用部署到应用服务 Linux](#deploy-an-app-to-app-service-linux)
    * [配置 JMS 资源适配器 (JMS RA)](#configure-the-jms-resource-adapter-jms-ra)
    * [了解如何配置 WildFly](#understand-how-to-configure-wildfly)
    * [通过 FTP 将启动和二进制项目上传到应用](#upload-startup-and-binary-artifacts-to-app-through-ftp)
        * [获取 FTP 部署凭据](#get-ftp-deployment-credentials)
        * [通过 FTP 将启动和二进制项目上传到应用](#upload-startup-and-binary-artifacts-to-app-through-ftp)
    * [测试 JBoss/WildFly 启动脚本和 CLI 命令以配置 JMS RA](#test-the-jbosswildfly-startup-script-and-cli-commands-to-configure-jms-ra)
        * [测试 startup.sh 脚本](#test-the-startupsh-script)
    * [重启远程 WildFly 应用服务器](#restart-the-remote-wildfly-app-server)
    * [将 WildFly/JBoss 日志流式传输到开发计算机](#stream-wildflyjboss-logs-to-a-dev-machine)
    * [在 Azure 上打开消息驱动的企业 bean](#open-the-message-driven-enterprise-bean-on-azure)
    * [其他信息](#additional-information)
* [迁移使用 WebSocket 的 Java 企业应用](#migrate-java-ee-app-that-uses-websockets)
    * [将应用部署到应用服务 Linux](#deploy-an-app-to-app-service-linux)
    * [在应用服务 Linux 上打开已迁移的应用](#open-the-migrated-app-on-app-service-linux)
* [后续步骤](#next-steps)

## <a name="what-you-will-migrate-to-cloud"></a>要迁移到云的内容
你要将 WildFly/JBoss 示例应用迁移到 Azure。 这些应用使用：

- Java Standard Edition (SE) 8
- Java Enterprise Edition (EE) 7
- [Java 规范请求 (JSR) 343 Java 消息服务 2.0 (JMS)](https://jcp.org/en/jsr/detail?id=343)
- [Java 规范请求 (JSR) 356 用于 WebSocket 的 Java API](https://jcp.org/en/jsr/detail?id=356)

迁移后，你将使用 Azure 服务总线来运行这些应用。

## <a name="prerequisites"></a>先决条件
若要将 Java Web 应用部署到 Azure，需要一个 Azure 订阅。 如果还没有 Azure 订阅，可以激活 [MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或注册获取[免费 Azure 帐户](https://azure.microsoft.com/free/)。

此外，需要符合以下先决条件：

- [Azure CLI](/cli/azure/get-started-with-azure-cli) 
- [Java 8](https://www.azul.com/downloads/azure-only/zulu/) 
- [Maven 3](https://maven.apache.org/) 
- [Git](https://github.com/)

## <a name="get-started"></a>入门
可以从头开始完成每个步骤，或者，可以绕过你熟悉的基本设置步骤。 无论如何，最终都会得到正常运行的代码。

### <a name="clone-and-prepare-the-git-repository"></a>克隆并准备 Git 存储库 

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/migrate-java-ee-app-to-azure-2
cd migrate-Java-EE-app-to-azure-2
yes | cp -rf .prep/* .
```

## <a name="build-the-sample-archive"></a>生成示例存档
使用 Maven 生成示例存档。 此步骤需要花费几分钟时间。

```bash
cd quickstart
mvn clean install
```

下面是示例输出： 

```bash
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO] 
[INFO] Quickstart: Parent
[INFO] Quickstart: app-client
[INFO] Quickstart: app-client - ejb
[INFO] Quickstart: app-client - client-simple
[INFO] Quickstart: app-client - ear
...
...
[INFO] Installing /Users/selvasingh/migrate-java-ee-app-to-azure-2/quickstart/wsba-participant-completion-simple/target/wsba-participant-completion-simple-sources.jar to /Users/selvasingh/.m2/repository/org/wildfly/quickstarts/wsba-participant-completion-simple/14.0.1.Final/wsba-participant-completion-simple-14.0.1.Final-sources.jar
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO] 
[INFO] Quickstart: Parent ................................. SUCCESS [  1.697 s]
[INFO] Quickstart: app-client ............................. SUCCESS [  0.015 s]
[INFO] Quickstart: app-client - ejb ....................... SUCCESS [  2.177 s]
[INFO] Quickstart: app-client - client-simple ............. SUCCESS [  0.646 s]
[INFO] Quickstart: app-client - ear ....................... SUCCESS [  0.723 s]
[INFO] Quickstart: batch-processing ....................... SUCCESS [  1.736 s]
[INFO] Quickstart: bean-validation ........................ SUCCESS [  0.482 s]
[INFO] Quickstart: bean-validation-custom-constraint ...... SUCCESS [  0.295 s]
[INFO] Quickstart: bmt .................................... SUCCESS [  0.291 s]
[INFO] Quickstart: cmt .................................... SUCCESS [  1.642 s]
[INFO] Quickstart: contacts-jquerymobile .................. SUCCESS [  1.351 s]
[INFO] Quickstart: deltaspike-authorization ............... SUCCESS [  0.265 s]
[INFO] Quickstart: deltaspike-beanbuilder ................. SUCCESS [  0.387 s]
[INFO] Quickstart: deltaspike-projectstage ................ SUCCESS [  0.150 s]
[INFO] Quickstart: ejb-asynchronous ....................... SUCCESS [  0.011 s]
[INFO] Quickstart: ejb-asynchronous - ejb ................. SUCCESS [  0.161 s]
[INFO] Quickstart: ejb-asynchronous - client .............. SUCCESS [  0.157 s]
[INFO] Quickstart: ejb-in-ear ............................. SUCCESS [  0.010 s]
[INFO] Quickstart: ejb-in-ear - ejb ....................... SUCCESS [  0.133 s]
[INFO] Quickstart: ejb-in-ear - web ....................... SUCCESS [  0.143 s]
[INFO] Quickstart: ejb-in-ear - ear ....................... SUCCESS [  0.216 s]
[INFO] Quickstart: ejb-in-war ............................. SUCCESS [  0.224 s]
[INFO] Quickstart: ejb-multi-server ....................... SUCCESS [  0.014 s]
[INFO] Quickstart: ejb-multi-server - app-one ............. SUCCESS [  0.012 s]
[INFO] Quickstart: ejb-multi-server - app-one - ejb ....... SUCCESS [  0.148 s]
[INFO] Quickstart: ejb-multi-server - app-one - ear ....... SUCCESS [  0.030 s]
[INFO] Quickstart: ejb-multi-server - app-two ............. SUCCESS [  0.014 s]
[INFO] Quickstart: ejb-multi-server - app-two - ejb ....... SUCCESS [  0.207 s]
[INFO] Quickstart: ejb-multi-server - app-two - ear ....... SUCCESS [  0.031 s]
[INFO] Quickstart: ejb-multi-server - app-main ............ SUCCESS [  0.015 s]
[INFO] Quickstart: ejb-multi-server - app-main - ejb ...... SUCCESS [  0.222 s]
[INFO] Quickstart: ejb-multi-server - app-main - web ...... SUCCESS [  0.252 s]
[INFO] Quickstart: ejb-multi-server - app-main - ear ...... SUCCESS [  0.078 s]
[INFO] Quickstart: ejb-multi-server - app-web ............. SUCCESS [  0.343 s]
[INFO] Quickstart: ejb-multi-server - client .............. SUCCESS [  0.277 s]
[INFO] Quickstart: ejb-security ........................... SUCCESS [  0.302 s]
[INFO] Quickstart: ejb-security-context-propagation ....... SUCCESS [  0.227 s]
[INFO] Quickstart: ejb-security-jaas ...................... SUCCESS [  0.368 s]
[INFO] Quickstart: ejb-security-programmatic-auth ......... SUCCESS [  0.155 s]
[INFO] Quickstart: ejb-throws-exception ................... SUCCESS [  0.009 s]
[INFO] Quickstart: ejb-throws-exception - ejb-api ......... SUCCESS [  0.115 s]
[INFO] Quickstart: ejb-throws-exception - ejb ............. SUCCESS [  0.108 s]
[INFO] Quickstart: ejb-throws-exception - web ............. SUCCESS [  0.145 s]
[INFO] Quickstart: ejb-throws-exception - ear ............. SUCCESS [  0.030 s]
[INFO] Quickstart: ejb-timer .............................. SUCCESS [  0.136 s]
[INFO] Quickstart: greeter ................................ SUCCESS [  0.187 s]
[INFO] Quickstart: HA Singleton Deployment ................ SUCCESS [  0.114 s]
[INFO] Quickstart: HA Singleton Service (parent) .......... SUCCESS [  0.038 s]
[INFO] Quickstart: HA Singleton Service - primary-only .... SUCCESS [  0.160 s]
[INFO] Quickstart: HA Singleton Service - with backups .... SUCCESS [  0.133 s]
[INFO] Quickstart: helloworld ............................. SUCCESS [  0.139 s]
[INFO] Quickstart: Hello World ClassFileTransformers ...... SUCCESS [  0.251 s]
[INFO] Quickstart: helloworld-html5 ....................... SUCCESS [  0.127 s]
[INFO] Quickstart: helloworld-jms ......................... SUCCESS [ 24.354 s]
[INFO] Quickstart: helloworld-mbean ....................... SUCCESS [  0.017 s]
[INFO] Quickstart: helloworld-mbean - helloworld-mbean-webapp SUCCESS [  0.271 s]
[INFO] Quickstart: helloworld-mbean - helloworld-mbean-service SUCCESS [  1.730 s]
[INFO] Quickstart: helloworld-mdb ......................... SUCCESS [  1.260 s]
[INFO] Quickstart: helloworld-mdb-propertysubstitution .... SUCCESS [  0.147 s]
[INFO] Quickstart: helloworld-mutual-ssl .................. SUCCESS [  0.343 s]
[INFO] Quickstart: helloworld-mutual-ssl-secured .......... SUCCESS [  0.331 s]
[INFO] Quickstart: helloworld-rf .......................... SUCCESS [  0.546 s]
[INFO] Quickstart: helloworld-rs .......................... SUCCESS [  0.106 s]
[INFO] Quickstart: helloworld-singleton ................... SUCCESS [  0.121 s]
[INFO] Quickstart: helloworld-ssl ......................... SUCCESS [  0.109 s]
[INFO] Quickstart: helloworld-ws .......................... SUCCESS [  0.827 s]
[INFO] Quickstart: hibernate4 ............................. SUCCESS [  0.481 s]
[INFO] Quickstart: hibernate .............................. SUCCESS [  0.229 s]
[INFO] Quickstart: http-custom-mechanism .................. SUCCESS [  0.012 s]
[INFO] Quickstart: http-custom-mechanism - webapp ......... SUCCESS [  0.196 s]
[INFO] Quickstart: inter-app .............................. SUCCESS [  0.009 s]
[INFO] Quickstart: inter-app - shared ..................... SUCCESS [  0.094 s]
[INFO] Quickstart: inter-app - appA ....................... SUCCESS [  0.111 s]
[INFO] Quickstart: inter-app - appB ....................... SUCCESS [  0.123 s]
[INFO] Quickstart: jaxrs-client ........................... SUCCESS [  0.286 s]
[INFO] Quickstart: jaxrs-jwt .............................. SUCCESS [  0.011 s]
[INFO] Quickstart: jaxrs-jwt - client ..................... SUCCESS [  0.135 s]
[INFO] Quickstart: jaxrs-jwt - service .................... SUCCESS [  0.513 s]
[INFO] Quickstart: jaxws-addressing ....................... SUCCESS [  0.010 s]
[INFO] Quickstart: jaxws-addressing - service ............. SUCCESS [  0.115 s]
[INFO] Quickstart: jaxws-addressing - client .............. SUCCESS [  0.693 s]
[INFO] Quickstart: jaxws-ejb .............................. SUCCESS [  0.009 s]
[INFO] Quickstart: jaxws-ejb - service .................... SUCCESS [  0.133 s]
[INFO] Quickstart: jaxws-ejb - client ..................... SUCCESS [  0.147 s]
[INFO] Quickstart: jaxws-pojo ............................. SUCCESS [  0.010 s]
[INFO] Quickstart: jaxws-pojo - service ................... SUCCESS [  0.125 s]
[INFO] Quickstart: jaxws-pojo - client .................... SUCCESS [  0.191 s]
[INFO] Quickstart: jaxws-retail ........................... SUCCESS [  0.011 s]
[INFO] Quickstart: jaxws-retail - service ................. SUCCESS [  2.587 s]
[INFO] Quickstart: jaxws-retail - client .................. SUCCESS [  0.153 s]
[INFO] Quickstart: jsonp .................................. SUCCESS [  0.136 s]
[INFO] Quickstart: kitchensink ............................ SUCCESS [  0.758 s]
[INFO] Quickstart: kitchensink-angularjs .................. SUCCESS [  0.806 s]
[INFO] Quickstart: kitchensink-ear ........................ SUCCESS [  0.009 s]
[INFO] Quickstart: kitchensink-ear - ejb .................. SUCCESS [  0.181 s]
[INFO] Quickstart: kitchensink-ear - web .................. SUCCESS [  0.166 s]
[INFO] Quickstart: kitchensink-ear - ear .................. SUCCESS [  0.032 s]
[INFO] Quickstart: kitchensink-jsp ........................ SUCCESS [  0.669 s]
[INFO] Quickstart: kitchensink-ml ......................... SUCCESS [  0.901 s]
[INFO] Quickstart: Kitchensink with Undertow.JS and AngularJS SUCCESS [  0.230 s]
[INFO] Quickstart: Kitchensink with Undertow.JS and Mustach SUCCESS [  0.031 s]
[INFO] Quickstart: logging ................................ SUCCESS [  0.092 s]
[INFO] Quickstart: logging-tools .......................... SUCCESS [  0.778 s]
[INFO] Quickstart: mail ................................... SUCCESS [  0.163 s]
[INFO] Quickstart: managed-executor-service ............... SUCCESS [  0.201 s]
[INFO] Quickstart: messaging-clustering-singleton ......... SUCCESS [  0.114 s]
[INFO] Quickstart: numberguess ............................ SUCCESS [  0.131 s]
[INFO] Quickstart: payment-cdi-event ...................... SUCCESS [  0.162 s]
[INFO] Quickstart: resteasy-jaxrs-client .................. SUCCESS [  0.094 s]
[INFO] Quickstart: security-domain-to-domain .............. SUCCESS [  0.007 s]
[INFO] Quickstart: security-domain-to-domain - ejb ........ SUCCESS [  0.088 s]
[INFO] Quickstart: security-domain-to-domain - web ........ SUCCESS [  0.122 s]
[INFO] Quickstart: security-domain-to-domain - ear ........ SUCCESS [  0.025 s]
[INFO] Quickstart: servlet-async .......................... SUCCESS [  0.133 s]
[INFO] Quickstart: servlet-filterlistener ................. SUCCESS [  0.125 s]
[INFO] Quickstart: servlet-security ....................... SUCCESS [  0.125 s]
[INFO] Quickstart: shopping-cart .......................... SUCCESS [  0.008 s]
[INFO] Quickstart: shopping-cart - server ................. SUCCESS [  0.095 s]
[INFO] Quickstart: shopping-cart - client ................. SUCCESS [  0.090 s]
[INFO] Quickstart: spring-greeter ......................... SUCCESS [  0.313 s]
[INFO] Quickstart: spring-kitchensink-basic ............... SUCCESS [  3.836 s]
[INFO] Quickstart: spring-kitchensink-springmvctest ....... SUCCESS [  4.767 s]
[INFO] Quickstart: spring-resteasy ........................ SUCCESS [  0.316 s]
[INFO] Quickstart: tasks-jsf .............................. SUCCESS [  0.826 s]
[INFO] Quickstart: tasks-rs ............................... SUCCESS [  0.194 s]
[INFO] Quickstart: temperature-converter .................. SUCCESS [  0.116 s]
[INFO] Quickstart: thread-racing .......................... SUCCESS [  0.305 s]
[INFO] Quickstart: websocket-client ....................... SUCCESS [  0.242 s]
[INFO] Quickstart: websocket-endpoint ..................... SUCCESS [  0.215 s]
[INFO] Quickstart: websocket-hello ........................ SUCCESS [  0.110 s]
[INFO] Quickstart: wicket-ear ............................. SUCCESS [  0.009 s]
[INFO] Quickstart: wicket-ear - ejb ....................... SUCCESS [  0.093 s]
[INFO] Quickstart: wicket-ear - war ....................... SUCCESS [  0.303 s]
[INFO] Quickstart: wicket-ear - ear ....................... SUCCESS [  0.149 s]
[INFO] Quickstart: wicket-war ............................. SUCCESS [  0.260 s]
[INFO] Quickstart: xml-jaxp ............................... SUCCESS [  0.175 s]
[INFO] Quickstart: jts .................................... SUCCESS [  0.008 s]
[INFO] Quickstart: jts - application-component-2 .......... SUCCESS [  0.146 s]
[INFO] Quickstart: jts - application-component-1 .......... SUCCESS [  0.114 s]
[INFO] Quickstart: ejb-remote ............................. SUCCESS [  0.011 s]
[INFO] Quickstart: ejb-remote - server-side ............... SUCCESS [  0.115 s]
[INFO] Quickstart: ejb-remote - client .................... SUCCESS [  0.109 s]
[INFO] Quickstart: jta-crash-rec .......................... SUCCESS [  0.163 s]
[INFO] Quickstart: wsat-simple ............................ SUCCESS [  0.240 s]
[INFO] Quickstart: wsba-coordinator-completion-simple ..... SUCCESS [  0.232 s]
[INFO] Quickstart: wsba-participant-completion-simple ..... SUCCESS [  0.225 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 01:16 min
[INFO] Finished at: 2019-02-09T11:01:57-08:00
[INFO] Final Memory: 203M/660M
[INFO] ------------------------------------------------------------------------

```

## <a name="build-and-run-console-app-to-send-and-receive-messages"></a>生成并运行控制台应用以发送和接收消息

### <a name="create-and-configure-azure-service-bus"></a>创建并配置 Azure 服务总线

1. 使用 CLI 登录到 Azure。

    ```bash
    az login
    ```
2. 设置环境变量以在运行时绑定机密，具体而言，需要设置 Azure 资源组名称和 Azure 服务总线信息。 可将这些信息导出到本地环境（例如，使用提供的 Bash shell 脚本模板）。

    ```bash
    cd helloworld-jms
    mkdir .scripts
    cp set-env-variables-template.sh .scripts/set-env-variables.sh
    ```
3. 修改 `.scripts/set-env-variables.sh` 并设置 Azure 资源组名称和 Azure 服务总线信息。 然后设置环境变量：
 
    ```bash
    source .scripts/set-env-variables.sh
    ```
4. 创建 Azure 服务总线：

    ```bash
    az group create --name ${RESOURCEGROUP_NAME} \
        --location ${REGION}
    
    az servicebus namespace create \
        --name  ${DEFAULT_SBNAMESPACE} \
        --resource-group ${RESOURCEGROUP_NAME}
    
    az servicebus queue create \
        --name ${SB_QUEUE} \
        --namespace-name ${DEFAULT_SBNAMESPACE} \
        --resource-group ${RESOURCEGROUP_NAME}
    
    az servicebus queue authorization-rule create \
        --name ${SB_SAS_POLICY} \
        --namespace-name ${DEFAULT_SBNAMESPACE} \
        --queue-name ${SB_QUEUE} \
        --resource-group ${RESOURCEGROUP_NAME} \
        --rights Listen Send
    
    az servicebus queue authorization-rule keys list \
        --name ${SB_SAS_POLICY} \
        --namespace-name ${DEFAULT_SBNAMESPACE} \
        --queue-name ${SB_QUEUE} \
        --resource-group ${RESOURCEGROUP_NAME}
                                                
    ```

    在显示的密钥值中，获取 <b>主密钥</b> 值。 打开 .scripts/set-env-variables.sh 文件，并将 primaryKey 设置为 SB_SAS_KEY 变量的值。
5. 导出环境变量：

    ```bash
    source .scripts/set-env-variables.sh
    ```

### <a name="build-and-run-the-console-app"></a>生成并运行控制台应用

使用 Maven 生成控制台应用，然后运行该应用。

```bash
mvn clean compile exec:java -Dexec.cleanupDaemonThreads=false

[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building Quickstart: helloworld-jms 14.0.1.Final
[INFO] ------------------------------------------------------------------------
...
...
[INFO] --- exec-maven-plugin:1.6.0:java (default-cli) @ helloworld-jms ---
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See https://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
Feb 10, 2019 9:28:31 AM org.jboss.as.quickstarts.jms.HelloWorldJMSClient main
INFO: Attempting to acquire connection factory "SBCF"
Feb 10, 2019 9:28:31 AM org.jboss.as.quickstarts.jms.HelloWorldJMSClient main
INFO: Found connection factory "SBCF" in JNDI
Feb 10, 2019 9:28:31 AM org.jboss.as.quickstarts.jms.HelloWorldJMSClient main
INFO: Attempting to acquire destination "QUEUE"
Feb 10, 2019 9:28:31 AM org.jboss.as.quickstarts.jms.HelloWorldJMSClient main
INFO: Found destination "QUEUE" in JNDI
Feb 10, 2019 9:28:37 AM org.jboss.as.quickstarts.jms.HelloWorldJMSClient main
INFO: Sending 1 messages with content: Hello, World!
Feb 10, 2019 9:28:37 AM org.jboss.as.quickstarts.jms.HelloWorldJMSClient main
INFO: Received message with content Hello, World!
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 8.763 s
[INFO] Finished at: 2019-02-10T09:28:38-08:00
[INFO] Final Memory: 33M/401M
[INFO] ------------------------------------------------------------------------
```
## <a name="migrate-a-message-driven-enterprise-bean-to-azure"></a>将消息驱动的企业 bean 迁移到 Azure

### <a name="prepare-environment"></a>准备环境

1. 将目录更改为 MDB：

    ```bash
    cd ../helloworld-mdb
    ```
2. 设置环境变量以在运行时绑定机密，具体而言，需要设置 Azure 资源组名称和 Azure 服务总线信息。 可将这些信息导出到本地环境（例如，使用提供的 Bash shell 脚本模板）。

    ```bash
    cp set-env-variables-template.sh .scripts/set-env-variables.sh
    ```
3. 修改 `.scripts/set-env-variables.sh` 并设置 Azure 资源组名称、Azure 服务总线信息和应用服务 Linux 信息。 可从上一篇练习 `../helloworld-jms/.scripts/set-env-variables.sh` 的脚本中复制上述大部分值，并添加应用服务 Linux 信息。 
4. 现在请设置环境变量：
 
    ```bash
    source .scripts/set-env-variables.sh
    ```

### <a name="deploy-an-app-to-azure-app-service-on-linux"></a>将应用部署到 Linux 上的 Azure 应用服务

1. 将 [Azure 应用服务的 Maven 插件](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)配置添加到 POM.xml，并将消息驱动的 bean 部署到 Linux 上的应用服务中的 WildFly：

    ```xml    
    <plugins> 
    
        <!--*************************************************-->
        <!-- Deploy to WildFly in App Service on Linux          -->
        <!--*************************************************-->
           
        <plugin>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.5.3</version>
            <configuration>
        
                <!-- Web App information -->
               <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
               <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
                <appName>${WEBAPP_NAME}</appName>
                <region>${REGION}</region>
        
                <!-- Java Runtime Stack for Web App on Linux-->
                <linuxRuntime>wildfly 14-jre8</linuxRuntime>
                
                <appSettings>
                    <property>
                        <name>DEFAULT_SBNAMESPACE</name>
                        <value>${DEFAULT_SBNAMESPACE}</value>
                    </property>
                    <property>
                        <name>SB_SAS_POLICY</name>
                        <value>${SB_SAS_POLICY}</value>
                    </property>
                    <property>
                        <name>SB_SAS_KEY</name>
                        <value>${SB_SAS_KEY}</value>
                    </property>
                    <property>
                        <name>PROVIDER_URL</name>
                        <value>${PROVIDER_URL}</value>
                    </property>
                    <property>
                        <name>SB_QUEUE</name>
                        <value>${SB_QUEUE}</value>
                    </property>
                </appSettings>
            </configuration>
        </plugin>
        ...
    </plugins>
    ```
2. 生成消息驱动的 bean：

    ```bash
    mvn package
    [INFO] Scanning for projects...
    [INFO] 
    [INFO] ------------------------------------------------------------------------
    [INFO] Building Quickstart: helloworld-mdb 14.0.1.Final
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    ...
    ...
    [INFO] --- maven-war-plugin:3.2.2:war (default-war) @ helloworld-mdb ---
    [INFO] Packaging webapp
    [INFO] Assembling webapp [helloworld-mdb] in [/Users/selvasingh/migrate-java-ee-app-to-azure-2/quickstart/helloworld-mdb/target/helloworld-mdb]
    [INFO] Processing war project
    [INFO] Copying webapp resources [/Users/selvasingh/migrate-java-ee-app-to-azure-2/quickstart/helloworld-mdb/src/main/webapp]
    [INFO] Webapp assembled in [84 msecs]
    [INFO] Building war: /Users/selvasingh/migrate-java-ee-app-to-azure-2/quickstart/helloworld-mdb/target/helloworld-mdb.war
    [INFO] 
    [INFO] --- maven-source-plugin:3.0.1:jar-no-fork (attach-sources) @ helloworld-mdb ---
    [INFO] Building jar: /Users/selvasingh/migrate-java-ee-app-to-azure-2/quickstart/helloworld-mdb/target/helloworld-mdb-sources.jar
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 3.908 s
    [INFO] Finished at: 2019-02-10T11:37:03-08:00
    [INFO] Final Memory: 34M/413M
    [INFO] ------------------------------------------------------------------------
    ```
3. 将消息驱动的 bean 部署到 Linux 上的应用服务
    
    ```bash
    mvn azure-webapp:deploy

    [INFO] 
    [INFO] ------------------------------------------------------------------------
    [INFO] Building Quickstart: helloworld-mdb 14.0.1.Final
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- azure-webapp-maven-plugin:1.5.3:deploy (default-cli) @ helloworld-mdb ---
    ...
    ...
    [INFO] Authenticate with Azure CLI 2.0
    [INFO] Target Web App doesn't exist. Creating a new one...
    [INFO] Creating App Service Plan 'helloworld-mdb-appservice-plan'...
    [INFO] Successfully created App Service Plan.
    [INFO] Successfully created Web App.
    [INFO] Trying to deploy artifact to helloworld-mdb...
    [INFO] Deploying the war file...
    [INFO] Successfully deployed the artifact to https://helloworld-mdb.azurewebsites.net
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 02:15 min
    [INFO] Finished at: 2019-02-10T11:41:06-08:00
    [INFO] Final Memory: 55M/362M
    [INFO] ------------------------------------------------------------------------

    ```

### <a name="configure-the-jms-resource-adapter-jms-ra"></a>配置 JMS 资源适配器 (JMS RA)
可以通过几个步骤来配置 JMS RA，使 Java EJB 能够配置远程 JMS 连接工厂和队列。 此远程设置将使用 AMQP 协议的 [Apache Qpid JMS 提供程序](https://qpid.apache.org/components/jms/index.html)指向 Azure 服务总线。 

#### <a name="understand-how-to-configure-wildfly"></a>了解如何配置 WildFly

在应用服务中，应用服务器的每个实例都是无状态的。 因此，在启动时必须配置每个实例才能支持应用程序所需的 Wildfly 配置。 启动时，可以通过提供一个启动 Bash 脚本进行配置。该脚本将调用 [JBoss/WildFly CLI 命令](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface)来设置数据源、消息传送提供程序以及任何其他依赖项。 需要创建一个 startup.sh 脚本，并将其放在 Web 应用的 `/home` 目录中。 该脚本将会：
 
1. 安装 WildFly 通用 JMS 提供程序模块并配置 JMS RA。 `module.xml` 描述通用 JMS 提供程序模块：

    ```xml
    <module xmlns="urn:jboss:module:1.1" name="org.jboss.genericjms.provider"> 
      <resources> 
          <resource-root path="proton-j-<version>.jar"/> 
          <resource-root path="qpid-jms-client-<version>jar"/>
          <resource-root path="slf4j-log4j12-<version>jar"/>
          <resource-root path="slf4j-api-<version>jar"/>
          <resource-root path="log4j-<version>jar"/>      
          <resource-root path="netty-buffer-<version>.jar" />
          <resource-root path="netty-codec-<version>.jar" />
          <resource-root path="netty-codec-http-<version>.jar" />
          <resource-root path="netty-common-<version>.jar" />
          <resource-root path="netty-handler-<version>.jar" />
          <resource-root path="netty-resolver-<version>.jar" />
          <resource-root path="netty-transport-<version>.jar" />
          <resource-root path="netty-transport-native-epoll-<version>-linux-x86_64.jar" />
          <resource-root path="netty-transport-native-kqueue-<version>-osx-x86_64.jar" />
          <resource-root path="netty-transport-native-unix-common-<version>.jar" /> 
          <resource-root path="qpid-jms-discovery-<version>jar" />
      </resources> 
    
       <dependencies> 
          <module name="javax.api"/> 
          <module name="javax.jms.api"/> 
      </dependencies> 
    </module>
    ```
2. 生成 `jndi.properties` 文件：

    ```bash
    echo "Generating jndi.properties file in /home/site/deployments/tools directory"
    echo "connectionfactory.SBF=amqps://${DEFAULT_SBNAMESPACE}.servicebus.windows.net?amqp.idleTimeout=120000&jms.username=${SB_SAS_POLICY}&jms.password=${SB_SAS_KEY}" > /home/site/deployments/tools/jndi.properties
    echo "queue.jmstestqueue=${SB_QUEUE}" >> /home/site/deployments/tools/jndi.properties
    echo "====== contents of /home/site/deployments/tools/jndi.properties ======"
    cat /home/site/deployments/tools/jndi.properties
    echo "====== EOF /home/site/deployments/tools/jndi.properties ======"
    ```
3. 将所有二进制 JAR、模块文件和生成的 jndi.properties 复制到 WildFly 配置位置：

    ```bash
    mkdir /opt/jboss/wildfly/modules/system/layers/base/org/jboss/genericjms/provider
    mkdir /opt/jboss/wildfly/modules/system/layers/base/org/jboss/genericjms/provider/main
    cp  /home/site/deployments/tools/*.jar /opt/jboss/wildfly/modules/system/layers/base/org/jboss/genericjms/provider/main/
    cp /home/site/deployments/tools/module.xml /opt/jboss/wildfly/modules/system/layers/base/org/jboss/genericjms/provider/main/
    cp /home/site/deployments/tools/jndi.properties /opt/jboss/wildfly/standalone/configuration/
    ```
4. 添加通用 JMS 提供程序模块：

    ```bash
    /subsystem=ee:list-add(name=global-modules, value={"name" => "org.jboss.genericjms.provider", "slot" =>"main"}
    /subsystem=naming/binding="java:global/remoteJMS":add(binding-type=external-context,module=org.jboss.genericjms.provider,class=javax.naming.InitialContext,environment=[java.naming.factory.initial=org.apache.qpid.jms.jndi.JmsInitialContextFactory,org.jboss.as.naming.lookup.by.string=true,java.naming.provider.url=/home/site/deployments/tools/jndi.properties])
    /subsystem=resource-adapters/resource-adapter=generic-ra:add(module=org.jboss.genericjms,transaction-support=XATransaction)
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd:add(class-name=org.jboss.resource.adapter.jms.JmsManagedConnectionFactory, jndi-name=java:/jms/SBF)
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd/config-properties=ConnectionFactory:add(value=SBF)
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd/config-properties=JndiParameters:add(value="java.naming.factory.initial=org.apache.qpid.jms.jndi.JmsInitialContextFactory;java.naming.provider.url=/home/site/deployments/tools/jndi.properties")
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd:write-attribute(name=security-application,value=true)
    /subsystem=ejb3:write-attribute(name=default-resource-adapter-name, value=generic-ra)
    ```
5. 要使更改生效，可能需要重新加载服务器：

    ```bash
    reload --use-current-server-config=true
    ```

[quickstart/helloworld-mdb/.scripts](https://github.com/Azure-Samples/migrate-Java-EE-app-to-azure-2/tree/master/quickstart/helloworldmdb/.scripts) 中提供了这些 JBoss CLI 命令、WildFly/JBoss 通用 JMS 提供程序模块说明 (`module.xml`) 和 JAR 

此外，可以直接从 AMQP 协议的 [Apache Qpid JMS 提供程序](https://qpid.apache.org/components/jms/index.html)下载 `Qpid` 和 `Proton-j` 库。

#### <a name="upload-startup-and-binary-artifacts-to-app-through-ftp"></a>通过 FTP 将启动和二进制项目上传到应用

##### <a name="get-ftp-deployment-credentials"></a>获取 FTP 部署凭据
使用 Azure CLI 获取 FTP 部署凭据：

```bash
az webapp deployment list-publishing-profiles -g ${RESOURCEGROUP_NAME} -n ${WEBAPP_NAME}
...

Here is the sample output: 
...
{
   ...
   ...
    "profileName": "helloworld-mdb - FTP",
    "publishMethod": "FTP",
    "publishUrl": "ftp://<FTP host name>.ftp.azurewebsites.windows.net/site/wwwroot",
    "userName": "helloworld-mdb\\$helloworld-mdb",
    "userPWD": ================ MASKED ======================,
    "webSystem": "WebSites"
}
   
```

将 FTP 主机名（例如 `<FTP host name>.ftp.azurewebsites.windows.net`）、用户名和用户密码存储在 `.scripts/set-env-variables.sh` 文件中。

##### <a name="upload-artifacts-to-app-through-ftp"></a>通过 FTP 将项目上传到应用

与 Linux 上的应用服务建立 FTP 连接以上传项目：

1. 切换到 `.scripts` 文件夹。 

    ```bash
    cd .scripts
    ```
2. 建立 FTP 连接
    
    ```bash
    ftp
    ftp> open waws-prod-mwh-007.ftp.azurewebsites.windows.net
    Trying 52.183.36.81...
    Connected to waws-prod-mwh-007.drip.azurewebsites.windows.net.
    220 Microsoft FTP Service
    Name (waws-prod-mwh-007.ftp.azurewebsites.windows.net:selvasingh): helloworld-mdb\\$helloworld-mdb
    331 Password required
    Password: 
    230 User logged in.
    Remote system type is Windows_NT.
    ftp> ascii
    200 Type set to A.
    ftp> passive
    Passive mode: off; fallback to active mode: off.
    ```
2. 上传 startup.sh。 

    ```bash
    ftp> put startup.sh
    local: startup.sh remote: startup.sh
    200 EPRT command successful.
    125 Data connection already open; Transfer starting.
    100% |*********************************************|  1291      211.78 KiB/s    --:-- ETA
    226 Transfer complete.
    1291 bytes sent in 00:00 (42.12 KiB/s)
    ```
3. 切换到 tools 目录。

    ```bash
    ftp> cd site/deployments/tools
    250 CWD command successful.
    ```
4. 上传 commands.cli 和 module.xml。

    ```bash
    ftp> put commands.cli
    local: commands.cli remote: commands.cli
    200 EPRT command successful.
    125 Data connection already open; Transfer starting.
    100% |*********************************************|  1477      242.49 KiB/s    --:-- ETA
    226 Transfer complete.
    1477 bytes sent in 00:00 (48.30 KiB/s)
    ftp> put module.xml
    local: module.xml remote: module.xml
    200 EPRT command successful.
    125 Data connection already open; Transfer starting.
    100% |*********************************************|  1280      206.06 KiB/s    --:-- ETA
    226 Transfer complete.
    1280 bytes sent in 00:00 (33.16 KiB/s)
    ```
5. 上传 JAR。

    ```bash
    ftp> binary
    200 Type set to I.
    ftp> mput *.jar
    mput log4j-1.2.17.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10103|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   478 KiB    1.73 MiB/s    00:00 ETA
    226 Transfer complete.
    489884 bytes sent in 00:00 (1.40 MiB/s)
    mput netty-buffer-4.1.32.Final.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10105|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   271 KiB  790.62 KiB/s    00:00 ETA
    226 Transfer complete.
    277778 bytes sent in 00:00 (612.65 KiB/s)
    mput netty-codec-4.1.32.Final.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10101|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   309 KiB    1.42 MiB/s    00:00 ETA
    226 Transfer complete.
    316671 bytes sent in 00:00 (1.20 MiB/s)
    mput netty-codec-http-4.1.32.Final.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10106|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   550 KiB    1.88 MiB/s    00:00 ETA
    226 Transfer complete.
    563215 bytes sent in 00:00 (1.55 MiB/s)
    mput netty-common-4.1.32.Final.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10104|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   573 KiB    2.03 MiB/s    00:00 ETA
    226 Transfer complete.
    586829 bytes sent in 00:00 (1.65 MiB/s)
    mput netty-handler-4.1.32.Final.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10108|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   410 KiB    1.72 MiB/s    00:00 ETA
    226 Transfer complete.
    420485 bytes sent in 00:00 (1.36 MiB/s)
    mput netty-resolver-4.1.32.Final.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10107|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************| 32800        5.49 MiB/s    00:00 ETA
    226 Transfer complete.
    32800 bytes sent in 00:00 (325.36 KiB/s)
    mput netty-transport-4.1.32.Final.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10109|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   452 KiB    1.69 MiB/s    00:00 ETA
    226 Transfer complete.
    463581 bytes sent in 00:00 (1.40 MiB/s)
    mput netty-transport-native-epoll-4.1.32.Final-linux-x86_64.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10110|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   137 KiB    1.85 MiB/s    00:00 ETA
    226 Transfer complete.
    141017 bytes sent in 00:00 (521.44 KiB/s)
    mput netty-transport-native-kqueue-4.1.32.Final-osx-x86_64.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10111|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   107 KiB   18.22 MiB/s    00:00 ETA
    226 Transfer complete.
    109800 bytes sent in 00:00 (143.52 KiB/s)
    mput netty-transport-native-unix-common-4.1.32.Final.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10112|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************| 33470        5.47 MiB/s    00:00 ETA
    226 Transfer complete.
    33470 bytes sent in 00:00 (366.26 KiB/s)
    mput proton-j-0.31.0.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10114|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   719 KiB    1.93 MiB/s    00:00 ETA
    226 Transfer complete.
    736444 bytes sent in 00:00 (1.67 MiB/s)
    mput qpid-jms-client-0.40.0.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10113|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************|   729 KiB    1.89 MiB/s    00:00 ETA
    226 Transfer complete.
    747044 bytes sent in 00:00 (1.63 MiB/s)
    mput qpid-jms-discovery-0.40.0.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10115|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************| 40531       21.01 MiB/s    00:00 ETA
    226 Transfer complete.
    40531 bytes sent in 00:00 (360.08 KiB/s)
    mput slf4j-api-1.7.25.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10116|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************| 41203       59.08 MiB/s    00:00 ETA
    226 Transfer complete.
    41203 bytes sent in 00:00 (338.90 KiB/s)
    mput slf4j-log4j12-1.7.25.jar [anpqy?]? y
    229 Entering Extended Passive Mode (|||10118|)
    125 Data connection already open; Transfer starting.
    100% |*********************************************| 12244        2.10 MiB/s    00:00 ETA
    226 Transfer complete.
    12244 bytes sent in 00:00 (169.29 KiB/s)
    ```

#### <a name="test-the-jbosswildfly-startup-script-and-cli-commands-to-configure-jms-ra"></a>测试 JBoss/WildFly 启动脚本和 CLI 命令以配置 JMS RA

可以测试用于配置数据源的 Bash 脚本，方法是从开发计算机建立 SSH 连接，然后在应用服务 Linux 上运行这些脚本：

1. 在第一个终端窗口中，运行以下命令建立远程连接：

    ```bash
    az webapp remote-connection create --resource-group ${RESOURCEGROUP_NAME} --name ${WEBAPP_NAME} &
    [1] 63235
    bash-3.2$ Auto-selecting port: 65428
    SSH is available { username: root, password: Docker! }
    Start your favorite client and connect to port 65428
    Websocket tracing disabled, use --verbose flag to enable
    Successfully connected to local server..
    ```
2. 在第二个终端窗口中运行以下命令： 

    ```bash
    ssh root@localhost -p 65428
    The authenticity of host '[localhost]:65428 ([127.0.0.1]:65428)' can't be established.
    ECDSA key fingerprint is SHA256:u/VkSFAFjoO9EkBT4zl1pNoWAzWAUdUeRjaHnsXNXlM.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '[localhost]:65428' (ECDSA) to the list of known hosts.
    root@localhost's password: 
      _____                               
      /  _  \ __________ _________   ____  
     /  /_\  \___   /  |  \_  __ \_/ __ \ 
    /    |    \/    /|  |  /|  | \/\  ___/ 
    \____|__  /_____ \____/ |__|    \___  >
            \/      \/                  \/ 
    A P P   S E R V I C E   O N   L I N U X
    
    Documentation: https://aka.ms/webapp-linux
    
    54cfe2dfa970:/home# ls -al
    total 12
    drwxrwxrwx    2 nobody   nobody        4096 Feb 11 17:54 .
    drwxr-xr-x   49 root     root          4096 Feb 10 19:40 ..
    drwxrwxrwx    2 nobody   nobody           0 Feb 10 19:40 .mono
    drwxrwxrwx    2 nobody   nobody           0 Feb 10 19:41 LogFiles
    drwxrwxrwx    2 nobody   nobody           0 Feb 10 19:40 d43fc68fefbe78c2a087a46f
    drwxrwxrwx    2 nobody   nobody           0 Feb 10 19:41 site
    -rwxrwxrwx    1 nobody   nobody        1291 Feb 11 17:46 startup.sh
    54cfe2dfa970:/home# 
    ```
3. 打开 VI 窗口以编辑 startup.sh

    ```bash
    c315a18b39d2:/home# vi startup.sh    
    ```
    
    下面是 vi 窗口中的示例输出： 

    ```bash
    echo "Generating jndi.properties file in /home/site/deployments/tools directory"^M
    echo "connectionfactory.SBF=amqps://${DEFAULT_SBNAMESPACE}.servicebus.windows.net?amqp.idleTimeout=120000&jms.username=${SB_SAS_POLICY}&jms.password=${SB_SAS_KEY}" > /home/site/deployments/tools/jndi.properties^M
    echo "queue.jmstestqueue=${SB_QUEUE}" >> /home/site/deployments/tools/jndi.properties^M
    echo "====== contents of /home/site/deployments/tools/jndi.properties ======"^M
    cat /home/site/deployments/tools/jndi.properties^M
    echo "====== EOF /home/site/deployments/tools/jndi.properties ======"^M
    mkdir /opt/jboss/wildfly/modules/system/layers/base/org/jboss/genericjms/provider^M
    mkdir /opt/jboss/wildfly/modules/system/layers/base/org/jboss/genericjms/provider/main^M
    cp  /home/site/deployments/tools/*.jar /opt/jboss/wildfly/modules/system/layers/base/org/jboss/gene
    cp /home/site/deployments/tools/module.xml /opt/jboss/wildfly/modules/system/layers/base/org/jboss/
    cp /home/site/deployments/tools/jndi.properties /opt/jboss/wildfly/standalone/configuration/^M
    /opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/home/site/deployments/tools/commands.cli^M
    ```
    删除“^M”行尾字符并保存文件。 还可以通过其他方法删除行尾字符。 请参阅[此文](https://marcelog.github.io/articles/mac_newline_to_unix_eol.html)。

##### <a name="test-the-startupsh-script"></a>测试 startup.sh 脚本

在 SSH 窗口中执行 `startup.sh`：

```bash
54cfe2dfa970:/home# source startup.sh
Generating jndi.properties file in /home/site/deployments/tools directory
====== contents of /home/site/deployments/tools/jndi.properties ======
connectionfactory.SBF=amqps://jmsservice}.servicebus.windows.net?amqp.idleTimeout=120000&jms.username=ListenAndSend&jms.password=[MASKED]
queue.jmstestqueue=
====== EOF /home/site/deployments/tools/jndi.properties ======
Picked up _JAVA_OPTIONS: -Djava.net.preferIPv4Stack=true
{"outcome" => "success"}
{"outcome" => "success"}
{"outcome" => "success"}
{"outcome" => "success"}
{"outcome" => "success"}
{"outcome" => "success"}
{
    "outcome" => "success",
    "response-headers" => {
        "operation-requires-reload" => true,
        "process-state" => "reload-required"
    }
}
{
    "outcome" => "success",
    "response-headers" => {"process-state" => "reload-required"}
}
```
#### <a name="restart-the-remote-wildfly-app-server"></a>重启远程 WildFly 应用服务器
     
 使用 Azure CLI 重启远程 WildFly 应用服务器：
    
 ```bash
 az webapp stop -g ${RESOURCEGROUP_NAME} -n ${WEBAPP_NAME}
 az webapp start -g ${RESOURCEGROUP_NAME} -n ${WEBAPP_NAME}
 ```

#### <a name="stream-wildflyjboss-logs-to-a-dev-machine"></a>将 WildFly/JBoss 日志流式传输到开发计算机

1. 为应用服务 Linux 中部署的 Java Web 应用配置日志：

    ```bash
    az webapp log config --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME} \
      --web-server-logging filesystem
    ```
2. 从本地计算机打开 Java Web 应用远程日志流：

    ```bash
    az webapp log tail --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME}
    ```

### <a name="open-the-message-driven-enterprise-bean-on-azure"></a>在 Azure 上打开消息驱动的企业 bean

1. 在应用服务 Linux 上打开 Web 应用：

    ```bash
    https://helloworld-mdb.azurewebsites.net
    ```

    ![Hello World MDB 插图](./media/migrate-java-apps-wild-fly/helloworld-mdb.png)
2. 在应用服务 Linux 发出的日志流中，可以看到：

    ```bash
    2019-02-13T03:05:53,821 INFO  [org.apache.qpid.jms.sasl.SaslMechanismFinder] (AmqpProvider :(2):[amqps://jmsservice.servicebus.windows.net:-1]) Best match for SASL auth was: SASL-PLAIN
    2019-02-13T03:05:53,828 INFO  [org.apache.qpid.jms.JmsConnection] (AmqpProvider :(2):[amqps://jmsservice.servicebus.windows.net:-1]) Connection ID:48eae295-9d89-4aa6-85e8-f26a9b43147e:1 connected to remote Broker: amqps://jmsservice.servicebus.windows.net
    2019-02-13T03:05:53.822173661Z 03:05:53,821 INFO  [org.apache.qpid.jms.sasl.SaslMechanismFinder] (AmqpProvider :(2):[amqps://jmsservice.servicebus.windows.net:-1]) Best match for SASL auth was: SASL-PLAIN
    2019-02-13T03:05:53.830453730Z 03:05:53,828 INFO  [org.apache.qpid.jms.JmsConnection] (AmqpProvider :(2):[amqps://jmsservice.servicebus.windows.net:-1]) Connection ID:48eae295-9d89-4aa6-85e8-f26a9b43147e:1 connected to remote Broker: amqps://jmsservice.servicebus.windows.net
    ...
    ...
    2019-02-13T03:05:53.931890422Z 03:05:53,930 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldQueueMDB] (default-threads - 1) Received Message from queue: This is message 1
    2019-02-13T03:05:53.958784514Z 03:05:53,957 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldQueueMDB] (default-threads - 2) Received Message from queue: This is message 2
    2019-02-13T03:05:53.977067441Z 03:05:53,976 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldQueueMDB] (default-threads - 3) Received Message from queue: This is message 3
    2019-02-13T03:05:53.995098869Z 03:05:53,994 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldQueueMDB] (default-threads - 17) Received Message from queue: This is message 4
    2019-02-13T03:05:54.014198793Z 03:05:54,013 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldQueueMDB] (default-threads - 18) Received Message from queue: This is message 5
    ```
3. 可以通过与应用服务 Linux 上的应用建立 SSH 连接并执行以下命令，来验证 JMS 配置：

    ```bash
    0cb1ce311a79:/home# /opt/jboss/wildfly/bin/jboss-cli.sh -c
    Picked up _JAVA_OPTIONS: -Djava.net.preferIPv4Stack=true
    [standalone@localhost:9990 /] :read-config-as-xml > config.xml
    [standalone@localhost:9990 /] quit
    ```

4. 检查是否存在 SBF 配置

    ```bash
    0cb1ce311a79:/home# cat config.xml | grep "SBF"
                        <connection-definition class-name=\"org.jboss.resource.adapter.jms.JmsManagedConnectionFactory\" jndi-name=\"java:/jms/SBF\" pool-name=\"sbf-cd\">
    ```
### <a name="additional-information"></a>其他信息

有关更多信息，请参阅： 
 
 - [在 JBoss/WildFly 中部署通用 JMS RA 适配器](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.1/html/configuring_messaging/resource_adapters#deploy_configure_generic_jms_resource_adapter)
 - [JBoss/WildFly CLI 指南](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface)

## <a name="migrate-java-ee-app-that-uses-websockets"></a>迁移使用 WebSocket 的 Java EE 应用

1. 将目录更改为 WebSocket 应用目录：

    ```bash
    cd ../websocket-hello
    ```
2. 设置环境变量以在运行时绑定机密，具体而言，需要设置 Azure 资源组名称和应用服务 Linux 信息。 可将这些信息导出到本地环境（例如，使用提供的 Bash shell 脚本模板）。

    ```bash
    mkdir .scripts
    cp set-env-variables-template.sh .scripts/set-env-variables.sh
    ```
3. 修改 `.scripts/set-env-variables.sh` 并设置 Azure 资源组名称和应用服务 Linux 信息。 然后设置环境变量：
 
    ```bash
    source .scripts/set-env-variables.sh
    ```

### <a name="deploy-an-app-to-app-service-linux"></a>将应用部署到应用服务 Linux

1. 将 [Azure 应用服务的 Maven 插件](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)配置添加到 POM.xml，并将消息驱动的 Bean 部署到应用服务 Linux 中的 WildFly：

    ```xml    
    <plugins> 
    
        <!--*************************************************-->
        <!-- Deploy to WildFly in App Service Linux          -->
        <!--*************************************************-->
           
        <plugin>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.5.3</version>
            <configuration>
        
                <!-- Web App information -->
               <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
               <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
                <appName>${WEBAPP_NAME}</appName>
                <region>${REGION}</region>
        
                <!-- Java Runtime Stack for Web App on Linux-->
                <linuxRuntime>wildfly 14-jre8</linuxRuntime>
                
            </configuration>
        </plugin>
        ...
    </plugins>
    ```
2. 生成消息驱动的 Bean 并将其部署到应用服务 Linux：

    ```bash
    mvn package
    [INFO] Scanning for projects...
    [INFO] 
    [INFO] ------------------------------------------------------------------------
    [INFO] Building Quickstart: websocket-hello 14.0.1.Final
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    ...
    ...
    [INFO] --- maven-war-plugin:3.2.2:war (default-war) @ websocket-hello ---
    [INFO] Packaging webapp
    [INFO] Assembling webapp [websocket-hello] in [/Users/selvasingh/migrate-java-ee-app-to-azure-2/quickstart/websocket-hello/target/websocket-hello]
    [INFO] Processing war project
    [INFO] Copying webapp resources [/Users/selvasingh/migrate-java-ee-app-to-azure-2/quickstart/websocket-hello/src/main/webapp]
    [INFO] Webapp assembled in [54 msecs]
    [INFO] Building war: /Users/selvasingh/migrate-java-ee-app-to-azure-2/quickstart/websocket-hello/target/websocket-hello.war
    [INFO] 
    [INFO] --- maven-source-plugin:3.0.1:jar-no-fork (attach-sources) @ websocket-hello ---
    [INFO] Building jar: /Users/selvasingh/migrate-java-ee-app-to-azure-2/quickstart/websocket-hello/target/websocket-hello-sources.jar
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 3.444 s
    [INFO] Finished at: 2019-02-11T22:58:35-08:00
    [INFO] Final Memory: 27M/318M
    [INFO] ------------------------------------------------------------------------
    ```
3. 将消息驱动的 bean 部署到 Linux 上的应用服务： 

    ```bash
    mvn azure-webapp:deploy
    ```

    下面是示例输出： 

    ```bash
    [INFO] Scanning for projects...
    [INFO] 
    [INFO] ------------------------------------------------------------------------
    [INFO] Building Quickstart: websocket-hello 14.0.1.Final
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- azure-webapp-maven-plugin:1.5.3:deploy (default-cli) @ websocket-hello ---
    [INFO] Authenticate with Azure CLI 2.0
    [INFO] Target Web App doesn't exist. Creating a new one...
    [INFO] Creating App Service Plan 'websocket-hello-app-appservice-plan'...
    [INFO] Successfully created App Service Plan.
    [INFO] Successfully created Web App.
    [INFO] Trying to deploy artifact to websocket-hello-app...
    [INFO] Deploying the war file...
    [INFO] Successfully deployed the artifact to https://websocket-hello-app.azurewebsites.net
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 01:41 min
    [INFO] Finished at: 2019-02-11T23:03:56-08:00
    [INFO] Final Memory: 57M/366M
    [INFO] ------------------------------------------------------------------------
    
    ```

### <a name="open-the-migrated-app-on-app-service-linux"></a>在应用服务 Linux 上打开已迁移的应用

```bash
open https://websocket-hello-app.azurewebsites.net
```

![WebSocket Hello 插图 ](./media/migrate-java-apps-wild-fly/websocket-hello.png)


祝贺！ 你已将现有的 Java 企业工作负荷迁移到 Azure：应用已迁移到应用服务 Linux，应用的消息传送系统已迁移到 Azure 服务总线。

## <a name="next-steps"></a>后续步骤
请参阅以下文章： 

- [Azure 应用服务的 Maven 插件](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme?view=azure-java-stable)
- [在 JBoss/WildFly 中部署通用 JMS RA 适配器](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.1/html/configuring_messaging/resource_adapters#deploy_configure_generic_jms_resource_adapter)
- [WildFly/JBoss 消息传送配置](https://docs.jboss.org/author/display/WFLY/Messaging+configuration)
- [JBoss/WildFly CLI 指南](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface)
- [面向 Java 开发人员的 Azure](https://docs.azure.cn/zh-cn/java/?view=azure-java-stable)
