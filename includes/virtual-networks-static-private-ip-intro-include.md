---
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 02/27/2019
ms.date: 04/01/2019
ms.author: v-yeche
ms.openlocfilehash: 30d97b76500c70549de504ad3210f4b1a79234af
ms.sourcegitcommit: 3b05a8982213653ee498806dc9d0eb8be7e70562
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2019
ms.locfileid: "59004537"
---
系统会从指定的范围自动为虚拟机 (VM) 分配专用 IP 地址，具体取决于部署时所在的子网。 该地址会由某个 VM 保留，直到该 VM 被删除。 Azure 从你创建 VM 时所在的子网动态分配下一个可用的专用 IP 地址。 如果需要已分配给 VM 的子网中的特定 IP 地址，请分配静态 IP 地址。