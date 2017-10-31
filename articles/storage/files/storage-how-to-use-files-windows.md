---
title: "在 Windows 中装载 Azure 文件共享并对其进行访问 | Microsoft Docs"
description: "在 Windows 中装载 Azure 文件共享并对其进行访问。"
services: storage
documentationcenter: na
author: forester123
manager: digimobile
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 09/19/2017
ms.date: 10/30/2017
ms.author: v-johch
ms.openlocfilehash: d875f71b5732a90a411023f163e7e8dda85e1484
ms.sourcegitcommit: 71c3744a54c69e7e322b41439da907c533faba39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2017
---
# <a name="mount-an-azure-file-share-and-access-the-share-in-windows"></a>在 Windows 中装载 Azure 文件共享并对其进行访问
[Azure 文件](storage-files-introduction.md)是易于使用的云文件系统。 可以在 Windows 和 Windows Server 中装载 Azure 文件共享。 本文介绍了三种在 Windows 中装载 Azure 文件共享的不同方式：使用文件资源管理器 UI、通过 PowerShell，以及通过命令提示符。 

若要将 Azure 文件共享装载到托管其的 Azure 区域之外（例如本地或其他 Azure 区域），OS 必须支持 SMB 3.0。 

可以将 Azure 文件共享装载到在 Azure VM 中或本地运行的 Windows 安装。 下表说明了哪些 OS 版本支持在哪个环境中装载文件共享：

| Windows 版本        | SMB 版本 | 可以在 Azure VM 中装载 | 可以在本地装载 |
|------------------------|-------------|-----------------------|----------------------|
| Windows 10<sup>1</sup>  | SMB 3.0 | 是 | 是 |
| Windows Server 2016    | SMB 3.0     | 是                   | 是                  |
| Windows 8.1            | SMB 3.0     | 是                   | 是                  |
| Windows Server 2012 R2 | SMB 3.0     | 是                   | 是                  |
| Windows Server 2012    | SMB 3.0     | 是                   | 是                  |
| Windows 7              | SMB 2.1     | 是                   | 否                   |
| Windows Server 2008 R2 | SMB 2.1     | 是                   | 否                   |

<sup>1</sup>Windows 10 版本 1507、1511、1607、1703 和 1709。

> [!Note]  
> 我们始终建议使用相对于 Windows 版本来说最新的 KB。

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>在 Windows 中装载 Azure 文件共享的先决条件 
* **存储帐户名**：若要装载 Azure 文件共享，需要存储帐户的名称。

* **存储帐户密钥**：若要装载 Azure 文件共享，需要主（或辅助）存储密钥。 目前不支持使用 SAS 密钥进行装载。

* **确保端口 445 处于打开状态**：Azure 文件使用 SMB 协议。 SMB 通过 TCP 端口 445 通信 - 请查看防火墙是否未阻止 TCP 端口 445 与客户端计算机通信。

## <a name="mount-the-azure-file-share-with-file-explorer"></a>使用文件资源管理器装载 Azure 文件共享
> [!Note]  
> 请注意，以下说明是在 Windows 10 上显示的，在较旧的版本上可能稍有不同。 

1. **打开文件资源管理器**：可以从“开始”菜单打开，也可以按 Win+E 快捷键打开。

2. **导航到窗口左侧的“此电脑”项。这样会更改功能区中的可用菜单。在“计算机”菜单中，选择“映射网络驱动器”**。

    ![“映射网络驱动器”下拉菜单的屏幕截图](./media/storage-how-to-use-files-windows/1_MountOnWindows10.png)

3. **在 Azure 门户中从“连接”窗格复制 UNC 路径**：有关如何查找此信息的详细说明，请参阅[此文](storage-how-to-use-files-portal.md#connect-to-file-share)。

    ![Azure 文件“连接”窗格中的 UNC 路径](./media/storage-how-to-use-files-windows/portal_netuse_connect.png)

4. **选择驱动器号并输入 UNC 路径。** 

    ![“映射网络驱动器”对话框的屏幕截图](./media/storage-how-to-use-files-windows/2_MountOnWindows10.png)

5. **使用带 `Azure\` 前缀的存储帐户名称作为用户名，使用存储帐户密钥作为密码。**

    ![网络凭据对话框的屏幕快照](./media/storage-how-to-use-files-windows/3_MountOnWindows10.png)

6. **根据需要使用 Azure 文件共享**。

    ![Azure 文件共享现已装载](./media/storage-how-to-use-files-windows/4_MountOnWindows10.png)

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
请参阅以下链接，获取有关 Azure 文件的更多信息。

* [常见问题](../storage-files-faq.md)
* [在 Windows 上进行故障排除](storage-troubleshoot-windows-file-connection-problems.md)      

### <a name="conceptual-article"></a>概念性文章

* [如何通过 Linux 使用 Azure 文件](../storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-files"></a>Azure 文件的工具支持
* [如何对 Azure 存储使用 AzCopy](../common/storage-use-azcopy.md?toc=%2fstorage%2ffiles%2ftoc.json)
* [将 Azure CLI 用于 Azure 存储](../common/storage-azure-cli.md?toc=%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [排查 Azure 文件问题 - Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [排查 Azure 文件问题 - Linux](storage-troubleshoot-linux-file-connection-problems.md)

### <a name="blog-posts"></a>博客文章
* [Azure 文件现已正式发布](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Azure 文件内部](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Azure File Service（Azure 文件服务简介）](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [将数据迁移到 Azure 文件](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>引用
* [.NET 存储客户端库参考](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [文件服务 REST API 参考](http://msdn.microsoft.com/library/azure/dn167006.aspx)
