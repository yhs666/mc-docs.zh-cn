---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 01/22/2019
ms.date: 03/04/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 028e50d1dbfde54138357300b63aaee2d7eb68d4
ms.sourcegitcommit: dd504a2a7f6bc060c3537fe467de518e97c89f8a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57196565"
---
高级存储帐户有以下可伸缩性目标：

| 总帐户容量 | 本地冗余存储帐户的总带宽 |
| --- | --- | 
| 磁盘容量：35 TB <br>快照容量：10 TB | 为入站<sup>1</sup> 和出站<sup>2</sup> 流量提供最高 50 Gbps 的带宽 |

<sup>1</sup> 发送到存储帐户的所有数据（请求）

<sup>2</sup> 从存储帐户接收的所有数据（响应）

如果要对非托管磁盘使用高级存储帐户并且应用程序超过了单个存储帐户的可伸缩性目标，可以考虑迁移到托管磁盘。 如果不想迁移到托管磁盘，请将应用程序构建为使用多个存储帐户。 然后，在这些存储帐户中将数据分区。 例如，如果要将 51-TB 的磁盘附加到多个 VM，请将这些磁盘分散在两个存储帐户中。 35 TB 是单个高级存储帐户的限制。 请确保单个高级存储帐户永远不会超过 35 TB 的设置磁盘。