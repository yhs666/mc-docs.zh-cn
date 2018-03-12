---
title: "从管理门户为 Azure Stack 启用备份 | Microsoft Docs"
description: "通过管理门户启用基础结构备份服务，以便出现故障时可以还原 Azure Stack。"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 12/15/2017
ms.date: 03/01/2018
ms.author: v-junlch
ms.openlocfilehash: 53de6d9750ddcea7f81f57703276c58e57af8fe6
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>从管理门户为 Azure Stack 启用备份

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

通过管理门户启用基础结构备份服务，以便 Azure Stack 可以生成备份。 出现故障时，可以使用这些备份还原环境。

> [!Note]  
> 通过控制台启用备份之前，需要配置备份服务。 可以使用 PowerShell 配置备份服务。 有关详细信息，请参阅[使用 PowerShell 为 Azure Stack 启用备份](azure-stack-backup-enable-backup-powershell.md)。

## <a name="enable-backup"></a>启用备份

1. 打开 Azure Stack 管理门户（网址为 [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external)）。
2. 选择“更多服务” > “基础结构备份”。 在“基础结构备份”边栏选项卡中选择“配置”。

    ![Azure Stack - 备份控制器设置](media\azure-stack-backup\azure-stack-backup-settings.png)上获取。

3. 键入**备份存储位置**的路径。 使用通用命名约定 (UNC) 字符串表示单独的设备上托管的文件共享的路径。 UNC 字符串指定资源（如共享文件或设备）的位置。 对于服务，可以使用 IP 地址。 若要确保备份数据在发生灾难后的可用性，设备应放置在单独的位置。
    > [!Note]  
    > 如果环境支持从 Azure Stack 基础结构网络到企业环境的名称解析，则可以使用 FQDN（而不是 IP）。
4. 使用域和用户名键入**用户名**。 例如，`Contoso\administrator`。
5. 键入用户的**密码**。
5. 再次键入密码以**确认密码**。
6. 在“加密密钥”框中提供预共享密钥。 使用此密钥加密备份文件。 请确保将此密钥存储在安全位置。 首次设置此密钥或将来轮换密钥后，都无法从此界面查看此密钥。 有关生成预共享密钥的详细说明，请按照[使用 PowerShell 为 Azure Stack 启用备份](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key)中的脚本进行操作。 
7. 选择“确定”以保存备份控制器设置。

若要执行备份，需要下载 Azure Stack Tools，然后在 Azure Stack 管理节点上运行 PowerShell cmdlet **Start-AzSBackup**。 有关详细信息，请参阅[备份 Azure Stack](azure-stack-backup-back-up-azure-stack.md )。

## <a name="next-steps"></a>后续步骤

 - 了解如何运行备份。 请参阅[备份 Azure Stack](azure-stack-backup-back-up-azure-stack.md )。
- 了解如何验证备份是否已运行。 请参阅[在管理门户中确认已完成的备份](azure-stack-backup-back-up-azure-stack.md )。

