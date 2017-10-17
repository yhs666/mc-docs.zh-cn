---
title: "使用虚拟网络扩展 HDInsight - Azure | Azure"
description: "了解如何使用 Azure 虚拟网络将 HDInsight 连接到其他云资源或数据中心内的资源"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 37b9b600-d7f8-4cb1-a04a-0b3a827c6dcc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 08/23/2017
ms.date: 10/23/2017
ms.author: v-yiso
ms.openlocfilehash: 640b8b9e2dff3cc047f80606a4bd92a5d9e93d76
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>使用 Azure 虚拟网络扩展 Azure HDInsight

了解如何将 HDInsight 与 [Azure 虚拟网络](../virtual-network/virtual-networks-overview.md)配合使用。 使用 Azure 虚拟网络可以实现以下方案：

* 直接从本地网络连接到 HDInsight。

* 在 Azure 虚拟网络中将 HDInsight 连接到数据存储。

* 直接访问不能通过 Internet 公开访问的 Hadoop 服务。 例如，Kafka API 或 HBase Java API。

> [!WARNING]
> 本文档中的信息要求你了解 TCP/IP 网络。 如果不熟悉 TCP/IP 网络，则应在修改生产网络之前，与熟悉 TCP/IP 网络的人合作。

## <a name="planning"></a>规划

下面是计划在虚拟网络中安装 HDInsight 时必须回答的问题：

