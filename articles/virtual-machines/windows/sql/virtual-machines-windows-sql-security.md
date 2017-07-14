---
title: "Azure 中 SQL Server 的安全注意事项 | Azure"
description: "本主题提供有关保护 Azure 虚拟机中运行的 SQL Server 的常规指南。"
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: d710c296-e490-43e7-8ca9-8932586b71da
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
origin.date: 06/02/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: b403119a4f33e58ec6d433c7fe0c8175889ea749
ms.sourcegitcommit: 51a25dbbf5f32fe524860b1bb107108122b47bf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# Azure 虚拟机中 SQL Server 的安全注意事项
<a id="security-considerations-for-sql-server-in-azure-virtual-machines" class="xliff"></a>

本主题包括总体安全指南，可帮助建立对 Azure 虚拟机 (VM) 中 SQL Server 实例的安全访问。

Azure 遵守多个行业法规和标准，使用户能够使用虚拟机中运行的 SQL Server 生成符合规定的解决方案。 有关 Azure 合规性的信息，请参阅 [Azure 信任中心](https://www.trustcenter.cn/)。

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## 控制对 SQL VM 的访问
<a id="control-access-to-the-sql-vm" class="xliff"></a>

创建 SQL Server 虚拟机时，请考虑如何谨慎控制有权访问虚拟机和 SQL Server 的人员。 通常，应执行以下操作：

- 仅限对此有需要的应用程序和客户端拥有 SQL Server 访问权限。
- 遵循管理用户帐户和密码的最佳做法。

在考虑这些要点时，以下部分提供了相关建议。

## 安全连接
<a id="secure-connections" class="xliff"></a>

创建具有库映像的 SQL Server 虚拟机时，“SQL Server 连接”选项提供以下选择：“本地（VM 内）”、“专用（虚拟网络内）”或“公共 (Internet)”。

![SQL Server 连接](./media/virtual-machines-windows-sql-security/sql-vm-connectivity-option.png)

为了达到最佳安全性，请为方案选择最严格的选项。 例如，如果正在运行访问同一 VM 上的 SQL Server 的应用程序，则“本地”选项最安全。 如果正在运行需要访问 SQL Server 的 Azure 应用程序，选择“专用”选项可确保与 SQL Server 的通信仅在指定的 [Azure 虚拟网络](../../../virtual-network/virtual-networks-overview.md)内安全进行。 如果需要对 SQL Server VM 的“公共” (internest) 访问权限，请确保遵循本主题中的其他最佳做法，降低受到攻击的可能性。

门户中选定的选项对 VM [网络安全组](../../../virtual-network/virtual-networks-nsg.md) (NSG) 使用入站安全规则，允许或拒绝到虚拟机的网络流量。 可修改或创建新的入站 NSG 规则，允许到 SQL Server 端口（默认为 1433）的流量。 还可指定允许通过此端口进行通信的特定 IP 地址。

![网络安全组规则](./media/virtual-machines-windows-sql-security/sql-vm-network-security-group-rules.png)

除了使用 NSG 规则限制网络流量外，还可以对虚拟机使用 Windows 防火墙。

如果通过经典部署模型使用终结点，不使用它们时，请删除虚拟机上的所有终结点。 有关在终结点上使用 ACL 的说明，请参阅[管理终结点上的 ACL](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint)。 对于使用 Resource Manager 的 VM 来说，无需执行此操作。

最后，考虑为 Azure 虚拟机中的 SQL Server 数据库引擎实例启用加密连接。 使用签名证书配置 SQL Server 实例。 有关详细信息，请参阅[启用到数据库引擎的加密连接](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine)和[连接字符串语法](https://msdn.microsoft.com/library/ms254500.aspx)。

## 使用非默认端口
<a id="use-a-non-default-port" class="xliff"></a>

默认情况下，SQL Server 侦听已知端口 1433。 为了提高安全性，请将 SQL Server 配置为侦听 1401 等非默认端口。 如果在 Azure 门户中配置 SQL Server 库映像，则可在“SQL Server 设置”边栏选项卡中指定此端口。

配置后，可通过两种方式进行预配：

- 对于 Resource Manager VM，可以在 VM 概述边栏选项卡中选择“SQL Server配置”。 这提供了更改端口的选项。

  ![在门户中更改 TCP 端口](./media/virtual-machines-windows-sql-security/sql-vm-change-tcp-port.png)

- 对于经典 VM 或未通过门户进行配置的 SQL Server VM，可通过远程连接到 VM 来手动配置端口。 有关配置步骤，请参阅[将服务器配置为侦听特定 TCP 端口](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port)。 如果使用此手动方法，还需添加 Windows 防火墙规则，允许该 TCP 端口上的传入流量。

> [!IMPORTANT]
> 如果 SQL Server 端口对公共 Internet 连接打开，则最好指定非默认端口。

SQL Server 侦听非默认端口时，必须在连接时指定该端口。 例如，考虑下服务器 IP 地址为 13.55.255.255，SQL Server 侦听端口 1401 的情况。 若要连接到 SQL Server，需在连接字符串中指定 `13.55.255.255,1401`。

## 管理帐户
<a id="manage-accounts" class="xliff"></a>

希望攻击者难以猜测帐户名或密码。 使用以下技巧会有所帮助：

- 创建一个唯一的本地管理员帐户，不要命名为 **Administrator**。

- 对所有帐户使用复杂的强密码。 若要深入了解如何创建强密码，请参阅[创建强密码](https://support.microsoft.com/instantanswers/9bd5223b-efbe-aa95-b15a-2fb37bef637d/create-a-strong-password)一文。

- 默认情况下，Azure 在 SQL Server 虚拟机安装期间会选择 Windows 身份验证。 因此，会禁用 **SA** 登录名，并由安装程序分配密码。 建议不要使用或启用 SA 登录名。 如果必须使用 SQL 登录名，请使用以下策略之一：

  - 创建一个名称唯一且具有 sysadmin 成员资格的 SQL 帐户。 可通过在预配期间启用 SQL 身份验证，从门户执行此操作。

    > [!TIP] 
    > 如果预配期间未启用 SQL 身份验证，则必须手动将身份验证模式更改为 SQL Server 和 Windows 身份验证模式。 有关详细信息，请参阅 [更改服务器身份验证模式](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode)。

  - 如果必须使用 SA 登录名，请在预配后启用该登录名，并分配新的强密码。

## 遵循本地最佳做法进行操作
<a id="follow-on-premises-best-practices" class="xliff"></a>

除了本主题中描述的做法之外，建议在适用的情况下查看和实施传统的本地安全操作。 有关详细信息，请参阅[安装 SQL Server 的安全注意事项](https://docs.microsoft.com/sql/sql-server/install/security-considerations-for-a-sql-server-installation)

## 后续步骤
<a id="next-steps" class="xliff"></a>

如果还对性能最佳做法感兴趣，请参阅 [Azure 虚拟机中 SQL Server 的性能最佳做法](virtual-machines-windows-sql-performance.md)。

若要了解与在 Azure VM 中运行 SQL Server 相关的其他主题，请参阅 [Azure 虚拟机上的 SQL Server 概述](virtual-machines-windows-sql-server-iaas-overview.md)。
