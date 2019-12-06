---
title: 测试 Azure 虚拟网络中的 Azure 虚拟机网络延迟 | Azure
description: 了解如何测试虚拟网络中 Azure 虚拟机之间的网络延迟
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 10/29/2019
ms.date: 11/25/2019
ms.author: v-yeche
ms.openlocfilehash: 1721baa31052f8ae7931d41085d26c5257be7697
ms.sourcegitcommit: 298eab5107c5fb09bf13351efeafab5b18373901
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2019
ms.locfileid: "74658168"
---
# <a name="testing-network-latency"></a>测试网络延迟

使用专用的工具测量网络延迟可以提供最准确的结果。 市售的工具包括 SockPerf（适用于 Linux）和 Latte（适用于 Windows），它们可以隔离和测量网络延迟，同时排除其他类型的延迟，例如应用程序延迟。 这些工具重点测量影响应用程序性能的网络流量类型，即 TCP 和 UDP。 其他常用连接工具（例如 ping）有时也可用于测量延迟，但这些结果对于实际工作负荷中使用的网络流量而言可能没有代表性。 这是因为，其中的大多数工具使用 ICMP 协议，而应用程序流量处理该协议的方式不同，因此结果可能不适用于使用 TCP 和 UDP 的工作负荷。 在准确测试大多数应用程序所用协议的网络延迟方面，SockPerf（适用于 Linux）和 Latte（适用于 Windows）生成的结果最为相关。 本文档将介绍这两个工具。

## <a name="overview"></a>概述

使用两个 VM（一个充当发送端，另一个充当接收端）建立双向信道，以朝两个方向发送和接收数据包来测量往返时间 (RTT)。

这些步骤可用于测量两个虚拟机 (VM) 甚至两台物理计算机之间的网络延迟。 延迟测量可用于以下方案：

- 为部署的 VM 之间的网络延迟建立基准
- 在进行下述相关更改后，比较网络延迟变化产生的效果：
    - OS 或网络堆栈软件，包括配置更改
    - VM 部署方法，例如部署到某个区域或 PPG
    - VM 属性，例如加速网络或大小更改
    - 虚拟网络，例如路由或筛选更改

### <a name="tools-for-testing"></a>测试工具
为了测试延迟，我们采用了两个不同的选项，一个选项针对基于 Windows 的系统，另一个针对基于 Linux 的系统

对于 Windows：latte.exe (Windows) [https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b)

