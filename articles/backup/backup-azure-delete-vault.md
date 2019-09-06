---
title: 删除 Azure 中的恢复服务保管库
description: 介绍如何删除恢复服务保管库。
services: backup
author: lingliw
manager: digimobile
ms.service: backup
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: v-lingwu
ms.openlocfilehash: 4db6dc60d6eece341d40b289214c198dd80433ce
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70103812"
---
# <a name="delete-a-recovery-services-vault"></a>删除恢复服务保管库

本文介绍如何删除 [Azure 备份](backup-overview.md)恢复服务保管库。 其中分别说明了如何删除依赖项，以及如何删除保管库。


## 开始之前 <a name="before-you-start"></a>

无法删除具有依赖项（例如，与保管库关联的受保护服务器或备份管理服务器）的恢复服务保管库。

- 无法删除包含备份数据的保管库（即，停止了保护，但保留了备份数据）。

- 如果删除包含依赖项的保管库，将显示以下错误：

  ![删除保管库错误](./media/backup-azure-delete-vault/error.png)

- 如果从门户中删除包含依赖项的本地受保护项（在 Azure 中使用 MARS、MABS 或 DPM 进行保护），将显示一条警告消息：

  ![删除受保护服务器错误](./media/backup-azure-delete-vault/error-message.jpg)

  
若要正常删除保管库，请选择与设置相匹配的方案，然后执行建议的步骤：