* 是否需要将 HDInsight 安装到现有的虚拟网络？ 或者，你是否在创建新的网络？

    如果使用现有的虚拟网络，则可能需要修改网络配置，然后才能安装 HDInsight。 有关详细信息，请参阅[将 HDInsight 添加到现有的虚拟网络](#existingvnet)部分。

* 是否需要将包含 HDInsight 的虚拟网络连接到另一个虚拟网络或本地网络？

    若要轻松地跨网络使用资源，可能需要创建自定义 DNS 并配置 DNS 转发。 有关详细信息，请参阅[连接多个网络](#multinet)部分。

* 是否需要限制/重定向 HDInsight 的入站或出站流量？

    HDInsight 必须与 Azure 数据中心的特定 IP 地址进行不受限制的通信。 此外还必须通过防火墙启用数个端口，以便进行客户端通信。 有关详细信息，请参阅[控制网络流量](#networktraffic)部分。

## <a id="existingvnet"></a>将 HDInsight 添加到现有的虚拟网络

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

使用本部分中的步骤，了解如何将 HDInsight 添加到现有 Azure 虚拟网络。

> [!NOTE]
> 无法将现有 HDInsight 群集添加到虚拟网络中。

1. 对虚拟网络使用经典模式还是资源管理器部署模式？

    HDInsight 3.4 及更高版本需要 Resource Manager 虚拟网络。 早期版本的 HDInsight 要求使用经典虚拟网络。

    如果现有网络是经典虚拟网络，则必须创建 Resource Manager 虚拟网络，然后将这两种网络连接起来。 [将经典 VNet 连接到新的 VNet](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md)。

    连接后，安装在 Resource Manager 网络中的 HDInsight 即可与经典网络中的资源交互。

2. 是否使用强制隧道？ 强制隧道是一种子网设置，可以强制出站 Internet 流量流向某个设备，以便进行检查和日志记录。 HDInsight 不支持强制隧道。 请在将 HDInsight 安装到子网中之前删除强制隧道，或者为 HDInsight 创建新的子网。

3. 是否使用网络安全组、用户定义路由或虚拟网络设备来限制进出虚拟网络的流量？

    作为托管服务，HDInsight 要求对 Azure 数据中心的多个 IP 地址进行不受限制的访问。 请更新任何现有的网络安全组或用户定义路由，以便与这些 IP 地址通信。

    HDInsight 托管多个服务，这些服务使用不同的端口。 不要阻止流向这些端口的流量。 请参阅[安全性](#security)部分，了解一系列允许通过虚拟设备防火墙的端口。

    若要查找现有的安全配置，请使用以下 Azure PowerShell 或 Azure CLI 命令：

    * 网络安全组

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        有关详细信息，请参阅[排查网络安全组问题](../virtual-network/virtual-network-nsg-troubleshoot-portal.md)文档。

        > [!IMPORTANT]
        > 网络安全组规则按规则优先级顺序应用。 将应用与流量模式匹配的第一条规则，而不应用该流量的其他规则。 权限级别从最高到最低排列的顺序规则。 有关详细信息，请参阅[使用网络安全组筛选网络流量](../virtual-network/virtual-networks-nsg.md)文档。

    * 用户定义路由

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        有关详细信息，请参阅[排查路由问题](../virtual-network/virtual-network-routes-troubleshoot-portal.md)文档。

4. 创建一个 HDInsight 群集，并在配置过程中选择 Azure 虚拟网络。 使用以下文档中的步骤了解群集创建过程：

    * [Create HDInsight using the Azure portal](hdinsight-hadoop-create-linux-clusters-portal.md)（使用 Azure 门户创建 HDInsight）
    * [Create HDInsight using Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)（使用 Azure PowerShell 创建 HDInsight）
    * [使用 Azure CLI 1.0 创建 HDInsight](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [使用 Azure 资源管理器模板创建 HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > 向虚拟网络添加 HDInsight 是一项可选的配置步骤。 请确保在配置群集时选择虚拟网络。

## <a id="multinet"></a>连接多个网络

多网络配置的最大难题是在网络之间进行名称解析。

Azure 为安装在虚拟网络中的 Azure 服务提供名称解析。 此内置的名称解析功能允许 HDInsight 使用完全限定的域名 (FQDN) 连接到以下资源：

* 在 Internet 上提供的任何资源。 例如，microsoft.com、google.com。

* 同一 Azure 虚拟网络中能够使用资源的内部 DNS 名称连接的的任何资源。 例如，在使用默认的名称解析时，下面是分配给 HDInsight 工作节点的内部 DNS 名称示例：

    * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.chinacloudapp.cn
    * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.chinacloudapp.cn

    这两个节点均可使用内部 DNS 名称直接相互通信，以及与 HDInsight 中的其他节点通信。

默认名称解析不允许 HDInsight 解析连接到虚拟网络的网络中的资源的名称。 例如，将本地网络加入虚拟网络很常见。 仅使用默认的名称解析时，HDInsight 不能通过名称访问本地网络中的资源。 反过来也是这样，本地网络中的资源不能通过名称访问虚拟网络中的资源。

> [!WARNING]
> 必须在创建 HDInsight 群集之前，先创建自定义 DNS 服务器并将虚拟网络配置为使用该服务器。

若要在虚拟网络和已连接网络中的资源之间启用名称解析，必须执行以下操作：

1. 在计划安装 HDInsight 的 Azure 虚拟网络中创建自定义 DNS 服务器。

2. 将虚拟网络配置为使用自定义 DNS 服务器。

3. 为虚拟网络找出 Azure 分配的 DNS 后缀。 该值类似于 `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.chinacloudapp.cn`。 有关查找 DNS 后缀的信息，请参阅[示例：自定义 DNS](#example-dns) 部分。

4. 配置 DNS 服务器之间的转发。 此配置取决于远程网络的类型。

    * 如果远程网络为本地网络，请将 DNS 配置如下：

        * 自定义 DNS（位于虚拟网络中）：

            * 将虚拟网络 DNS 后缀的请求转发到 Azure 递归解析程序 (168.63.129.16)。 Azure 处理虚拟网络中资源的请求

            * 将其他所有请求转发到本地 DNS 服务器。 本地 DNS 处理所有其他名称解析请求，甚至是 Internet 资源（如 microsoft.com）的请求。

        * __本地 DNS__： 将虚拟网络 DNS 后缀的请求转发到自定义 DNS 服务器。 然后，自定义 DNS 服务器转发给 Azure 递归解析程序。

        此配置将包含虚拟网络 DNS 后缀的完全限定的域名请求路由至自定义 DNS 服务器。 所有其他请求（甚至包括对公共 Internet 地址的请求）由本地 DNS 服务器处理。

    * 如果远程网络为另一 Azure 虚拟网络，请将 DNS 配置如下：

        * 自定义 DNS（位于每个虚拟网络中）：

            * 对虚拟网络 DNS 后缀的请求将转发到自定义 DNS 服务器。 每个虚拟网络中的 DNS 负责解析其网络中的资源。

            * 将所有其他请求转发到 Azure 递归解析程序。 递归解析程序负责解析本地资源和 Internet 资源。

        每个网络的 DNS 服务器根据 DNS 后缀将请求转发到其他服务器。 其他请求使用 Azure 递归解析程序进行解析。

    有关每个配置的示例，请参阅[示例：自定义 DNS](#example-dns) 部分。

有关详细信息，请参阅 [VM 和角色实例的名称解析](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server)文档。

## <a name="directly-connect-to-hadoop-services"></a>直接连接到 Hadoop 服务

HDInsight 上的大多数文档假定你可以通过 Internet 访问群集。 例如，这些文档假定你可以连接到 https://CLUSTERNAME.azurehdinsight.cn 上的群集。 此地址使用公共网关，如果你使用了 NSG 或 UDR 限制来自 Internet 的访问，则该网关不可用。

若要通过虚拟网络连接到 Ambari 以及其他网页，请使用以下步骤：

1. 若要发现 HDInsight 群集节点的内部完全限定的域名 (FQDN)，请使用以下方法之一：

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

    在返回的节点列表中，查找头节点的 FQDN，并使用这些 FQDN 连接到 Ambari 和其他 Web 服务。 例如，使用 `http://<headnode-fqdn>:8080` 访问 Ambari。

    > [!IMPORTANT]
    > 在头节点上托管的一些服务一次只能在一个节点上处于活动状态。 如果尝试在一个头节点上访问服务并且它返回 404 错误，请切换到其他头节点。

2. 若要确定服务可用的节点和端口，请参阅 [HDInsight 的 Hadoop 服务所用的端口](./hdinsight-hadoop-port-settings-for-services.md)一文。

## <a id="networktraffic"></a> 控制网络流量

可以使用以下方法控制 Azure 虚拟网络中的网络流量：

* 网络安全组 (NSG)：用于筛选网络的入站和出站流量。 有关详细信息，请参阅[使用网络安全组筛选网络流量](../virtual-network/virtual-networks-nsg.md)文档。

    > [!WARNING]
    > HDInsight 不支持限制出站流量。

* 用户定义路由 (UDR)：定义流量在网络中的资源之间的流动方式。 有关详细信息，请参阅[用户定义路由和 IP 转发](../virtual-network/virtual-networks-udr-overview.md)文档。

* 网络虚拟设备：复制设备（例如防火墙和路由器）的功能。 有关详细信息，请参阅[网络设备](https://azure.microsoft.com/solutions/network-appliances)文档。

作为托管服务，HDInsight 要求对 Azure 云中的 Azure 运行状况和管理服务进行不受限制的访问。 使用 NSG 和 UDR 时，必须确保这些服务仍可与 HDInsight 通信。

HDInsight 在多个端口上公开服务。 使用虚拟设备防火墙时，必须允许将端口上的流量用于这些服务。 有关详细信息，请参阅[所需端口]部分。

### <a id="hdinsight-ip"></a> HDInsight 与网络安全组和用户定义路由

如果计划使用网络安全组或用户定义路由来控制网络流量，请在安装 HDInsight 之前执行以下操作：

1. 确定计划用于 HDInsight 的 Azure 区域。

2. 确定 HDInsight 所需的 IP 地址。 有关详细信息，请参阅 [HDInsight 所需的 IP 地址](#hdinsight-ip)部分。

3. 为计划将 HDInsight 安装到其中的子网创建或修改网络安全组或用户定义路由。

    * 网络安全组：在端口 443 上允许来自 IP 地址的入站流量。
    * 用户定义路由： 创建到每个 IP 地址的路由，将“下一跃点类型”设置为“Internet”。

有关网络安全组或用户定义路由的详细信息，请参阅以下文档：

* [网络安全组](../virtual-network/virtual-networks-nsg.md)

* [用户定义路由](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a>强制隧道

强制隧道是用户定义的路由配置，用于将子网中的所有流量强制流向特定的网络或位置，例如本地网络。 HDInsight 不支持强制隧道。

## <a id="hdinsight-ip"></a> 所需 IP 地址

> [!IMPORTANT]
> Azure 运行状况和管理服务必须能够与 HDInsight 通信。 如果使用网络安全组或用户定义路由，则允许来自这些服务的 IP 地址的流量访问 HDInsight。
>
> 如果不使用网络安全组或用户定义的路由来控制流量，则可以忽略本部分。

如果使用网络安全组或用户定义的路由，则必须允许来自 Azure 运行状况和管理服务的流量发往 HDInsight。 使用以下步骤来查找必须允许的 IP 地址：

1. 必须始终允许来自以下 IP 地址的流量：

    | IP 地址 | 允许的端口 | 方向 |
    | ---- | ----- | ----- |
    | 168.61.49.99 | 443 | 入站 |
    | 23.99.5.239 | 443 | 入站 |
    | 168.61.48.131 | 443 | 入站 |
    | 138.91.141.162 | 443 | 入站 |

2. 如果 HDInsight 群集位于以下区域之一，则必须允许针对该区域列出的 IP 地址发出的流量：

    > [!IMPORTANT]
    > 如果未列出所用的 Azure 区域，则仅使用步骤 1 中所列的四个 IP 地址。

    | 国家/地区 | 区域 | 允许的 IP 地址 | 允许的端口 | 方向 |
    | ---- | ---- | ---- | ---- | ----- |
    | 亚洲 | 东亚 | 23.102.235.122</br>52.175.38.134 | 443 | 入站 |
    | &nbsp; | 东南亚 | 13.76.245.160</br>13.76.136.249 | 443 | 入站 |
    | 澳大利亚 | 澳大利亚东部 | 104.210.84.115</br>13.75.152.195 | 443 | 入站 |
    | &nbsp; | 澳大利亚东南部 | 13.77.2.56</br>13.77.2.94 | 443 | 入站 |
    | 巴西 | 巴西南部 | 191.235.84.104</br>191.235.87.113 | 443 | 入站 |
    | 加拿大 | 加拿大东部 | 52.229.127.96</br>52.229.123.172 | 443 | 入站 |
    | &nbsp; | 加拿大中部 | 52.228.37.66</br>52.228.45.222 | 443 | 入站 |
    | 中国 | 中国北部 | 42.159.96.170</br>139.217.2.219 | 443 | 入站 |
    | &nbsp; | 中国东部 | 42.159.198.178</br>42.159.234.157 | 443 | 入站 |
    | 欧洲 | 欧洲北部 | 52.164.210.96</br>13.74.153.132 | 443 | 入站 |
    | &nbsp; | 西欧| 52.166.243.90</br>52.174.36.244 | 443 | 入站 |
    | 德国 | 德国中部 | 51.4.146.68</br>51.4.146.80 | 443 | 入站 |
    | &nbsp; | 德国东北部 | 51.5.150.132</br>51.5.144.101 | 443 | 入站 |
    | 印度 | 印度中部 | 52.172.153.209</br>52.172.152.49 | 443 | 入站 |
    | 日本 | 日本东部 | 13.78.125.90</br>13.78.89.60 | 443 | 入站 |
    | &nbsp; | 日本西部 | 40.74.125.69</br>138.91.29.150 | 443 | 入站 |
    | 韩国 | 韩国中部 | 52.231.39.142</br>52.231.36.209 | 433 | 入站 |
    | &nbsp; | 韩国南部 | 52.231.203.16</br>52.231.205.214 | 443 | 入站
    | 英国 | 英国西部 | 51.141.13.110</br>51.141.7.20 | 443 | 入站 |
    | &nbsp; | 英国南部 | 51.140.47.39</br>51.140.52.16 | 443 | 入站 |
    | 美国 | 美国中部 | 13.67.223.215</br>40.86.83.253 | 443 | 入站 |
    | &nbsp; | 美国中北部 | 157.56.8.38</br>157.55.213.99 | 443 | 入站 |
    | &nbsp; | 美国中西部 | 52.161.23.15</br>52.161.10.167 | 443 | 入站 |
    | &nbsp; | 美国西部 2 | 52.175.211.210</br>52.175.222.222 | 443 | 入站 |

    若要获取用于 Azure 政府版的 IP 地址的信息，请参阅 [Azure 政府智能 + 分析](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics)文档。

3. 如果在虚拟网络中使用自定义 DNS 服务器，还必须允许从 __168.63.129.16__ 访问。 该地址是 Azure 的递归解析程序。 有关详细信息，请参阅 [VM 和角色实例的名称解析](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)文档。

有关详细信息，请参阅[控制网络流量](#networktraffic)部分。

## <a id="hdinsight-ports"></a> 所需端口

如果计划使用网络虚拟设备防火墙来保护虚拟网络，则必须允许以下端口的出站流量：

* 53
* 443
* 1433
* 11000-11999
* 14000-14999

如需特定服务的端口列表，请参阅[由 HDInsight 上的 Hadoop 服务使用的端口](hdinsight-hadoop-port-settings-for-services.md)文档。

有关虚拟设备防火墙规则的详细信息，请参阅[虚拟设备方案](../virtual-network/virtual-network-scenario-udr-gw-nva.md)文档。

## <a id="hdinsight-nsg"></a>示例：网络安全组与 HDInsight

本部分的示例演示如何创建允许 HDInsight 与 Azure 管理服务通信的网络安全组规则。 在使用示例之前，请调整 IP 地址，使之与所用 Azure 区域的 IP 地址匹配。 有关此方面的信息，可参阅 [HDInsight 与网络安全组和用户定义路由](#hdinsight-ip)部分。

### <a name="azure-resource-management-template"></a>Azure 资源管理模板

以下资源管理模板创建一个虚拟网络，该网络限制入站流量，但允许来自 HDInsight 所需的 IP 地址的流量。 该模板还在虚拟网络中创建 HDInsight 群集。

* [部署安全的 Azure 虚拟网络和 HDInsight Hadoop 群集](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> 更改此示例中使用的 IP 地址，使之与要使用的 Azure 区域匹配。 有关此方面的信息，可参阅 [HDInsight 与网络安全组和用户定义路由](#hdinsight-ip)部分。

### <a name="azure-powershell"></a>Azure PowerShell

使用以下 PowerShell 脚本创建可限制入站流量的虚拟网络，但允许来自中国北部区域的 IP 地址的流量。

> [!IMPORTANT]
> 更改此示例中使用的 IP 地址，使之与要使用的 Azure 区域匹配。 有关此方面的信息，可参阅 [HDInsight 与网络安全组和用户定义路由](#hdinsight-ip)部分。

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"
# Get the Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get the region the Virtual network is in.
$location = $vnet.Location
# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzureRmNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "blockeverything" `
        -Description "Block everything else" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "*" `
        -SourceAddressPrefix "Internet" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Deny `
        -Priority 500 `
        -Direction Inbound
# Set the changes to the security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply the NSG to the subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

> [!IMPORTANT]
> 此示例演示如何添加规则，以便在所需的 IP 地址上允许入站流量。 它不包含限制从其他源进行入站访问的规则。
>
> 以下示例演示了如何从 Internet 启用 SSH 访问：
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a>Azure CLI

使用以下步骤创建一个虚拟网络，该网络限制入站流量，但允许来自 HDInsight 所需的 IP 地址的流量。

1. 使用以下命令创建名为 `hdisecure` 的新网络安全组。 将 **RESOURCEGROUPNAME** 替换为包含 Azure 虚拟网络的资源组。 将 **LOCATION** 替换为组创建的位置（区域）。

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    在创建组后，将收到有关新组的信息。

2. 使用以下命令将规则添加新的网络安全组，这些规则允许从 Azure HDInsight 运行状况和管理服务通过端口 443 发起的入站通信。 将 **RESOURCEGROUPNAME** 替换为包含 Azure 虚拟网络的资源组的名称。

    > [!IMPORTANT]
    > 更改此示例中使用的 IP 地址，使之与要使用的 Azure 区域匹配。 有关此方面的信息，可参阅 [HDInsight 与网络安全组和用户定义路由](#hdinsight-ip)部分。

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n block --protocol "*" --source-port-range "*" --destination-port-range "*" --source-address-prefix "Internet" --destination-address-prefix "VirtualNetwork" --access "Deny" --priority 500 --direction "Inbound"
    ```

3. 若要检索此网络安全组的唯一标识符，请使用以下命令：

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    此命令返回类似于以下文本的值：

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    如果没有得到预期的结果，请在命令中的 ID两侧使用双引号。

4. 使用以下命令将网络安全组应用于子网。 将 GUID 和 RESOURCEGROUPNAME 值替换为从上一步骤中返回的值。 将 VNETNAME 和 SUBNETNAME 替换为要创建的虚拟网络名称和子网名称。

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    此命令完成后，即可将 HDInsight 安装到虚拟网络中。

> [!IMPORTANT]
> 这些步骤只会实现对 Azure 云中 HDInsight 运行状况和管理服务的访问。 任何从虚拟网络外部对 HDInsight 群集的其他访问会被阻止。 若要从虚拟网络之外启用访问，必须添加其他的虚拟网络安全组规则。
>
> 以下示例演示了如何从 Internet 启用 SSH 访问：
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <a id="example-dns"></a> 示例：DNS 配置

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>在虚拟网络和连接的本地网络之间进行名称解析

此示例作出以下假设：

* 你有一个使用 VPN 网关连接到本地网络的 Azure 虚拟网络。

* 虚拟网络中的自定义 DNS 服务器运行 Linux 或 Unix 作为操作系统。

* [Bind](https://www.isc.org/downloads/bind/) 安装在自定义 DNS 服务器上。

在虚拟网络的自定义 DNS 服务器上，执行以下操作：

1. 使用 Azure PowerShell 或 Azure CLI 查找虚拟网络的 DNS 后缀：

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. 在虚拟网络的自定义 DNS 服务器上，使用以下文本作为 `/etc/bind/named.conf.local` 文件的内容：

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.chinacloudapp.cn" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    将 `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.chinacloudapp.cn` 值替换为虚拟网络的 DNS 后缀。

    此配置将针对虚拟网络 DNS 后缀的所有 DNS 请求路由到 Azure 递归解析程序。

2. 在虚拟网络的自定义 DNS 服务器上，使用以下文本作为 `/etc/bind/named.conf.options` 文件的内容：

    ```
    // Clients to accept requests from
    // TODO: Add the IP range of the joined network to this list
    acl goodclients {
        10.0.0.0/16; # IP address range of the virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent to the following
            forwarders {
                192.168.0.1; # Replace with the IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```

    * 将 `10.0.0.0/16` 值替换为虚拟网络的 IP 地址范围。 此项允许该范围内的名称解析请求地址。

    * 将本地网络的 IP 地址范围添加到 `acl goodclients { ... }` 部分。  此项允许本地网络中的资源发出的名称解析请求。

    * 将值 `192.168.0.1` 替换为本地 DNS 服务器的 IP 地址。 此项将所有其他的 DNS 请求路由到本地 DNS 服务器。

3. 若要使用此配置，请重启 Bind。 例如，`sudo service bind9 restart`。

4. 向本地 DNS 服务器添加条件转发器。 配置条件转发器，将对步骤 1 中 DNS 后者的请求发送到自定义 DNS 服务器。

    > [!NOTE]
    > 请参阅 DNS 软件的文档，详细了解如何添加条件转发器。

完成这些步骤后，即可使用完全限定的域名 (FQDN) 连接到任一网络中的资源。 现在可以将 HDInsight 安装到虚拟网络。

### <a name="name-resolution-between-two-connected-virtual-networks"></a>在两个连接的虚拟网络之间进行名称解析

此示例作出以下假设：

* 你有两个使用 VPN 网关或对等互连进行连接的 Azure 虚拟网络。

* 两个网络中的自定义 DNS 服务器运行 Linux 或 Unix 作为操作系统。

* [Bind](https://www.isc.org/downloads/bind/) 安装在自定义 DNS 服务器上。

1. 使用 Azure PowerShell 或 Azure CLI 查找两个虚拟网络的 DNS 后缀：

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. 在自定义 DNS 服务器上，使用以下文本作为 `/etc/bind/named.config.local` 文件的内容。 在两个虚拟网络中对自定义 DNS 服务器进行这种更改。

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.chinacloudapp.cn" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    将 `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.chinacloudapp.cn` 值替换为另一虚拟网络的 DNS 后缀。 此项将对远程网络 DNS 后缀的请求路由到该网络中的自定义 DNS。

3. 在两个虚拟网络的自定义 DNS 服务器上，使用以下文本作为 `/etc/bind/named.conf.options` 文件的内容：

    ```
    // Clients to accept requests from
    acl goodclients {
        10.1.0.0/16; # The IP address range of one virtual network
        10.0.0.0/16; # The IP address range of the other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```

    * 将 `10.0.0.0/16` 和 `10.1.0.0/16` 值替换为虚拟网络的 IP 地址范围。 此项允许每个网络中的资源发出 DNS 服务器的请求。

    任何不是针对虚拟网络 DNS 后缀的请求（例如，microsoft.com）均由 Azure 递归解析程序处理。

4. 若要使用此配置，请重启 Bind。 例如，两个 DNS 服务器上的 `sudo service bind9 restart`。

完成这些步骤后，即可使用完全限定的域名 (FQDN) 连接到虚拟网络中的资源。 现在可以将 HDInsight 安装到虚拟网络。

## <a name="next-steps"></a>后续步骤

* 如需通过端到端示例来了解如何将 HDInsight 配置为连接到本地网络，请参阅[将 HDInsight 连接到本地网络](./connect-on-premises-network.md)。

* 有关 Azure 虚拟网络的详细信息，请参阅 [Azure 虚拟网络概述](../virtual-network/virtual-networks-overview.md)。

* 有关网络安全组的详细信息，请参阅[网络安全组](../virtual-network/virtual-networks-nsg.md)。

* 有关用户定义路由的详细信息，请参阅[用户定义路由和 IP 转发](../virtual-network/virtual-networks-udr-overview.md)。
