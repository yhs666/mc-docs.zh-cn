---
 title: include 文件 description: include 文件 services: vpn-gateway author: WenJason ms.service: vpn-gateway ms.topic: include origin.date: 08/02/2018 ms.date: 09/02/2018 ms.author: v-jay ms.custom: include file
---

|**SKU**   | S2S/VNet 到 VNet<br>隧道 | **P2S<br>连接** | **聚合<br>吞吐量基准** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| 最大 30*                         | 最大 128\*\*             | 650 Mbps                    |
|**VpnGw2**| 最大 30*                         | 最大 128\*\*             | 1 Gbps                      |
|VpnGw3| 最大 30*                         | 最大 128\*\*             | 1.25 Gbps                   |
|**基本** | 最大 10 个                         | 最大 128               | 100 Mbps                    | 

* (**) 如果需要更多连接，请联系支持人员。 这仅适用于 IKEv2，SSTP 的连接数不能增加。

* 聚合吞吐量基准基于对通过单个网关聚合的多个隧道的测量。 受 Internet 流量情况和应用程序行为影响，该吞吐量无法保证。

* 可在 [定价](https://www.azure.cn/pricing/details/vpn-gateway) 页上找到定价信息。

* 可在 [SLA](https://www.azure.cn/support/sla/vpn-gateway/) 页上查看 SLA（服务级别协议）信息。

* 仅在使用资源管理器部署模型的情况下才支持将 VpnGw1、VpnGw2 和 VpnGw3 用于 VPN 网关。