方案 | 删除依赖项以删除保管库的步骤 |
-- | --
我的本地文件和文件夹已备份到 Azure，并已使用 Azure 备份代理 (MARS) 进行保护 | 执行“删除备份数据和备份项”中的步骤 - [对于 MARS 代理](#delete-backup-items-from-mars-management-console)
我的本地计算机已在 Azure 中使用 MABS（Microsoft Azure 备份服务器）或 DPM (System Center Data Protection Manager) 进行保护 | 执行“删除备份数据和备份项”中的步骤 - [对于 MABS 代理](#delete-backup-items-from-mabs-management-console)
我在云中具有一些受保护的项（例如 laaS VM、Azure 文件共享等）  | 执行“删除备份数据和备份项”中的步骤 - [对于云中的受保护项](#delete-protected-items-in-cloud)
我在本地和云中都有一些受保护的项 | 按以下顺序执行“删除备份数据和备份项”中的步骤： <br> - [对于云中的受保护项](#delete-protected-items-in-cloud)<br> - [对于 MARS 代理](#delete-backup-items-from-mars-management-console) <br> - [对于 MABS 代理](#delete-backup-items-from-mabs-management-console)
我在本地或云中没有任何受保护的项，但仍然收到保管库删除错误 | 执行[使用 Azure 资源管理器客户端删除恢复服务保管库](#delete-the-recovery-services-vault-using-azure-resource-manager-client)中的步骤


## <a name="delete-protected-items-in-cloud"></a>删除云中的受保护项

在继续操作之前，请阅读 **[此部分](#before-you-start)** ，以了解依赖项和保管库删除过程。

若要停止保护并删除备份数据，请执行以下操作：

1. 在门户中选择“恢复服务保管库” > “备份项”，然后选择云中的受保护项（例如 Azure 虚拟机、Azure 存储（Azure 文件）、Azure VM 上的 SQL，等等）。  

    ![选择备份类型](./media/backup-azure-delete-vault/azure-storage-selected.png)

2. 右键单击该备份项。 根据该备份项是否受保护，菜单将显示“停止备份”或“删除备份数据”。  

    - 对于“停止备份”，请从下拉菜单中选择“删除备份数据”。   输入备份项的**名称**（区分大小写），选择**原因**，输入**备注**，然后单击“停止备份”。 

        ![选择备份类型](./media/backup-azure-delete-vault/stop-backup-item.png)

    - 对于“删除备份数据”，请输入备份项的名称（区分大小写），选择**原因**，输入**备注**，然后单击“删除”。   

         ![删除备份数据](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. 检查“通知”![删除备份数据](./media/backup-azure-delete-vault/messages.png)。  完成后，服务将显示以下消息：“正在停止 <备份项> 的备份并删除备份数据。  已成功完成该操作”。 
6. 单击“备份项”菜单中的“刷新”，以检查是否删除了备份项。  

      ![删除备份数据](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="delete-protected-items-on-premises"></a>删除本地受保护的项

在继续操作之前，请阅读 **[此部分](#before-you-start)** ，以了解依赖项和保管库删除过程。

1. 在保管库仪表板菜单中，单击“备份基础结构”  。
2. 根据本地方案选择以下选项：

      - 对于“Azure 备份代理”，请选择“受保护的服务器” > “Azure 备份代理”，然后选择要删除的服务器。    

        ![选择保管库，以打开它的仪表板](./media/backup-azure-delete-vault/identify-protected-servers.png)

      - 对于“Azure 备份服务器”/“DPM”，请选择“备份管理服务器”。    **** 选择要删除的服务器。 


          ![选择保管库，打开其仪表板](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. 此时会显示包含警告消息的“删除”边栏选项卡。 

     ![删除备份数据](./media/backup-azure-delete-vault/delete-protected-server.png)

     查看警告消息，以及许可复选框中提供的说明。
    
    > [!NOTE]
    >- 如果受保护的服务器已与 Azure 服务同步，并且备份项存在，则许可复选框将显示相关备份项的数量，以及用于查看备份项的链接。
    >- 如果受保护的服务器未与 Azure 服务同步，并且备份项存在，则许可复选框将显示备份项的数量。
    >- 如果备份项不存在，则许可复选框将要求确认删除。

4. 选中许可复选框并单击“删除”。 




5. 检查“通知”![删除备份数据](./media/backup-azure-delete-vault/messages.png)。  完成后，服务将显示以下消息：“正在停止 <备份项> 的备份并删除备份数据。  已成功完成该操作”。 
6. 单击“备份项”菜单中的“刷新”，以检查是否删除了备份项。  

现在，可以继续从管理控制台中删除备份项：
    
   - [使用 MARS 保护的项](#delete-backup-items-from-mars-management-console)
    - [使用 MABS 保护的项](#delete-backup-items-from-mabs-management-console)


### 从 MARS 管理控制台中删除备份项 <a name="delete-backup-items-from-mars-management-console"></a>

从 MARS 管理控制台中删除备份项

- 启动 MARS 管理控制台，转到“操作”窗格并选择“计划备份”   。
- 从“修改或停止计划的备份”向导中选择“停止使用此备份计划并删除所有存储的备份”选项，然后单击“下一步”    。

    ![修改或停止计划的备份](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- 在“停止计划的备份”向导中单击“完成”。  

    ![停止计划的备份](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- 系统会提示输入安全 PIN。 若要生成该 PIN，请执行以下步骤：
  - 登录到 Azure 门户。
  - 浏览到“恢复服务保管库”   > “设置”   >   “属性”。
  - 单击“安全 PIN”下的“生成”   。 复制此 PIN。 （此 PIN 的有效时间仅为五分钟。）
- 在管理控制台（客户端应用）中粘贴此 PIN 并单击“确定”。 

  ![安全 PIN](./media/backup-azure-delete-vault/security-pin.png)

- 在“修改备份进度”向导中，会看到“删除的备份数据会保留 14 天。   该时间过后，备份数据会被永久删除。”  

    ![删除备份基础结构](./media/backup-azure-delete-vault/deleted-backup-data.png)

删除本地的备份项以后，请在门户中完成后续步骤。

### <a name="delete-backup-items-from-mabs-management-console"></a>从 MABS 管理控制台中删除备份项 

从 MABS 管理控制台中删除备份项

**方法 1**：若要停止保护并删除备份数据，请执行以下步骤：

1.  在 DPM 管理员控制台中，单击导航栏上的“保护”  。
2.  在显示窗格中，选择要删除的保护组成员。 通过右键单击选择“停止对组成员的保护”选项。 
3.  在“停止保护”对话框中，选择“删除受保护的数据” > “删除在线存储”复选框，然后单击“停止保护”。    

    ![删除在线存储](./media/backup-azure-delete-vault/delete-storage-online.png)

受保护成员状态现在更改为“停用副本可用”。 

4. 右键单击停用保护组，然后选择“删除停用保护”。 

    ![删除停用保护](./media/backup-azure-delete-vault/remove-inactive-protection.png)

5. 在“删除停用保护”窗口中，选择“删除在线存储”，然后单击“确定”。   

    ![删除磁盘上的和联机的副本](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

**方法 2**：启动 **MABS 管理**控制台。 在“选择数据保护方法”部分，取消选择“我需要在线保护”。  

  ![选择数据保护方法](./media/backup-azure-delete-vault/data-protection-method.png)

删除本地的备份项以后，请在门户中完成后续步骤。


## <a name="delete-the-recovery-services-vault"></a>删除恢复服务保管库

1. 删除所有依赖项后，在保管库菜单中滚动到“概要”窗格。 
2. 确认是否未列出任何“备份项”、“备份管理服务器”或“复制的项”。    如果项仍显示在保管库中，请参阅[开始之前](#before-you-start)部分。

3. 当保管库中没有其他任何项时，请单击保管库仪表板上的“删除”  。

    ![删除备份数据](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. 若确认要删除保管库，请单击“是”  。 随即会删除该保管库，门户返回“新建”服务菜单。 

## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>使用 Azure 资源管理器客户端删除恢复服务保管库

仅当已删除所有依赖项后，仍然收到“保管库删除错误”时，才建议使用删除恢复服务保管库的选项。 

- 在“概要”窗格中的保管库菜单内，确认是否未列出任何“备份项”、“备份管理服务器”或“复制的项”。     如果存在备份项，请参阅[开始之前](#before-you-start)部分。
- 重试[从门户删除保管库](#delete-the-recovery-services-vault)。
- 如果删除了所有依赖项，但仍收到“保管库删除错误”，请使用 ARMClient 工具执行以下步骤： 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. 从[此处](https://chocolatey.org/)安装 chocolatey，若要安装 ARMClient，请运行以下命令：

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. 登录到 Azure 帐户并运行以下命令：

    `ARMClient.exe login [environment name]`

3. 在 Azure 门户中，收集所要删除的保管库的订阅 ID 和资源组名称。

有关 ARMClient 命令的详细信息，请参阅此[文档](https://github.com/projectkudu/ARMClient/blob/master/README.md)。

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>使用 Azure 资源管理器客户端删除恢复服务保管库

1. 使用订阅 ID、资源组名称和保管库名称运行以下命令。 如果没有任何依赖项，则运行该命令会删除保管库。

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. 如果该保管库不是空的，则会出现错误“由于此保管库中存在现有资源，因此无法将其删除”。 若要删除保管库中受保护的项/容器，请执行以下操作：

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. 在 Azure 门户中，确认要删除该保管库。


## <a name="next-steps"></a>后续步骤

[了解](backup-azure-recovery-services-vault-overview.md)恢复服务保管库。<br/>
[了解](backup-azure-manage-windows-server.md)如何监视和管理恢复服务保管库。
