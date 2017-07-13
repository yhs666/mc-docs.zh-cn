---
title: "用于 IntelliJ 的 Azure 工具包中的新增功能 | Azure"
description: "了解 Azure Toolkit for IntelliJ 中的最新功能。"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 46ed791f-df59-416a-809e-f52345ad973c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
origin.date: 12/22/2016
ms.date: 02/14/2017
ms.author: v-junlch
ms.openlocfilehash: d3025c57e5d17b9d5162164d0f7cce82205a1707
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# Azure Toolkit for IntelliJ 中的新增功能
<a id="whats-new-in-the-azure-toolkit-for-intellij" class="xliff"></a>
## Azure Toolkit for IntelliJ 版本
<a id="azure-toolkit-for-intellij-releases" class="xliff"></a>
本文包含有关 Azure Toolkit for IntelliJ 的不同版本和最新更新的信息。

> [!NOTE]
> 另外还有 Azure Toolkit for Eclipse IDE。 有关详细信息，请参阅 [Azure Toolkit for Eclipse]。

### 2016 年 8 月 26 日
<a id="august-26-2016" class="xliff"></a>
用于 IntelliJ 的 Azure 工具包 - 2016 年 8 月版包含以下增强功能：

* **自定义 JDK 分发版**。 用于 IntelliJ 的 Azure 工具包现在支持指定任意 JDK 版本并将其部署到 Azure WebApp 容器：
  * 除了 Azure 提供的 JDK 以外，还可以从 Azul Systems 在 Azure 上提供的多种 Zulu OpenJDK 版本中进行选择。
  * 还可以指定自己的 JDK 分发版，前提是将它以 ZIP 文件的形式上传到存储帐户。
* **Azure 资源管理器视图增强**：
  * 支持使用 Azure 的新 Resource Manager 模型管理虚拟机：无需退出 IDE，即可列出、创建和删除基于 Resource Manager 的虚拟机。
  * 使用 Azure 的 Resource Manager 为存储帐户 Blob 管理提供支持，对管理“经典”存储帐户的现有功能做出补充。
* **Microsoft JDBC Driver 6.0 for SQL Server**。 此项更新包括适用于 Microsoft SQL Server 的最新 JDBC 驱动程序 (v6.0)，现在，该驱动程序以库的形式提供，可以轻松添加到 Java 项目，从而取代旧版本。

### 2016 年 6 月 29 日
<a id="june-29-2016" class="xliff"></a>
Azure Toolkit for IntelliJ - 2016 年 6 月版包含以下增强功能：

* **需要 Java 8**。 Azure Toolkit for IntelliJ 现在需要 Java 8，不过此需求仅适用于此工具包 - 应用程序可以继续使用 Azure 支持的所有 Java 版本。
* **支持最新的 Java JDK**。 Azure Toolkit for IntelliJ 现在支持最新版本的 Java JDK。
* **支持 Azure SDK v2.9.1**。 最新版 Azure SDK 现在是 Azure Toolkit for IntelliJ 的最低必备组件。
* **集成示例**。 Azure Toolkit for IntelliJ 目前精选了数个示例应用程序，可帮助开发人员快速入门。
* **HDInsight 工具集成**。 Azure 的 HDInsight 工具现在随附于 Azure Toolkit for IntelliJ。 
* **远程调试 Java Web 应用**。 Azure Toolkit for IntelliJ 现在支持对 Azure 应用服务上的 Java Web 应用进行远程调试。

### 2016 年 4 月 12 日
<a id="april-12-2016" class="xliff"></a>
Azure Toolkit for IntelliJ - 2016 年 4 月版包含以下增强功能：

* **支持 Azure SDK v2.9.0**。 最新版 Azure SDK 现在是 Azure Toolkit for IntelliJ 的最低必备组件。
* **与 Azure Web 应用支持有关的其他可用性、响应能力和性能改进**。 工具包中多项与 Azure 通信方式的性能优化会让 UI 响应速度更快。
* **能够在 IntelliJ 内删除 Azure 中现有的 Web 应用程序容器**。 Azure Toolkit for IntelliJ 现在允许删除现有的 Azure Web 容器，而不需要退出 IntelliJ。

## 另请参阅
<a id="see-also" class="xliff"></a>
有关 Azure Toolkits for Java IDE 的详细信息，请参阅以下链接：

* [Azure Toolkit for Eclipse]
  * [安装 Azure Toolkit for Eclipse]
  * [Azure Toolkit for Eclipse 的新增功能]
* [Azure Toolkit for IntelliJ]
  * [安装 Azure Toolkit for IntelliJ]
  * *用于 IntelliJ 的 Azure 工具包中的新增功能（本文）*

有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心]。

<!-- URL List -->

[Azure Toolkit for Eclipse]:./azure-toolkit-for-eclipse.md
[Azure Toolkit for IntelliJ]:./azure-toolkit-for-intellij.md
[安装 Azure Toolkit for Eclipse]:./azure-toolkit-for-eclipse-installation.md
[安装 Azure Toolkit for IntelliJ]:./azure-toolkit-for-intellij-installation.md
[Azure Toolkit for Eclipse 的新增功能]:./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]:./azure-toolkit-for-intellij-whats-new.md

[Azure Java 开发人员中心]:/develop/java/