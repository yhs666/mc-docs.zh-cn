---
title: Azure IoT Edge 入门 (Linux) | Microsoft Docs
description: 了解如何在 Linux 计算机上构建网关，以及 Azure IoT Edge 的重要概念，如模块和 JSON 配置文件。
services: iot-hub
documentationcenter: ''
author: Derek1101
manager: timlt
editor: ''
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 09/19/2017
ms.author: v-yiso
ms.custom: H1Hack27Feb2017
ms.date: 11/20/2017
ms.openlocfilehash: 9a9067edfb7ee16964394ed5a7cad7e4b50647c9
ms.sourcegitcommit: 9a89fa2b33cbd84be4d8270628567bf0925ae11e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2017
ms.locfileid: "24058551"
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>在 Linux 上浏览 Azure IoT Edge 体系结构

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>如何运行示例

**build.sh** 脚本在 **iot-edge** 存储库本地副本的 **build** 文件夹中生成输出。 此输出包括此示例中使用的两个 IoT Edge 模块。

生成脚本将 **liblogger.so** 放在 **build/modules/logger/** 文件夹中，将 **libhello\_world.so** 放在 **build/modules/hello_world/** 文件夹中。 按示例 JSON 设置文件中所示，将这些路径用于 module path 值。

hello\_world\_sample 过程使用 JSON 配置文件的路径作为命令行参数。 以下示例 JSON 文件在 SDK 存储库的以下路径中提供：**samples/hello\_world/src/hello\_world\_lin.json**。 除非修改了生成脚本，将 IoT Edge 模块或示例可执行文件放置在非默认位置，否则，此配置文件可按原样工作。

> [!NOTE]
> 模块路径相对于从中启动 hello\_world\_sample 可执行文件的当前工作目录，而不是可执行文件所在的目录。 示例 JSON 配置文件默认为在当前工作目录中写入“log.txt”。

```json
{
    "modules" :
    [
        {
            "name" : "logger",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "name" : "hello_world",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/hello_world/libhello_world.so"
            }
            },
            "args" : null
        }
    ],
    "links":
    [
        {
            "source": "hello_world",
            "sink": "logger"
        }
    ]
}
```

1. 导航到 iot-edge 存储库本地副本根目录中的 build 文件夹。

1. 运行以下命令：

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

