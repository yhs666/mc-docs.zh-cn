---
title: "使用 Azure Toolkit for Eclipse 启用会话相关性"
description: "了解如何使用 Azure Toolkit for Eclipse 启用会话相关性。"
services: 
documentationCenter: java
authors: rmcmurray
manager: wpickett
editor: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 11/01/2016
ms.author: v-junlch
ms.openlocfilehash: 0c62323fda292affa74983a33e82dfc987fd14de
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# 启用会话相关性
<a id="enable-session-affinity" class="xliff"></a> #
在 Azure Toolkit for Eclipse 中，你可以为角色启用 HTTP 会话相关性或“粘性会话”。 下图显示了用于启用会话相关性功能的“负载均衡”属性对话框： 

![][ic719492]

## 为角色启用会话相关性
<a id="to-enable-session-affinity-for-your-role" class="xliff"></a>
1. 在 Eclipse 的项目资源管理器中右键单击角色，单击“Azure”，然后单击“负载均衡”。
2. 在“WorkerRole1 负载均衡属性”对话框中： 
   1. 选中“为此角色启用 HTTP 会话相关性(粘性会话)”。
   2. 对于“要使用的输入终结点”，请选择要使用的输入终结点，例如“http (public:80, private:8080)”。 应用程序必须使用此终结点作为其 HTTP 终结点。 你可以为角色启用多个终结点，但只能选择其中一个来支持粘性会话。
   3. 重新生成你的应用程序。

启用后，如果你有多个角色实例，则来自特定客户端的 HTTP 请求将继续由同一角色实例处理。

Eclipse 工具包是通过在每个角色实例中安装名为应用程序请求路由 (ARR) 的特殊 IIS 模块来做到这一点的。 ARR 将 HTTP 请求重新路由到相应的角色实例。 该工具包将自动重新配置选定的终结点，使传入的 HTTP 流量先路由到 ARR 软件。 该工具包还会创建 Java 服务器配置为侦听的新内部终结点。 ARR 正是使用此终结点将 HTTP 流量重新路由到相应的角色实例。 这样，多实例部署中的每个角色实例将充当所有其他实例的反向代理，从而实现了粘性会话。

## 有关会话相关性的说明
<a id="notes-about-session-affinity" class="xliff"></a>
* 会话相关性在计算模拟器中不起作用。 可以在计算模拟器中应用这些设置而不会干扰你的生成过程或计算模拟器执行，但该功能本身在计算模拟器中不能正常运行。
* 启用会话相关性会导致部署在 Azure 中占用更多的磁盘空间量，因为当你的服务在 Azure 云中启动时，会在角色实例中下载并安装其他软件。
* 初始化每个角色需要更长的时间。
* 将会添加一个充当上述流量路由器的内部终结点。

有关如何在启用会话相关性的情况下维护会话数据的示例，请参阅 [如何使用会话相关性来维护会话数据][How to Maintain Session Data with Session Affinity]。

## 另请参阅
<a id="see-also" class="xliff"></a>
[适用于 Eclipse 的 Azure 工具包][Azure Toolkit for Eclipse]

[在 Eclipse 中为 Azure 创建 Hello World 应用程序][Creating a Hello World Application for Azure in Eclipse]

[安装 Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse] 

[如何使用会话相关性来维护会话数据][How to Maintain Session Data with Session Affinity]

有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心][Azure Java Developer Center]。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How to Maintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->