---
title: "将 HDInsight 连接到本地网络 - Azure HDInsight | Azure"
description: "了解如何在 Azure 虚拟网络中创建 HDInsight 群集，然后将其连接到本地网络。 了解如何使用自定义 DNS 服务器在 HDInsight 和本地网络之间配置名称解析。"
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 07/11/2017
ms.date: 07/31/2017
ms.author: v-dazen
ms.openlocfilehash: 5e273d889eee561e5b1823ead3ad0a4a3ca8f9cd
ms.sourcegitcommit: 2e85ecef03893abe8d3536dc390b187ddf40421f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="connect-hdinsight-to-your-on-premise-network"></a>将 HDInsight 连接到本地网络

了解如何使用 Azure 虚拟网络和 VPN 网关将 HDInsight 连接到本地网络。 本文档提供以下信息：

* 如何创建连接到本地网络的 Azure 虚拟网络。

* 如何在虚拟网络和本地网络之间启用 DNS 名称解析。

* 如何使用网络安全组限制从 Internet 到 HDInsight 的访问。

* 如何发现 HDInsight 在虚拟网络上提供的端口。

## <a name="create-the-virtual-network-configuration"></a>创建虚拟网络配置

请参阅以下文档，了解如何创建连接到本地网络的 Azure 虚拟网络：

