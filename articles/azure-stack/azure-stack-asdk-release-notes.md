---
title: "Azure Stack 开发工具包发行说明 | Microsoft Docs"
description: "Azure Stack 开发工具包的改进、修复和已知问题。"
services: azure-stack
documentationcenter: 
author: andredm7
manager: femila
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/05/2018
ms.date: 03/01/2018
ms.author: v-junlch
ms.openlocfilehash: 6bb5cd4e346dd873bdc305d5c8103589cda6cc2e
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure Stack 开发工具包发行说明

*适用于：Azure Stack 开发工具包*

这些发行说明提供 Azure Stack 开发工具包的改进、修复和已知问题的相关信息。 如果不确定所运行的版本，可以[使用门户检查版本](azure-stack-updates.md#determine-the-current-version)。

## <a name="build-201801032"></a>内部版本 20180103.2

### <a name="new-features-and-fixes"></a>新功能和修复

- 请参阅 Azure Stack 集成系统的 Azure Stack 1712 更新版发行说明的[新功能和修复](azure-stack-update-1712.md#new-features-and-fixes)部分。

    > [!IMPORTANT]
    > **新功能和修复**部分所列的某些项仅与 Azure Stack 集成系统相关。

### <a name="known-issues"></a>已知问题
 
#### <a name="deployment"></a>部署
- 在部署期间，必须使用 IP 地址指定时间服务器。

#### <a name="infrastructure-management"></a>基础结构管理
- 请不要在“基础架构备份”边栏选项卡上启用基础结构备份。
- 缩放单位节点的概要信息中不显示基础板管理控制器 (BMC) IP 地址和模型。 这是 Azure Stack 开发工具包中的预期行为。

#### <a name="portal"></a>门户
- 可能会在门户中看到空白的仪表板。 若要恢复仪表板，请选择门户右上角的齿轮图标，然后选择“还原默认设置”。
- 查看资源组的属性时，“移动”按钮已禁用。 这是预期的行为。 目前不支持在订阅之间移动资源组。
-  对于可在下拉列表中选择订阅、资源组或位置的任何工作流，可能会遇到一个或多个以下问题：

   - 可能会在列表顶部看到一个空白行。 应该仍能按预期选择项。
   - 如果下拉列表中的项列表很短，可能无法查看任何项名称。
   - 如果有多个用户订阅，资源组的下拉列表可能是空的。 

   若要解决后两个问题，可以输入订阅或资源组的名称（如果知道），或者可以改用 PowerShell。

- 看到“需要激活”警告警报，提示注册 Azure Stack 开发工具包。 这是预期的行为。
- 如果在任何“基础结构角色”警报中单击“组件”链接，生成的“概述”边栏选项卡会尝试加载但失败。 此外，“概述”边栏选项卡不会超时。
- 删除用户订阅生成孤立的资源。 解决方法是先删除用户资源或整个资源组，然后再删除用户订阅。
- 无法使用 Azure Stack 门户查看订阅的权限。 解决方法是使用 PowerShell 验证权限。
 
#### <a name="marketplace"></a>应用商店
- 出于兼容性考虑，此版本中将会删除一些 Marketplace 项。 在进一步验证后，会重新启用这些项。
- 用户无需订阅就能浏览整个 Marketplace，并且将会看到计划和产品等管理项。 对用户而言，这些项是非功能性的。
 
#### <a name="compute"></a>计算
- 用户可以使用相应的选项创建包含异地冗余存储的虚拟机。 此配置会导致虚拟机创建失败。 
- 可以配置只包含一个容错域和一个更新域的虚拟机可用性集。
- 没有任何可用于创建虚拟机规模集的 Marketplace 体验。 可以使用模板来创建规模集。
- 无法在门户中使用虚拟机规模集的缩放设置。 解决方法是使用 [Azure PowerShell](/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set)。 由于 PowerShell 版本差异，必须使用 `-Name` 参数，而不是 `-VMScaleSetName`。

#### <a name="networking"></a>网络
- 无法使用门户创建采用公共 IP 地址的负载均衡器。 解决方法是使用 PowerShell 创建负载均衡器。
- 创建网络负载均衡器时，必须创建网络地址转换 (NAT) 规则。 否则，在创建负载均衡器之后尝试添加 NAT 规则时会收到错误。
- 如果在“网络”下单击“连接”来设置 VPN 连接，会列出“VNet 到 VNet”作为可能的连接类型。 请不要选择此选项。 目前，仅支持“站点到站点(IPsec)”选项。
- 创建 VM 并与公共 IP 地址创建关联之后，无法取消该 IP 地址与虚拟机 (VM) 的关联。 取消关联看似正常运行，但以前分配的公共 IP 地址仍与原始 VM 相关联。 即使将 IP 地址重新分配给新的 VM（通常名为“VIP 交换”），也还会发生这种行为。 以后尝试通过此 IP 地址建立连接都会导致连接到原先关联的 VM，而不是新的 VM。 目前只有在创建新的 VM 时，才能使用新的公共 IP 地址。
- Azure Stack 操作员可能无法部署、删除、修改 VNET 或网络安全组。 此问题主要出现于同一个包的后续更新尝试。 此问题的原因是发生了目前正在调查的更新打包问题。
- 内部负载均衡 (ILB) 对 MAC 地址的后端 VM 进行不恰当的处理，导致 Linux 实例损坏。
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- 最长可能需要在一小时后，租户才能在新的 SQL 或 MySQL SKU 中创建数据库。 
- 不支持在 SQL 和 MySQL 托管服务器中直接创建不是由资源提供程序执行的项，这可能导致不匹配的状态。

#### <a name="app-service"></a>应用服务
- 在订阅中创建第一个 Azure 函数之前，用户必须先注册存储资源提供程序。
 
#### <a name="usage-and-billing"></a>使用情况和计费
- 公共 IP 地址使用计量数据针对每条记录显示相同的 *EventDateTime* 值，而不是创建记录时显示的 *TimeDate* 时间戳。 目前，无法使用此数据来执行公共 IP 地址用量的准确计帐。

#### <a name="identity"></a>标识

在已部署 Azure Active Directory 联合身份身份验证服务 (ADFS) 的环境中，**azurestack\azurestackadmin** 帐户不再是默认提供程序订阅的所有者。 如果不使用 **azurestack\azurestackadmin** 登录到**管理门户/adminmanagement 终结点**，可以使用 **azurestack\cloudadmin** 帐户，以便可以管理和使用默认提供程序订阅。

> [!IMPORTANT]
> 即使 **azurestack\cloudadmin** 帐户是 ADFS 部署环境中默认提供程序订阅的所有者，它也没有通过 RDP 连接到主机的权限。 根据需要继续使用 **azurestack\azurestackadmin** 帐户或本地管理员帐户来登录、访问和管理主机。

## <a name="build-201711221"></a>内部版本 20171122.1

### <a name="new-features-and-fixes"></a>新功能和修复

- 请参阅 Azure Stack 集成系统的 Azure Stack 1711 更新版发行说明的[新功能和修复](azure-stack-update-1711.md#new-features-and-fixes)部分。

    > [!IMPORTANT]
    > **新功能和修复**部分所列的某些项仅与 Azure Stack 集成系统相关。

### <a name="known-issues"></a>已知问题
 
#### <a name="deployment"></a>部署
- 在部署期间，必须使用 IP 地址指定时间服务器。

#### <a name="infrastructure-management"></a>基础结构管理
- 请不要在“基础架构备份”边栏选项卡上启用基础结构备份。
- 缩放单位节点的概要信息中不显示基础板管理控制器 (BMC) IP 地址和模型。 这是 Azure Stack 开发工具包中的预期行为。

#### <a name="portal"></a>门户
- 可能会在门户中看到空白的仪表板。 若要恢复仪表板，请选择门户右上角的齿轮图标，然后选择“还原默认设置”。
- 查看资源组的属性时，“移动”按钮已禁用。 这是预期的行为。 目前不支持在订阅之间移动资源组。
-  对于可在下拉列表中选择订阅、资源组或位置的任何工作流，可能会遇到一个或多个以下问题：

   - 可能会在列表顶部看到一个空白行。 应该仍能按预期选择项。
   - 如果下拉列表中的项列表很短，可能无法查看任何项名称。
   - 如果有多个用户订阅，资源组的下拉列表可能是空的。 

   若要解决后两个问题，可以输入订阅或资源组的名称（如果知道），或者可以改用 PowerShell。

- 看到“需要激活”警告警报，提示注册 Azure Stack 开发工具包。 这是预期的行为。
- 如果在任何“基础结构角色”警报中单击“组件”链接，生成的“概述”边栏选项卡会尝试加载但失败。 此外，“概述”边栏选项卡不会超时。
- 删除用户订阅生成孤立的资源。 解决方法是先删除用户资源或整个资源组，然后再删除用户订阅。
- 无法使用 Azure Stack 门户查看订阅的权限。 解决方法是使用 PowerShell 验证权限。
 
#### <a name="marketplace"></a>应用商店
- 尝试使用“从 Azure 添加”选项将项添加到 Azure Stack Marketplace 时，可能无法看到所有可供下载的项。
- 用户无需订阅就能浏览整个 Marketplace，并且将会看到计划和产品等管理项。 对用户而言，这些项是非功能性的。
 
#### <a name="compute"></a>计算
- 用户可以使用相应的选项创建包含异地冗余存储的虚拟机。 此配置会导致虚拟机创建失败。 
- 可以配置只包含一个容错域和一个更新域的虚拟机可用性集。
- 没有任何可用于创建虚拟机规模集的 Marketplace 体验。 可以使用模板来创建规模集。
- 无法在门户中使用虚拟机规模集的缩放设置。 解决方法是使用 [Azure PowerShell](/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set)。 由于 PowerShell 版本差异，必须使用 `-Name` 参数，而不是 `-VMScaleSetName`。

#### <a name="networking"></a>网络
- 无法使用门户创建采用公共 IP 地址的负载均衡器。 解决方法是使用 PowerShell 创建负载均衡器。
- 创建网络负载均衡器时，必须创建网络地址转换 (NAT) 规则。 否则，在创建负载均衡器之后尝试添加 NAT 规则时会收到错误。
- 如果在“网络”下单击“连接”来设置 VPN 连接，会列出“VNet 到 VNet”作为可能的连接类型。 请不要选择此选项。 目前，仅支持“站点到站点(IPsec)”选项。
- 创建 VM 并与公共 IP 地址创建关联之后，无法取消该 IP 地址与虚拟机 (VM) 的关联。 取消关联看似正常运行，但以前分配的公共 IP 地址仍与原始 VM 相关联。 即使将 IP 地址重新分配给新的 VM（通常名为“VIP 交换”），也还会发生这种行为。 以后尝试通过此 IP 地址建立连接都会导致连接到原先关联的 VM，而不是新的 VM。 目前只有在创建新的 VM 时，才能使用新的公共 IP 地址。
- Azure Stack 操作员可能无法部署、删除、修改 VNET 或网络安全组。 此问题主要出现于同一个包的后续更新尝试。 此问题的原因是发生了目前正在调查的更新打包问题。
- 内部负载均衡 (ILB) 对 MAC 地址的后端 VM 进行不恰当的处理，导致 Linux 实例损坏。
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- 最长可能需要在一小时后，租户才能在新的 SQL 或 MySQL SKU 中创建数据库。 
- 不支持在 SQL 和 MySQL 托管服务器中直接创建不是由资源提供程序执行的项，这可能导致不匹配的状态。

    > [!NOTE]
    > 分别参阅 [SQL](/azure-stack/azure-stack-sql-resource-provider-deploy) 和 [MySQL](/azure-stack/azure-stack-mysql-resource-provider-deploy) 安装文章，获取版本兼容性指南。

#### <a name="app-service"></a>应用服务
- 在订阅中创建第一个 Azure 函数之前，用户必须先注册存储资源提供程序。
 
#### <a name="usage-and-billing"></a>使用情况和计费
- 公共 IP 地址使用计量数据针对每条记录显示相同的 *EventDateTime* 值，而不是创建记录时显示的 *TimeDate* 时间戳。 目前，无法使用此数据来执行公共 IP 地址用量的准确计帐。

#### <a name="identity"></a>标识

在已部署 Azure Active Directory 联合身份身份验证服务 (ADFS) 的环境中，**azurestack\azurestackadmin** 帐户不再是默认提供程序订阅的所有者。 如果不使用 **azurestack\azurestackadmin** 登录到**管理门户/adminmanagement 终结点**，可以使用 **azurestack\cloudadmin** 帐户，以便可以管理和使用默认提供程序订阅。

> [!IMPORTANT]
> 即使 **azurestack\cloudadmin** 帐户是 ADFS 部署环境中默认提供程序订阅的所有者，它也没有通过 RDP 连接到主机的权限。 根据需要继续使用 **azurestack\azurestackadmin** 帐户或本地管理员帐户来登录、访问和管理主机。


## <a name="build-201710201"></a>内部版本 20171020.1

### <a name="improvements-and-fixes"></a>改进和修复

若要查看 20171020.1 内部版本的改进和修复列表，请参阅 Azure Stack 集成系统 1710 发行说明的[改进和修复](azure-stack-update-1710.md#improvements-and-fixes)部分。 “其他质量改进和修复”部分所列的一些项仅与集成系统相关。

此外，已做出以下修复：
- 修复了计算资源提供程序显示未知状态的问题。
- 修复了在创建配额，并随后尝试查看计划详细信息时，管理员门户中可能不显示配额的问题。

### <a name="known-issues"></a>已知问题

#### <a name="powershell"></a>PowerShell
- AzureRM 1.2.11 PowerShell 模块版本随附了重大更改列表。 
 
#### <a name="deployment"></a>部署
- 在部署期间，必须使用 IP 地址指定时间服务器。

#### <a name="infrastructure-management"></a>基础结构管理
- 请不要在“基础架构备份”边栏选项卡上启用基础结构备份。
- 缩放单位节点的概要信息中不显示基础板管理控制器 (BMC) IP 地址和模型。 这是 Azure Stack 开发工具包中的预期行为。

#### <a name="portal"></a>门户
- 可能会在门户中看到空白的仪表板。 若要恢复仪表板，请选择门户右上角的齿轮图标，然后选择“还原默认设置”。
- 查看资源组的属性时，“移动”按钮已禁用。 这是预期的行为。 目前不支持在订阅之间移动资源组。
-  对于可在下拉列表中选择订阅、资源组或位置的任何工作流，可能会遇到一个或多个以下问题：

   - 可能会在列表顶部看到一个空白行。 应该仍能按预期选择项。
   - 如果下拉列表中的项列表很短，可能无法查看任何项名称。
   - 如果有多个用户订阅，资源组的下拉列表可能是空的。 

   若要解决后两个问题，可以输入订阅或资源组的名称（如果知道），或者可以改用 PowerShell。

- 看到“需要激活”警告警报，提示注册 Azure Stack 开发工具包。 这是预期的行为。
- 在“需要激活”警告警报详细信息中，请不要单击 **AzureBridge** 组件的链接。 否则，“概述”边栏选项卡不会成功加载，且不超时。
- 在管理员门户中，可能会在“通知”区域中看到“提取租户时出错”错误。 可以放心地忽略此错误。
- 删除用户订阅生成孤立的资源。 解决方法是先删除用户资源或整个资源组，然后再删除用户订阅。
- 无法使用 Azure Stack 门户查看订阅的权限。 解决方法是使用 PowerShell 验证权限。
 
#### <a name="marketplace"></a>应用商店
- 尝试使用“从 Azure 添加”选项将项添加到 Azure Stack Marketplace 时，可能无法看到所有可供下载的项。
- 用户无需订阅就能浏览整个 Marketplace，并且将会看到计划和产品等管理项。 对用户而言，这些项是非功能性的。
 
#### <a name="compute"></a>计算
- 用户可以使用相应的选项创建包含异地冗余存储的虚拟机。 此配置会导致虚拟机创建失败。 
- 可以配置只包含一个容错域和一个更新域的虚拟机可用性集。
- 没有任何可用于创建虚拟机规模集的 Marketplace 体验。 可以使用模板来创建规模集。
- 无法在门户中使用虚拟机规模集的缩放设置。 解决方法是使用 [Azure PowerShell](/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set)。 由于 PowerShell 版本差异，必须使用 `-Name` 参数，而不是 `-VMScaleSetName`。

#### <a name="networking"></a>网络
- 无法使用门户创建采用公共 IP 地址的负载均衡器。 解决方法是使用 PowerShell 创建负载均衡器。
- 创建网络负载均衡器时，必须创建网络地址转换 (NAT) 规则。 否则，在创建负载均衡器之后尝试添加 NAT 规则时会收到错误。
- 如果在“网络”下单击“连接”来设置 VPN 连接，会列出“VNet 到 VNet”作为可能的连接类型。 请不要选择此选项。 目前，仅支持“站点到站点(IPsec)”选项。
- 创建 VM 并与公共 IP 地址创建关联之后，无法取消该 IP 地址与虚拟机 (VM) 的关联。 取消关联看似正常运行，但以前分配的公共 IP 地址仍与原始 VM 相关联。 即使将 IP 地址重新分配给新的 VM（通常名为“VIP 交换”），也还会发生这种行为。 以后尝试通过此 IP 地址建立连接都会导致连接到原先关联的 VM，而不是新的 VM。 目前只有在创建新的 VM 时，才能使用新的公共 IP 地址。
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- 最长可能需要在一小时后，租户才能在新的 SQL 或 MySQL SKU 中创建数据库。 
- 不支持在 SQL 和 MySQL 托管服务器中直接创建不是由资源提供程序执行的项，这可能导致不匹配的状态。

#### <a name="app-service"></a>应用服务
- 在订阅中创建第一个 Azure 函数之前，用户必须先注册存储资源提供程序。
 
#### <a name="usage-and-billing"></a>使用情况和计费
- 公共 IP 地址使用计量数据针对每条记录显示相同的 *EventDateTime* 值，而不是创建记录时显示的 *TimeDate* 时间戳。 目前，无法使用此数据来执行公共 IP 地址用量的准确计帐。

