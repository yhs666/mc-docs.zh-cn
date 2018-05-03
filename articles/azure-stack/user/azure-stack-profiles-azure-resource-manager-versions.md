---
title: Azure Stack 中的配置文件支持的资源提供程序 API 版本 | Microsoft Docs
description: 了解 Azure Stack 中的配置文件支持的 Azure 资源管理器版本。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 2E21C8DE-D540-4C1C-A0EF-1B7125DB7A6E
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/28/2018
ms.date: 04/23/2018
ms.author: v-junlch
ms.reviewer: sijuman
ms.openlocfilehash: 2025ed183b29b4a19c8b67924680b675c5a0b284
ms.sourcegitcommit: 85828a2cbfdb58d3ce05c6ef0bc4a24faf4d247b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2018
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Azure Stack 中的配置文件支持的资源提供程序 API 版本

Azure 资源提供程序提供可通过 Azure 资源管理器进行部署和管理的资源。 每个提供程序提供可对资源使用的操作。 部分常见资源提供程序包括 Microsoft.Compute（提供虚拟机）、Microsoft.Storage（提供存储帐户资源）和 Microsoft.Web（提供与 Web 应用相关的资源）。 有关详细信息，请参阅[资源提供程序和类型](/azure-resource-manager/resource-manager-supported-services)。

下面是每个资源提供程序的表格，其中指出了使用配置文件时 Azure Stack 支持的 API 版本。

### <a name="microsoftauthorization"></a>Microsoft.Authorization

