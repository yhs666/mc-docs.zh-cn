---
title: 删除 Azure 中的恢复服务保管库
description: 介绍如何删除恢复服务保管库。
services: backup
author: lingliw
manager: digimobile
ms.service: backup
ms.topic: conceptual
ms.date: 04/12/19
ms.author: v-lingwu
ms.openlocfilehash: e782286951705dcbbd669dc8366127db04a81dc0
ms.sourcegitcommit: f9d082d429c46cee3611a78682b2fc30e1220c87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2019
ms.locfileid: "59566343"
---
# <a name="delete-a-recovery-services-vault"></a>删除恢复服务保管库

本文介绍如何删除 [Azure 备份](backup-overview.md)恢复服务保管库。 其中分别说明了如何删除依赖项，以及如何删除保管库和强制删除保管库。


## <a name="before-you-start"></a>开始之前

在开始之前必须知道，无法删除注册了服务器的，或者存有备份数据的恢复服务保管库。

- 若要正常删除某个保管库，请取消注册其中的服务器、删除保管库数据，然后删除该保管库。
- 如果你尝试删除一个仍具有依赖项的保管库，则系统会发出错误消息。 需要手动删除保管库的依赖项，包括：
    - 备份的项
    - 受保护的服务器
    - 备份管理服务器（Azure 备份服务器、DPM）![选择保管库以打开其仪表板](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)
- 如果你不想要保留恢复服务保管库中的任何数据，并想要删除此保管库，可以强制删除它。
- 如果尝试删除保管库但未成功，此保管库仍配置为接收备份数据。


## <a name="delete-a-vault-from-the-azure-portal"></a>在 Azure 门户中删除保管库

