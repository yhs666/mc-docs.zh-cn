---
title: 在 Azure 门户中管理存储帐户设置 - Azure 存储 | Microsoft Docs
description: 了解如何在 Azure 门户中管理存储帐户设置，包括配置访问控制设置、重新生成帐户访问密钥、更改访问层，或修改帐户使用的复制类型。 此外，了解如何在门户中删除存储帐户。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 06/20/2019
ms.date: 08/05/2019
ms.author: v-jay
ms.openlocfilehash: 38481d46730512fe3648dc7e7898a68b0c84c346
ms.sourcegitcommit: 193f49f19c361ac6f49c59045c34da5797ed60ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68732400"
---
# <a name="manage-storage-account-settings-in-the-azure-portal"></a>在 Azure 门户中管理存储帐户设置

[Azure 门户](https://portal.azure.cn)中提供了存储帐户的各种设置。 本文介绍其中的一些设置及其用法。

## <a name="access-control"></a>访问控制

Azure 存储支持通过基于角色的访问控制 (RBAC)，针对 Blob 存储和队列存储使用 Azure Active Directory 进行授权。 有关使用 Azure AD 进行授权的详细信息，请参阅[使用 Azure Active Directory 授予对 Azure blob 和队列的访问权限](storage-auth-aad.md)。

Azure 门户中的“访问控制”设置提供了一种将 RBAC 角色分配给用户、组、服务主体和托管标识的简单方法  。 有关分配 RBAC 角色的详细信息，请参阅[使用 RBAC 管理 blob 和队列数据的访问权限](storage-auth-aad-rbac.md)。

## <a name="tags"></a>Tags

Azure 存储支持 Azure 资源管理器标记，以使用自定义分类组织 Azure 资源。 可将标记应用于存储帐户，以便以逻辑方式在订阅中对其进行分组。

对于存储帐户，标记名称不能超过 128 个字符，标记值不能超过 256 个字符。

有关详细信息，请参阅[使用标记来组织 Azure 资源](../../azure-resource-manager/resource-group-using-tags.md)。

## <a name="access-keys"></a>访问密钥

当你创建存储帐户时，Azure 会生成两个 512 位存储帐户访问密钥。 这些密钥可用于通过共享密钥授权访问你的存储帐户。 可以在不中断应用程序的情况下轮换和重新生成密钥，我们建议定期执行该操作。

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

[!INCLUDE [storage-recommend-azure-ad-include](../../../includes/storage-recommend-azure-ad-include.md)]

### <a name="view-account-keys-and-connection-string"></a>查看帐户密钥和连接字符串

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

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

创建存储帐户后，可以修改其配置。 例如，可以更改数据的复制方式，或者将帐户的访问层从“热”更改为“冷”。 在 [Azure 门户](https://portal.azure.cn)中导航到自己的存储帐户，找到并单击“设置”下的“配置”以查看和/或更改帐户配置   。

更改存储帐户配置可能导致费用增加。 有关更多详细信息，请参阅 [Azure 存储定价](https://azure.cn/pricing/details/storage/)页。

## <a name="delete-a-storage-account"></a>删除存储帐户

若要删除不再使用的存储帐户，请在 [Azure 门户](https://portal.azure.cn)中导航到该存储帐户，然后单击“删除”  。 删除存储帐户将删除整个帐户，包括该帐户中的所有数据。

> [!WARNING]
> 无法恢复已删除的存储帐户，也无法检索删除之前该存储帐户包含的任何内容。 删除帐户前请务必备份要保存的任何内容。 对于帐户中的任务资源也是如此，一旦你删除了一个 Blob、表、队列或文件，则它会被永久删除。
> 

如果尝试删除与 Azure 虚拟机关联的存储帐户，则会显示一条错误消息，指出存储帐户仍在使用。 有关如何排查此错误的帮助，请参阅[排查删除存储帐户时的错误](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md)。

## <a name="next-steps"></a>后续步骤

- [Azure 存储帐户概述](storage-account-overview.md)
- [创建存储帐户](storage-quickstart-create-account.md)
