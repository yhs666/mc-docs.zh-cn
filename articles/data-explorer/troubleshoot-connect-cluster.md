---
title: 排查 Azure 数据资源管理器群集连接失败的问题
description: 本文介绍在 Azure 数据资源管理器中连接到群集的故障排除步骤。
author: orspod
ms.author: v-biyu
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
origin.date: 09/24/2018
ms.date: 05/01/2019
ms.openlocfilehash: bc49fa3e6de9c3e3be5ae829f2ce94bbf02d46a8
ms.sourcegitcommit: bf3df5d77e5fa66825fe22ca8937930bf45fd201
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59686661"
---
# <a name="troubleshoot-failure-to-connect-to-a-cluster-in-azure-data-explorer"></a>故障排除：无法在 Azure 数据资源管理器中连接到群集

如果无法在 Azure 数据资源管理器中连接到群集，请执行以下步骤。

1. 确保连接字符串正确。 其格式应为：`https://<ClusterName>.<Region>.kusto.chinacloudapi.cn`，如以下示例所示：`https://docscluster.westus.kusto.chinacloudapi.cn`。

1. 确保有足够的权限。 如果没有，将收到未经授权的响应。

    有关权限的详细信息，请参阅[管理数据库权限](manage-database-permissions.md)。 如有必要，请与群集管理员联系，使他们可以将你添加为相应的角色。

1. 验证是否已删除群集：查看订阅中的活动日志。

1. 查看 [Azure 服务健康状况仪表板](https://www.azure.cn/zh-cn/home/features/products-by-region)。 在你尝试连接到群集的区域查找 Azure 数据资源管理器的状态。

    如果状态不佳（绿色复选标记），请在状态改善后尝试连接到群集。

1. 解决问题时如仍需帮助，请打开 [Azure 门户](https://portal.azure.cn/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)中的支持请求。