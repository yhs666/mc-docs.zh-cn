# [负载均衡器文档](index.md)

# 概述
## [什么是负载均衡器？](load-balancer-overview.md)
<!-- Not Available ## [What is Load Balancer Standard?](load-balancer-standard-overview.md)-->
## [公共负载均衡器](load-balancer-internet-overview.md)
## [Internal Load Balancer（内部负载均衡器）](load-balancer-internal-overview.md)
## [了解负载均衡器探测](load-balancer-custom-probe-overview.md)
<!-- Not Available ## [Understand High Availability Ports](load-balancer-ha-ports-overview.md)-->
## [Azure Resource Manager 支持](load-balancer-arm.md)
<!-- Not Available ## [IPv6 support](load-balancer-ipv6-overview.md)-->
## [多个 VIP](load-balancer-multivip-overview.md)
## [了解出站连接](load-balancer-outbound-connections.md)
<!-- Not Available ## [Standard Load Balancer and Availability Zones](load-balancer-standard-availability-zones.md)-->

# 入门

## [配置内部负载均衡器](load-balancer-get-started-ilb-arm-portal.md)
### [配置内部负载均衡器 (PowerShell)](load-balancer-get-started-ilb-arm-ps.md)
### [配置内部负载均衡器 (CLI)](load-balancer-get-started-ilb-arm-cli.md)
### [配置内部负载均衡器（模板）](load-balancer-get-started-ilb-arm-template.md)

## [配置公共负载均衡器](load-balancer-get-started-internet-portal.md)
### [配置公共负载均衡器 (PowerShell)](load-balancer-get-started-internet-arm-ps.md)
### [配置公共负载均衡器 (CLI)](load-balancer-get-started-internet-arm-cli.md)
### [配置公共负载均衡器（模板）](load-balancer-get-started-internet-arm-template.md)

<!-- Not Available ## [Create public Load Balancer with IPv6](load-balancer-ipv6-internet-ps.md) -->
<!-- Not Available ### [Create public Load Balancer with IPv6 (CLI)](load-balancer-ipv6-internet-cli.md) -->
<!-- Not Available ### [Create public Load Balancer with IPv6 (Template)](load-balancer-ipv6-internet-template.md) -->

<!-- Not Available ## [Create a zone redundant public Load Balancer Standard](load-balancer-get-started-internet-az-portal.md) -->
<!-- Not Available ### [Create a zone redundant public Load Balancer Standard (PowerShell)](load-balancer-get-started-internet-az-powershell.md) -->
<!-- Not Available ### [Create a zone redundant public Load Balancer Standard (CLI)](load-balancer-get-started-internet-az-cli.md) -->

# 如何
## [配置负载均衡器的 TCP 空闲超时](load-balancer-tcp-idle-timeout.md)
## [配置负载均衡器的分布模式](load-balancer-distribution-mode.md)
## [为 SQL AlwaysOn 配置内部负载均衡器](load-balancer-configure-sqlao.md)
## [为云服务配置多个 VIP](load-balancer-multivip.md)
## [结合使用负载均衡服务](../traffic-manager/traffic-manager-load-balancing-azure.md?toc=%2fload-balancer%2ftoc.json)
## [使用多个 IP 配置](load-balancer-multiple-ip.md)
### [使用多个 IP 配置 (CLI)](load-balancer-multiple-ip-cli.md)
### [使用多个 IP 配置 (PowerShell)](load-balancer-multiple-ip-powershell.md)
<!-- Not Available ## [Log analytics for Azure Load Balancer](load-balancer-monitor-log.md) -->
<!-- Not Available ## [Configuring DHCPv6 for Linux VMs](load-balancer-ipv6-for-linux.md)-->
<!-- Not Available ## [Configure High Availability Ports for Internal Load Balancer](load-balancer-configure-ha-ports.md)-->

## 故障排除
### [对 Azure 负载均衡器进行故障排除](load-balancer-troubleshoot.md)

## 经典部署模型文章
### [出站连接（经典）](load-balancer-outbound-connections-classic.md)
### [为云服务配置内部负载均衡器](load-balancer-get-started-ilb-classic-cloud.md)
#### [为云服务配置内部负载均衡器 (PowerShell)](load-balancer-get-started-ilb-classic-ps.md)
#### [为云服务配置内部负载均衡器 (CLI)](load-balancer-get-started-ilb-classic-cli.md)
### [配置公共负载均衡器（经典 PowerShell）](load-balancer-get-started-internet-classic-ps.md)
#### [配置公共负载均衡器（经典云）](load-balancer-get-started-internet-classic-cloud.md)
#### [配置公共负载均衡器（经典 CLI）](load-balancer-get-started-internet-classic-cli.md)

# 参考
## [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network)
## [Azure CLI](https://docs.azure.cn/zh-cn/cli/network/lb?view=azure-cli-latest)
## [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.network.models)
## [Java](https://docs.azure.cn/java/api/com.microsoft.azure.management.network)
<!-- Not Available ## [Node.js](http://azure.github.io/azure-sdk-for-node/azure-arm-network/latest/LoadBalancers.html) -->
<!-- Not Available ## [Ruby](http://www.rubydoc.info/gems/azure_mgmt_network/Azure/ARM/Network/LoadBalancers) -->
<!-- Not Available ## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.mgmt.network.operations.html#azure.mgmt.network.operations.LoadBalancersOperations) -->
## [REST](https://msdn.microsoft.com/library/azure/mt163651.aspx)

# 相关内容
## [应用程序网关](/application-gateway/)
## [Express Route](/expressroute/)
## [虚拟网络](/virtual-network/)
## [VPN 网关](/vpn-gateway/)
## [虚拟机](/virtual-machines/)
## [流量管理器](/traffic-manager/)
<!-- Not Available [dns](/dns/) -->

# 资源
## [价格](https://www.azure.cn/pricing/details/load-balancer/)
## [定价计算器](https://www.azure.cn/pricing/calculator/)
## [服务更新](https://www.azure.cn/what-is-new/)

<!--Update_Description: wording update, update link -->
<!-- ms.date: 04/02/2018 -->