---
title: 使用专用前端 IP 地址配置 Azure 应用程序网关
description: 本文提供有关如何使用专用前端 IP 地址配置应用程序网关的信息
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
origin.date: 02/26/2019
ms.date: 09/10/2019
ms.author: v-junlch
ms.openlocfilehash: 3fe068c7d2374dc4cb2c2a587c950639c3a8703a
ms.sourcegitcommit: 843028f54c4d75eba720ac8874562ab2250d5f4d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70857261"
---
# <a name="configure-an-application-gateway-with-an-internal-load-balancer-ilb-endpoint"></a>使用内部负载均衡器 (ILB) 终结点配置应用程序网关

可向 Azure 应用程序网关配置面向 Internet 的 VIP 或不向 Internet 公开的内部终结点（也称为内部负载均衡器 (ILB) 终结点（使用前端 IP 地址的专用 IP）。 对于不向 Internet 公开的内部业务线应用程序，使用前端专用 IP 地址配置网关的做法非常有效。 对于位于不向 Internet 公开的安全边界内的多层应用程序中的服务和层也很有用，但仍需要执行循环负载分散、会话粘性或安全套接字层 (SSL) 终止。

本文逐步讲解如何在 Azure 门户中使用前端专用 IP 地址配置应用程序网关。

本文介绍如何执行以下操作：

- 为应用程序网关创建专用前端 IP 配置
- 使用专用前端 IP 配置创建应用程序网关


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="log-in-to-azure"></a>登录 Azure

在 <https://portal.azure.cn> 登录 Azure 门户

## <a name="create-an-application-gateway"></a>创建应用程序网关

Azure 需要一个虚拟网络才能在创建的资源之间通信。 可以创建新的虚拟网络，或者使用现有的虚拟网络。 本示例将创建新的虚拟网络。 可以在创建应用程序网关的同时创建虚拟网络。 在独立的子网中创建应用程序网关实例。 在本示例中创建两个子网：一个用于应用程序网关，另一个用于后端服务器。

1. 单击 Azure 门户左上角的“新建”。 
2. 选择“网络”  ，然后在“特别推荐”列表中选择“应用程序网关”  。
3. 输入 *myAppGateway* 作为应用程序网关的名称，输入 *myResourceGroupAG* 作为新资源组的名称。
4. 接受其他设置的默认值，然后单击“确定”  。
5. 依次单击“选择虚拟网络”、“新建”，然后输入虚拟网络的以下值：  
   - myVNet* - 虚拟网络的名称。
   - 10.0.0.0/16* - 虚拟网络地址空间。
   - *myAGSubnet* - 子网名称。
   - *10.0.0.0/24* - 子网地址空间。  
     ![private-frontendip-1](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-1.png)
6. 单击“确定”  创建虚拟网络和子网。
7. 选择“专用”作为“前端 IP 配置”（默认为动态 IP 地址分配）。 所选子网的第一个可用地址将分配为前端 IP 地址。
8. 若要从子网地址范围中选择专用 IP（静态分配），请单击“选择特定的专用 IP 地址”框并指定 IP 地址。 
   > [!NOTE]
   > IP 地址类型（静态或动态）一经分配，以后便不可更改。
9. 选择协议和端口的侦听器配置以及 WAF 配置（如果需要），然后单击“确定”。
    ![private-frontendip-2](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-2.png)
10. 检查摘要页上的设置，然后单击“确定”  创建网络资源和应用程序网关。 创建应用程序网关可能需要几分钟时间，请等到部署成功完成，然后转到下一部分。

## <a name="add-backend-pool"></a>添加后端池

后端池用于将请求路由到为请求提供服务的后端服务器。 后端可以包含 NIC、虚拟机规模集、公共 IP、内部 IP、完全限定的域名 (FQDN) 和多租户后端（例如 Azure 应用服务）。 本示例使用虚拟机作为目标后端。 我们可以使用现有的虚拟机，或者创建新的虚拟机。 本示例将创建两个虚拟机，供 Azure 用作应用程序网关的后端服务器。 为此，我们将会：

1. 创建两个新的 VM *myVM* 和 *myVM2*，用作后端服务器。
2. 在虚拟机上安装 IIS，以验证是否成功创建了应用程序网关。
3. 将后端服务器添加到后端池。

### <a name="create-a-virtual-machine"></a>创建虚拟机

1. 单击“新建”  。
2. 单击“计算”，然后在“特色”列表中选择“Windows Server 2016 Datacenter”。  
3. 为虚拟机输入以下值：
   - *myVM* - 作为虚拟机的名称。
   - *azureuser* - 作为管理员用户名。
   - *Azure123456!* - 密码。
   - 选择“使用现有资源组”，然后选择“myResourceGroupAG”   。
4. 单击 **“确定”** 。
5. 选择“DS1_V2”作为虚拟机的大小，然后单击“选择”   。
6. 请确保选择 **myVNet** 作为虚拟网络，子网是 **myBackendSubnet**。
7. 单击“禁用”  以禁用启动诊断。
8. 创建“确定”  ，检查“摘要”页上的设置，然后单击“创建”  。

### <a name="install-iis"></a>安装 IIS

1. 打开 PowerShell 并使用订阅登录到 Azure。

   ```azurepowershell
   Connect-AzAccount -Environment AzureChinaCloud
   ```

2. 运行以下命令以在虚拟机上安装 IIS：

   ```azurepowershell
   Set-AzVMExtension `
   
     -ResourceGroupName myResourceGroupAG `
   
     -ExtensionName IIS `
   
     -VMName myVM `
   
     -Publisher Microsoft.Compute `
   
     -ExtensionType CustomScriptExtension `
   
     -TypeHandlerVersion 1.4 `
   
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' -Location ChinaNorth  
   ```



3. 使用刚刚完成的步骤创建第二个虚拟机并安装 IIS。 输入 myVM2 作为其名称，并作为 Set-AzVMExtension 中的 VMName。

### <a name="add-backend-servers-to-backend-pool"></a>将后端服务器添加到后端池

1. 单击“所有资源”  ，然后单击 **myAppGateway**。
2. 单击“后端池”  。 默认池已随应用程序网关自动创建。 单击 **appGatewayBackendPool**。
3. 单击“添加目标”  将所创建的每个虚拟机添加到后端池。
   ![private-frontendip-4](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-4.png)
4. 单击“保存”  。

## <a name="test-the-application-gateway"></a>测试应用程序网关

1. 通过单击门户中的“前端 IP 配置”  边栏选项卡，查看分配的前端 IP。
    ![private-frontendip-5](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-5.png)
2. 复制专用 IP 地址，然后将其粘贴到同一 VNet 中的 VM 或与此 VNet 连接的本地 VM 的浏览器地址栏中，并尝试访问应用程序网关。

## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何执行以下操作：

- 为应用程序网关创建专用前端 IP 配置
- 使用专用前端 IP 配置创建应用程序网关

如果要监视后端的运行状况，请参阅[应用程序网关诊断](/application-gateway/application-gateway-diagnostics)。

<!-- Update_Description: wording update -->