---
title: "Azure IoT Edge 入门 (Windows) | Azure"
description: "了解如何在 Windows 计算机上构建 Azure IoT Edge 网关，以及 Azure IoT Edge 的重要概念，如模块和 JSON 配置文件。"
services: iot-hub
documentationcenter: 
author: Derek1101
manager: timlt
editor: 
ms.assetid: 9aff3724-5e4e-40ec-b95a-d00df4f590c5
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 06/07/2017
ms.author: v-yiso
ms.custom: H1Hack27Feb2017
ms.date: 07/03/2017
ms.openlocfilehash: 64be8705f01aca50b86c397d3fecac71f7b82aa0
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 在 Windows 上浏览 Azure IoT Edge 体系结构
<a id="explore-azure-iot-edge-architecture-on-windows" class="xliff"></a>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## 如何运行示例
<a id="how-to-run-the-sample" class="xliff"></a>

build.cmd 脚本在 iot-edge 存储库本地副本的 build 文件夹中生成输出。 此输出包括此示例中使用的两个 IoT Edge 模块。

生成脚本将 **logger.dll** 放在 **build\\modules\\logger\\Debug** 文件夹中，将 **hello\_world.dll** 放在 **build\\modules\\hello_world\\Debug** 文件夹中。 如以下 JSON 设置文件中所示，将这些路径用于 **module path** 值。

hello\_world\_sample 过程使用 JSON 配置文件的路径作为命令行参数。 以下示例 JSON 文件在 SDK 存储库的以下路径中提供：**samples\\hello\_world\\src\\hello\_world\_win.json**。 除非修改了生成脚本，将 IoT Edge 模块或示例可执行文件放置在非默认位置，否则，此配置文件可按原样工作。

> [!NOTE]
> 模块路径相对于 hello\_world\_sample.exe 所在的目录。 示例 JSON 配置文件默认为在当前工作目录中写入“log.txt”。

```json
{
  "modules": [
    {
      "name": "logger",
      "loader": {
        "name": "native",
        "entrypoint": {
          "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
        }
      },
      "args": { "filename": "log.txt" }
    },
    {
      "name": "hello_world",
      "loader": {
        "name": "native",
        "entrypoint": {
          "module.path": "..\\..\\..\\modules\\hello_world\\Debug\\hello_world.dll"
        }
      },
      "args": null
      }
  ],
  "links": [
    {
      "source": "hello_world",
      "sink": "logger"
    }
  ]
}
```

1. 导航到 iot-edge 存储库本地副本根目录中的 build 文件夹。

1. 运行以下命令：

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