* [使用 Azure 门户](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [使用 Azure PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [使用 Azure CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a>配置名称解析

若要让 HDInsight 和已连接网络中的资源通过名称通信，必须执行以下操作：

* 在 Azure 虚拟网络中创建自定义 DNS 服务器。

* 将虚拟网络配置为使用自定义 DNS 服务器而非默认的 Azure 递归解析程序。

* 配置自定义 DNS 服务器和本地 DNS 服务器之间的转发。

该配置启用以下行为：

* 将完全限定的域名（其中包含虚拟网络的 DNS 后缀）的请求转发到自定义 DNS 服务器。 自定义 DNS 服务器然后会将这些请求转发到 Azure 递归解析程序，由后者返回 IP 地址。

* 将所有其他请求转发到本地 DNS 服务器。 甚至会将对公共 Internet 资源（例如 microsoft.com）的请求转发到本地 DNS 服务器进行名称解析。

### <a name="create-a-custom-dns-server"></a>创建自定义 DNS 服务器

> [!IMPORTANT]
> 必须先创建并配置 DNS 服务器，此后再将 HDInsight 安装到虚拟网络中。

若要创建使用 [Bind](https://www.isc.org/downloads/bind/) DNS 软件的 Linux VM，请执行以下步骤：

> [!NOTE]
> 以下步骤使用 [Azure 门户](https://portal.azure.cn)来创建 Azure 虚拟机。 若要了解创建虚拟机的其他方式，请参阅[创建 VM - Azure CLI](../virtual-machines/linux/quick-create-cli.md) 和[创建 VM - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) 文档。

1. 在 [Azure 门户](https://portal.azure.cn)中，选择“+”、“虚拟机”、“全部查看”、“Ubuntu Server 16.04 LTS”。

2. 在“基本信息”  边栏选项卡中输入以下信息：

    * 名称：用于标识此虚拟机的友好名称。 例如，DNSProxy。
    * 用户名：SSH 帐户的名称。
    * SSH 公钥或密码：SSH 帐户的身份验证方法。 建议使用公钥，因为更安全。 有关详细信息，请参阅[为 Linux VM 创建和使用 SSH 密钥](../virtual-machines/linux/mac-create-ssh-keys.md)文档。
    * 资源组：选择“使用现有资源组”，然后选择包含此前创建的虚拟网络的资源组。
    * 位置：选择虚拟网络所在的位置。

    ![虚拟机基本配置](./media/connect-on-premises-network/vm-basics.png)

    将其他项保留默认值，然后选择“确定”。

3. 在“选择大小”边栏选项卡中，选择 VM 大小。 就本教程来说，请选择规模最小且成本最低的选项。 若要继续，请使用“选择”按钮。

4. 在“设置”  边栏选项卡中输入以下信息：

    * 虚拟网络：选择此前创建的虚拟网络。

    * 子网：选择虚拟网络的默认子网。 请勿选择 VPN 网关使用的子网。

    * 诊断存储帐户：选择现有的存储帐户，或新建一个。

    ![虚拟网络设置](./media/connect-on-premises-network/virtual-network-settings.png)

    将其他项保留默认值，然后选择“确定”继续。

5. 在“购买”边栏选项卡中，选择用于创建虚拟机的“购买”按钮。

6. 创建虚拟机以后，将会显示其“概览”边栏选项卡。 从左侧列表中选择“属性”。 保存“公共 IP 地址”和“专用 IP 地址”值。 下一部分会用到它。

    ![公共和专用 IP 地址](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>安装和配置 Bind（DNS 软件）

1. 使用 SSH 连接到虚拟机的公共 IP 地址。 以下示例连接到位于 40.68.254.142 的虚拟机：

    ```bash
    ssh 40.68.254.142
    ```

2. 若要安装 Bind，请通过 SSH 会话使用以下命令：

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. 若要配置 Bind，以便将名称解析请求转发到本地 DNS 服务器，请使用以下文本作为 `/etc/bind/named.conf.options` 文件的内容：

        acl goodclients {
            10.0.0.0/16; # Replace with the IP address range of the virtual network
            10.1.0.0/16; # Replace with the IP address range of the on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with the IP address of the on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform to RFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > 将 `goodclients` 节中的值替换为虚拟网络和本地网络的 IP 地址范围。 此节定义此 DNS 服务器从其接受请求的地址。
    >
    > 将 `forwarders` 节中的 `192.168.0.1` 项替换为本地 DNS 服务器的 IP 地址。 此项将 DNS 请求路由到本地 DNS 服务器进行解析。

    若要编辑该文件，请使用以下命令：

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    若要保存文件，请使用 Ctrl+X、Y，然后按 Enter。

4. 在 SSH 会话中使用以下命令：

    ```bash
    hostname -f
    ```

    此命令返回类似于以下文本的值：

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.chinacloudapp.cn

    `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.chinacloudapp.cn` 文本是此虚拟网络的 DNS 后缀。 保存该值，因为以后会用到。

5. 若要配置 Bind，以便为虚拟网络中的资源解析 DNS 名称，请使用以下文本作为 `/etc/bind/named.conf.local` 文件的内容：

        // Replace the following with the DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.chinacloudapp.cn" {
            type forward;
            forwarders {168.63.129.16;}; # The Azure recursive resolver
        };

    > [!IMPORTANT]
    > 必须将 `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.chinacloudapp.cn` 替换为此前检索的 DNS 后缀。

    若要编辑该文件，请使用以下命令：

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    若要保存文件，请使用 Ctrl+X、Y，然后按 Enter。

6. 若要启动 Bind，请使用以下命令：

    ```ssh
    sudo service bind9 restart
    ```

7. 若要验证 Bind 能否解析本地网络中资源的名称，请使用以下命令：

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > 将 `dns.mynetwork.net` 替换为本地网络中资源的完全限定的域名 (FQDN)。
    >
    > 将 `10.0.0.4` 替换为虚拟网络中自定义 DNS 服务器的内部 IP 地址。

    显示的响应类似于以下文本：

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>将虚拟网络配置为使用自定义 DNS 服务器

若要将虚拟网络配置为使用自定义 DNS 服务器而非 Azure 递归解析程序，请执行以下步骤：

1. 在 [Azure 门户](https://portal.azure.cn)中选择虚拟网络，然后选择“DNS 服务器”。

2. 选择“自定义”，并输入自定义 DNS 服务器的内部 IP 地址。 最后，选择“保存”。

    ![设置网络的自定义 DNS 服务器](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-the-on-premises-dns-server"></a>配置本地 DNS 服务器

前一部分已将自定义 DNS 服务器配置为将请求转发到本地 DNS 服务器。 接下来，必须将本地 DNS 服务器配置为将请求转发到自定义 DNS 服务器。

有关 DNS 服务器配置的具体步骤，请参阅 DNS 服务器软件的文档。 请查找配置条件转发器的步骤。

条件转发仅转发对特定 DNS 后缀的请求。 在此示例中，必须为虚拟网络的 DNS 后缀配置转发器。 对此后缀的请求应转发到自定义 DNS 服务器的 IP 地址。 

以下文本是 Bind DNS 软件的条件转发器配置的示例：

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.chinacloudapp.cn" {
        type forward;
        forwarders {10.0.0.4;}; # The custom DNS server
    };

若要了解如何在 Windows Server 2016 上使用 DNS，请参阅 [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) 文档。

配置本地 DNS 服务器以后，即可在本地网络中使用 `nslookup` 来验证能否解析虚拟网络中的名称。 下面为示例 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.chinacloudapp.cn 196.168.0.4
```

该示例使用位于 196.168.0.4 的本地 DNS 服务器来解析自定义 DNS 服务器的名称。 将该 IP 地址替换为本地 DNS 服务器的 IP 地址。 将 `dnsproxy` 地址替换为自定义 DNS 服务器的完全限定的域名。

## <a name="optional-control-network-traffic"></a>可选：控制网络流量

可以使用网络安全组 (NSG) 或用户定义路由 (UDR) 来控制网络通信。 NSG 用于筛选入站和出站流量，以及允许或拒绝流量。 UDR 用于控制虚拟网络、Internet 和本地网络中各资源之间的流量。

> [!WARNING]
> HDInsight 要求从 Azure 云中的特定 IP 地址进行入站访问，以及进行不受限制的出站访问。 使用 NSG 或 UDR 控制流量时，必须执行以下步骤：
>
> 1. 找到虚拟网络所在位置的 IP 地址。 如需按位置列出的必需 IP，请参阅[必需 IP 地址](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip)。
>
> 2. 允许来自 IP 地址的入站流量。
>
>    * NSG：在端口 443 上允许来自 Internet 的入站流量。
>    * UDR：将路由的“下一跃点”类型设置为“Internet”。

如需使用 Azure PowerShell 或 Azure CLI 来创建 NSG 的示例，请参阅[使用 Azure 虚拟网络扩展 HDInsight](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) 文档。

## <a name="create-the-hdinsight-cluster"></a>创建 HDInsight 群集

> [!WARNING]
> 必须先配置自定义 DNS 服务器，然后才能将 HDInsight 安装到虚拟网络中。

通过[使用 Azure 门户创建 HDInsight 群集](./hdinsight-hadoop-create-linux-clusters-portal.md)文档中的步骤创建 HDInsight 群集。

> [!WARNING]
> * 在群集创建过程中，必须选择虚拟网络所在的位置。
>
> * 在配置的“高级设置”部分，必须选择此前创建的虚拟网络和子网。

## <a name="connecting-to-hdinsight"></a>连接到 HDInsight

HDInsight 上的大多数文档假定你可以通过 Internet 访问群集。 例如，这些文档假定你可以连接到 https://CLUSTERNAME.azurehdinsight.cn 上的群集。 此地址使用公共网关，如果你使用了 NSG 或 UDR 限制来自 Internet 的访问，则该网关不可用。

若要通过虚拟网络直接连接到 HDInsight，请使用以下步骤：

1. 若要发现 HDInsight 群集节点的内部完全限定的域名，请使用以下方法之一：

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

2. 若要确定在其上提供服务的端口，请参阅[由 HDInsight 上的 Hadoop 服务使用的端口](./hdinsight-hadoop-port-settings-for-services.md)文档。

    > [!IMPORTANT]
    > 托管在头节点上的某些服务一次只能在一个节点上处于活动状态。 如果尝试在一个头节点上访问某个服务时失败，请切换到其他头节点。

## <a name="next-steps"></a>后续步骤

* 若要详细了解如何在虚拟网络中使用 HDInsight，请参阅[使用 Azure 虚拟网络扩展 HDInsight](./hdinsight-extend-hadoop-virtual-network.md)。

* 有关 Azure 虚拟网络的详细信息，请参阅 [Azure 虚拟网络概述](../virtual-network/virtual-networks-overview.md)。

* 有关网络安全组的详细信息，请参阅[网络安全组](../virtual-network/virtual-networks-nsg.md)。

* 有关用户定义路由的详细信息，请参阅[用户定义路由和 IP 转发](../virtual-network/virtual-networks-udr-overview.md)。