---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 09/23/2019
ms.date: 10/28/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 17ab144d982494fd54ec98d1aacce04566dfee83
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72958461"
---
本地冗余存储 (LRS) 在单个数据中心内将数据复制三次。 LRS 在给定的一年内提供至少 99.999999999%（11 个 9）的对象持久性。 与其他选项相比，LRS 是成本最低的复制选项，提供的持久性也最低。

如果发生数据中心级灾难（例如火灾或洪灾），则使用 LRS 的存储帐户中的所有副本都可能会丢失或无法恢复。 为了减轻此风险，Azure 建议使用异地冗余存储 (GRS)。

只有在数据已写入到所有三个副本后，到 LRS 存储帐户的写入请求才会成功返回。

在以下情况下，可能希望使用 LRS：

* 如果应用程序存储着在发生数据丢失时可轻松重构的数据，则可以选择 LRS。
