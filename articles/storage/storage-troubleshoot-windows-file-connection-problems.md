---
title: "排查 Windows 中的 Azure 文件存储问题 | Microsoft Docs"
description: "排查 Windows 中的 Azure 文件存储问题"
services: storage
documentationcenter: 
author: forester123
manager: digimobile
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/28/2017
ms.date: 08/14/2017
ms.author: v-haiqya
ms.openlocfilehash: 35b9c0d2c11fe18599c730cf2147c8ff2e17b0f6
ms.sourcegitcommit: c8b577c85a25f9c9d585f295b682e835fa861dd0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-windows"></a>排查 Windows 中的 Azure 文件存储问题

本文列出了从 Windows 客户端连接时与 Azure 文件存储相关的常见问题， 并提供了这些问题的可能原因和解决方法。 除了本文中的故障排除步骤之外，还可以使用 [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) 来确保 Windows 客户端环境满足相应的先决条件。 AzFileDiagnostics 会自动检测本文中提及的大多数症状，并帮助设置环境以获得最佳性能。 还可以在 [Azure 文件共享疑难解答](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares)中找到此信息，该疑难解答提供相关步骤来帮助解决在连接/映射/装载 Azure 文件共享时遇到的问题。

<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>装载或卸载 Azure 文件共享时出现“错误 53”、“错误 67”或“错误 87”

尝试从本地或其他数据中心装载文件共享时，可能会出现以下错误：

- 发生系统错误 53。 找不到网络路径。
- 发生系统错误 67。 找不到网络名称。
- 发生系统错误 87。 参数不正确。

### <a name="cause-1-unencrypted-communication-channel"></a>原因 1：通信通道未加密

出于安全原因，如果信道未加密，且未从 Azure 文件共享所驻留的数据中心尝试连接，则到 Azure 文件共享的连接将受阻。 仅当用户的客户端 OS 支持 SMB 加密时，才会提供信道加密。

Windows 8、Windows Server 2012 及更高版本的每次系统协商均要求其包含支持加密的 SMB 3.0。

### <a name="solution-for-cause-1"></a>原因 1 的解决方案

从符合下述某个条件的客户端进行连接：

- 满足使用 Windows 8 和 Windows Server 2012 或更高版本这一要求
- 从虚拟机进行连接时，该虚拟机所在的数据中心必须是用于 Azure 文件共享的 Azure 存储帐户所在的数据中心

### <a name="cause-2-port-445-is-blocked"></a>原因 2：端口 445 被阻

如果端口 445 到 Azure 文件存储数据中心的出站通信受阻，则可能会出现“系统错误53”或“系统错误 67”。 如需大致了解允许或禁止从端口 445 进行访问的 ISP，请访问 [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx)。

