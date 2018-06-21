---
title: Azure AD Connect：后续步骤以及如何管理 Azure AD Connect | Microsoft Docs
description: 了解如何扩展 Azure AD Connect 的默认配置和操作任务。
services: active-directory
documentationcenter: ''
author: alexchen2016
manager: digimobile
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/12/2017
ms.date: 07/31/2017
ms.author: v-junlch
ms.openlocfilehash: 19e4d583613f3af2fd22fa1b96e7dbaa474f578d
ms.sourcegitcommit: cd0f14ddb0bf91c312d5ced9f38217cfaf0667f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2017
ms.locfileid: "20763953"
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>后续步骤以及如何管理 Azure AD Connect
使用本文中介绍的操作流程，根据组织的需要和要求自定义 Azure Active Directory (Azure AD) Connect。  

## <a name="add-additional-sync-admins"></a>添加更多的同步管理员
默认情况下，只有执行安装的用户和本地管理员才能管理安装的同步引擎。 要使其他用户能够访问和管理同步引擎，请在本地服务器上找到名为 ADSyncAdmins 的组，并将这些用户添加到此组中。

## <a name="start-a-scheduled-synchronization-task"></a>启动计划的同步任务
如果需要运行同步任务，可以通过再次运行 Azure AD Connect 向导来执行此操作。  需要提供 Azure AD 凭据。  在向导中，选择“自定义同步选项”任务，并一直单击“下一步”直到完成向导。 最后，请确保已选中“初始配置完成后立即启动同步过程”框。

![启动同步](./media/active-directory-aadconnect-whats-next/startsynch.png)

有关 Azure AD Connect 同步计划程序的详细信息，请参阅 [Azure AD Connect 计划程序](active-directory-aadconnectsync-feature-scheduler.md)。

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Azure AD Connect 中提供的其他任务
在完成 Azure AD Connect 的初始安装后，随时可以从 Azure AD Connect 启动页或桌面快捷方式再次启动向导。  在再次完成向导的过程中，会发现，它会以“其他任务”的形式提供一些新选项。  

下表提供了这些任务的摘要以及每个任务的简要描述。

![其他任务列表](./media/active-directory-aadconnect-whats-next/addtasks.png)

| 其他任务 | 说明 |
| --- | --- |
| **查看所选方案** |查看当前的 Azure AD Connect 解决方案。  包括常规设置、同步的目录和同步设置等。 |
| **自定义同步选项** |更改当前配置，包括在配置中添加其他 Active Directory 林，或启用同步选项（如用户、组、设备或密码写回）。 |
| **启用暂存模式** |对未立即同步且未导出到 Azure AD 或本地 Active Directory 的信息进行暂存。  使用此功能可在同步前进行预览。 |

## <a name="next-steps"></a>后续步骤
了解有关[将本地标识与 Azure Active Directory 集成](active-directory-aadconnect.md)的详细信息。

<!-- Update_Description: wording update -->