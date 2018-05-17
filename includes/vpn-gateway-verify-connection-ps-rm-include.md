---
 title: include 文件 description: include 文件 services: vpn-gateway author: cherylmc ms.service: vpn-gateway ms.topic: include origin.date: 03/29/2018 ms.date: 05/08/2018 ms.author: v-junlch ms.custom: include file
---
可以验证连接是否成功，方法是使用“Get-AzureRmVirtualNetworkGatewayConnection”cmdlet，带或不带“-Debug”。 

1. 使用以下 cmdlet 示例，配置符合自己需要的值。 如果出现提示，请选择“A”运行“所有”。 在此示例中，“ -Name”是指要测试的连接的名称。

    ```powershell
    Get-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
    ```
2. cmdlet 运行完毕后，查看该值。 在以下示例中，连接状态显示为“已连接”，且可以看到入口和出口字节数。
   
    ```
    "connectionStatus": "Connected",
    "ingressBytesTransferred": 33509044,
    "egressBytesTransferred": 4142431
    ```

<!-- ms.date: 05/08/2018 -->