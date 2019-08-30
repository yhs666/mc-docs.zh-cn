---
title: include 文件
description: include 文件
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 07/24/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: e742ede73effa5b3ec0b65084da4ffb7d0689912
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993044"
---
* 有效的 Azure 帐户。 （如果没有帐户，只需几分钟即可创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。）

* **Windows**

    * [Python 2.x 或 3.x](https://www.python.org/downloads/)。 请确保根据安装程序的要求，使用 32 位或 64 位安装。 在安装过程中出现提示时，请确保将 Python 添加到特定于平台的环境变量中。 如果使用 Python 2.x，则可能需要[安装或升级 pip  - Python 包管理系统](https://pip.pypa.io/en/stable/installing/)。

    * 如果使用的是 Windows 操作系统，请确保已安装正确版本的 [Visual C++ Redistributable Package](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)，以便使用 Python 中的本机 DLL。 建议使用最新版本。

    * 如果需要，请使用命令 `pip install azure-iothub-device-client` 安装 [azure-iothub-device-client](https://pypi.org/project/azure-iothub-device-client/) 包

    * 如果需要，请使用命令 `pip install azure-iothub-service-client` 安装 [azure-iothub-service-client](https://pypi.org/project/azure-iothub-service-client/) 包

* **Mac OS**

    对于 Mac 操作系统，需要 Python 3.7.0（或 2.7）+ libboost-1.67 + curl 7.61.1（全部通过 homebrew 安装）。 任何其他发行版/操作系统都可能会嵌入不同版本的 boost 和依赖项，这些版本将无法正常工作并将在运行时导致 ImportError。

    适用于 `azure-iothub-service-client` 和 `azure-iothub-device-client` 的 pip  包目前仅供 Windows OS 使用。 对于 Linux/Mac 操作系统，请参阅[准备适用于 Python 的开发环境](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md)一文中特定于 Linux 和 Mac 操作系统的部分。

> [!NOTE]
> 在示例中导入 iothub_client 时出现了几个错误报告。 有关处理 **ImportError** 问题的详细信息，请参阅[处理 ImportError 问题](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues)。
