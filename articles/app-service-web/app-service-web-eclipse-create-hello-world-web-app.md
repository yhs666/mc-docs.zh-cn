---
title: "使用 Eclipse 创建基本的 Azure Web 应用 | Azure"
description: "本教程说明如何使用 Azure Toolkit for Eclipse 创建适用于 Azure 的 Hello World Web 应用。"
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
origin.date: 04/25/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: f2da4a22b0b4e0faf07c1b5d88655f5ac32bcf1a
ms.sourcegitcommit: e9f431f6ee60196bbae604e7d8152c6ef48ead1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a>使用 Eclipse 创建基本的 Azure Web 应用
本教程说明如何使用 [Azure Toolkit for Eclipse] 创建一个基本的 Hello World 应用程序，并将其部署到 Azure 作为 Web 应用。 为简单起见显示了一个基本 JSP 示例，但就 Azure 部署而言，相似的步骤也适用于 Java servlet。

完成本教程后，应用程序会在 Web 浏览器中如下图所示：

![Hello World 应用预览](./media/app-service-web-eclipse-create-hello-world-web-app/9.png)

## <a name="prerequisites"></a>先决条件
* Java 开发人员工具包 (JDK) 1.8 或更高版本。
* Eclipse IDE for Java EE Developers，Luna 或更高版本。 可从 <http://www.eclipse.org/downloads/> 下载。
* 一个基于 Java 的 Web 服务器或应用程序服务器（例如 [Apache Tomcat] 或 [Jetty]）的分发。
* 一个 Azure 订阅，可从 <https://www.azure.cn/pricing/1rmb-trial/> 或 <http://www.azure.cn/pricing/overview/> 获取。
* [Azure Toolkit for Eclipse]。 有关安装 Azure 工具包的信息，请参阅 [安装适用于 Eclipse 的 Azure 工具包]。

## <a name="to-create-a-hello-world-application"></a>创建 Hello World 应用程序
首先，我们从创建 Java 项目开始。

1. 启动 Eclipse。在 Eclipse 中的菜单上，依次单击“文件”、新建和“动态 Web 项目”。 （如果在单击“文件”、“新建”后未看到“动态 Web 项目”作为可用项目列出，则执行以下操作：依次单击“文件”、“新建”、“项目...”，展开“Web”，单击“动态 Web 项目”，然后单击“下一步”。） 
2. 在本教程中，项目命名为 **MyWebApp**。 屏幕应与下图中所示类似：

    ![创建新的动态 Web 项目][02]
3. 单击“完成” 。

4. 在 Eclipse 的项目资源管理器视图中，展开“MyHelloWorld”。 右键单击“WebContent”，并依次单击“新建”、“JSP 文件”。

5. 在“新建 JSP 文件”对话框中，将文件命名为 **index.jsp**。 将父文件夹保留为 MyHelloWorld/WebContent，如下所示：

   ![ic659262](./media/app-service-web-eclipse-create-hello-world-web-app/ic659262.png)

6. 对于本教程，请在“选择 JSP 模板”对话框中选择“新建 JSP 文件(html)”，然后单击“完成”。

