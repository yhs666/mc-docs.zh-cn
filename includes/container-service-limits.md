---
title: include 文件
description: include 文件
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: include
origin.date: 10/11/2018
ms.date: 12/03/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 2c14871498661643fa75e6d3eb2e77495d78f2a8
ms.sourcegitcommit: 33421c72ac57a412a1717a5607498ef3d8a95edd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/26/2018
ms.locfileid: "53785201"
---
| 资源 | 默认限制 |
| --- | :--- |
| 每个订阅的最大群集数 | 100 |
| 每个群集的最大节点数 | 100 |
| 每个节点的最大 Pod 数：带 Kubenet 的[基本网络][basic-networking] | 110 |
| 每个节点的最大 Pod 数：带 Azure CNI 的[高级网络][advanced-networking] | Azure CLI 部署：30<sup>1</sup><br />资源管理器模板：30<sup>1</sup><br />门户部署：30 |

<sup>1</sup> 使用 Azure CLI 或资源管理器模板部署 AKS 群集时，此值是可以配置的，最大可以配置为**每节点 110 个 Pod**。 在部署 AKS 群集以后，或者在使用 Azure 门户部署群集的情况下，不能配置每节点的最大 Pod 数。<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://support.azure.cn/zh-cn/support/support-azure/