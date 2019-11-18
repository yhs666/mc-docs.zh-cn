---
title: Azure Functions 异地灾难恢复 | Microsoft Docs
description: 如何使用地理区域进行故障转移并在 Azure Functions 中使用灾难恢复。
services: functions
documentationcenter: na
author: wesmc7777
manager: jeconnoc
keywords: azure functions, 模式, 最佳做法, functions
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: azure-functions
ms.topic: conceptual
origin.date: 08/29/2019
ms.date: 10/28/2019
ms.author: v-junlch
ms.openlocfilehash: 28e37b289aef8919c11621b3323ed3c8f5031922
ms.sourcegitcommit: 7d2ea8a08ee329913015bc5d2f375fc2620578ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034477"
---
# <a name="azure-functions-geo-disaster-recovery"></a>Azure Functions 异地灾难恢复

在整个区域或数据中心遭遇停机的情况下，在不同区域进行计算以实现连续处理就显得至关重要。  本文将介绍一些可以用来部署函数以实现灾难恢复的策略。

## <a name="basic-concepts"></a>基本概念

Azure Functions 在特定区域运行。  若要提高可用性，可将相同函数部署到多个区域。  在多个区域中时，可以让函数以“主动/主动”或“主动/被动”模式运行。    

* 主动/主动。 两个区域都主动接收事件（采用重复或循环方式）。 建议将“主动/主动”用于 HTTPS 函数，与 Azure Front Door 组合使用。
* 主动/被动。 一个区域主动接收事件，另一个区域（次要区域）处于空闲状态。  需要进行故障转移时，次要区域激活，开始接管处理操作。  建议将它用于非 HTTP 功能，例如服务总线和事件中心。

## <a name="activeactive-for-non-https-functions"></a>用于非 HTTPS 功能的“主动/主动”模式

仍可针对非 HTTPS 功能实现“主动/主动”部署。  但是，需要考虑这两个区域如何互相交互或协调。  如果向两个区域部署了同一函数应用，每个区域都在同一服务总线队列上触发，那么在对该队列执行取消排队操作时，这两个区域属于竞争性使用者。  虽然这意味着每条消息仅由多个实例中的一个进行处理，但也意味着在单个服务总线上仍然存在单点故障。  如果部署两个服务总线队列（一个在主要区域，一个在次要区域），这两个函数应用指向其区域队列，则现在的难点在于，如何在这两个区域之间分发队列消息。  通常情况下，这意味着每个发布者都会尝试将消息发布到两个区域，  每条消息都由两个活动的函数应用进行处理。  虽然这样会形成一个“主动/主动”模式，但也会在复制计算以及在何时合并数据或以何种方式合并数据方面制造其他难题。  出于这些原因，建议由非 HTTPS 触发器来使用“主动/主动”模式。

## <a name="activepassive-for-non-https-functions"></a>用于非 HTTPS 功能的“主动/被动”模式

使用“主动/被动”模式时，每次只有一个函数处理一条消息，但在发生灾难时可以故障转移到次要区域。  Azure Functions 可以与 [Azure 服务总线异地恢复](../service-bus-messaging/service-bus-geo-dr.md)配合使用。


