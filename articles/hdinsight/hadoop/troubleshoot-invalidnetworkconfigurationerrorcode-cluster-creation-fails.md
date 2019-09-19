---
title: 在 Azure HDInsight 中创建群集失败并出现 InvalidNetworkConfigurationErrorCode
description: 在 Azure HDInsight 中创建群集失败并出现 InvalidNetworkConfigurationErrorCode 的各种原因
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 08/05/2019
ms.date: 09/23/2019
ms.openlocfilehash: ef9874522d81e0273964d16b0d94f714bf95531b
ms.sourcegitcommit: 43f569aaac795027c2aa583036619ffb8b11b0b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921375"
---
# <a name="cluster-creation-fails-with-invalidnetworkconfigurationerrorcode-in-azure-hdinsight"></a>在 Azure HDInsight 中创建群集失败并出现 InvalidNetworkConfigurationErrorCode

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方法。

如果看到错误代码 `InvalidNetworkConfigurationErrorCode` 和说明“虚拟网络配置与 HDInsight 要求不兼容”，这往往表示群集的[虚拟网络配置](../hdinsight-plan-virtual-network-deployment.md)有问题。 请根据错误说明中的余下内容，按照以下部分所述解决问题。

## <a name="hostname-resolution-failed"></a>“主机名解析失败”

### <a name="issue"></a>问题

错误说明中包含“主机名解析失败”。

### <a name="cause"></a>原因

此错误与某个自定义 DNS 配置问题相关。 虚拟网络中的 DNS 服务器可以将 DNS 查询转发到 Azure 的递归解析程序，以便解析该虚拟网络中的主机名（有关详细信息，请参阅[虚拟网络中的名称解析](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)）。 可以通过虚拟 IP 168.63.129.16 访问 Azure 的递归解析程序。 只能从 Azure VM 访问此 IP。 因此，如果使用本地 DNS 服务器，或者 DNS 服务器是不属于群集 vNet 的 Azure VM，则此 IP 不起作用。

### <a name="resolution"></a>解决方法

1. 通过 SSH 连接到属于群集的 VM，并运行命令 `hostname -f`。 此命令将返回主机的完全限定域名（在以下说明中称为 `<host_fqdn>`）。

1. 然后运行命令 `nslookup <host_fqdn>`（例如 `nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net`）。 如果此命令将名称解析为 IP 地址，则表示 DNS 服务器工作正常。 在这种情况下，请提交有关 HDInsight 的支持案例，我们将调查你的问题。 请在支持案例中包含执行的故障排除步骤。 这有助于我们更快解决问题。

1. 如果以上命令未返回 IP 地址，请运行 `nslookup <host_fqdn> 168.63.129.16`（例如 `nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net 168.63.129.16`）。 如果此命令能够解析 IP，则表示 DNS 服务器未将查询转发到 Azure 的 DNS，或者它不是与群集处于同一 vNet 中的 VM。

1. 如果你没有任何可充当群集 vNet 中的自定义 DNS 服务器的 Azure VM，则需要先添加此 VM。 在 vNet 中创建一个要配置为 DNS 转发器的 VM。

1. 在 vNet 中部署 VM 后，在此 VM 上配置 DNS 转发规则。 将所有 iDNS 名称解析请求转发到 168.63.129.16，将剩余的请求转发到 DNS 服务器。 [此处](../hdinsight-plan-virtual-network-deployment.md)提供了一个示例来演示如何为自定义 DNS 服务器完成此设置。

1. 添加此 VM 的 IP 地址作为虚拟网络 DNS 配置的第一个 DNS 条目。

---

## <a name="failed-to-connect-to-azure-storage-account"></a>“无法连接到 Azure 存储帐户”

### <a name="issue"></a>问题

错误说明中包含“无法连接到 Azure 存储帐户”或“无法连接到 Azure SQL”。

### <a name="cause"></a>原因

Azure 存储和 SQL 没有固定的 IP 地址，因此，我们需要允许与所有 IP 建立出站连接，以允许访问这些服务。 确切的解决步骤取决于设置的是网络安全组 (NSG) 还是用户定义的规则 (UDR)。 有关这些配置的详细信息，请参阅有关[使用网络安全组和用户定义的路由控制 HDInsight 网络流量](../hdinsight-plan-virtual-network-deployment.md#hdinsight-ip)的部分。

### <a name="resolution"></a>解决方法

* 如果群集使用[网络安全组 (NSG)](../../virtual-network/virtual-network-vnet-plan-design-arm.md)。

    转到 Azure 门户，并找到与其中部署了群集的子网关联的 NSG。 在“出站安全规则”部分，允许不受限制地对 Internet 进行出站访问（请注意，此处的**优先级**编号越小，表示优先级越高）。  另外，在“子网”部分确认此 NSG 是否已应用到群集子网。 

* 如果群集使用[用户定义的路由 (UDR)](../../virtual-network/virtual-networks-udr-overview.md)。

    转到 Azure 门户，并找到与其中部署了群集的子网关联的路由表。 找到子网的路由表后，检查其中的 **routes** 节。

    如果定义了路由，请确保部署了群集的区域的 IP 地址存在路由，并且每个路由的 **NextHopType** 是 **Internet**。 应该为上述文章中所述的每个所需 IP 地址定义一个路由。

---

### <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
