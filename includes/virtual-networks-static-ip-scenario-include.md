---
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 11/09/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.openlocfilehash: 2aa29babfa71e8f401a66753a4402f13f3db8986
ms.sourcegitcommit: db9c7f1a7bc94d2d280d2f43d107dc67e5f6fa4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193183"
---
## <a name="scenario"></a>方案
为了更好地说明如何为 VM 配置静态 IP 地址，本文档使用以下方案。

![VNet 方案](./media/virtual-networks-static-ip-scenario-include/static-ip-scenario.png)

在此方案中，将在 **FrontEnd** 子网中创建一个名为 **DNS01** 的 VM，并将它设置为使用静态 IP 地址 **192.168.1.101**。