---
title: 导出或删除 Azure 门户设置 | Microsoft Docs
description: 了解如何在 Azure 门户中导出或删除用户设置、专用仪表板和自定义设置。
services: azure-portal
keywords: ''
author: santhoshsomayajula
origin.date: 05/18/2018
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: azure-portal
ms.custom: ''
manager: mtillman
ms.author: v-biyu
ms.openlocfilehash: 017912655c5782e0651648db5188c018e35e2963
ms.sourcegitcommit: 9642fa6b5991ee593a326b0e5c4f4f4910f50742
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64855232"
---
# <a name="export-or-delete-user-settings"></a>导出或删除用户设置

你可以使用 Azure 门户中的设置和功能来创建自定义体验。 有关自定义设置的信息存储在 Azure 中。 可以导出或删除以下用户数据：

* Azure 门户中的专用仪表板
* 用户设置，例如最喜欢的订阅或目录，以及上次登录的目录
* 主题和其他自定义门户设置

在删除设置之前，建议先导出并检查设置。 重新生成仪表板或重做自定义设置可能非常耗时。

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="export-or-delete-your-portal-settings"></a>导出或删除门户设置

1. 登录到 [Azure 门户](http://portal.azure.cn)。
2. 在门户的头部区域，选择“设置”。

    ![显示门户设置齿轮的屏幕截图](media/azure-portal-export-delete-settings/azure-portal-settings-icon.png)

3. 选择“导出所有设置”或“删除所有设置和专用仪表板”。

    ![显示了门户导出和删除设置的屏幕截图](media/azure-portal-export-delete-settings/azure-portal-export-delete-settings.png)

      下表介绍了这些操作。

      | 操作 | 说明 |
      | --- | --- |
      | **导出所有设置** | 创建包含你的用户设置（例如颜色主题、收藏夹以及专用仪表板）的一个 .json 文件。|
      | **删除所有设置和专用仪表板** | 删除指向专用仪表板的所有链接以及你对门户所做的其他自定义设置。 |

> [!NOTE]
> 由于用户设置是动态的并且存在数据损坏风险，因此你无法从 .json 文件导入设置。
>
>


## <a name="next-steps"></a>后续步骤

* [创建并共享 Azure 仪表板](azure-portal-dashboard-share-access.md)
* [对收藏夹执行添加、删除和排序操作](azure-portal-add-remove-sort-favorites.md)
