---
title: 在删除 Azure SQL 数据库托管实例后删除子网 | Microsoft Docs
description: 了解如何在删除 Azure SQL 数据库托管实例后删除 Azure 虚拟网络。
services: sql-database
ms.service: sql-database
ms.subservice: management
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: douglas, carlrab, sstein
manager: digimobile
origin.date: 06/26/2019
ms.date: 08/19/2019
ms.openlocfilehash: 85811e803711c6a95861ccfec23e110fe5ff6af9
ms.sourcegitcommit: 52ce0d62ea704b5dd968885523d54a36d5787f2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69544191"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-database-managed-instance"></a>在删除 Azure SQL 数据库托管实例后删除子网

本文提供有关如何在删除子网中的最后一个 Azure SQL 数据库托管实例后，手动删除该子网的指导。

SQL 数据库使用[虚拟群集](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture)来存储删除的托管实例。 此虚拟群集可以在实例删除后存在 12 小时，让你可以在同一子网中快速创建托管实例。 可以免费保留空的虚拟群集。 在此期间，无法删除与该虚拟群集关联的子网。

如果不希望等待 12 小时，而更愿意立即删除虚拟群集及其子网，可以手动这样做。 请通过 Azure 门户或虚拟群集 API 手动删除虚拟群集。

> [!NOTE]
> 若要成功删除，该虚拟群集不能包含任何托管实例。

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>在 Azure 门户中删除虚拟群集

若要使用 Azure 门户删除虚拟群集，请搜索虚拟群集资源。

![Azure 门户的屏幕截图，其中突出显示了搜索框](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

找到要删除的虚拟群集后，请选择此资源，然后选择“删除”。  系统会提示你确认删除该虚拟群集。

![Azure 门户“虚拟群集”仪表板的屏幕截图，其中突出显示了“删除”选项](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

Azure 门户通知区域会显示已删除虚拟群集的确认消息。 成功删除虚拟群集后，会立即释放子网，供重复使用。

> [!TIP]
> 如果没有托管实例显示在虚拟群集中，而你无法删除虚拟群集，请确保没有正在进行的实例部署。 这包括已启动的和已取消的仍在进行的部署。 实例部署到的资源组的“查看部署”选项卡会指示正在进行的任何部署。 在这种情况下，请等待部署完成，删除托管的实例，然后删除虚拟群集。

## <a name="delete-virtual-cluster-by-using-the-api"></a>使用 API 删除虚拟群集

若要通过 API 删除虚拟群集，请使用[虚拟群集删除方法](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete)中指定的 URI 参数。

## <a name="next-steps"></a>后续步骤

- 有关概述，请参阅[什么是托管实例？](sql-database-managed-instance.md)。
- 了解[托管实例中的连接体系结构](sql-database-managed-instance-connectivity-architecture.md)。
- 了解如何[修改托管实例的现有虚拟网络](sql-database-managed-instance-configure-vnet-subnet.md)
- 有关如何创建虚拟网络、创建托管实例，以及从数据库备份还原数据库的教程，请参阅[创建 Azure SQL 数据库托管实例](sql-database-managed-instance-get-started.md)。
- 有关 DNS 问题，请参阅[配置自定义 DNS](sql-database-managed-instance-custom-dns.md)。
