---
title: include 文件
description: include 文件
services: networking
author: rockboyfor
ms.service: networking
ms.topic: include
origin.date: 08/16/2018
ms.date: 09/01/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 2081545d60c884016aecd935d06557c8d133dcc3
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52651010"
---
<a name="virtual-networking-limits-classic"></a>以下限制仅适用于每个订阅通过经典部署模型托管的网络资源。
<!--Not Available on [view your current resource usage against your subscription limits](../articles/networking/check-usage-against-limits.md)-->

| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 虚拟网络 |50 |100 |
| 本地网络站点 |20 个 |联系支持人员 |
| 每个虚拟网络的 DNS 服务器数 |20 个 |100 |
| 每个虚拟网络的专用 IP 地址数 |4096 |4096 |
| 虚拟机或角色实例的单 NIC 并发 TCP 或 UDP 流数 |500K |500K |
| 网络安全组 (NSG) |100 |200 |
| 每个 NSG 的 NSG 规则数 |200 |400 |
| 用户定义路由表数 |100 |200 |
| 每个路由表的用户定义的路由数 |100 |400 |
| 公共 IP 地址 (动态) |5 |联系支持人员 |
| 保留的公共 IP 地址 |20 个 |联系支持人员 |
| 每个部署的公共 VIP |5 |联系支持人员 |
| 每个部署的私有 VIP (ILB) |1 |1 |
| 终结点访问控制列表 (ACL) |50 |50 |

<a name="azure-resource-manager-virtual-networking-limits"></a>
#### <a name="networking-limits---azure-resource-manager"></a>网络限制 - Azure 资源管理器
以下限制仅适用于通过每个订阅的每个区域的 Azure Resource Manager 进行管理的网络资源。
<!--Not Available on [view your current resource usage against your subscription limits](../articles/networking/check-usage-against-limits.md)-->

> [!NOTE]
> 我们最近将所有默认限制提高到了最大限制。 如果没有 **最大限制** 列，则资源没有可调整的限制。 如果过去已通过客户支持提高了这些上限，因此看不到下述更新的限制，可[免费提交联机客户支持请求](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| 资源 | 默认限制 | 最大限制 |
| --- | --- |
| 虚拟网络 |50 |1000 |
| 每个虚拟网络的子网数 |1000 |10000 |
| 每个虚拟网络的虚拟网络对等互连数 |10 个 |50 |
| 每个虚拟网络的 DNS 服务器数 |9 |25 |
| 每个虚拟网络的专用 IP 地址数 |16384** |16384 |
| 每个网络接口的专用 IP 地址数 |256 |1024 |
| 虚拟机或角色实例的单 NIC 并发 TCP 或 UDP 流数 |500K |500K |
| 网络接口 (NIC) |24000** |24000 |
| 网络安全组 (NSG) |100 |5000 |
| 每个 NSG 的 NSG 规则数 |1000** |1000 |
<!-- 不可用于 | 为安全规则中的源或目标指定的 IP 地址和范围数 |2000 |4000 | -->
<!-- 不可用于 | 应用程序安全组 |200 |500 | -->
<!-- 不可用于 | 每个 IP 配置和每个 NIC 的应用程序安全组数 |10 个 |20 个 | -->
<!-- 不可用于 | 每个应用程序安全组的 IP 配置数 |1000 |4000 | -->
<!-- 不可用于 | 可在网络安全组的所有安全规则中指定的应用程序安全组数 |50 |100 | -->
| 用户定义路由表数 |100 |200 |
| 每个路由表的用户定义的路由数 |100 |400 |
| 每个 VPN 网关的点到站点根证书数 |20 个 |20 个 |
<a name="publicip-address"></a>
#### <a name="public-ip-address-limits"></a>公共 IP 地址限制

| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 公共 IP 地址数 - 动态 |（基本）60 |联系支持人员 |
| 公共 IP 地址数 - 静态 |（基本）20 |联系支持人员 |
<!-- 不可用于 | 公共 IP 地址数 - 静态 |（标准）20 |联系支持人员 | -->

<a name="load-balancer"></a>
#### <a name="load-balancer-limits"></a>负载均衡器限制
以下限制仅适用于通过每个订阅的每个区域的 Azure Resource Manager 进行管理的网络资源。
<!--Not Available on [view your current resource usage against your subscription limits](../articles/networking/check-usage-against-limits.md)-->

| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 负载均衡器 | 100 | 1000 |
| 每个资源的规则数，基本 | 150 | 250 |
<!-- Not Avaiable on Rules per resource, Standard  -->
<!-- Not Avaiable on Rules per IP configuration --> | 前端 IP 配置数，基本 | 10 | 200 | <!-- Not Avaiable on Frontend IP configurations, Standard -->
| 后端池，基本 | 100，单个可用性集 | - | <!-- Not Avaiable on Backend pool, Standard  -->
<!-- Not Avaiable on Backend resources per Load Balancer, Standard * -->
<!-- Not Avaiable on HA Ports, Standard 1 per internal frontend  -->

<!-- Not Avaiable on | Rules per resource, Standard | 1250 | 1500 | -->
<!-- Not Avaiable on | Rules per IP configuration | 299 |299 | -->
<!-- Not Avaiable on | Frontend IP configurations, Standard | 10 | 600 | -->
<!-- Not Avaiable on | Backend pool, Standard | 1000, single VNet | contact support | -->
<!-- Not Avaiable on | Backend resources per Load Balancer, Standard * | 150 | 150 |-->
<!-- Not Avaiable on | HA Ports, Standard | 1 per internal frontend | - | -->

** 多达 150 个资源，独立虚拟机、可用性集和虚拟机规模集的任意组合。

如果需要在默认值的基础上提高限制，请[与支持人员联系](https://www.azure.cn/support/contact/)。
<!-- ms.date: 05/07/2018 -->
