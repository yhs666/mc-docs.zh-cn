---
title: "对 Linux 中的 Azure 文件存储问题进行故障排除| Microsoft Docs"
description: "对 Linux 中的 Azure 文件存储问题进行故障排除"
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
origin.date: 07/11/2017
ms.date: 08/14/2017
ms.author: v-haiqya
ms.openlocfilehash: 2e681e0815d86b1291809402af7d156102911456
ms.sourcegitcommit: c8b577c85a25f9c9d585f295b682e835fa861dd0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a>对 Linux 中的 Azure 文件存储问题进行故障排除

本文列出了从 Linux 客户端连接时与 Azure 文件存储相关的常见问题。 它还提供了这些问题的可能原因和解决方法。

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>尝试打开文件时“[权限被拒绝] 超出了磁盘配额”

在 Linux 中，收到类似下文的错误消息：

**<filename> [permission denied] Disk quota exceeded**

### <a name="cause"></a>原因

已达到文件所允许的并发打开句柄数上限。

### <a name="solution"></a>解决方案

关闭某些句柄以减少并发打开句柄数，然后重试操作。 有关详细信息，请参阅 [Azure 存储性能和可伸缩性核对清单](storage-performance-checklist.md)。

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-file-storage-in-linux"></a>在 Linux 中将文件复制到 Azure 文件存储或从中复制文件时速度慢

-   如果没有特定的 I/O 大小下限要求，建议使用 1 MB 的 I/O 大小获得最佳性能。
-   如果知道使用写入扩展的文件的最终大小，并且当尚未在文件上写入的尾部包含零时软件没有兼容性问题，请提前设置文件大小，而不是使每次写入都成为扩展写入。
-   使用正确的复制方法：
    -   为两个文件共享之间的任何传输使用 [AzCopy](storage-use-azcopy.md#file-copy)。
    -   在本地计算机上的文件共享之间使用 [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/)。

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>因为重新连接超时而发生“装载错误(112): 主机已关闭”

当客户端长时间处于空闲状态时，Linux 客户端上出现“112”装载错误。 在长时间处于空闲状态后，客户端将断开连接，并发生连接超时。  

### <a name="cause"></a>原因

连接可能由于以下原因而处于空闲状态：

-   网络通信发生故障，导致使用默认的“soft”装载选项时阻止 TCP 与服务器重新建立连接
-   最近的重新连接修复，较旧的内核中未提供这些修复

### <a name="solution"></a>解决方案

Linux 内核中的此重新连接问题现已在以下更改中进行了修复：

- [修复重新连接以在 socket 重新连接后缩短 smb3 会话的重新连接延迟时间](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [在 socket 重新连接后立即调用 echo 服务](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [CIFS：修复重新连接期间潜在的内存损坏](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [CIFS：修复重新连接期间潜在的互斥双锁（对于内核 v4.9 及更高版本）](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

但是，这些更改可能尚未移植到所有的 Linux 发行版。 在以下常用 Linux 内核中执行了此修复和其他重新连接修复： 4.4.40、4.8.16 和 4.9.1。 可以通过升级到建议的这些内核版本之一来完成此修复。

### <a name="workaround"></a>解决方法

可以通过指定硬装载来解决此问题。 这会强制客户端等到建立连接或者显式中断为止，可用于避免由于网络超时而引起的错误。 但是，此解决方法可能会导致无限期等待。 请准备好根据需要停止连接。

如果无法升级到最新内核版本，可通过将每隔 30 秒或更少的时间间隔便会对其进行写入操作的文件保留在 Azure 文件共享中来解决此问题。 这必须是一个写入操作，例如在文件上重新写入创建或修改日期。 否则，可能会得到缓存的结果，且操作可能不会触发重新连接。

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a>使用 SMB 3.0 装载 Azure 文件存储时出现“装载错误(115): 操作现在正在进行”

### <a name="cause"></a>原因

某些 Linux 发行版尚不支持 SMB 3.0 中的加密功能，如果用户尝试使用 SMB 3.0 装载 Azure 文件存储，可能会由于缺少功能而收到“115”错误消息。

### <a name="solution"></a>解决方案

在 4.11 内核中引入了适用于 Linux 的 SMB 3.0 加密功能。 使用此功能可从本地或不同 Azure 区域装载 Azure 文件共享。 在发布时，此功能已向后移植到 Ubuntu 17.04 和 Ubuntu 16.10。 如果 Linux SMB 客户端不支持加密，请使用 SMB 2.1 从文件存储帐户所在的同一数据中心上的 Azure Linux VM 装载 Azure 文件存储。

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Linux VM 上装载的 Azure 文件共享的性能低下

### <a name="cause"></a>原因

性能低下的一个可能原因是禁用了缓存。

### <a name="solution"></a>解决方案

要检查是否禁用了缓存，请查找 **cache=** 条目。 

**Cache=none** 指示缓存已禁用。  使用默认的装载命令重新装载共享，或者在装载命令中显式添加 **cache=strict** 选项，确保默认缓存或“strict”缓存模式已启用。

在某些情况下，**serverino** 装载选项可能会导致 **ls** 命令针对每个目录条目运行 stat。 当列出大型目录时，此行为会导致性能降级。 可在 **/etc/fstab** 条目中检查装载选项：

`//azureuser.file.core.chinacloudapi.cn/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

还可以通过运行 **sudo mount | grep cifs** 命令并检查其输出（例如以下示例输出）来检查是否使用了正确的选项：

`//mabiccacifs.file.core.chinacloudapi.cn/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

如果不存在 **cache=strict** 或 **serverino** 选项，请通过运行此[文档](storage-how-to-use-files-linux.md)中的装载命令卸载并再次装载 Azure 文件存储。 然后，重新检查 **/etc/fstab** 条目是否具有正确的选项。

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>将文件从 Windows 复制到 Linux 时，时间戳丢失

在 Linux/Unix 平台上，如果文件 1 和文件 2 由不同的用户拥有，则 **cp -p** 命令将失败。

### <a name="cause"></a>原因

COPYFILE 中的强制标志 **f** 导致在 Unix 上执行 **cp -p -f**。 此命令也无法保留不归你拥有的文件的时间戳。

### <a name="workaround"></a>解决方法

使用存储帐户用户来复制文件：

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

<!--Update_Description: wording update - deleted error11->
