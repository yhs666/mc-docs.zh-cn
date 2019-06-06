---
title: Azure Stack 1904 的已知问题 | Microsoft Docs
description: 了解 Azure Stack 1904 的已知问题。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/10/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: hectorl
ms.lastreviewed: 05/10/2019
ms.openlocfilehash: 186de021a53801a85eab15a69c288d9b435f9291
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249155"
---
# <a name="azure-stack-1904-known-issues"></a>Azure Stack 1904 的已知问题

本文列出了 Azure Stack 版本 1904 中的已知问题。 每当发现新的问题，此列表就会更新。

> [!IMPORTANT]  
> 在应用更新之前，请先查看本部分。

## <a name="update-process"></a>更新过程

- 适用于：此问题适用于所有支持的版本。
- 原因：尝试安装 Azure Stack 更新时，更新状态可能会显示失败并更改为 **PreparationFailed**。 这是因为更新资源提供程序 (URP) 无法正确将文件从存储容器传输到内部基础结构共享进行处理。
- 补救措施：从版本 1901 (1.1901.0.95) 开始，可以通过再次单击“立即更新”（而不是“恢复”）来解决此问题。   然后，URP 会清理上次尝试更新时下载的文件，并重新开始下载。
- 发生次数：常见

## <a name="portal"></a>门户

### <a name="add-on-plans"></a>附加计划

- 适用于：此问题适用于所有支持的版本。
- 原因：即使从用户订阅中删除计划，也无法删除作为附加计划添加到用户订阅的计划。 该计划将一直保留，直到引用附加计划的订阅也被删除。
- 补救措施：无缓解措施。
- 发生次数：常见

### <a name="administrative-subscriptions"></a>管理订阅

- 适用于：此问题适用于所有支持的版本。
- 原因：不应使用版本 1804 中引入的两个管理订阅。 这两种订阅类型为“计量订阅”和“消耗订阅”。  
- 补救措施：这些订阅将从版本 1905 开始暂停使用，最终将被删除。 如果有资源在这两个订阅上运行，请在版本 1905 之前的用户订阅中重新创建这些资源。
- 发生次数：常见

### <a name="subscription-resources"></a>订阅资源

- 适用于：此问题适用于所有支持的版本。
- 原因：删除用户订阅生成孤立的资源。
- 补救措施：先删除用户资源或整个资源组，然后再删除用户订阅。
- 发生次数：常见

### <a name="subscription-permissions"></a>订阅权限

- 适用于：此问题适用于所有支持的版本。
- 原因：无法使用 Azure Stack 门户查看订阅的权限。
- 补救措施：使用 [PowerShell 验证权限](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermroleassignment)。
- 发生次数：常见

### <a name="marketplace-management"></a>市场管理

- 适用于：此问题适用于版本 1904。
- 原因：登录到管理员门户时未显示市场管理屏幕。
- 补救措施：刷新浏览器。
- 发生次数：间歇性

### <a name="upload-blob"></a>上传 blob

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中尝试使用“OAuth(preview)”选项上传 Blob 时，任务将会失败并出现错误消息。
- 补救措施：使用 SAS 选项上传 Blob。
- 发生次数：常见

## <a name="networking"></a>网络

### <a name="load-balancer"></a>负载均衡器

#### <a name="add-backend-pool"></a>添加后端池

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户尝试将**后端池**添加到**负载均衡器**时，该操作失败并出现错误消息“无法更新负载均衡器...”。 
- 补救措施：使用 PowerShell、CLI 或资源管理器模板将后端池关联到负载均衡器资源。
- 发生次数：常见

#### <a name="create-inbound-nat"></a>创建入站 NAT

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户尝试为**负载均衡器**创建**入站 NAT 规则**时，该操作失败并出现错误消息“无法更新负载均衡器...”。 
- 补救措施：使用 PowerShell、CLI 或资源管理器模板将后端池关联到负载均衡器资源。
- 发生次数：常见

#### <a name="create-load-balancer"></a>创建负载均衡器

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“创建负载均衡器”窗口显示一个用于创建“标准”负载均衡器 SKU 的选项。   Azure Stack 不支持此选项。
- 补救措施：改用“基本”负载均衡器选项。
- 发生次数：常见

#### <a name="public-ip-address"></a>公共 IP 地址

- 适用于：此问题适用于所有支持的版本。
- 原因：在用户门户中，“创建公共 IP 地址”窗口显示一个用于创建“标准”SKU 的选项。   Azure Stack 不支持“标准”SKU。 
- 补救措施：对公共 IP 地址改用“基本”SKU。
- 发生次数：常见

## <a name="compute"></a>计算

### <a name="vm-boot-diagnostics"></a>VM 启动诊断

- 适用于：此问题适用于所有支持的版本。
- 原因：创建新的 Windows 虚拟机 (VM) 时，可能会显示以下错误：**无法启动虚拟机 'vm-name'。错误：无法更新 VM 'vm-name' 的串行输出设置**。
如果在 VM 上启用了启动诊断，但删除了启动诊断存储帐户，则会发生该错误。
- 补救措施：使用以前所用的相同名称重新创建存储帐户。
- 发生次数：常见

### <a name="virtual-machine-scale-set"></a>虚拟机规模集

#### <a name="centos"></a>CentOS

- 适用于：此问题适用于所有支持的版本。
- 原因：虚拟机规模集 (VMSS) 创建体验提供基于 CentOS 的版本 7.2 作为部署选项。 Azure Stack 不提供 CentOS 7.2。
- 补救措施：为部署选择另一操作系统，或者使用一个 Azure 资源管理器模板，指定另一个已在部署之前由操作员从市场下载的 CentOS 映像。
- 发生次数：常见

#### <a name="remove-scale-set"></a>删除规模集

- 适用于：此问题适用于所有支持的版本。
- 原因：无法从“虚拟机规模集”边栏选项卡中删除规模集。 
- 补救措施：选择要删除的规模集，然后在“概述”窗格中单击“删除”按钮。  
- 发生次数：常见

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH 访问

- 适用于：此问题适用于所有支持的版本。
- 原因：如果使用创建时已启用 SSH 授权的 Ubuntu 18.04 VM，则无法使用 SSH 密钥登录。
- 补救措施：在预配后使用针对 Linux 扩展的 VM 访问权限来实现 SSH 密钥，或者使用基于密码的身份验证。
- 发生次数：常见

### <a name="compute-host-agent-alert"></a>计算主机代理警报

- 适用于：这是版本 1904 中的一个新问题。
- 原因：重启缩放单元中的节点后，出现“计算主机代理”警告。 重启操作会更改计算主机代理服务的默认启动设置。
- 补救措施：
  - 可以忽略此警报。 代理无响应并不会影响操作员和用户的操作或用户应用程序。 如果手动关闭此警报，它会在 24 小时后再次出现。
  - Microsoft 支持人员可以通过更改服务的启动设置来修正此问题。 这需要开具支持票证。 如果再次重启节点，会出现新的警报。
- 发生次数：常见

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->

## <a name="next-steps"></a>后续步骤

- [查看更新活动清单](azure-stack-release-notes-checklist.md)
- [查看安全更新列表](azure-stack-release-notes-security-updates-1904.md)
