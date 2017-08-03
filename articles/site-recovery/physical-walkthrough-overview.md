---
title: "使用 Azure Site Recovery 将物理本地服务器复制到 Azure | Azure"
description: "概述如何使用 Azure Site Recovery 服务将本地 Windows/Linux 物理服务器上运行的工作负荷复制到 Azure。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/27/2017
ms.date: 07/31/2017
ms.author: v-yeche
ms.openlocfilehash: e56975dcd76f5c740f7a094df4b2e7a2aff58924
ms.sourcegitcommit: 66db84041f1e6e77ef9534c2f99f1f5331a63316
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="replicate-physical-servers-to-azure-with-site-recovery"></a>使用 Site Recovery 将物理服务器复制到 Azure

本文概述在 Azure 门户中使用 [Azure Site Recovery](site-recovery-overview.md) 服务将本地 Windows/Linux 物理服务器复制到 Azure 的必要步骤。

## <a name="step-1-review-architecture-and-prerequisites"></a>步骤 1：查看体系结构和先决条件

开始部署前，先查看方案体系结构，并务必了解创建部署所需的全部组件。

转到[步骤 1：查看体系结构](physical-walkthrough-architecture.md)

## <a name="step-2-review-prerequisites"></a>步骤 2：查看先决条件

务必满足每个部署组件的先决条件：

- **Azure 先决条件**：必须有 Azure 帐户、Azure 网络和存储帐户。
- **本地 Site Recovery 组件**：必须有运行本地 Site Recovery 组件的计算机。
- **复制的计算机**：要复制的服务器必须遵守本地和 Azure 要求。

转到[步骤 2：查看先决条件和限制](physical-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>步骤 3：规划容量

必须确定所需的复制资源，才能执行完整部署。 若要执行快速部署来测试环境，可以跳过这一步。

转到[步骤 3：规划容量](physical-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>步骤 4：规划网络

需要执行一些网络规划，确保在故障转移发生后 Azure VM 可以连接到网络，并具有正确的 IP 地址。

转到[步骤 4：规划网络](physical-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>步骤 5：准备 Azure 资源

开始前，设置 Azure 网络和存储。 

转到[步骤 5：准备 Azure 资源](physical-walkthrough-prepare-azure.md)

## <a name="step-6-set-up-a-vault"></a>步骤 6：创建保管库

创建恢复服务保管库来安排和管理复制。 创建保管库时，指定要复制的内容，以及将内容复制到的目标位置。

转到[步骤 6：创建保管库](physical-walkthrough-create-vault.md)

## <a name="step-7-configure-source-and-target-settings"></a>步骤 7：配置源和目标设置

配置源和目标 (Azure) 站点的设置。 源设置包括运行统一安装程序来安装本地 Site Recovery 组件。

转到[步骤 7：设置源和目标](physical-walkthrough-source-target.md)

## <a name="step-8-set-up-a-replication-policy"></a>步骤 8：设置复制策略

设置复制策略，以指定应如何复制物理服务器。

转到[步骤 8：设置复制策略](physical-walkthrough-replication.md)

## <a name="step-9-install-the-mobility-service"></a>步骤 9：安装移动服务

必须在要复制的每个服务器上安装移动服务。 可通过多种方法（推送或拉取安装）安装移动服务。

转到[步骤 9：安装移动服务](physical-walkthrough-install-mobility.md)

## <a name="step-10-enable-replication"></a>步骤 10：启用复制

在服务器上运行移动服务后，可为它启用复制。 启用后，将进行 VM 初始复制。

转到[步骤 10：启用复制](physical-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>步骤 11：运行测试故障转移

初始复制完成并运行增量复制后，可以运行测试故障转移，以确保一切都符合预期。

转到[步骤 11：运行测试故障转移](physical-walkthrough-test-failover.md)

<!--Update_Description: new article about walkthrought overview from physical to azure -->