若要了解是否由此造成“系统错误 53”，可使用 Portqry 查询 TCP:445 终结点。 如果 TCP:445 终结点显示为“已筛选”，则表示 TCP端口受阻。 示例查询如下：

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.chinacloudapi.cn -p TCP -e 445`

如果 TCP 端口 445 受到网络路径中的规则阻止，将显示以下输出：

  `TCP port 445 (microsoft-ds service): FILTERED`

若要详细了解如何使用 Portqry，请参阅 [Portqry.exe 命令行实用工具说明](https://support.microsoft.com/help/310099)。

### <a name="solution-for-cause-2"></a>原因 2 的解决方案

与 IT 部门配合，向 [Azure IP 范围](https://www.microsoft.com/download/details.aspx?id=42064)开放端口 445 出站通信。

### <a name="cause-3-ntlmv1-is-enabled"></a>原因 3：NTLMv1 已启用

如果在客户端上启用 NTLMv1 通信，则可能发生系统错误 53 或系统错误 87。 Azure 文件存储仅支持 NTLMv2 身份验证。 启用 NTLMv1 会降低客户端的安全性。 因此，将阻止 Azure 文件存储的通信。 

若要确定这是否是错误原因，请验证以下注册表子项是否设为值 3：

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

有关详细信息，请参阅 TechNet 上的 [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) 主题。

### <a name="solution-for-cause-3"></a>原因 3 的解决方案

在下述注册表子项中将 **LmCompatibilityLevel** 值还原为默认值 3：

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>复制到 Azure 文件共享时出现错误 1816“处理此命令时没有足够的配额可用”

### <a name="cause"></a>原因

达到并发开放句柄数的上限时，会发生错误 1816。在计算机中装载文件共享时，会为计算机中的文件启用开放句柄。

### <a name="solution"></a>解决方案

关闭一些句柄，减少并发打开句柄的数量，再重试。 有关详细信息，请参阅 [Azure 存储性能和可伸缩性核对清单](storage-performance-checklist.md)。

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-file-storage-in-windows"></a>在 Windows 中将文件复制到 Azure 文件存储或从中复制文件时速度慢

尝试将文件传输到 Azure 文件服务时，可能会出现性能下降的情况。

- 如果没有特定的 I/O 大小下限要求，建议使用 1 MB 的 I/O 大小获得最佳性能。
-   如果知道使用写入进行扩展的文件的最终大小，并且当尚未在文件上写入的尾部包含零时软件没有兼容性问题，请提前设置文件大小，而不是使每次写入都成为扩展写入。
-   使用正确的复制方法：
    -   为两个文件共享之间的任何传输使用 [AzCopy](storage-use-azcopy.md#file-copy)。
    -   在本地计算机上的文件共享之间使用 [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/)。

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Windows 8.1 或 Windows Server 2012 R2 的注意事项

对于运行 Windows 8.1 或 Windows Server 2012 R2 的客户端，请确保安装有 [KB3114025](https://support.microsoft.com/help/3114025) 修补程序。 该修补程序可提升创建和关闭句柄时的性能。

可运行以下脚本，检查是否安装了该修补程序：

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

如果已安装，显示以下输出：

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Azure 应用商店中的 Windows Server 2012 R2 映像默认安装有修补程序 KB3114025。

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a>在“我的电脑”中没有文件夹带有驱动器号

如果以管理员身份使用 net use 来映射 Azure 文件共享，则会缺失共享。

### <a name="cause"></a>原因

默认情况下，Windows 文件资源管理器不以管理员身份运行。 如果通过管理性命令提示符运行 net use，则可以管理员身份映射网络驱动器。 由于映射的驱动器以用户为中心，如果在其他用户帐户上安装了这些驱动器，则登录的用户帐户不会显示它们。

### <a name="solution"></a>解决方案
通过非管理员命令行中装载共享。 或者，可按照[此 TechNet 主题](https://technet.microsoft.com/library/ee844140.aspx)配置 **EnableLinkedConnections** 注册表值。

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>如果存储帐户包含正斜杠，则 net use 命令会失败

### <a name="cause"></a>原因

net use 命令将正斜杠 (/) 解释为命令行选项。 如果用户帐户名称以正斜杠开头，则驱动器映射会失败。

### <a name="solution"></a>解决方案

可使用下述某个步骤解决此问题：

- 运行以下 PowerShell 命令：

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  可以在批处理文件中通过以下方式运行该命令：

  `Echo new-smbMapping ... | powershell -command –`

- 将密钥用双引号括起以解决此问题（除非第一个字符是正斜杠）。 如果是，可使用交互模式并单独输入密码，或者重新生成密钥来获取不以正斜杠开头的密钥。

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-file-storage-drive"></a>应用程序或服务无法访问装载的 Azure 文件存储驱动器

### <a name="cause"></a>原因

根据用户装载驱动器。 如果在运行应用程序或服务时，所使用的用户帐户不同于装载驱动器的帐户，则应用程序看不到该驱动器。

### <a name="solution"></a>解决方案

请使用以下解决方案之一：

- 使用应用程序所在的同一用户帐户装载驱动器。 可以使用 PsExec 之类的工具。
- 传递 net use 命令的用户名和密码参数中的存储帐户名和密钥。

按这些说明操作以后，可能会在为系统/网络服务帐户运行 net use 时出现以下错误消息：“发生系统错误 1312。 指定的登录会话不存在。 可能已终止该会话。” 若发生此情况，请确保传递到 net use 的用户名包括域信息（例如“[storage account name].file.core.chinacloudapi.cn”）。

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>“正在将文件复制到不支持加密的目标”错误

通过网络复制文件时，会在源计算机上解密该文件，以明文形式进行传输，然后在目标计算机上重新加密该文件。 但是，你可能会在尝试复制加密的文件时看到以下错误：“正在将文件复制到不支持加密的目标。”

### <a name="cause"></a>原因
如果你使用的是加密文件系统 (EFS)，则可能会出现此问题。 可将 BitLocker 加密的文件复制到 Azure 文件存储。 但是，Azure 文件存储不支持 NTFS EFS。

### <a name="workaround"></a>解决方法
若要通过网络复制文件，必须先解密该文件。 使用以下方法之一：

- 使用 copy /d 命令。 该命令可以在目标计算机上将加密的文件另存为解密的文件。
- 设置以下注册表项：
  - 路径 = HKLM\Software\Policies\Microsoft\Windows\System
  - 值类型 = DWORD
  - 名称= CopyFileAllowDecryptedRemoteDestination
  - 值= 1

请注意，设置注册表项会影响所有针对网络共享进行的复制操作。

<!--Update_Description: wording update-->
