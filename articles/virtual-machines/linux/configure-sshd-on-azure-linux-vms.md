---
title: "在 Azure Linux 虚拟机上配置 SSHD | Azure"
description: "按安全最佳实践配置 SSHD，并将 SSH 锁定到 Azure Linux 虚拟机。"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
origin.date: 11/21/2016
ms.date: 01/13/2017
ms.author: v-dazen
ms.openlocfilehash: 8c78532ff419e45c5d92b0a0fe89bcdc667ae908
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="configure-sshd-on-azure-linux-vms"></a>在 Azure Linux VM 上配置 SSHD

本文说明如何在 Linux 上锁定 SSH 服务器，以便使用 SSH 密钥而不是密码提供最佳实践安全性并加快 SSH 登录过程。  为了进一步锁定 SSHD，我们将禁止根用户登录，通过已批准的组列表限制允许登录的用户，禁用 SSH 协议版本 1，设置最小密钥位，并配置空闲用户的自动注销。  本文的要求如下：一个 Azure 帐户（[获取试用帐户](https://www.azure.cn/pricing/1rmb-trial/)）以及 [SSH 公钥和私钥文件](mac-create-ssh-keys.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)。

## <a name="quick-commands"></a>快速命令

使用以下设置配置 `/etc/ssh/sshd_config` ：

### <a name="disable-password-logins"></a>禁用密码登录

```bash
PasswordAuthentication no
```

### <a name="disable-login-by-the-root-user"></a>禁止根用户登录

```bash
PermitRootLogin no
```

### <a name="allowed-groups-list"></a>允许的组列表

```bash
AllowGroups wheel
```

### <a name="allowed-users-list"></a>允许的用户列表

```bash
AllowUsers ahmet ralph
```

### <a name="disable-ssh-protocol-version-1"></a>禁用 SSH 协议版本 1

```bash
Protocol 2
```

### <a name="minimum-key-bits"></a>最小密钥位

```bash
ServerKeyBits 2048
```

### <a name="disconnect-idle-users"></a>断开空闲用户的连接

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="detailed-walkthrough"></a>详细演练

SSHD 是在 Linux VM 上运行的 SSH 服务器。  SSH 是从 MacBook、Linux 工作站上的 shell 或 Windows 上的 Bash 运行的客户端。  SSH 也是一个协议，用于保护和加密工作站与 Linux VM 之间的通信，让 SSH 也充当 VPN（虚拟专用网络）。

在本文中，必须让用户在 Linux VM 中保持登录，使整个演练得以顺畅进行。  建立 SSH 连接后，只要未关闭窗口，它将保持为打开的会话。  使一个终端处于已登录状态，可以在进行重大更改时，更改 SSHD 服务而不必锁定该服务。  如果用户在进行重大 SSHD 配置时被锁定在 Linux VM 之外，则可利用 Azure 提供的功能，通过 [Azure VM 访问扩展](using-vmaccess-extension.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)重置重大的 SSHD 配置。

因此，我们会打开两个终端，并从它们通过 SSH 登录到 Linux VM。  我们使用第一个终端更改 SSHD 配置文件，然后重新启动 SSHD 服务。  重新启动服务后，我们会使用第二个终端测试这些更改。  由于我们要禁用 SSH 密码并严重依赖于 SSH 密钥，因此如果 SSH 密钥不正确而关闭 VM 连接，VM 将永久被锁定，并且没有人能够登录。这样，只能将它删除后再重新创建。

## <a name="disable-password-logins"></a>禁用密码登录

保护 Linux VM 的最快捷方法是禁用密码登录。  启用密码登录后，在 Web 上爬行的机器人将立即开始尝试使用 SSH 暴力猜测 Linux VM 的密码。  完全禁用密码登录可使 SSH 服务器忽略所有密码登录尝试。

```bash
PasswordAuthentication no
```

## <a name="disable-login-by-the-root-user"></a>禁止根用户登录

根据 Linux 最佳实践，`root` 用户不应通过 SSH 或 `sudo su` 登录。  需要根级别权限的所有命令始终都应通过 `sudo` 命令运行，后者将记录所有操作以便进一步审核。  禁止 `root` 用户通过 SSH 登录是安全最佳实践步骤，可确保只有获得授权的用户可以通过 SSH 登录。

```bash
PermitRootLogin no
```

## <a name="allowed-groups-list"></a>允许的组列表

SSH 提供了限制用户和组的方法，以允许或禁止用户和组通过 SSH 登录。  此功能使用列表来批准或拒绝特定用户和组登录。  将 wheel 组设置到 `AllowGroups` 列表可将批准的通过 SSH 登录限制为只是 wheel 组中的用户帐户。

```bash
AllowGroups wheel
```

## <a name="allowed-users-list"></a>允许的用户列表

将 SSH 登录限制为只是用户是完成 `AllowGroups` 实现的同一任务的更具体方法。  

```bash
AllowUsers ahmet ralph
```

## <a name="disable-ssh-protocol-version-1"></a>禁用 SSH 协议版本 1

SSH 协议版本 1 不安全，应禁用。  SSH 协议版本 2 是当前版本，提供通过 SSH 登录到服务器的安全方法。  禁用 SSH 版本 1 将拒绝尝试使用 SSH 版本 1 与 SSH 服务器建立连接的任何 SSH 客户端。  仅允许使用 SSH 版本 2 连接与 SSH 服务器协商连接。

```bash
Protocol 2
```

## <a name="minimum-key-bits"></a>最小密钥位

按照安全最佳实践，将禁用密码 SSH 登录，仅允许使用 SSH 密钥向 SSH 服务器进行身份验证。  可以使用不同长度的密钥（以位为单位）来创建这些 SSH 密钥。  最佳实践指出长度为 2048 位的密钥是可接受的最小密钥强度。  从理论上讲，小于 2048 位的密钥可能被破坏。  将 `ServerKeyBits` 设置为 `2048` 以后，系统会允许使用的密钥为 2048 位或以上的连接，拒绝密钥不到 2048 位的连接。

```bash
ServerKeyBits 2048
```

## <a name="disconnect-idle-users"></a>断开空闲用户的连接

SSH 能够断开这样的用户的连接：具有打开的连接且空闲持续时间超过设定的秒段。  只保留处于活动状态的这些用户的打开会话可限制 Linux VM 的暴露。

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="restart-sshd"></a>重新启动 SSHD

若要启用 `/etc/ssh/sshd_config` 中的设置，请重新启动 SSHD 进程，该进程重新启动 SSH 服务器。  用于重新启动 SSH 服务器的终端窗口将保持打开状态而不会丢失打开的 SSH 会话。  若要测试新的 SSH 服务器设置，请使用第二个终端窗口或选项卡。  使用单独的终端测试 SSH 连接，可返回并在第一个终端中对 `/etc/ssh/sshd_config` 进行其他更改，而不会被重大 SSHD 更改锁定。  

### <a name="on-redhat-centos-and-fedora"></a>在 Redhat、Centos 和 Fedora 上

```bash
service sshd restart
```

### <a name="on-debian--ubuntu"></a>在 Debian 和 Ubuntu 上

```bash
service ssh restart
```

## <a name="reset-sshd-using-azure-reset-access"></a>使用 Azure 重置访问重置 SSHD

如果你由于 SSHD 配置发生重大更改而被锁定，可以使用 Azure VM 访问扩展将 SSHD 配置重置回原始配置。

将任何示例名称替换为你自己的名称。

```azurecli
azure vm reset-access \
--resource-group myResourceGroup \
--name myVM \
--reset-ssh
```

## <a name="install-fail2ban"></a>安装 Fail2ban

强烈建议安装并设置开放源代码应用 Fail2ban，以阻止使用暴力破解通过 SSH 登录到你的 Linux VM 的重复尝试。  Fail2ban 记录通过 SSH 登录的重复失败尝试，然后创建防火墙规则以阻止尝试所源自的 IP 地址。

* [Fail2ban 主页](http://www.fail2ban.org/wiki/index.php/Main_Page)

## <a name="next-steps"></a>后续步骤

现在，已在 Linux VM 上配置并锁定 SSH 服务器，可以按照其他安全最佳实践进行操作了。  

* [管理用户、SSH，并使用 VMAccess 扩展检查或修复 Azure Linux VM 上的磁盘](using-vmaccess-extension.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)

* [使用 Azure CLI 加密 Linux VM 上的磁盘](encrypt-disks.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)

* [Azure Resource Manager 模板中的访问权限和安全性](dotnet-core-3-access-security.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)
