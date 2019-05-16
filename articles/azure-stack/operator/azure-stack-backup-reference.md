---
title: Azure Stack 基础结构备份服务参考 | Microsoft Docs
description: 本文包含 Azure Stack 基础结构备份服务的参考资料。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/12/2019
ms.date: 03/04/2019
ms.author: v-jay
ms.reviewer: hectorl
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 24b2f3d95e0cda56066347ed64f6418cfb971390
ms.sourcegitcommit: 05aa4e4870839a3145c1a3835b88cf5279ea9b32
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2019
ms.locfileid: "64529590"
---
# <a name="infrastructure-backup-service-reference"></a>基础结构备份服务参考

## <a name="azure-backup-infrastructure"></a>Azure 备份基础结构

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

Azure Stack 由许多服务构成，其中包括门户、Azure 资源管理器和基础结构管理体验。 Azure Stack 的设备类管理体验侧重于降低解决方案操作员所面临的复杂性。

基础结构备份设计用来在内部解决为基础结构服务备份和还原数据时所面临的复杂性，从而确保操作员可以集中精力管理解决方案并维护对用户的 SLA。

为避免将备份存储在同一系统上，要求将备份数据导出到外部共享。 要求使用外部共享使得管理员可以根据现有的公司业务连续性/灾难恢复策略来灵活地决定要将数据存储在何处。 

### <a name="infrastructure-backup-components"></a>基础结构备份组件

基础结构备份包括以下组件：

 - **基础结构备份控制器**  
 基础结构备份控制器随每个 Azure Stack 云一起实例化并驻留在其中。
 - **备份资源提供程序**  
 备份资源提供程序（备份 RP）包括了用户界面和应用程序编程接口 (API) 来公开 Azure Stack 基础结构的基本备份功能。

#### <a name="infrastructure-backup-controller"></a>基础结构备份控制器

基础结构备份控制器是为 Azure Stack 云实例化的一项 Service Fabric 服务。 备份资源是在区域级别创建的，并且从 AD、CA、Azure 资源管理器、CRP、SRP、NRP、KeyVault 和 RBAC 捕获特定于区域的服务数据。 

### <a name="backup-resource-provider"></a>备份资源提供程序

备份资源提供程序在 Azure Stack 门户中提供了用于进行基本配置并列出备份资源的用户界面。 操作员可以在用户界面中执行以下操作：

 - 通过提供外部存储位置、凭据和加密密钥首次启用备份
 - 查看已完成创建的备份资源和正在创建的资源
 - 修改备份控制器在其中放置备份数据的存储位置
 - 修改备份控制器用来访问外部存储位置的凭据
 - 修改备份控制器用来加密备份的加密密钥 


## <a name="backup-controller-requirements"></a>备份控制器要求

本部分介绍了基础结构备份的重要要求。 建议在为 Azure Stack 实例启用备份之前仔细查看此信息，并且在进行部署和执行后续操作的过程中按需重新参阅。

这些要求包括：

  - **软件要求** - 介绍了支持的存储位置和大小调整指南。 
  - **网络要求** - 介绍了不同存储位置的网络要求。  

### <a name="software-requirements"></a>软件要求

#### <a name="supported-storage-locations"></a>支持的存储位置

| 存储位置                                                                 | 详细信息                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 在可信网络环境中的存储设备上托管的 SMB 文件共享 | 位于部署了 Azure Stack 的数据中心内或位于其他数据中心内的 SMB 共享。 多个 Azure Stack 实例可以使用同一个文件共享。 |
| Azure 上的 SMB 文件共享                                                          | 目前不支持。                                                                                                                                 |
| Azure 上的 Blob 存储                                                            | 目前不支持。                                                                                                                                 |

#### <a name="supported-smb-versions"></a>支持的 SMB 版本

| SMB | 版本 |
|-----|---------|
| SMB | 3.x     |

#### <a name="storage-location-sizing"></a>存储位置大小调整 

基础结构备份控制器将按需备份数据。 建议每天至少进行两次备份，并且保留最多七天的备份。 

**至少 1811**

| 环境规模 | 预计的备份大小 | 所需的空间总量 |
|-------------------|--------------------------|--------------------------------|
| 4-16 个节点        | 20 GB                    | 280 GB                        |
| ASDK              | 10 GB                    | 140 GB                        |

