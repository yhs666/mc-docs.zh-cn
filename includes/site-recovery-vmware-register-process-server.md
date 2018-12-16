---
author: rockboyfor
ms.service: site-recovery
ms.topic: include
origin.date: 10/26/2018
ms.date: 12/10/2018
ms.author: v-yeche
ms.openlocfilehash: 0b77129af221da74f69e33b6eaf9ec9adc393d72
ms.sourcegitcommit: 5f2849d5751cb634f1cdc04d581c32296e33ef1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53029194"
---
* 使用远程桌面连接连接到进程服务器虚拟机。
* 可通过单击桌面上可用的快捷方式启动 cspsconfigtool.exe。 （如果这是第一次登录到进程服务器，该工具则会自动启动）。
  - 配置服务器的完全限定名称 (FQDN) 或 IP 地址
  - 配置服务器侦听时所在的端口。 值应为 443
  - 连接到配置服务器所需的连接通行短语。
  - 要为该进程服务器配置的数据传输端口。 保留原来的默认值，除非已在环境中将其更改为其他端口号。

    ![注册进程服务器](./media/site-recovery-vmware-register-process-server/register-ps.png)
* 单击保存按钮以保存配置并注册进程服务器。