对于 Linux：SockPerf (Linux) [https://github.com/mellanox/sockperf](https://github.com/mellanox/sockperf)

使用这些工具可确保仅测量 TCP 或 UDP 有效负载传送时间，而不测量应用程序未使用的，且不影响应用程序性能的 ICMP (Ping) 或其他数据包类型。

### <a name="tips-for-an-optimal-vm-configuration"></a>有关最佳 VM 配置的提示：

- 使用最新版本的 Windows 或 Linux
- 启用加速网络以获得最佳结果
    
    <!--Not Available on (/virtual-machines/linux/co-location)-->
    
- 大型 VM 的性能往往优于小型 VM

### <a name="tips-for-analysis"></a>有关分析的提示

- 完成部署、配置和优化后尽早建立基线
- 始终将新结果与基线进行比较，或者在进行受控的更改后将每次测试结果进行比较
- 每当观测到更改或做出规划后重复测试

## <a name="testing-vms-running-windows"></a>测试运行 Windows 的 VM

## <a name="get-latteexe-onto-the-vms"></a>将 Latte.exe 加载到 VM

下载最新版本：[https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b)

考虑将 Latte.exe 放在单独的文件夹中，例如 c:\tools

## <a name="allow-latteexe-through-the-windows-firewall"></a>允许 Latte.exe 通过 Windows 防火墙

在接收端上的 Windows 防火墙中创建“允许”规则，以允许 Latte.exe 流量抵达。 最简单的方法是按名称允许整个 Latte.exe 程序，而不是允许特定的 TCP 端口入站。

如下所示允许 Latte.exe 通过 Windows 防火墙：

netsh advfirewall firewall add rule program=\<PATH\>\\latte.exe name=&quot;Latte&quot; protocol=any dir=in action=allow enable=yes profile=ANY

例如，如果已将 Latte.exe 复制到 &quot;c:\\tools&quot; 文件夹中，则此命令为：

```cmd
netsh advfirewall firewall add rule program=c:\tools\latte.exe name="Latte" protocol=any dir=in action=allow enable=yes profile=ANY
```

## <a name="running-latency-tests"></a>运行延迟测试

在接收端启动 latte.exe（从 CMD 运行，而不要从 PowerShell 运行）：

latte -a &lt;Receiver IP address&gt;:&lt;port&gt; -i &lt;iterations&gt;

大约 65000 次迭代就足以返回代表性的结果。

可输入任意可用端口号。

如果 VM 的 IP 地址为 10.0.0.4，则命令如下：

```cmd
latte -a 10.0.0.4:5005 -i 65100
```

在发送端启动 latte.exe（从 CMD 运行，而不要从 PowerShell 运行）：

latte -c -a \<Receiver IP address\>:\<port\> -i \<iterations\>

生成的命令与接收端上的命令相同，只是添加了 &quot;-c&quot; 来指示这是“客户端”或发送端

```cmd
latte -c -a 10.0.0.4:5005 -i 65100
```

等待结果。 该命令可能需要几分钟时间才能完成，具体取决于 VM 之间的距离。 考虑先运行较少的迭代次数以使测试成功，然后再运行较长的测试。

## <a name="testing-vms-running-linux"></a>测试运行 Linux 的 VM

使用 SockPerf。 可从 [https://github.com/mellanox/sockperf](https://github.com/mellanox/sockperf) 获取该工具

### <a name="install-sockperf-on-the-vms"></a>在 VM 上安装 SockPerf

在 Linux VM（发送端和接收端）上，运行以下命令以在 VM 上准备 SockPerf。 提供的命令适用于主要分发版。

#### <a name="for-centos-follow-these-steps"></a>对于 CentOS，请执行以下步骤：

<!--Not Avaialble on RHEL-->

```bash
# CentOS - Install GIT and other helpful tools
sudo yum install gcc -y -q
sudo yum install git -y -q
sudo yum install gcc-c++ -y
sudo yum install ncurses-devel -y
sudo yum install -y automake
sudo yum install -y autoconf
```

#### <a name="for-ubuntu-follow-these-steps"></a>对于 Ubuntu，请执行以下步骤：
```bash
#Ubuntu - Install GIT and other helpful tools
sudo apt-get install build-essential -y
sudo apt-get install git -y -q
sudo apt-get install -y autotools-dev
sudo apt-get install -y automake
sudo apt-get install -y autoconf
```

#### <a name="for-all-distros-copy-compile-and-install-sockperf-according-to-the-following-steps"></a>对于所有分发版，请按以下步骤复制、编译并安装 SockPerf：
```bash
#Bash - all distros

#From bash command line (assumes git is installed)
git clone https://github.com/mellanox/sockperf
cd sockperf/
./autogen.sh
./configure --prefix=

#make is slower, may take several minutes
make

#make install is fast
sudo make install
```

### <a name="run-sockperf-on-the-vms"></a>在 VM 上运行 SockPerf

SockPerf 安装完成后，便可以在 VM 上运行延迟测试了。 

首先，在接收端启动 SockPerf。

可输入任意可用端口号。 本示例使用端口 12345：
```bash
#Server/Receiver - assumes server's IP is 10.0.0.4:
sudo sockperf sr --tcp -i 10.0.0.4 -p 12345
```
服务器正在侦听，客户端可以通过服务器侦听的端口（在本例中为 12345）开始将数据包发送到服务器。

等待大约 100 秒就足以返回有代表性的结果，如以下示例中所示：
```bash
#Client/Sender - assumes server's IP is 10.0.0.4:
sockperf ping-pong -i 10.0.0.4 --tcp -m 350 -t 101 -p 12345  --full-rtt
```

等待结果。 迭代次数根据 VM 的距离而异。 考虑先运行大约 5 秒的短时间测试以使测试成功，然后再运行较长时间的测试。

本 SockPerf 示例使用 350 字节消息大小，因为它是典型的平均大小数据包。 可以调大或调小消息大小，使结果能够更准确地代表 VM 上运行的工作负荷。

## <a name="next-steps"></a>后续步骤
<!--Not Available on [Azure Proximity Placement Group](/virtual-machines/linux/co-location)-->

* 了解如何根据方案[优化 VM 的网络](../virtual-network/virtual-network-optimize-network-bandwidth.md)。
* 阅读有关如何[为虚拟机分配带宽](../virtual-network/virtual-machine-network-throughput.md)的信息
* 通过 [Azure 虚拟网络常见问题解答 (FAQ)](../virtual-network/virtual-networks-faq.md) 了解详细信息

<!-- Update_Description: new article about virtual network test latency -->
<!--NEW.date: 11/25/2019-->