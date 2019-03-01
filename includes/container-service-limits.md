---
title: include 文件
description: include 文件
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: include
origin.date: 10/11/2018
ms.date: 03/04/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: ec34d0c412545fface0fa03c8f4d6d6d0ccc980e
ms.sourcegitcommit: 1e5ca29cde225ce7bc8ff55275d82382bf957413
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "56903348"
---
| 资源 | 默认限制 |
| --- | :--- |
| 每个订阅的最大群集数 | 100 |
| 每个群集的最大节点数 | 100 |
| 每个节点的最大 Pod 数：带 Kubenet 的[基本网络][basic-networking] | 110 |
| 每个节点的最大 Pod 数：带 Azure CNI 的[高级网络][advanced-networking] | Azure CLI 部署：30<sup>1</sup><br />资源管理器模板：30<sup>1</sup><br />门户部署：30 |

<sup>1</sup> 使用 Azure CLI 或资源管理器模板部署 AKS 群集时，此值是可以配置的，最大可以配置为**每节点 110 个 Pod**。 在部署 AKS 群集以后，或者在使用 Azure 门户部署群集的情况下，不能配置每节点的最大 Pod 数。<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://support.azure.cn/zh-cn/support/support-azure/