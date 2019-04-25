---
title: 教程 - 创建具有 Web 应用程序防火墙的应用程序网关 - Azure 门户 | Microsoft Docs
description: 了解如何使用 Azure 门户创建具有 Web 应用程序防火墙的应用程序网关。
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
origin.date: 03/25/2019
ms.date: 04/16/2019
ms.author: v-junlch
ms.openlocfilehash: fd23ff654715e49062cdd0646c4ceca59dcd66fc
ms.sourcegitcommit: bf3df5d77e5fa66825fe22ca8937930bf45fd201
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59686477"
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-using-the-azure-portal"></a>使用 Azure 门户创建具有 Web 应用程序防火墙的应用程序网关

> [!div class="op_single_selector"]
>
> - [Azure 门户](application-gateway-web-application-firewall-portal.md)
> - [PowerShell](tutorial-restrict-web-traffic-powershell.md)
> - [Azure CLI](tutorial-restrict-web-traffic-cli.md)
>
> 

本教程演示如何使用 Azure 门户创建具有 [Web 应用程序防火墙](application-gateway-web-application-firewall-overview.md) (WAF) 的[应用程序网关](application-gateway-introduction.md)。 WAF 使用 [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 规则保护应用程序。 这些规则包括针对各种攻击（例如 SQL 注入、跨站点脚本攻击和会话劫持）的保护。 创建应用程序网关后，可对其进行测试，以确保正常工作。 使用 Azure 应用程序网关可为端口分配侦听器、创建规则以及向后端池添加资源，以便将应用程序 Web 流量定向到特定资源。 为方便演示，本文使用了一种简单的设置，其中包括一个公共前端 IP、一个用于在此应用程序网关上托管单个站点的基本侦听器、两个用于后端池的虚拟机，以及一个基本请求路由规则。

在本文中，学习如何：

> [!div class="checklist"]
> * 创建启用 WAF 的应用程序网关
> * 创建用作后端服务器的虚拟机
> * 创建存储帐户和配置诊断

![Web 应用程序防火墙示例](./media/application-gateway-web-application-firewall-portal/scenario-waf.png)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>登录 Azure

通过 [https://portal.azure.cn](https://portal.azure.cn) 登录到 Azure 门户

## <a name="create-an-application-gateway"></a>创建应用程序网关

若要在创建的资源之间实现通信，需要设置虚拟网络。 在本示例中创建了两个子网：一个用于应用程序网关，另一个用于后端服务器。 可以在创建应用程序网关的同时创建虚拟网络。

1. 单击 Azure 门户左上角的“新建”。
2. 选择“网络”，然后在“特色”列表中选择“应用程序网关”。
3. 输入应用程序网关的以下值：

   - *myAppGateway* - 应用程序网关的名称。
   - *myResourceGroupAG* - 新资源组。

     ![新建应用程序网关](./media/application-gateway-web-application-firewall-portal/application-gateway-create.png)

4. 接受其他设置的默认值，然后单击“确定”。
5. 依次单击“选择虚拟网络”、“新建”，然后输入虚拟网络的以下值：

   - *myVNet* - 虚拟网络的名称。
   - *10.0.0.0/16* - 虚拟网络地址空间。
   - *myAGSubnet* - 子网名称。
   - *10.0.0.0/24* - 子网地址空间。

     ![创建虚拟网络](./media/application-gateway-web-application-firewall-portal/application-gateway-vnet.png)

6. 单击“确定”创建虚拟网络和子网。
7. 依次单击“选择公共 IP 地址”、“新建”，然后输入公共 IP 地址的名称。 在本示例中，公共 IP 地址名为 *myAGPublicIPAddress*。 接受其他设置的默认值，然后单击“确定”。
8. 对于应用程序网关的层，选择 *WAF*。 接受侦听器配置的默认值，让 Web 应用程序防火墙保留禁用状态，然后单击“确定”。

    ![](./media/application-gateway-web-application-firewall-portal/application-gateway-create2.png)

9. 复查摘要页上的设置，然后单击“确定”以创建网络资源和应用程序网关。 创建应用程序网关可能需要几分钟时间，请等到部署成功完成，然后转到下一部分。

## <a name="add-backend-pool"></a>添加后端池

后端池用于将请求路由到将为请求提供服务的后端服务器。 后端池可以包含 NIC、虚拟机规模集、公共 IP、内部 IP、完全限定的域名 (FQDN) 和多租户后端（例如 Azure 应用服务）。 需要将后端目标添加到后端池。

本示例使用虚拟机作为目标后端。 我们可以使用现有的虚拟机，或者创建新的虚拟机。 本示例将创建两个虚拟机，供 Azure 用作应用程序网关的后端服务器。 为此，我们将会：

1. 创建新的子网 *myBackendSubnet*，我们将在该子网中创建新的 VM。 
2. 创建两个新的 VM *myVM* 和 *myVM2*，用作后端服务器。
3. 在虚拟机上安装 IIS，以验证是否成功创建了应用程序网关。
4. 将后端服务器添加到后端池。

### <a name="add-a-subnet"></a>添加子网

按以下步骤将子网添加到已创建的虚拟网络：

1. 在 Azure 门户的左侧菜单上选择“所有资源”，在搜索框中输入 *myVNet*，然后从搜索结果中选择 **myVNet**。

2. 从左侧菜单选择“子网”，然后选择“+ 子网”。 

   ![创建子网](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. 在“添加子网”页中输入 *myBackendSubnet* 作为子网的**名称**，然后选择“确定”。

### <a name="create-a-virtual-machine"></a>创建虚拟机

1. 在 Azure 门户中，选择“创建资源”。 此时会显示“新建”窗口。
2. 选择“计算”，然后在“特色”列表中选择“Windows Server 2016 Datacenter”。 此时会显示“创建虚拟机”页。<br>应用程序网关可将流量路由到其后端池中使用的任何类型的虚拟机。 本示例使用 Windows Server 2016 Datacenter。
3. 对于以下虚拟机设置，请在“基本信息”选项卡中输入相应值：
   - **资源组**：选择 **myResourceGroupAG** 作为资源组名称。
   - **虚拟机名称**：输入 *myVM* 作为虚拟机的名称。
   - **用户名**：输入 *azureuser* 作为管理员用户名。
   - **密码**：输入 *Azure123456!* 作为管理员密码。
4. 接受其他默认值，然后选择“下一步:**磁盘”。  
5. 接受“磁盘”选项卡的默认值，然后选择“下一步:**网络”。
6. 在“网络”选项卡上，验证是否已选择 **myVNet** 作为**虚拟网络**，以及是否已将“子网”设置为 **myBackendSubnet**。 接受其他默认值，然后选择“下一步:**管理”。<br>应用程序网关可与其所在的虚拟网络外部的实例进行通信，但我们需要确保已建立 IP 连接。 
7. 在“管理”选项卡上，将“启动诊断”设置为“关闭”。 接受其他默认值，然后选择“复查 + 创建”。
8. 在“复查 + 创建”选项卡上复查设置，更正任何验证错误，然后选择“创建”。
9. 等待虚拟机创建完成，然后再继续操作。

### <a name="install-iis-for-testing"></a>安装 IIS 用于测试

本示例将在虚拟机上安装 IIS，目的仅仅是验证 Azure 是否已成功创建应用程序网关。 

1. 打开 powershell 并使用订阅登录。

2. 运行以下命令以在虚拟机上安装 IIS： 

   ```azurepowershell
   Set-AzVMExtension `
     -ResourceGroupName myResourceGroupAG `
     -ExtensionName IIS `
     -VMName myVM `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location ChinaNorth
   ```

3. 使用以前完成的步骤创建第二个虚拟机并安装 IIS。 使用 *myVM2* 作为虚拟机名称，以及作为 **Set-AzVMExtension** cmdlet 的 **VMName** 设置。

### <a name="add-backend-servers-to-backend-pool"></a>将后端服务器添加到后端池

1. 选择“所有资源”，然后选择“myAppGateway”。

2. 从左侧菜单中选择“后端池”。 当你创建应用程序网关时，Azure 自动创建了默认池 **appGatewayBackendPool**。 

3. 选择“appGatewayBackendPool”。

4. 在“目标”下，从下拉列表中选择“虚拟机”。

5. 在“虚拟机”和“网络接口”下，从下拉列表中选择 **myVM** 和 **myVM2** 虚拟机及其关联的网络接口。

   ![添加后端服务器](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. 选择“其他安全性验证” 。

## <a name="create-a-storage-account-and-configure-diagnostics"></a>创建存储帐户和配置诊断

### <a name="create-a-storage-account"></a>创建存储帐户

在本教程中，应用程序网关使用存储帐户来存储用于检测和防范目的的数据。 也可以使用 Azure Monitor 日志或事件中心来记录数据。

1. 单击 Azure 门户左上角的“新建”。
2. 选择“存储”，然后选择“存储帐户 - blob、文件、表、队列”。
3. 输入存储帐户的名称，对于资源组选择“使用现有资源组”，然后选择 **myResourceGroupAG**。 在此示例中，存储帐户名称是 *myagstore1*。 接受其他设置的默认值，然后单击“创建”。

### <a name="configure-diagnostics"></a>配置诊断

配置诊断以将数据记录到 ApplicationGatewayAccessLog、ApplicationGatewayPerformanceLog 和 ApplicationGatewayFirewallLog 日志中。

1. 在左侧菜单中，单击“所有资源”，然后选择 *myAppGateway*。
2. 在“监视”下面，单击“诊断日志”。
3. 单击“添加诊断设置”。
4. 输入 *myDiagnosticsSettings* 作为诊断设置的名称。
5. 选择“存档到存储帐户”，然后单击“配置”以选择前面创建的 *myagstore1* 存储帐户。
6. 选择要收集和保留的应用程序网关日志。
7. 单击“保存” 。

    ![配置诊断](./media/application-gateway-web-application-firewall-portal/application-gateway-diagnostics.png)

## <a name="test-the-application-gateway"></a>测试应用程序网关

虽然不需 IIS 即可创建应用程序网关，但本快速入门中安装了它，用来验证 Azure 是否已成功创建应用程序网关。 使用 IIS 测试应用程序网关：

1. 在“概览”页上找到应用程序网关的公共 IP 地址。

    ![记下应用程序网关的公共 IP 地址](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

    也可选择“所有资源”，在搜索框中输入 *myAGPublicIPAddress*，然后在搜索结果中将其选中。 Azure 会在“概览”页上显示公共 IP 地址。
1. 复制该公共 IP 地址，并将其粘贴到浏览器的地址栏。
1. 检查响应。 有效的响应中会确认已成功创建应用程序网关，并且它可以成功连接到后端。

    ![测试应用程序网关](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>清理资源

如果不再需要通过应用程序网关创建的资源，请删除资源组。 删除资源组时，也会删除应用程序网关和及其所有的相关资源。 

若要删除资源组，请执行以下操作：

1. 在 Azure 门户的左侧菜单上选择“资源组”。
2. 在“资源组”页的列表中搜索“myResourceGroupAG”，然后将其选中。
3. 在“资源组”页上，选择“删除资源组”。
4. 在“键入资源组名称”字段中输入“myResourceGroupAG”，然后选择“删除”

## <a name="next-steps"></a>后续步骤

本文介绍了如何执行以下操作：

> [!div class="checklist"]
> * 创建启用 WAF 的应用程序网关
> * 创建用作后端服务器的虚拟机
> * 创建存储帐户和配置诊断

若要了解有关应用程序网关及其关联资源的详细信息，请继续阅读操作指南文章。

<!-- Update_Description: wording update -->