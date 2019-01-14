---
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 11/09/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.openlocfilehash: f48df3ee1947827ca1277712b7666fdfdd8fb307
ms.sourcegitcommit: db9c7f1a7bc94d2d280d2f43d107dc67e5f6fa4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193182"
---
虚拟网络中的 IaaS 虚拟机 (VM) 和 PaaS 角色实例根据它们连接到的子网自动接收来自你指定的范围的专用 IP 地址。 该地址会被这些 VM 和角色实例保留，直到这些 VM 和角色实例被停用。 通过从 PowerShell、Azure CLI 或 Azure 门户停止 VM 或角色实例来解除其授权。 在这些情况下，重新启动该 VM 或角色实例后，它会从 Azure 基础结构接收一个可用的 IP 地址，该 IP 地址可能与以前使用的 IP 地址不相同。 如果从来宾操作系统关闭 VM 或角色实例，该 VM 或角色实例将保留它的 IP 地址。  

在某些情况下（例如，如果你的 VM 要运行 DNS 或者将作为域控制器），你会希望 VM 或角色实例具有静态 IP 地址。 可通过设置静态专用 IP 地址来实现此目的。