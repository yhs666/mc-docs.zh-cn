---
title: include 文件
description: include 文件
services: azure-policy
author: WenJason
ms.service: azure-policy
ms.topic: include
origin.date: 05/18/2018
ms.date: 02/18/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 920c25f6e1fa3a3bc2b8b92360a183c7cf94c14d
ms.sourcegitcommit: 2a020ee232b901b13c9f1c4d27ad65228a34d58b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2019
ms.locfileid: "68400200"
---
## <a name="deleting-personal-information"></a>删除个人信息

[!INCLUDE [gdpr-intro-sentence.md](gdpr-intro-sentence.md)]

导入和导出操作期间，个人信息与导入/导出服务（通过门户和 API）相关。 导入/导出过程中使用的数据包括：

- 联系人姓名
- 电话号码
- 电子邮件
- 街道地址
- 城市
- 邮政编码
- 状态
- 国家/地区/省/区域
- 驱动器 ID
- 承运商帐号
- 运输跟踪号

导入/导出作业创建时，用户应提供联系人信息和送货地址。 个人信息最多存储在两个不同位置：作业中和门户设置中（可选）。 如果在导出过程的“回邮送货信息”部分中勾选了标签为“将承运商和回邮地址保存为默认设置”的复选框，则个人信息仅会存储在门户设置中   。

可通过以下方式删除联系人个人信息：

- 与作业一并保存的数据会随作业一并删除。 用户可手动删除作业，已完成作业会在 90 天后自动删除。 可通过 REST API 或 Azure 门户手动删除作业。 若要在 Azure 门户中删除作业，请转到导入/导出作业，然后单击命令栏中的“删除”  。 若要详细了解如何通过 REST API 删除导入/导出作业，请参阅[删除导入/导出作业](../articles/storage/common/storage-import-export-cancelling-and-deleting-jobs.md)。

- 可通过删除门户设置，删除门户设置中保存的联系人信息。 可以按照以下步骤删除门户设置：
  - 登录到 [Azure 门户](https://portal.azure.cn)。
  - 单击“设置”  图标 ![Azure 设置图标](media/storage-import-export-delete-personal-info/azure-settings-icon.png)
  - 单击“导出所有设置”  （以将当前设置保存到 `.json` 文件）。
  - 单击“删除所有设置和专用仪表板”，删除包括保存的联系人信息在内的所有设置  。

有关详细信息，请在[信任中心](https://www.trustcenter.cn/cloudservices/azure.html)查看 Azure 隐私策略