7. 在 Eclipse 中打开 index.jsp 文件后，添加文本以动态显示 **Hello World!** 现有 `<body>` 元素中。 更新后的 `<body>` 内容应与下图中所示类似：
   ```
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
8. 保存 index.jsp。

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>将应用程序部署到 Azure Web 应用容器
有多种方式可以将 Java Web 应用程序部署到 Azure。 本教程说明其中一个最简单的方式：将应用程序部署到 Azure Web 应用容器，无需特殊的项目类型或额外的工具。 Azure 为提供 JDK 及 Web 容器软件，因此不需要自己上传；只需要 Java Web 应用。 这样，应用程序的发布过程只需数秒，连一分钟都不用。

1. 在 Eclipse 的项目资源管理器中，右键单击“MyWebApp”。
2. 在上下文菜单中选择“Azure”，然后单击“发布为 Azure Web 应用...”

    ![发布为 Azure Web 应用](./media/app-service-web-eclipse-create-hello-world-web-app/1.png)

3. 在“部署 Web 应用”页上单击“创建...”。

   ![](media\app-service-web-eclipse-create-hello-world-web-app\2.png) 

4. 输入应用服务的名称。
  
   在“Web 容器”下拉菜单中，为应用程序选择适当的软件。

   当前，可以从 Tomcat 8、Tomcat 7 或 Jetty 9 中选择。 Azure 将提供所选软件的最新分发版，并且该版本基于由 JDK 8 创建并由 Azure 提供的 JDK 最新分发版运行。 

   在“订阅”下拉菜单中，选择要用于此部署的订阅。

   “应用服务计划”菜单列出了与选定资源组关联的应用服务计划。 （应用服务计划指定了 Web 应用的位置、定价层以及计算实例大小等信息。 单个应用服务计划可用于多个 Web 应用，这也是从特定 Web 应用部署中单独维护它的原因。）

   可以选择现有的应用服务计划（如果有），也可以创建新的应用服务计划：

   * 在“新建”文本框中，为新的应用服务计划指定名称。

   * 在“位置”下拉菜单中，为计划选择适当的 Azure 数据中心位置。
   
   * 在“定价层”下拉菜单中，为计划选择适当的定价。 对于测试，可以选择“Free_F1”。

   ![](media\app-service-web-eclipse-create-hello-world-web-app\3.png)  

   在“资源组”下拉菜单中，选择要与 Web 应用关联的资源组。 （使用 Azure 资源组可以将相关资源组织在一起，以便于将它们一起删除。）

   可以选择现有的资源组（如果有），也可以创建新的资源组：

   * 在“新建”文本框中，为新的资源组指定名称。
   * 所创建的资源组与在上面配置或创建的应用服务计划具有相同的位置。    

   ![](media\app-service-web-eclipse-create-hello-world-web-app\4.png)  

   默认情况下，Azure 会自动将最新的 Java 8 发行版作为 JVM 部署到 Web 应用容器。 但是，可以根据 Web 应用的需要指定 JVM 的其他版本和分发版。 如果要指定 Web 应用的 JDK，请单击“JDK”  选项卡，并选择下列选项之一：

   * 默认：此选项会部署最新的的 Java 8 发行版。

   * 第三方：使用此选项可从 Azure 所提供的 JDK 列表中进行选择。
   
   * 下载 URL：此选项可以指定自己的 JDK 发行版，此发行版必须打包为 ZIP 文件并上传到公开可用的下载位置或可以访问的 Azure 存储帐户。

   ![](media\app-service-web-eclipse-create-hello-world-web-app\5.png)  

   单击“创建”。

5. 单击以下图表上的“部署”，将项目部署到 Azure Web 应用。 第一次生成时可能需要花费几分钟时间。 “Azure 活动日志”将显示在 Eclipse 的选项卡式视图部分中。

   ![](media\app-service-web-eclipse-create-hello-world-web-app\6.png)  

   ![](media\app-service-web-eclipse-create-hello-world-web-app\7.png)  

6. 部署成功后，“Azure 活动日志”将状态显示为“已发布”。 单击“已发布”（如下图所示），浏览器将打开部署的实例。

   ![](media\app-service-web-eclipse-create-hello-world-web-app\8.png)  

   ![](media\app-service-web-eclipse-create-hello-world-web-app\9.png)  

> [!WARNING]
> 此时，已将 Azure 应用程序部署到云中。 但在继续进行之前，应认识到，即使已部署的应用程序没有运行，也会继续累加订阅的计费时间。 因此，从 Azure 订阅中删除不需要的部署极为重要。
> 
> 
## <a name="updating-your-web-app"></a>更新 Web 应用
更新现有运行中的 Azure Web 应用是一个快速而简单的过程，可以使用两个更新选项：

* 可以更新现有 Java Web 应用的部署。
* 可以将其他 Java 应用程序发布到同一个 Web 应用容器。

对于任一情况，过程都是相同的，只需几秒钟即可完成：

1. 在 Eclipse 项目资源管理器中，右键单击要更新或添加到现有 Web 应用容器的 Java 应用程序。
2. 出现上下文菜单时，请选择“Azure”，然后单击“发布为 Azure Web 应用...”
3. 由于之前已登录，因此将看到现有 Web 应用容器的列表。 选择要对其发布或重新发布 Java 应用程序的 Web 应用容器，然后单击“确定”。

几秒钟后，“Azure 活动日志”视图会将已更新的部署显示为“已发布”，可以在 Web 浏览器中验证已更新的应用程序。

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>启动、停止或重启现有 Web 应用
若要启动或停止现有的 Azure Web 应用容器（包括其中所有已部署的 Java 应用程序），可以使用“Azure 资源管理器”视图。

如果“Azure 资源管理器”视图尚未打开，可以依次单击 Eclipse 中的“窗口”菜单、“显示视图”、“其他...”、“Azure”和“Azure 资源管理器”将它打开。 如果事先未尚未登录，系统将提示登录。

显示“Azure 资源管理器”视图后，使用以下步骤来启动或停止 Web 应用：  

1. 展开“Azure”节点。
2. 展开“Web 应用”节点。 
3. 右键单击所需的 Web 应用。
4. 出现上下文菜单时，单击“启动”、“停止”或“重启”。 请注意，菜单选项可识别上下文，因此，只能停止正在运行的 Web 应用，或启动当前未运行的 Web 应用。

    ![停止现有的 Web 应用][13]

## <a name="to-delete-your-deployment"></a>删除部署

执行以下步骤，删除用于 Eclipse 的 Azure 工具包中的部署。

1. 在 Eclipse 的项目资源管理器中，右键单击“MyHelloWorld”。

2. 单击“Azure”，然后单击“发布为 Azure Web 应用”。

   ![](./media/app-service-web-eclipse-create-hello-world-web-app/1.png)

3. 在“部署 Web 应用”页上选择要删除的 Web 应用，然后单击“删除...”。

   ![](media\app-service-web-eclipse-create-hello-world-web-app\10.png)

   单击“是”。 

   ![](media\app-service-web-eclipse-create-hello-world-web-app\11.png)

   可以看到 Web 应用在数分钟后删除。

   ![](media\app-service-web-eclipse-create-hello-world-web-app\12.png)

   ![](media\app-service-web-eclipse-create-hello-world-web-app\13.png)

## <a name="next-steps"></a>后续步骤
有关 Azure Toolkits for Java IDE 的详细信息，请参阅以下链接：

* [Azure Toolkit for Eclipse]
  * [安装适用于 Eclipse 的 Azure 工具包]
  * *在 Eclipse 中创建 Azure 的 Hello World Web 应用（本文）*
  * [Azure Toolkit for Eclipse 的新增功能]
* [Azure Toolkit for IntelliJ]
  * [安装 Azure Toolkit for IntelliJ]
  * [在 IntelliJ 中创建 Azure 的 Hello World Web 应用]
  * [Azure Toolkit for IntelliJ 中的新增功能]

有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心]。

有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。

<!-- URL List -->

[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure Toolkit for IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[在 IntelliJ 中创建 Azure 的 Hello World Web 应用]: ./app-service-web-intellij-create-hello-world-web-app.md
[安装适用于 Eclipse 的 Azure 工具包]: ../azure-toolkit-for-eclipse-installation.md
[安装 Azure Toolkit for IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Azure Toolkit for Eclipse 的新增功能]: ../azure-toolkit-for-eclipse-whats-new.md
[Azure Toolkit for IntelliJ 中的新增功能]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java 开发人员中心]: /develop/java/
[Web 应用概述]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png