1. 打开保管库仪表板。  
2. 在仪表板中单击“删除”。 确认是否要删除。

    ![选择保管库，以打开它的仪表板](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

如果收到错误，请依次删除[备份项](#remove-backup-items)、[基础结构服务器](#remove-backup-infrastructure-servers)、[恢复点](#remove-azure-backup-agent-recovery-points)和保管库。

![删除保管库错误](./media/backup-azure-delete-vault/error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>强制删除恢复服务保管库

可以使用 PowerShell 强制删除保管库。 强制删除意味着永久删除该保管库以及所有关联的备份数据。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


强制删除保管库：

1. 使用 `Connect-AzAccount` 命令登录到 Azure 订阅，并按照屏幕上的说明操作。

   ```powershell
    Connect-AzureRmAccount -Environment AzureChinaCloud
   ```
2. 首次使用 Azure 备份时，必须使用 [Register-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.Resources/Register-azResourceProvider) 在订阅中注册 Azure 恢复服务提供程序。

   ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

3. 使用管理员特权打开 PowerShell 窗口。
4. 运行 `Set-ExecutionPolicy Unrestricted` 命令以撤消任何限制。
5. 运行以下命令，从 chocolately.org 下载 Azure 资源管理器客户端包。

    `iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

6. 运行以下命令，安装 Azure 资源管理器 API 客户端。

   `choco.exe install armclient`

7. 在 Azure 门户中，收集所要删除的保管库的订阅 ID 和资源组名称。
8. 在 PowerShell 中，使用订阅 ID、资源组名称和保管库名称运行以下命令。 运行此命令后，它会删除保管库及其所有依赖项。

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
9. 如果该保管库不是空的，则会出现错误“由于此保管库中存在现有资源，因此无法将其删除”。 若要删除保管库中包含的项，请执行以下操作：

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

10. 在 Azure 门户中，确认要删除该保管库。


## <a name="remove-vault-items-and-delete-the-vault"></a>删除保管库项并删除保管库

这些过程通过一些示例来说明如何删除备份数据和基础结构服务器。 删除保管库中的所有内容后，可以删除该保管库。

### <a name="remove-backup-items"></a>删除备份项

此过程通过一个示例说明如何从 Azure 文件中删除备份数据。

1. 单击“备份项” > “Azure 存储(Azure 文件)”

    ![选择备份类型](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

2. 右键单击要删除的每个 Azure 文件项，然后单击“停止备份”。

    ![选择备份类型](./media/backup-azure-delete-vault/stop-backup-item.png)


3. 在“停止备份” > “选择选项”中，选择“删除备份数据”。
4. 键入项的名称，然后单击“停止备份”。
   - 这表示你确认要删除该项。
   - 确认后，“停止备份”按钮将会激活。
   - 如果保留（而不是删除）该数据，便无法删除保管库。

     ![删除备份数据](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. （可选）提供删除数据的原因，并添加备注。
6. 若要确认删除作业是否已完成，请检查 Azure 消息。 ![删除备份数据](./media/backup-azure-delete-vault/messages.png)上获取。
7. 该作业完成后，服务会发送以下消息：“备份过程已停止，备份数据已删除”。
8. 删除列表中的项后，请单击“备份项”菜单上的“刷新”，以查看保管库中的项。

      ![删除备份数据](./media/backup-azure-delete-vault/empty-items-list.png)


### <a name="remove-backup-infrastructure-servers"></a>删除备份基础结构服务器

1. 在保管库仪表板菜单中，单击“备份基础结构”。
2. 单击“备份管理服务器”以查看服务器。

    ![选择保管库，以打开它的仪表板](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

2. 右键单击该项并选择“删除”。

    ![选择备份类型](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

3. 上获取。 在“停止备份” > “选择选项”中，选择“删除备份数据”。
4. 键入项的名称，然后单击“停止备份”。
   - 这表示你确认要删除该项。
   - 确认后，“停止备份”按钮将会激活。
   - 如果保留（而不是删除）该数据，便无法删除保管库。

     ![删除备份数据](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. （可选）提供删除数据的原因，并添加备注。
6. 若要确认删除作业是否已完成，请检查 Azure 消息。 ![删除备份数据](./media/backup-azure-delete-vault/messages.png)上获取。
7. 该作业完成后，服务会发送以下消息：“备份过程已停止，备份数据已删除”。
8. 删除列表中的项后，请单击“备份项”菜单上的“刷新”，以查看保管库中的项。


### <a name="remove-azure-backup-agent-recovery-points"></a>删除 Azure 备份代理恢复点

1. 在保管库仪表板菜单中，单击“备份基础结构”。
2. 单击“备份管理服务器”以查看基础结构服务器。

    ![选择保管库，以打开它的仪表板](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. 在“受保护的服务器”列表中，单击“Azure 备份代理”。

    ![选择备份类型](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. 在使用 Azure 备份代理保护的服务器列表中单击该服务器。

    ![选择受保护的具体服务器](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. 在选定服务器的仪表板上，单击“删除”。

    ![删除选定服务器](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. 在“删除”菜单上，键入项名称，再单击“删除”。
   - 这表示你确认要删除该项。
   - 确认后，“停止备份”按钮将会激活。
   - 如果保留（而不是删除）该数据，便无法删除保管库。

     ![删除备份数据](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. （可选）提供删除数据的原因，并添加备注。
8. 若要确认删除作业是否已完成，请检查 Azure 消息。 ![删除备份数据](./media/backup-azure-delete-vault/messages.png)上获取。
7. 该作业完成后，服务会发送以下消息：“备份过程已停止，备份数据已删除”。
8. 删除列表中的项后，请单击“备份项”菜单上的“刷新”，以查看保管库中的项。






### <a name="delete-the-vault-after-removing-dependencies"></a>删除依赖项后删除保管库

1. 删除所有依赖项后，在保管库菜单中滚动到“概要”窗格。
2. 确认是否未列出任何“备份项”、“备份管理服务器”或“复制的项”。 如果保管库中仍有项，请删除这些项。

2. 当保管库中没有其他任何项时，请单击保管库仪表板上的“删除”。

    ![删除备份数据](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. 若确认要删除保管库，请单击“是”。 随即会删除该保管库，门户返回“新建”服务菜单。

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>如果我停止了备份过程，但仍保留数据，会怎么样？

如果停止备份过程，但意外保留了数据，则必须先按前面的部分中所述删除备份数据。

## <a name="next-steps"></a>后续步骤

[了解](backup-azure-recovery-services-vault-overview.md)恢复服务保管库。



<!-- Update_Description: wording update -->