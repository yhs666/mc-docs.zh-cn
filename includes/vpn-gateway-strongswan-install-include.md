---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 08/14/2019
ms.date: 09/02/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 3a2cd01b724d6f9acaa19dc71f5d80e5ab74a2ec
ms.sourcegitcommit: 3f0c63a02fa72fd5610d34b48a92e280c2cbd24a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70131686"
---
以下配置用于执行下面的步骤：

  | | |
  |---|---|
  |Computer| Ubuntu Server 18.04|
  |依赖项| strongSwan |


使用以下命令安装所需的 strongSwan 配置：

```
sudo apt install strongswan
```

```
sudo apt install strongswan-pki
```

```
sudo apt install libstrongswan-extra-plugins
```

使用以下命令安装 Azure 命令行接口：

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

[有关如何安装 Azure CLI 的其他说明](/cli/install-azure-cli-apt?view=azure-cli-latest)
