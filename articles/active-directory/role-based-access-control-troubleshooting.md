---
title: "Azure RBAC 故障排除 | Microsoft Docs"
description: "获取有关基于角色的访问控制资源问题或疑问的帮助。"
services: azure-portal
documentationcenter: na
author: alexchen2016
manager: digimobile
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/12/2017
ms.date: 08/22/2017
ms.author: v-junlch
ms.reviewer: rqureshi
ms.openlocfilehash: 40621521d5fbe550c45c10412e026aa49874266f
ms.sourcegitcommit: 0f2694b659ec117cee0110f6e8554d96ee3acae8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="role-based-access-control-troubleshooting"></a>基于角色的访问控制故障排除

本文回答有关使用角色授予的特定访问权限的常见问题，以便你了解在 Azure 门户中使用角色时可以预期什么，并可以排查访问权限问题。 以下三种角色涵盖所有资源类型：

- 所有者  
- 参与者  
- 读取器  

所有者和参与者对管理体验具有完全访问权限，但是参与者无法向其他用户授予访问权限。 具有读者角色事情会变得更加有趣，因此，我们着重介绍读者角色。 有关如何授予访问权限的详细信息，请参阅[基于角色的访问控制入门文章](./role-based-access-control-configure.md)。

## <a name="app-service-workloads"></a>应用服务工作负荷
### <a name="write-access-capabilities"></a>写访问功能
如果为用户授予单个 Web 应用的只读访问权限，某些功能可能会被禁用，这可能不是你所期望的。 以下管理功能需要对 Web 应用具有写入权限（参与者或所有者），并且不适用于任何只读方案。

- 命令（例如启动、停止等。）
- 更改设置（如常规配置、缩放设置、备份设置和监视设置）
- 访问发布凭据和其他机密（如应用设置和连接字符串）
- 流式传输日志
- 诊断日志配置
- 控制台（命令提示符）
- 活动和最新部署（适用于本地 Git 持续部署）
- 估计费用
- Web 测试
- 虚拟网络（只在虚拟网络是由具有写入权限的用户在以前配置时，才对读者可见）。

如果无法访问以上任何磁贴，则需要让管理员提供对 Web 应用的“参与者”访问权限。

### <a name="dealing-with-related-resources"></a>处理相关资源
由于存在几个相互作用的不同资源，Web 应用程序是复杂的。 下面是包含几个网站的典型资源组：

![Web 应用程序资源组](./media/role-based-access-control-troubleshooting/website-resource-model.png)

因此，如果只授予某人对 Web 应用的访问权限，则 Azure 门户中的网站边栏选项卡上的很多功能将被禁用。

这些项需要对与网站对应的应用服务计划具有写入权限：  

- 查看 Web 应用的定价层（免费或标准）  
- 规模配置（实例数、虚拟机大小、自动缩放设置）  
- 配额（存储空间、带宽、CPU）  

这些项需要对包含网站的整个资源组具有写入权限：  

- SSL 证书和绑定（SSL 证书可以在同一资源组和地理位置中的站点之间共享）  
- 警报规则  
- 自动缩放设置  
- Application Insights 组件  
- Web 测试  

## <a name="virtual-machine-workloads"></a>虚拟机工作负荷
与 Web 应用程序很类似，虚拟机边栏选项卡上的某些功能需要对虚拟机或资源组中的其他资源具有写访问权限。

虚拟机与域名、虚拟网络、存储帐户和警报规则相关。

这些项需要对虚拟机具有写入权限：

- 终结点  
- IP 地址  
- 磁盘  
- 扩展  

这些项需要对虚拟机和其所在的资源组（以及域名）具有写入权限：  

- 可用性集  
- 负载均衡集  
- 警报规则  

如果无法访问以上任何磁贴，则需要让管理员提供对资源组的“参与者”访问权限。

## <a name="see-more"></a>另请参阅
- [基于角色的访问控制](role-based-access-control-configure.md)：Azure 门户中的 RBAC 入门。
- [内置角色](role-based-access-built-in-roles.md)：获取有关 RBAC 中标配角色的详细信息。
- [Azure RBAC 中的自定义角色](role-based-access-control-custom-roles.md)：了解如何创建自定义角色，以满足访问需要。
- [创建访问权限更改历史记录报告](role-based-access-control-access-change-history-report.md)：记录 RBAC 中的角色分配更改。

<!--Update_Description: wording update -->
