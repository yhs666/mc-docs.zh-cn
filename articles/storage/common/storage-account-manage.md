---
title: 在 Azure 门户中管理存储帐户设置 - Azure 存储 | Microsoft Docs
description: 了解如何在 Azure 门户中管理存储帐户设置，包括配置访问控制设置、重新生成帐户访问密钥、更改访问层，或修改帐户使用的复制类型。 此外，了解如何在门户中删除存储帐户。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 09/11/2018
ms.date: 09/24/2018
ms.author: v-jay
ms.openlocfilehash: 8a1e87943fec94bf83485a1a08eeb04454b31535
ms.sourcegitcommit: 0081fb238c35581bb527bdd704008c07079c8fbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523743"
---
# <a name="manage-storage-account-settings-in-the-azure-portal"></a>在 Azure 门户中管理存储帐户设置

[Azure 门户](https://portal.azure.cn)中提供了存储帐户的各种设置。 本文介绍其中的一些设置及其用法。

## <a name="access-keys"></a>访问密钥

当你创建存储帐户时，Azure 会生成两个 512 位存储帐户访问密钥。 这些密钥可用于通过共享密钥授权访问你的存储帐户。 可以在不中断应用程序的情况下轮换和重新生成密钥，我们建议定期执行该操作。

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

### <a name="view-and-copy-access-keys"></a>查看和复制访问密钥

若要查看存储帐户凭据，请执行以下操作：

1. 导航到 [Azure 门户](https://portal.azure.cn)。
2. 找到自己的存储帐户。
3. 在存储帐户概述的“设置”部分，选择“访问密钥”。 此时会显示帐户访问密钥，以及每个密钥的完整连接字符串。
4. 找到“key1”下面的“密钥”值，单击“复制”按钮复制该帐户密钥。
5. 或者，可以复制整个连接字符串。 找到“密钥 1”下面的“连接字符串”值，单击“复制”按钮复制该连接字符串。

    ![显示如何在 Azure 门户中查看访问密钥的屏幕截图](media/storage-manage-account/portal-connection-string.png)

### <a name="regenerate-access-keys"></a>重新生成访问密钥

我们建议定期重新生成访问密钥，以帮助保护存储帐户的安全。 系统会分配两个访问密钥，以便可以轮换密钥。 轮换密钥时，需确保应用程序在整个轮换过程中能够持续访问 Azure 存储。 

> [!WARNING]
> 重新生成访问密钥可能会影响依赖于存储帐户密钥的所有应用程序或 Azure 服务。 使用帐户密钥访问存储帐户的任何客户端必须更新为使用新密钥，其中包括媒体服务、云、桌面和移动应用程序，以及适用于 Azure 存储的图形用户界面应用程序，例如 [Azure存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)。 

遵循以下过程轮换存储帐户密钥：

1. 将应用程序代码中的连接字符串更新为使用辅助密钥。
2. 为存储帐户重新生成主访问密钥。 在 Azure 门户中的“访问密钥”边栏选项卡上，单击“重新生成密钥 1”，然后单击“是”确认要生成新密钥。
3. 更新代码中的连接字符串以引用新的主访问密钥。
4. 以相同方式重新生成辅助访问密钥。

## <a name="account-configuration"></a>帐户配置

创建存储帐户后，可以修改其配置。 例如，可以更改数据的复制方式，或者将帐户的访问层从“热”更改为“冷”。 在 [Azure 门户](https://portal.azure.cn)中导航到自己的存储帐户，找到并单击“设置”下的“配置”以查看和/或更改帐户配置。

更改存储帐户配置可能导致费用增加。 有关更多详细信息，请参阅 [Azure 存储定价](https://azure.cn/pricing/details/storage/)页。

## <a name="delete-a-storage-account"></a>删除存储帐户
若要删除不再使用的存储帐户，请在 [Azure 门户](https://portal.azure.cn)中导航到该存储帐户，然后单击“删除”。 删除存储帐户将删除整个帐户，包括该帐户中的所有数据。

> [!WARNING]
> 无法恢复已删除的存储帐户，也无法检索删除之前该存储帐户包含的任何内容。 删除帐户前请务必备份要保存的任何内容。 对于帐户中的任务资源也是如此 — 一旦你删除了一个 Blob、表、队列或文件 ，则它会被永久删除。
> 

如果尝试删除与 Azure 虚拟机关联的存储帐户，则会显示一条错误消息，指出存储帐户仍在使用。 有关如何排查此错误的帮助，请参阅[排查删除存储帐户时的错误](../blobs/storage-troubleshoot-vhds.md)。

## <a name="next-steps"></a>后续步骤

- [Azure 存储帐户概述](storage-account-overview.md)
- [创建存储帐户](storage-quickstart-create-account.md)
