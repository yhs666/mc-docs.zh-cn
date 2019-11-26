---
title: Azure Stack 故障排除 | Microsoft Docs
description: Azure Stack 故障排除。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/16/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.reviewer: prchint
ms.lastreviewed: 10/16/2019
ms.openlocfilehash: d072230012008e7c1da7f3aa6f974fd4e60ce475
ms.sourcegitcommit: 7dfb76297ac195e57bd8d444df89c0877888fdb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020032"
---
# <a name="azure-stack-troubleshooting"></a>Azure Stack 故障排除

本文档提供 Azure Stack 集成环境的故障排除信息。 有关 Azure Stack 开发工具包的帮助，请参阅 [ASDK 故障排除](../asdk/asdk-troubleshooting.md)或获取 [Azure Stack MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home)上的专家的帮助。 

## <a name="frequently-asked-questions"></a>常见问题

这些部分包含指向涵盖发送到 Azure 客户支持服务 (CSS) 的常见问题的文档的链接。

### <a name="purchase-considerations"></a>购买注意事项

* [Azure Stack 概述](azure-stack-overview.md)

### <a name="updates-and-diagnostics"></a>更新和诊断

* [如何在 Azure Stack 中使用诊断工具](azure-stack-diagnostics.md)
* [如何验证 Azure Stack 系统状态](azure-stack-diagnostic-test.md)
* [更新包发布频率](azure-stack-servicing-policy.md#update-package-release-cadence)

### <a name="supported-operating-systems-and-sizes-for-guest-vms"></a>来宾 VM 支持的操作系统和大小

* [Azure Stack 上支持的来宾操作系统](azure-stack-supported-os.md)
* [Azure Stack 支持的 VM 大小](../user/azure-stack-vm-sizes.md)

### <a name="azure-marketplace"></a>Azure 市场

* [可供 Azure Stack 使用的 Azure 市场项](azure-stack-marketplace-azure-items.md)

### <a name="manage-capacity"></a>管理容量

#### <a name="memory"></a>内存

若要增加 Azure Stack 的总可用内存容量，可以添加内存。 在 Azure Stack 中，物理服务器也称为缩放单元节点。 属于单个缩放单元的所有缩放单元节点必须具有[相同的内存量](azure-stack-manage-storage-physical-memory-capacity.md)。

#### <a name="retention-period"></a>保留期

云操作员可以使用保留期设置来指定时间间隔天数（0 到 9999 天），在此期间，任何已删除的帐户都有可能能够恢复。 默认保留期设置为 **0** 天。 将值设置为 **0** 表示任何已删除的帐户会立即超出保留期，并标记为定期进行垃圾回收。

* [设置保留期](azure-stack-manage-storage-accounts.md#set-the-retention-period)

### <a name="security-compliance-and-identity"></a>安全性、合规性和标识  

#### <a name="manage-rbac"></a>管理 RBAC

Azure Stack 中的用户可以是订阅、资源组或服务的每个实例的读取者、所有者或参与者。

* [Azure Stack 管理 RBAC](azure-stack-manage-permissions.md)

如果 Azure 资源的内置角色不能满足组织的特定需求，则你可以创建自己的自定义角色。 对于本教程，你将使用 Azure PowerShell 创建名为 Reader Support Tickets 的自定义角色。

* [教程：使用 Azure PowerShell 为 Azure 资源创建自定义角色](/role-based-access-control/tutorial-custom-role-powershell)

### <a name="manage-usage-and-billing-as-a-csp"></a>以 CSP 身份管理使用情况和计费

* [以 CSP 身份管理用量和计费](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)
* [创建 CSP 或 APSS 订阅](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)

选择用于 Azure Stack 的共享服务帐户的类型。 可以用来注册多租户 Azure Stack 的订阅类型为：

* 云解决方案提供商
* 合作伙伴共享服务订阅

### <a name="get-scale-unit-metrics"></a>获取缩放单元指标

可以使用 PowerShell 获取戳记使用情况信息，不需 CSS 的帮助。 若要获取戳记使用率，请执行以下操作： 

1. 创建 PEP 会话
2. 运行 test-azurestack
3. 退出 PEP 会话
4. 使用 invoke-command 调用运行 get-azurestacklog -filterbyrole seedring
5. 提取 seedring .zip，可以从运行 test-azurestack 的 ERCS 文件夹中获得验证报表

有关详细信息，请参阅 [Azure Stack 诊断](azure-stack-configure-on-demand-diagnostic-log-collection.md#to-run-get-azurestacklog-on-azure-stack-integrated-systems)。

## <a name="troubleshoot-deployment"></a>排查部署问题 
### <a name="general-deployment-failure"></a>常见的部署失败
如果安装期间发生失败，可以使用部署脚本的 -rerun 选项从失败的步骤重新开始部署。  

### <a name="template-validation-error-parameter-osprofile-is-not-allowed"></a>不允许模板验证错误参数 osProfile

如果在模板验证期间遇到错误消息，该消息指出系统不允许参数 'osProfile'，请确保对以下组件使用正确的 API 版本：

- [计算](/azure-stack/user/azure-stack-profiles-azure-resource-manager-versions#microsoftcompute)
- [网络](/azure-stack/user/azure-stack-profiles-azure-resource-manager-versions#microsoftnetwork)

若要将 VHD 从 Azure 复制到 Azure Stack，请使用 [AzCopy 7.3.0](/azure-stack/user/azure-stack-storage-transfer#download-and-install-azcopy)。 请联系供应商以解决映像本身的问题。 若要详细了解 Azure Stack 的 WALinuxAgent 要求，请参阅 [Azure LinuX 代理](azure-stack-linux.md#azure-linux-agent)。

### <a name="deployment-fails-due-to-lack-of-external-access"></a>部署因缺少外部访问而失败
如果部署在需要外部访问的阶段失败，则会返回一个异常，如以下示例所示：

```
An error occurred while trying to test identity provider endpoints: System.Net.WebException: The operation has timed out.
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.GetResponse(WebRequest request)
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.ProcessRecord()at, <No file>: line 48 - 8/12/2018 2:40:08 AM
```
如果发生此错误，请查看[部署网络流量文档](deployment-networking.md)，确保满足所有最低的网络要求。 合作伙伴也可使用网络检查器工具（在合作伙伴工具包中提供）。

其他部署失败通常是由于在连接到 Internet 上的资源时出现问题。

若要验证能否连接到 Internet 上的资源，可以执行以下步骤：

1. 打开 PowerShell。
2. 通过 Enter-PSSession 连接到 WAS01 或任何 ERCs VM。
3. 运行以下 cmdlet： 
   ```powershell
   Test-NetConnection login.chinacloudapi.cn -port 443
   ```

如果此命令失败，请验证TOR 交换机以及任何其他的网络设备是否已配置为[允许网络流量](azure-stack-network.md)。

## <a name="troubleshoot-virtual-machines"></a>对虚拟机进行故障排除
### <a name="default-image-and-gallery-item"></a>默认映像和库项
在 Azure Stack 中部署 VM 之前，必须先添加 Windows Server 映像和库项。


### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk"></a>我已删除某些虚拟机，但仍在磁盘上看到 VHD 文件
此行为是设计使然：

* 删除 VM 时，不会删除 VHD。 磁盘是资源组中的独立资源。
* 删除存储帐户后，Azure 资源管理器会立即反映删除结果，但其中的磁盘仍保留在存储中，直到运行垃圾收集为止。

如果看到“孤立的”VHD，必须知道它们是否包含在已删除的存储帐户的文件夹中。 如果未删除存储帐户，则正常情况下，这些 VHD 仍在存储帐户中。

可以在[管理存储帐户](azure-stack-manage-storage-accounts.md)中详细了解如何配置保留阈值和按需回收。

## <a name="troubleshoot-storage"></a>排查存储问题
### <a name="storage-reclamation"></a>存储回收
回收的容量最长可能需要在 14 小时后才显示在门户中。 空间回收取决于多种因素，包括块 Blob 存储中内部容器文件的用量百分比。 因此，我们无法保证运行垃圾收集器时可回收的空间量，这取决于删除的数据量。

### <a name="azure-storage-explorer-not-working-with-azure-stack"></a>Azure 存储资源管理器不兼容 Azure Stack 
 
如果在断开连接的情况下使用集成系统，建议使用企业证书颁发机构 (CA)。 以 Base-64 格式导出根证书，然后将其导入 Azure 存储资源管理器。 确保从 ARM 终结点中删除尾随反斜杠 (/)。 有关详细信息，请参阅[准备连接到 Azure Stack](/azure-stack/user/azure-stack-storage-connect-se#prepare-for-connecting-to-azure-stack)。
 

## <a name="troubleshooting-app-service"></a>应用服务故障排除
### <a name="create-aadidentityappps1-script-fails"></a>Create-AADIdentityApp.ps1 脚本失败

如果应用服务所需的 Create-AADIdentityApp.ps1 脚本失败，请确保在运行该脚本时包含所选的 -AzureStackAdminCredential 参数。 有关详细信息，请参阅[在 Azure Stack 上部署应用服务的先决条件](azure-stack-app-service-before-you-get-started.md#create-an-azure-active-directory-app)。

