---
title: include 文件
description: include 文件
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: include
origin.date: 09/12/2018
ms.date: 06/28/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 07a22412bba6b1e6ddeb47a85e99cf518f18bdc7
ms.sourcegitcommit: ab87d30f4435c3b7c03f7edd33c9f374b7fe88c9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540086"
---
在开始配置之前，请验证是否符合以下条件：

* 如果已有一个要连接的虚拟网络，请确认本地网络的任何子网都不与要连接到的虚拟网络重叠。 虚拟网络不需要网关子网，并且不能包含任何虚拟网络网关。 如果没有虚拟网络，可以使用本文中的步骤创建一个。
* 获取中心区域的 IP 地址范围。 中心是一个虚拟网络，为中心区域指定的地址范围不能与要连接到的任何现有虚拟网络重叠。 此外，它也不能与本地连接到的地址范围重叠。 如果熟悉本地网络配置中的 IP 地址范围，则需咨询能够提供此类详细信息的人员。
* 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

<!--Update_Description: new articles on virtual wan tutorial wan requirements  -->
<!--ms.date: 07/01/2019-->