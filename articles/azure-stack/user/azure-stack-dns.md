---
title: Azure Stack 中的 DNS | Microsoft Docs
description: Azure Stack 中的 DNS
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/28/2018
ms.date: 03/22/2018
ms.author: v-junlch
ms.openlocfilehash: 601977fdb1a3123e5d986416afc412207cb19798
ms.sourcegitcommit: 61fc3bfb9acd507060eb030de2c79de2376e7dd3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2018
---
# <a name="dns-in-azure-stack"></a>Azure Stack 中的 DNS

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

Azure Stack 包括以下 DNS 功能：
- 支持 DNS 主机名解析
- 使用 API 创建和管理 DNS 区域和记录

## <a name="support-for-dns-hostname-resolution"></a>支持 DNS 主机名解析
可以为公共 IP 资源指定一个 DNS 域名标签，以创建 *domainnamelabel.location*.cloudapp.azurestack.external 到 Azure Stack 托管的 DNS 服务器中公共 IP 地址的映射。  

例如，如果创建公共 IP 资源并以 **contoso** 作为本地 Azure Stack 位置的域名标签，则完全限定域名 (FQDN) **contoso.local.cloudapp.azurestack.external** 会解析为资源的公共 IP 地址。 可以使用此 FQDN 创建自定义域 CNAME 记录，该记录指向 Azure Stack 中的公共 IP 地址。

> [!IMPORTANT]
> 所创建的每个域名标签在其 Azure Stack 位置中必须是唯一的。

如果使用门户创建公共 IP 地址，它如下所示：

![创建公共 IP 地址](./media/azure-stack-whats-new-dns/image01.png)

如果想要将公共 IP 地址与负载均衡资源相关联，此配置会很有用。 例如，你可能有负载均衡器来处理来自 Web 应用程序的请求。 在负载均衡器的背后，是位于一个或多个虚拟机上的网站。 现在可以通过 DNS 名称来访问负载均衡网站，而不必通过 IP 地址。

## <a name="create-and-manage-dns-zones-and-records-using-api"></a>使用 API 创建和管理 DNS 区域和记录
可以在 Azure Stack 中创建和管理 DNS 区域和记录。  

Azure Stack 使用与 Azure DNS API 一致的 API 提供与 Azure 类似的 DNS 服务。  通过在 Azure Stack DNS 中托管域，用户可以使用与其他 Azure 服务相同的凭据、API、工具、计费方式和支持来管理 DNS 记录。 

由于很明显的原因，Azure Stack DNS 的基础结构会比 Azure 的更为精简。 因此，范围、规模和性能会取决于 Azure Stack 部署的规模和其部署环境。  所以，像性能、可用性、全局分发和高可用性 (HA) 等指标可能因部署而异。

## <a name="comparison-with-azure-dns"></a>与 Azure DNS 比较
Azure Stack 中的 DNS 与 Azure 中的 DNS 类似，但主要有两个例外情况：
- **不支持 AAAA 记录**

    Azure Stack 不支持 AAAA 记录，因为 Azure Stack 不支持 IPv6 地址。  这是 Azure DNS 与 Azure Stack DNS 之间的主要差异。
- **不是多租户**

    与 Azure 不同，Azure Stack 中的 DNS 服务不是多租户。 因此每个租户不能创建相同的 DNS 区域。 仅首个订阅尝试创建区域会成功，后续请求都会失败。  这是已知问题，也是 Azure DNS 和 Azure Stack DNS 之间的主要差异。 此问题将在未来版本中解决。

此外，Azure Stack DNS 在实现标记、元数据、Etag 和限制的方式方面也有一些细微差异。

以下信息仅适用于 Azure Stack DNS，并会与 Azure DNS 稍有不同。 

### <a name="tags-metadata-and-etags"></a>标记、元数据和 Etag

**标记**

Azure Stack DNS 支持在 DNS 区域资源上使用 Azure 资源管理器标记。 它不支持在 DNS 记录集上使用标记，不过作为替代方法，在 DNS 记录集上支持“元数据”，如下所述。

Metadata

作为记录集标记的替代方法，Azure Stack DNS 支持使用“元数据”批注记录集。 与标记相类似，通过元数据可将名称/值对与每个记录集相关联。 这非常有用，例如可用于记录每个记录集的用途。 与标记不同的是，元数据不能用于提供 Azure 帐单的筛选视图，且不能在 Azure 资源管理器策略中指定。

**Etag**

假设两个人或两个进程尝试同时修改一条 DNS 记录。 哪一个占先？ 占先方是否知道他们/它们覆盖了其他人/进程创建的更改？

Azure Stack DNS 使用 Etag 来安全地处理对同一资源的并发更改。 Etag 与 Azure 资源管理器“标记”不同。 每个 DNS 资源（区域或记录集）都有与其相关联的 Etag。 只要检索资源，就会检索其 Etag。 更新资源时，可以选择传递回 Etag 以便 Azure Stack DNS 可以验证服务器上的 Etag 是否匹配。 由于对资源的每次更新都会导致重新生成 Etag，Etag 不匹配表示发生了并发更改。 当创建新的资源时也可以使用 Etag，以确保该资源不存在。

默认情况下，Azure Stack DNS PowerShell 使用 Etag 来阻止对区域和记录集的并发更改。 可选 -Overwrite 开关可用于取消 Etag 检查，这种情况下会覆盖发生的所有并发更改。

Etag 是在 Azure Stack DNS REST API 级别使用 HTTP 标头指定的。 下表给出了它们的行为：

| 标头 | 行为|
|--------|---------|
| 无   | PUT 始终成功（没有 Etag 检查）|
| If-match| 只有当资源存在并且 Etag 匹配时，PUT 才会成功|
| If-match *| 只有当资源存在时，PUT 才会成功|
| If-none-match *| 只有当资源不存在时，PUT 才会成功|

### <a name="limits"></a>限制

使用 Azure Stack DNS 时，以下默认限制适用：

| 资源| 默认限制|
|---------|--------------|
| 每个订阅的区域数| 100|
| 每个区域的记录集数| 5000|
| 每个记录集的记录数| 20 个|

## <a name="next-steps"></a>后续步骤
[适用于 Azure Stack 的 iDNS 简介](azure-stack-understanding-dns.md)

<!-- Update_Description: wording update -->