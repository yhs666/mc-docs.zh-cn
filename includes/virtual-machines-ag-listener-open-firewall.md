---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 3e51d6a91c61299189e07e1200681991ff29ce05
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676156"
---
在此步骤中，创建一个防火墙规则以打开负载均衡终结点的探测端口（同样采用之前指定的 59999）和另一规则来打开可用性组侦听程序端口。 由于在包含可用性组副本的 VM 上创建了负载均衡的终结点，需要打开相应 VM 上的探测端口和侦听程序端口。

1. 在托管副本的虚拟机上，启动“具有高级安全性的 Windows 防火墙”。

2. 右键单击“入站规则”，然后单击“新建规则”。

3. 在“规则类型”页中，选择“端口”，然后单击“下一步”。

4. 在“协议和端口”页面中，选择“TCP”，然后选择“特定本地端口”框中的类型“59999”，再单击“下一步”。

5. 在“操作”页上，保持选中“允许连接”，并单击“下一步”。

6. 在“配置文件”页上，接受默认设置，然后单击“下一步”。

7. 在“名称”页的“名称”文本框中指定规则名称，例如 **Always On Listener Probe Port**，然后单击“完成”。

8. 为可用性组侦听程序端口重复上述步骤（按之前在脚本的 $EndpointPort 参数中指定的那样），然后指定合适的规则名称，例如 **Always On Listener Port**。

<!-- Update_Description: update meta properties -->