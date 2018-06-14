---
title: include 文件
description: include 文件
services: virtual-machines-linux
author: rockboyfor
ms.service: virtual-machines-linux
ms.topic: include
origin.date: 04/16/2018
ms.date: 06/04/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 83b0e6b5d5595c495e4e89e9dafff127402c8d1e
ms.sourcegitcommit: 6f42cd6478fde788b795b851033981a586a6db24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2018
ms.locfileid: "34702892"
---
## <a name="overview-of-ssh-and-keys"></a>SSH 和密钥的概述

SSH 是一种加密的连接协议，可用于通过不安全的连接进行安全登录。 它是适用于 Azure 中托管的 Linux VM 的默认连接协议。 虽然 SSH 本身提供加密连接，但是将密码用于 SSH 连接仍使 VM 易受到强力破解攻击或猜测密码。 使用 SSH 连接到 VM 的更安全且首选的方法是使用公钥-私钥对，也称为 SSH 密钥。 

* *公钥*放置在 Linux VM 上或者要对其使用公钥加密的任何其他服务中。

* 私钥是在建立 SSH 连接时向 Linux VM 呈现的内容，以验证身份。 请保护好私钥， 不要透露给其他人。

根据组织的安全策略，可重复使用单个公钥-私钥对来访问多个 Azure VM 和服务。 无需对要访问的每个 VM 或服务使用单独的密钥对。 

公钥可与任何人共享；但只有你（或本地安全基础结构）才拥有私钥。
<!-- Update_Description: new articles on include file about virtual machines common ssh overview -->
<!--ms.date: 06/04/2018-->