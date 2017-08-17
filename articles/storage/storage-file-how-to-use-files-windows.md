---
title: "在 Windows 中装载 Azure 文件共享并对其进行访问 | Azure"
description: "在 Windows 中装载 Azure 文件共享并对其进行访问。"
services: storage
documentationcenter: na
author: hayley244
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 05/27/2017
ms.date: 08/14/2017
ms.author: v-haiqya
ms.openlocfilehash: aede626b5b114b16e17b5288cdc3787d9df82cb7
ms.sourcegitcommit: c8b577c85a25f9c9d585f295b682e835fa861dd0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2017
---
# <a name="mount-an-azure-file-share-and-access-the-share-in-windows"></a>在 Windows 中装载 Azure 文件共享并对其进行访问
[Azure 文件存储](storage-dotnet-how-to-use-files.md)是 Microsoft 推出的易用云文件系统。 可以在 Windows 和 Windows Server 中装载 Azure 文件共享。 本文介绍了三种在 Windows 中装载 Azure 文件共享的不同方式：使用文件资源管理器 UI、通过 PowerShell，以及通过命令提示符。 

若要将 Azure 文件共享装载到其被托管时所在的 Azure 区域之外（例如本地或其他 Azure 区域），OS 必须支持 SMB 3.x。 下表显示最近发行的 Windows 的 SMB 版本：

| Windows 版本 | SMB 版本 | 支持从 Azure VM 装载 | 支持从本地装载 | 最小建议 KB |
|----|----|----|----|----|
| Windows 10 版本 1703 | SMB 3.1.1 | 是 | 是 | |
| Windows Server 2016 | SMB 3.1.1 | 是 | 是 | [KB4015438](https://support.microsoft.com/help/4015438) |
| Windows 10 版本 1607 | SMB 3.1.1 | 是 | 是 | [KB4015438](https://support.microsoft.com/help/4015438) | 
| Windows 10 版本 1511 | SMB 3.1.1 | 是 | 是 | [KB4013198](https://support.microsoft.com/help/4013198) |
| Windows 10 版本 1507 | SMB 3.1.1 | 是 | 是 | [KB4012606](https://support.microsoft.com/help/4012606) | 
| Windows 8.1 | SMB 3.0.2 | 是 | 是 | [KB4012216](https://support.microsoft.com/help/4012216) |
| Windows Server 2012 R2 | SMB 3.0.2 | 是 | 是 | [KB4012216](https://support.microsoft.com/help/4012216) |
| Windows Server 2012 | SMB 3.0 | 是 | 是 | [KB4012214](https://support.microsoft.com/help/4012214) |
| Windows 7 | SMB 2.1 | 是 | 否 | [KB4012215](https://support.microsoft.com/help/4012215) |
| Windows Server 2008 R2 | SMB 2.1 | 是 | 否 | [KB4012215](https://support.microsoft.com/help/4012215) |

> [!Note]  
> 我们始终建议使用相对于 Windows 版本来说最新的 KB。 设置最小建议 KB 的目的是为不喜欢更新的 IT 管理员提供包含 SMB 修补程序的最新包。

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>在 Windows 中装载 Azure 文件共享的先决条件 
* **存储帐户名**：若要装载 Azure 文件共享，需要存储帐户的名称。

* **存储帐户密钥**：若要装载 Azure 文件共享，需要主（或辅助）存储密钥。 目前不支持使用 SAS 密钥进行装载。

* **确保端口 445 已打开**：Azure 文件存储使用 SMB 协议。 SMB 通过 TCP 端口 445 通信 - 请查看防火墙是否未阻止 TCP 端口 445 与客户端计算机通信。

## <a name="mount-the-azure-file-share-with-file-explorer"></a>使用文件资源管理器装载 Azure 文件共享
> [!Note]  
> 请注意，以下说明是在 Windows 10 上显示的，在较旧的版本上可能稍有不同。 

1. **打开文件资源管理器**：可以从“开始”菜单打开，也可以按 Win+E 快捷键打开。

2. **导航到窗口左侧的“此电脑”项。这样会更改功能区中的可用菜单。在“计算机”菜单中，选择“映射网络驱动器”**。

    ![“映射网络驱动器”下拉菜单的屏幕截图](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. **在 Azure 门户中从“连接”窗格复制 UNC 路径**：有关如何查找此信息的详细说明，请参阅[此文](storage-file-how-to-use-files-portal.md#connect-to-file-share)。

    ![Azure 文件存储“连接”窗格中的 UNC 路径](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. **选择驱动器号并输入 UNC 路径。** 

    ![“映射网络驱动器”对话框的屏幕截图](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. **使用带 `Azure\` 前缀的存储帐户名称作为用户名，使用存储帐户密钥作为密码。**

    ![网络凭据对话框的屏幕快照](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. **根据需要使用 Azure 文件共享**。

    ![Azure 文件共享现已装载](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. **做好卸载（或断开连接）Azure 文件共享的准备后，即可在文件资源管理器中右键单击“网络位置”下对应于共享的条目，并选择“断开连接”**。

## <a name="mount-the-azure-file-share-with-powershell"></a>使用 PowerShell 装载 Azure 文件共享
1. **使用以下命令装载 Azure 文件共享**：记得将 `<storage-account-name>`、`<share-name>`、`<storage-account-key>` 和 `<desired-drive-letter>` 替换为适当的信息。

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.chinacloudapi.cn\<share-name>" -Credential $credential
    ```

2. **根据需要使用 Azure 文件共享**。

3. **完成后，使用以下命令卸载 Azure 文件共享**。

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> 可以对 `New-PSDrive` 使用 `-Persist` 参数，使 Azure 文件共享在装载后对 OS 的其余部分可见。

## <a name="mount-the-azure-file-share-with-command-prompt"></a>使用命令提示符装载 Azure 文件共享
1. **使用以下命令装载 Azure 文件共享**：记得将 `<storage-account-name>`、`<share-name>`、`<storage-account-key>` 和 `<desired-drive-letter>` 替换为适当的信息。

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.chinacloudapi.cn\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. **根据需要使用 Azure 文件共享**。

3. **完成后，使用以下命令卸载 Azure 文件共享**。

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> 可以将 Azure 文件共享配置为在重新启动后自动重新连接，只需将凭据持久保存在 Windows 中即可。 以下命令会持久保存凭据：
>   ```
>   cmdkey /add:<storage-account-name>.file.core.chinacloudapi.cn /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a>后续步骤
请参阅以下链接以获取有关 Azure 文件存储的更多信息。

* [常见问题](storage-files-faq.md)
* [故障排除](storage-troubleshoot-windows-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>概念性文章和视频
* [Azure 文件存储：适用于 Windows 和 Linux 的顺畅的云 SMB 文件系统](https://www.azure.cn/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [如何将 Azure 文件存储与 Linux 配合使用](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a>Azure 文件存储的工具支持
* [对 Azure 存储使用 Azure PowerShell](storage-powershell-guide-full.md)
* [如何对 Azure 存储使用 AzCopy](storage-use-azcopy.md)
* [将 Azure CLI 用于 Azure 存储](storage-azure-cli.md#create-and-manage-file-shares)
* [排查 Azure 文件存储问题](/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a>博客文章
* [Azure 文件存储现已正式发布](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Azure 文件存储内部](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Azure File Service（Azure 文件服务简介）](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [将数据迁移到 Azure 文件](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>引用
* [.NET 存储客户端库参考](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [文件服务 REST API 参考](http://msdn.microsoft.com/library/azure/dn167006.aspx)