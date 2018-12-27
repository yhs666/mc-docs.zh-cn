---
title: 使用 Python 将事件发送到 Azure 事件中心 | Azure
description: 使用 Python 将事件发送到事件中心入门
services: event-hubs
author: ShubhaVijayasarathy
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
origin.date: 07/26/2018
ms.date: 12/10/2018
ms.author: v-biyu
ms.openlocfilehash: 51e484005574cc409320666e6587ec37791db91a
ms.sourcegitcommit: 547436d67011c6fe58538cfb60b5b9c69db1533a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676888"
---
<!-- Verify successfully-->
# <a name="send-events-to-event-hubs-using-python"></a>使用 Python 将事件发送到事件中心

Azure 事件中心是一个大数据流式处理平台和事件引入服务，每秒能够接收和处理数百万个事件。 事件中心可以处理和存储分布式软件和设备生成的事件、数据或遥测。 可以使用任何实时分析提供程序或批处理/存储适配器转换和存储发送到数据中心的数据。 有关事件中心的详细概述，请参阅[事件中心概述](event-hubs-about.md)和[事件中心功能](event-hubs-features.md)。

本教程介绍如何从以 Python 编写的应用程序中将事件发送到事件中心。 

> [!NOTE]
> 可以从 [GitHub](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) 下载此用作示例的快速入门，将 `EventHubConnectionString` 和 `EventHubName` 字符串替换为事件中心值，并运行它。 或者，可以按照本教程中的步骤创建自己的解决方案。

## <a name="prerequisites"></a>先决条件

若要完成本教程，需要满足以下先决条件：

- Python 3.4 或更高版本。


## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>创建事件中心命名空间和事件中心
第一步是使用 [Azure 门户](https://portal.azure.cn)创建事件中心类型的命名空间，并获取应用程序与事件中心进行通信所需的管理凭据。 若要创建命名空间和事件中心，请按照[本文](event-hubs-create.md)中的步骤进行操作，然后继续执行本教程的以下步骤。

## <a name="install-python-package"></a>安装 Python 包

若要安装事件中心的 Python 包，请打开其路径中包含 Python 的命令提示符，然后运行以下命令： 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-send-events"></a>创建用于发送事件的 Python 脚本

接下来，创建将事件发送到事件中心的 Python 应用程序：

1. 打开常用的 Python 编辑器，如 [Visual Studio Code][Visual Studio Code]。
2. 创建名为 send.py 的脚本。 此脚本将向事件中心发送 100 个事件。
3. 将以下代码粘贴到 send.py 中，将 ADDRESS、USER 和 KEY 值替换为你在上一节中从 Azure 门户获取的值： 

```python
import sys
import logging
import datetime
import time
import os

from azure.eventhub import EventHubClient, Sender, EventData

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.chinacloudapi.cn/myeventhub"
# "amqps://<mynamespace>.servicebus.chinacloudapi.cn/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.chinacloudapi.cn/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"

try:
    if not ADDRESS:
        raise ValueError("No EventHubs URL supplied.")

    # Create Event Hubs client
    client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
    sender = client.add_sender(partition="0")
    client.run()
    try:
        start_time = time.time()
        for i in range(100):
            print("Sending message: {}".format(i))
            sender.send(EventData(str(i)))
    except:
        raise
    finally:
        end_time = time.time()
        client.stop()
        run_time = end_time - start_time
        logger.info("Runtime: {} seconds".format(run_time))

except KeyboardInterrupt:
    pass
```

## <a name="run-application-to-send-events"></a>运行应用程序来发送事件

若要运行此脚本，请打开其路径中包含 Python 的命令提示符，然后运行以下命令：

```bash
start python send.py
```

祝贺！ 现在已向事件中心发送消息。
 
## <a name="next-steps"></a>后续步骤
在此快速入门中，已使用 Python 向事件中心发送消息。 若要了解如何使用 Python 从事件中心接收事件，请参阅[从事件中心接收事件 - Python](event-hubs-python-get-started-receive.md)。

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[trial account]: https://www.azure.cn/pricing/1rmb-trial/
<!-- Update_Description: new articles on event hubs python get started send -->

<!--ms.date: 09/30/2018-->