**Pre-1811**

| 环境规模 | 预计的备份大小 | 所需的空间总量 |
|-------------------|--------------------------|--------------------------------|
| 4-16 个节点，ASDK  | 10 GB                     | 140 GB                        |

### <a name="network-requirements"></a>网络要求

| 存储位置                                                                 | 详细信息                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 在可信网络环境中的存储设备上托管的 SMB 文件共享 | 如果 Azure Stack 实例驻留在具有防火墙的环境中，则端口 445 是必需的。 基础结构备份控制器将通过端口 445 启动到 SMB 文件服务器的连接。 |
| 若要使用文件服务器的 FQDN，必须可以通过 PEP 解析该名称             |                                                                                                                                                                                         |

> [!Note]  
> 无需打开任何入站端口。

### <a name="encryption-requirements"></a>加密要求

从 1901 版本开始，基础结构备份服务将在执行云恢复期间，使用包含公钥的证书 (.CER) 来加密备份数据，并使用包含私钥的证书 (.PFX) 来解密备份数据。   
 - 该证书用于传输密钥，而不会用于建立经过身份验证的安全通信。 出于此原因，该证书可以是自签名证书。 Azure Stack 无需验证此证书的根或信任，因此无需外部 Internet 访问权限。

自签名证书有两个部分，一个部分包含公钥，另一个部分包含私钥：
 - 加密备份数据：包含公钥的证书（导出到 .CER 文件）用于加密备份数据
 - 解密备份数据：包含私钥的证书（导出到 .PFX 文件）用于解密备份数据

内部机密轮换不会管理包含公钥的证书 (.CER)。 若要轮换证书，需要创建新的自签名证书，并使用新的文件 (.CER) 更新备份设置。  
 - 所有现有备份将使用以前的公钥保持加密状态。 新备份将使用新的公钥。 

出于安全原因，Azure Stack 不会保留云恢复期间使用的包含私钥的证书 (.PFX)。 在云恢复期间，需要显式提供此文件。  

**后向兼容性模式**从 1901 版本开始，加密密钥支持已弃用，将来的版本会将其删除。 如果已从 1811 版本更新，并且已使用加密密钥启用了备份，则 Azure Stack 将继续使用加密密钥。 至少有 3 个版本会继续支持后向兼容性模式。 在此之后，需要使用证书。 
 * 从加密密钥更新到证书是单向操作。  
 * 所有现有备份将使用加密密钥保持加密状态。 新备份将使用证书。 

## <a name="infrastructure-backup-limits"></a>基础结构备份限制

在计划、部署和操作 Azure Stack 实例时请考虑这些限制。 下表介绍了这些限制。

### <a name="infrastructure-backup-limits"></a>基础结构备份限制

| 限制标识符                                                 | 限制        | 注释                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| 备份类型                                                      | 仅限完整    | 基础结构备份控制器仅支持完整备份。 不支持增量备份。                                          |
| 计划的备份                                                | 计划和手动  | 备份控制器支持计划备份和按需备份                                                                                 |
| 最大并发备份作业数                                   | 1            | 备份控制器的每个实例仅支持一个活动备份作业。                                                                  |
| 网络交换机配置                                     | 不在范围内 | 管理员必须使用 OEM 工具备份网络交换机配置。 请参阅每个 OEM 供应商提供的 Azure Stack 文档。 |
| 硬件生命周期主机                                          | 不在范围内 | 管理员必须使用 OEM 工具备份硬件生命周期主机。 请参阅每个 OEM 供应商提供的 Azure Stack 文档。      |
| 最大文件共享数                                    | 1            | 只能使用一个文件共享来存储备份数据                                                                                        |
| 备份应用服务、函数、SQL、mysql 资源提供程序数据 | 不在范围内 | 请参阅已发布的用于部署和管理由 Microsoft 创建的增值 RP 的指南。                                                  |
| 备份第三方资源提供程序                              | 不在范围内 | 请参阅已发布的用于部署和管理由第三方供应商创建的增值 RP 的指南。                                          |

## <a name="next-steps"></a>后续步骤

 - 若要详细了解基础结构备份服务，请参阅[使用基础结构备份服务对 Azure Stack 进行备份和数据恢复](azure-stack-backup-infrastructure-backup.md)。

<!-- Update_Description: update metedata properties -->