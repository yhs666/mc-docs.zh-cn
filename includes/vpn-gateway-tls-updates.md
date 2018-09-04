---
 title: include 文件 description: include 文件 services: vpn-gateway author: WenJason ms.service: vpn-gateway ms.topic: include origin.date: 07/30/2018 ms.date: 09/02/2018 ms.author: v-jay ms.custom: include file
---
>[!NOTE]
>从 2018 年 7 月 1 日开始，Azure VPN 网关将不再支持 TLS 1.0 和 1.1。 VPN 网关将仅支持 TLS 1.2。 若要保留支持，请参阅[通过更新启用对 TLS1.2 的支持](#tls1)。

另外，TLS 也将于 2018 年 7 月 1 日起弃用以下旧算法：

* RC4 (Rivest Cipher 4)
* DES（数据加密算法）
* 3DES（三重数据加密算法）
* MD5（消息摘要 5）

### <a name="tls1"></a>如何在 Windows 7 和 Windows 8.1 中启用对 TLS 1.2 的支持？

[!INCLUDE [tls 1.2](vpn-gateway-tls-include.md)]
