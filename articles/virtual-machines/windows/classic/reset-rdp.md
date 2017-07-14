---
title: "在 Azure 的 Windows VM 上重置密码或远程桌面配置 | Azure"
description: "了解如何使用 Azure 门户或 Azure PowerShell 在通过经典部署模型创建的 Windows VM 上重置帐户密码或远程桌面服务。"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 03/14/2017
ms.date: 04/17/2017
ms.author: v-dazen
ms.openlocfilehash: 3aeab15a8473998e030efd5559a5d2065856be17
ms.sourcegitcommit: 7d2235bfc3dc1e2f64ed8beff77e87d85d353c4f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# 如何在使用经典部署模型创建的 Windows VM 中重置远程桌面服务或其登录密码
<a id="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-the-classic-deployment-model" class="xliff"></a>
> [!IMPORTANT]
> Azure 具有用于创建和处理资源的两个不同的部署模型：[Resource Manager 和经典](../../../resource-manager-deployment-model.md)。 本文介绍如何使用经典部署模型。 Azure 建议大多数新部署使用 Resource Manager 模型。 还可对[使用 Resource Manager 部署模型创建的 VM 执行这些步骤](../reset-rdp.md)。

如果无法连接到 Windows 虚拟机 (VM)，可以重置本地管理员密码或重置远程桌面服务配置。 可以使用 Azure 门户或 Azure PowerShell 中的 VM 访问扩展重置密码。

## 重置配置或凭据的方式
<a id="ways-to-reset-configuration-or-credentials" class="xliff"></a>
可以根据需要，通过多种不同的方式重置远程桌面服务和凭据：

- [使用 Azure 门户进行重置](#azure-portal)
- [使用 Azure PowerShell 进行重置](#vmaccess-extension-and-powershell)

## Azure 门户
<a id="azure-portal" class="xliff"></a>
可使用 [Azure 门户](https://portal.azure.cn)重置远程桌面服务。 若要展开门户菜单，请单击左上角的三栏，然后单击“虚拟机(经典)”：

![浏览 Azure VM](./media/reset-rdp/Portal-Select-Classic-VM.png)

选择 Windows 虚拟机，然后单击“重置远程...” 。 此时将显示以下用于重置远程桌面配置的对话框：

![重置 RDP 配置页面](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

还可以重置本地管理员帐户的用户名和密码。 从 VM 中单击“支持 + 故障排除” > “重置密码”。 此时会显示密码重置边栏选项卡：

![密码重置页](./media/reset-rdp/Portal-PW-Reset-Windows.png)

输入新用户名和密码，然后单击“保存”。

## VMAccess 扩展和 PowerShell
<a id="vmaccess-extension-and-powershell" class="xliff"></a>
确保在虚拟机上安装 VM 代理。 只要 VM 代理可用，就无需事先安装 VMAccess 扩展。 使用以下命令验证是否已在虚拟机上安装 VM 代理。 （分别将“myCloudService”和“myVM”替换为云服务和 VM 的名称。 若要了解这些名称，可运行不带任何参数的 `Get-AzureVM`。）

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

如果 **write-host** 命令显示 **True**，则已安装 VM 代理。 如果该命令显示 **False**，请参阅 Azure 博客文章 [VM 代理和扩展 - 第 2 部分](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409)中的说明和下载链接。

如果使用门户创建虚拟机，检查 `$vm.GetInstance().ProvisionGuestAgent` 是否返回 **True**。 如果不是，使用以下命令进行设置：

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

此命令可防止在后续步骤中运行 **Set-AzureVMExtension** 命令时出现以下错误：“在设置 IaaS VM Access 扩展前，必须对 VM 对象启用预配来宾代理。”

### **重置本地管理员帐户密码**
<a id="reset-the-local-administrator-account-password" class="xliff"></a>
使用当前的本地管理员帐户名和新密码创建登录凭据，然后运行 `Set-AzureVMAccessExtension` ，如下所示。

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension -vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

如果键入不同于当前帐户的名称，VMAccess 扩展将重命名本地管理员帐户，将密码分配到该帐户，并发出远程桌面注销命令。 如果禁用本地管理员帐户，则 VMAccess 扩展将启用它。

这些命令也可重置远程桌面服务配置。

### **重置远程桌面服务配置**
<a id="reset-the-remote-desktop-service-configuration" class="xliff"></a>
若要重置远程桌面服务配置，请运行以下命令：

```powershell
Set-AzureVMAccessExtension -vm $vm | Update-AzureVM
```

VMAccess 扩展在虚拟机上运行两个命令：

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

此命令启用允许传入远程桌面流量（使用 TCP 端口 3389）的内置 Windows 防火墙组。

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

此命令将 fDenyTSConnections 注册表值设置为 0，以启用远程桌面连接。

## 后续步骤
<a id="next-steps" class="xliff"></a>
如果 Azure VM 访问扩展无响应，并且密码无法重置，可以[脱机重置本地 Windows 密码](../reset-local-password-without-agent.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。 此方法是一个更高级的方案，需将问题 VM 的虚拟硬盘连接到另一个 VM。 请首先按照本文中所述的步骤操作，将脱机密码重置方法作为最后的选择。

[Azure VM 扩展和功能](../extensions-features.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)

[使用 RDP 或 SSH 连接到 Azure 虚拟机](/virtual-machines/linux/overview)

[对与基于 Windows 的 Azure 虚拟机的远程桌面连接进行故障排除](../troubleshoot-rdp-connection.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)
