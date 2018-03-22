---
title: 了解 Azure Stack 中的 DNS | Microsoft Docs
description: 了解 Azure Stack 中的 DNS 特性和功能
services: azure-stack
documentationcenter: ''
author: ScottNapolitan
manager: darmour
editor: ''
ms.assetid: 60f5ac85-be19-49ac-a7c1-f290d682b5de
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 09/25/2017
ms.date: 03/09/2018
ms.author: v-junlch
ms.openlocfilehash: b671adb6b0528c4838ddba2b95855c017cc4aa44
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="introducing-idns-for-azure-stack"></a>适用于 Azure Stack 的 iDNS 简介

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

iDNS 是 Azure Stack 中的一项功能，可用于解析外部 DNS 名称（例如 http://www.bing.com)）。
它还可用于注册内部虚拟网络名称。 如此一来，就可按名称（而非 IP 地址）解析同一虚拟网络上的 VM，而不需要提供自定义 DNS 服务器条目。

这是 Azure 中的内置功能，但也可以在 Windows Server 2016 和 Azure Stack 中使用。

## <a name="what-does-idns-do"></a>iDNS 有什么作用？
使用 Azure Stack 中的 iDNS 可以获得以下功能，而无需指定自定义 DNS 服务器条目：

- 适用于租户工作负荷的共享 DNS 名称解析服务。
- 适用于租户虚拟网络内的名称解析和 DNS 注册的权威 DNS 服务。
- 从租户 VM 解析 Internet 名称的递归 DNS 服务。 租户不再需要指定自定义 DNS 条目，就可以解析 Internet 名称（例如，www.bing.com）。

你仍然可以沿用自己的 DNS ，也可以使用自定义 DNS 服务器（如果想要的话）。 但现在，如果只是想要能够解析 Internet DNS 名称，而且能够连接到同一虚拟网络中的其他虚拟机，则无需指定任何项目也能正常工作。

## <a name="what-does-idns-not-do"></a>iDNS 不可以做什么？
iDNS 不允许针对可以从虚拟网络外部解析的名称创建 DNS 记录。

在 Azure 中，可以选择指定能够与公共 IP 地址关联的 DNS 名称标签。 你可以选择标签（前缀），但 Azure 会根据创建公共 IP 地址所在的区域选择后缀。

![DNS 名称标签的屏幕截图](./media/azure-stack-understanding-dns-in-tp2/image3.png)

在上图中，Azure 将会在 DNS 中为该区域下指定的 DNS 名称标签创建“A”记录。 前缀和后缀一起构成完全限定域名 (FQDN)，此域名可以从公共 Internet 上的任何位置解析。

Azure Stack 仅支持用于内部名称注册的 iDNS，因此它无法执行以下操作：

- 在现有的托管 DNS 区域（例如，local.azurestack.external）下创建 DNS 记录。
- 创建 DNS 区域（例如 Contoso.com）。
- 在你自己的自定义 DNS 区域下创建记录。
- 支持购买域名。


