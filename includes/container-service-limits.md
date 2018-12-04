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
ms.openlocfilehash: fac88c526d34fe63c181babb0d1c950abf133ebf
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676550"
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
[azure-support]: https://www.azure.cn/support/support-azure