使用基于角色的访问控制来管理组织中用户可对资源执行的操作。 使用这一组操作可以定义角色、将角色分配给用户或组，以及获取有关权限的信息。 有关详细信息，请参阅[授权](https://docs.microsoft.com/rest/api/authorization/)。

| 资源类型 | API 版本 |
|---------------------|--------------------|
| 锁 | 2017-04-01 |
| 操作 | 2015-07-01 |
| 权限 | 2015-07-01 |
| 策略分配 | 2016-12-01 (2017-06-01-preview) |
| 策略定义 | 2016-12-01 |
| 提供程序操作 | 2015-07-01-preview |
| 角色分配 | 2015-07-01 |
| 角色定义 | 2015-07-01 |

### <a name="microsoftcommerce"></a>Microsoft.Commerce

| 资源类型 | API 版本 |
|----------------------------------|----------------------|
| 委托的提供程序订阅 | 2015-06-01 - preview |
| 委托的使用情况聚合 | 2015-06-01 - preview |
| 估算资源开支 | 2015-06-01 - preview |
| 操作 | 2015-06-01 - preview |
| 订阅方使用情况聚合 | 2015-06-01 - preview |
| 使用情况聚合 | 2015-06-01 - preview |

### <a name="microsoftcompute"></a>Microsoft.Compute

使用 Azure 计算 API 可通过编程方式访问虚拟机及其支持资源。 有关详细信息，请参阅 [Azure 计算](https://docs.microsoft.com/en-us/rest/api/compute/)。

| 资源类型 | API 版本 |
|---------------------------------------------------------------|-------------|
| 可用性集 | 2016-03-30 |
| 位置 | 2016-03-30 |
| 位置/操作 | 2016-03-30 |
| 位置/发布者 | 2016-03-30 |
| 位置/使用情况 | 2016-03-30 |
| 位置/VM 大小 | 2016-03-30 |
| 操作 | 2016-03-30 |
| 虚拟机 | 2016-03-30 |
| 虚拟机/扩展 | 2016-03-30 |
| 虚拟机规模集 | 2016-03-30 |
| 虚拟机规模集/扩展 | 2016-03-30 |
| 虚拟机规模集/网络接口 | 2016-03-30 |
| 虚拟机规模集/虚拟机 | 2016-03-30 |
| 虚拟机规模集/虚拟机/网络接口 | 2016-03-30 |

### <a name="microsoftgallery"></a>Microsoft.Gallery

| 资源类型 | API 版本 |
|------------------|-------------|
| 特选 | 2015-04-01 |
| 特选内容 | 2015-04-01 |
| 特选摘录 | 2015-04-01 |
| 库项 | 2015-04-01 |
| 操作 | 2015-04-01 |
| 门户 | 2015-04-01 |
| 搜索 | 2015-04-01 |
| 建议 | 2015-04-01 |

### <a name="microsoftinsights"></a>Microsoft.Insights

| 资源类型 | API 版本 |
|--------------------|--------------------|
| 警报规则 | 2016-03-01 |
| 事件类别 | 2017-03-01-preview |
| 事件类型 | 2017-03-01-preview |
| 指标定义 | 2016-03-01 |
| 操作 | 2015-04-01 |

### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

管理 Key Vault，以及 Key Vault 中的密钥、机密和证书。 有关详细信息，请参阅 [Azure Key Vault REST API 参考](https://docs.microsoft.com/rest/api/keyvault/)。

| 资源类型 | API 版本 |
|-------------------------|--------------|
| 操作 | 2016-10-01 |
| 保管库 | 2016-10-01 |
| 保管库/访问策略 | 2016-10-01 |
| 保管库/机密 | 2016-10-01 |

### <a name="microsoftkeyvaultadmin"></a>Microsoft.Keyvault.Admin

管理 Key Vault，以及 Key Vault 中的密钥、机密和证书。 有关详细信息，请参阅 [Azure Key Vault REST API 参考](https://docs.microsoft.com/rest/api/keyvault/)。

| 资源类型 | API 版本 |
|------------------|--------------------|
| 位置 | 2017-02-01-preview |
| 位置/配额 | 2017-02-01-preview |

### <a name="microsoftnetwork"></a>Microsoft.Network

操作调用结果是可用网络云操作列表的表示形式。 有关详细信息，请参阅[操作 REST API](https://docs.microsoft.com/rest/api/operation/)。

| 资源类型 | API 版本 |
|---------------------------|--------------|
| 连接 | 2015-06-15 |
| DNS 区域 | 2016-04-01 |
| 负载均衡器 | 2015-06-15 |
| 本地网络网关 | 2015-06-15 |
| 位置 | 2016-04-01 |
| 位置/操作结果 | 2016-04-01 |
| 位置/操作 | 2016-04-01 |
| 位置/使用情况 | 2016-04-01 |
| 网络接口 | 2015-06-15 |
| 网络安全组 | 2015-06-15 |
| 操作 | 2015-06-15 |
| 公共 IP 地址 | 2015-06-15 |
| 路由表 | 2015-06-15 |
| 虚拟网络网关 | 2015-06-15 |
| 虚拟网络 | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

使用 Azure 资源管理器可以部署和管理 Azure 解决方案的基础结构。 可在资源组中组织相关的资源，并使用 JSON 模板部署资源。 有关使用资源管理器部署和管理资源的简介，请参阅 [Azure 资源管理器概述](/azure-resource-manager/resource-group-overview)。

| 资源类型 | API 版本 |
|-----------------------------------------|-------------------|
| 应用程序注册 | 2015-01-01 |
| 检查资源名称 | 2015-012016-09-01 |
| 委托的提供程序 | 2015-01-01 |
| 委托的提供程序/产品（服务） | 2015-01-01 |
| 委托的提供程序/产品（服务）/估算价格 | 2015-01-01 |
| 部署 | 2016-0209-01 |
| 部署/操作 | 2016-0209-01 |
| 扩展元数据 | 2015-01-01 |
| 链接 | 2015-012016-09-01 |
| 位置 | 2015-01-01 |
| 产品 | 2015-01-01 |
| 操作 | 2015-01-01 |
| 提供程序 | 2015-012017-08-01 |
| 资源组 | 2015-012016-09-01 |
| 资源 | 2015-012016-09-01 |
| 订阅 | 2015-012016-09-01 |
| 订阅/位置 | 2015-012016-09-01 |
| 订阅/操作结果 | 2015-012016-09-01 |
| 订阅/提供程序 | 2015-012017-08-01 |
| 订阅/资源组 | 2015-012016-09-01 |
| 订阅/资源组/资源 | 2015-012016-09-01 |
| 订阅/资源 | 2015-012016-09-01 |
| 订阅/标记名称 | 2016-0609-01 |
| 订阅/标记名称/标记值 | 2016-0609-01 |
| 租户 | 2015-012017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage 

使用存储资源提供程序 (SRP) 可通过编程方式管理存储帐户和密钥。 有关详细信息，请参阅 [Azure 存储资源提供程序 REST API 参考](https://docs.microsoft.com/rest/api/storagerp/)。

| 资源类型 | API 版本 |
|-------------------------|--------------|
| 检查名称可用性 | 2016-01-01 |
| 位置 | 2016-01-01 |
| 位置/配额 | 2016-01-01 |
| 操作 | 2016-01-01 |
| 存储帐户 | 2016-01-01 |
| 用途 | 2016-01-01 |

## <a name="next-steps"></a>后续步骤

- [安装适用于 Azure Stack 的 PowerShell](azure-stack-powershell-install.md)
- [配置 Azure Stack 用户的 PowerShell 环境](azure-stack-powershell-configure-user.md)  

