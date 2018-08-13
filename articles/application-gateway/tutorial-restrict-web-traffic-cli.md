---
title: 启用 Web 应用程序防火墙 - Azure CLI
description: 了解如何通过 Azure CLI 在应用程序网关上使用 Web 应用程序防火墙限制 Web 流量。
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
origin.date: 07/14/2018
ms.date: 08/07/2018
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: b0f22148effd0beedb5e1ac66662ced8f5dbc4dc
ms.sourcegitcommit: a1c6a743b4be62477e7debfc9ea5f03afca2bc8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2018
ms.locfileid: "39625144"
---
# <a name="tutorial-enable-web-application-firewall-using-the-azure-cli"></a>教程：使用 Azure CLI 启用 Web 应用程序防火墙

在[应用程序网关](overview.md)上使用 [Web 应用程序防火墙](waf-overview.md) (WAF) 限制流量。 WAF 使用 [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 规则保护应用程序。 这些规则包括针对各种攻击（例如 SQL 注入、跨站点脚本攻击和会话劫持）的保护。 

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 设置网络
> * 创建启用 WAF 的应用程序网关
> * 创建虚拟机规模集
> * 创建存储帐户和配置诊断

![Web 应用程序防火墙示例](./media/tutorial-restrict-web-traffic-cli/scenario-waf.png)

如果需要，可以使用 [Azure PowerShell](tutorial-restrict-web-traffic-powershell.md) 完成本教程中的步骤。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

如果选择在本地安装并使用 CLI，本教程要求运行 Azure CLI 2.0.4 或更高版本。 若要查找版本，请运行 `az --version`。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。

## <a name="create-a-resource-group"></a>创建资源组

资源组是在其中部署和管理 Azure 资源的逻辑容器。 使用 [az group create](/cli/group#az_group_create) 创建名为 *myResourceGroupAG* 的 Azure 资源组。

```azurecli 
az group create --name myResourceGroupAG --location chinanorth
```

## <a name="create-network-resources"></a>创建网络资源

虚拟网络和子网用于提供与应用程序网关及其关联资源的网络连接。 分别使用 [az network vnet create](/cli/network/vnet#az_network_vnet_create) 和 [az network vnet subnet create](/cli/network/vnet/subnet#az_network_vnet_subnet_create) 创建名为 *myVNet* 的虚拟网络和名为 *myAGSubnet* 的子网。 使用 [az network public-ip create](/cli/network/public-ip#az_network_public_ip_create) 创建名为 *myAGPublicIPAddress* 的公共 IP 地址。

```azurecli
az network vnet create `
  --name myVNet `
  --resource-group myResourceGroupAG `
  --location chinanorth `
  --address-prefix 10.0.0.0/16 `
  --subnet-name myBackendSubnet `
  --subnet-prefix 10.0.1.0/24

az network vnet subnet create `
  --name myAGSubnet `
  --resource-group myResourceGroupAG `
  --vnet-name myVNet `
  --address-prefix 10.0.2.0/24

az network public-ip create `
  --resource-group myResourceGroupAG `
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway-with-a-waf"></a>创建具有 WAF 的应用程序网关

可以使用 [az network application-gateway create](/cli/network/application-gateway#az_application_gateway_create) 创建名为 *myAppGateway* 的应用程序网关。 使用 Azure CLI 创建应用程序网关时，请指定配置信息，例如容量、sku 和 HTTP 设置。 将应用程序网关分配给之前创建的 *myAGSubnet* 和 *myPublicIPSddress*。

```azurecli
az network application-gateway create `
  --name myAppGateway `
  --location chinanorth `
  --resource-group myResourceGroupAG `
  --vnet-name myVNet `
  --subnet myAGSubnet `
  --capacity 2 `
  --sku WAF_Medium `
  --http-settings-cookie-based-affinity Disabled `
  --frontend-port 80 `
  --http-settings-port 80 `
  --http-settings-protocol Http `
  --public-ip-address myAGPublicIPAddress

az network application-gateway waf-config set `
  --enabled true `
  --gateway-name myAppGateway `
  --resource-group myResourceGroupAG `
  --firewall-mode Detection `
  --rule-set-version 3.0
```

创建应用程序网关可能需要几分钟时间。 创建应用程序网关后，可以看到它的这些新功能：

- *appGatewayBackendPool* - 应用程序网关必须至少具有一个后端地址池。
- *appGatewayBackendHttpSettings* - 指定将端口 80 和 HTTP 协议用于通信。
- *appGatewayHttpListener* - 与 *appGatewayBackendPool* 关联的默认侦听器。
- *appGatewayFrontendIP* - 将 *myAGPublicIPAddress* 分配给 *appGatewayHttpListener*。
- *rule1* - 与 *appGatewayHttpListener* 关联的默认路由规则。

## <a name="create-a-virtual-machine-scale-set"></a>创建虚拟机规模集

在此示例中，将创建虚拟机规模集，以便为应用程序网关的后端池提供两个服务器。 规模集中的虚拟机与 *myBackendSubnet* 子网相关联。 若要创建规模集，可以使用 [az vmss create](/cli/vmss#az_vmss_create)。

```azurecli
az vmss create `
  --name myvmss `
  --resource-group myResourceGroupAG `
  --image UbuntuLTS `
  --admin-username azureuser `
  --admin-password Azure123456! `
  --instance-count 2 `
  --vnet-name myVNet `
  --subnet myBackendSubnet `
  --vm-sku Standard_DS2 `
  --upgrade-policy-mode Automatic `
  --app-gateway myAppGateway `
  --backend-pool-name appGatewayBackendPool
```

### <a name="install-nginx"></a>安装 NGINX

```azurecli
az vmss extension set `
  --publisher Microsoft.Azure.Extensions `
  --version 2.0 `
  --name CustomScript `
  --resource-group myResourceGroupAG `
  --vmss-name myvmss `
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"],"commandToExecute": "./install_nginx.sh" }'
```

## <a name="test-the-application-gateway"></a>测试应用程序网关

若要获取应用程序网关的公共 IP 地址，请使用 [az network public-ip show](/cli/network/public-ip#az_network_public_ip_show)。 复制该公共 IP 地址，并将其粘贴到浏览器的地址栏。

```azurepowershell
az network public-ip show `
  --resource-group myResourceGroupAG `
  --name myAGPublicIPAddress `
  --query [ipAddress] `
  --output tsv
```

![在应用程序网关中测试基 URL](./media/tutorial-restrict-web-traffic-cli/application-gateway-nginxtest.png)

## <a name="clean-up-resources"></a>清理资源

当不再需要资源组、应用程序网关以及所有相关资源时，请将其删除。

```azurecli
az group delete --name myResourceGroupAG --location chinanorth
```

## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 设置网络
> * 创建启用 WAF 的应用程序网关
> * 创建虚拟机规模集
> * 创建存储帐户和配置诊断

> [!div class="nextstepaction"]
> [使用 SSL 终端创建应用程序网关](./tutorial-ssl-cli.md)

<!-- Update_Description: code update -->