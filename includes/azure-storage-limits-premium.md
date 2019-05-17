---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 03/23/2019
ms.date: 04/29/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 8374991fa41528c17d7aef43391b7c648f623d88
ms.sourcegitcommit: 9642fa6b5991ee593a326b0e5c4f4f4910f50742
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64860125"
---
高级性能、常规用途 v1 或 v2 存储帐户有以下可伸缩性目标：

| 总帐户容量                            | 本地冗余存储帐户的总带宽                     |
| ------------------------------------------------- | --------------------------------------------------------------------------- |
| 磁盘容量：35 TB <br>快照容量：10 TB | 为入站<sup>1</sup> 和出站<sup>2</sup> 流量提供最高 50 Gbps 的带宽 |

<sup>1</sup> 发送到存储帐户的所有数据（请求）

<sup>2</sup> 从存储帐户接收的所有数据（响应）

如果要对非托管磁盘使用高级性能存储帐户并且应用程序超过了单个存储帐户的可伸缩性目标，可以考虑迁移到托管磁盘。 如果不想迁移到托管磁盘，请将应用程序构建为使用多个存储帐户。 然后，在这些存储帐户中将数据分区。 例如，如果要将 51-TB 的磁盘附加到多个 VM，请将这些磁盘分散在两个存储帐户中。 35 TB 是单个高级存储帐户的限制。 请确保单个高级性能存储帐户永远不会超过 35 TB 的预配磁盘。