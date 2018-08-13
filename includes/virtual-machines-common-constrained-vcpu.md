---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/09/2018
ms.date: 07/30/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: d08bf4dac12e7a8c57b35fba55909039d3d80cb2
ms.sourcegitcommit: 62a0389a743cd18665bd127e9bf14800867008eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2018
ms.locfileid: "39625654"
---
某些数据库工作负荷（例如 SQL Server 或 Oracle）要求高内存、高存储和高 I/O 带宽，但不要求高核心计数。 许多数据库工作负荷不是 CPU 密集型的。 Azure 提供特定的 VM 大小，允许你对 VM vCPU 计数进行限制，在保留相同的内存、存储和 I/O 带宽的同时，降低软件许可成本。

可以将 vCPU 计数限制为原始 VM 大小的一半或四分之一。 这些新的 VM 大小有一个用于指定活动 vCPU 数的后缀，方便你对其进行标识。
<!-- Not Available on GS series -->

对 SQL Server 或 Oracle 收取的许可费用受限于新 vCPU 计数，其他产品的费用应以新 vCPU 计数为基础。 这会导致 VM 规格相对于活动（可计费）vCPU 的比率增加 50% 到 75%。 这些新的 VM 大小仅在 Azure 中提供，可以让工作负荷提高 CPU 利用率，所需费用只是许可费用（按核心计）的一部分。 目前的计算费用（包括 OS 许可）仍与原始大小一样。 有关详细信息，请参阅 [Azure VM sizes for more cost-effective database workloads](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/)（数据库工作负荷性价比更高的 Azure VM 大小）。

| Name                | vCPU | 规格           |
|---------------------|------|-----------------|
| Standard_E4-2s_v3   | 2    | 与 E4s_v3 相同  |
| Standard_E8-4s_v3   | 4    | 与 E8s_v3 相同  |
| Standard_E8-2s_v3   | 2    | 与 E8s_v3 相同  |
| Standard_E16-8s_v3  | 8    | 与 E16s_v3 相同 |
| Standard_E16-4s_v3  | 4    | 与 E16s_v3 相同 |
| Standard_E32-16_v3  | 16   | 与 E32s_v3 相同 |
| Standard_E32-8s_v3  | 8    | 与 E32s_v3 相同 |
| Standard_E64-32s_v3 | 32   | 与 E64s_v3 相同 |
| Standard_E64-16s_v3 | 16   | 与 E64s_v3 相同 |
| Standard_DS11-1_v2  | 1    | 与 DS11_v2 相同 |
| Standard_DS12-2_v2  | 2    | 与 DS12_v2 相同 |
| Standard_DS12-1_v2  | 1    | 与 DS12_v2 相同 |
| Standard_DS13-4_v2  | 4    | 同 DS13_v2 |
| Standard_DS13-2_v2  | 2    | 同 DS13_v2 |
| Standard_DS14-8_v2  | 8    | 同 DS14_v2 |
| Standard_DS14-4_v2  | 4    | 同 DS14_v2 |
<!-- Not Available on Standard M, E, GS seriese -->
<!-- Update_Description: wording update-->