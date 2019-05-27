---
title: 快速入门 - 使用 Azure 应用程序网关定向 Web 流量 - Azure CLI | Microsoft Docs
description: 了解如何使用 Azure CLI 创建 Azure 应用程序网关，用以将 Web 流量重定向到后端池中的虚拟机。
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
origin.date: 01/08/2019
ms.date: 05/20/2019
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: b76dbb1db105c497a454c3ca786e2a63548bc8ee
ms.sourcegitcommit: dc0db00da570f0c57f4a1398797fc158a2c423c5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65960891"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>快速入门：使用 Azure 应用程序网关定向 Web 流量 - Azure CLI

本快速入门介绍如何使用 Azure CLI 创建应用程序网关。  创建应用程序网关后，可对其进行测试，以确保正常工作。 使用 Azure 应用程序网关可为端口分配侦听器、创建规则以及向后端池添加资源，以便将应用程序 Web 流量定向到特定资源。 为方便演示，本文使用了一种简单的设置，其中包括一个公共前端 IP、一个用于在此应用程序网关上托管单个站点的基本侦听器、两个用于后端池的虚拟机，以及一个基本请求路由规则。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="prerequisites"></a>先决条件

### <a name="azure-cli"></a>Azure CLI

如果选择在本地安装并使用 CLI，请运行 Azure CLI 2.0.4 或更高版本。 若要查找版本，请运行 **az --version**。 有关安装或升级的信息，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

### <a name="resource-group"></a>资源组

在 Azure 中，可将相关的资源分配到资源组。 使用 [az group create](/cli/group#az-group-create) 创建资源组。 

以下示例在“chinanorth”位置创建名为“myResourceGroupAG”的资源组。

```azurecli 
az group create --name myResourceGroupAG --location chinanorth
```

### <a name="required-network-resources"></a>所需的网络资源 

Azure 需要一个虚拟网络才能在创建的资源之间通信。  应用程序网关子网只能包含应用程序网关。 不允许其他资源。  可为应用程序网关创建新的子网，或者使用现有的子网。 本示例将创建两个子网：一个用于应用程序网关，另一个用于后端服务器。 可根据用例将应用程序网关的前端 IP 配置为公共或专用 IP。 本示例选择了公共前端 IP。

若要创建虚拟网络和子网，请使用 [az network vnet create](/cli/network/vnet#az-network-vnet-create)。 运行 [az network public-ip create](https://docs.azure.cn/zh-cn/cli/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) 即可创建公共 IP 地址。

```azurecli
az network vnet create `
  --name myVNet `
  --resource-group myResourceGroupAG `
  --location chinanorth `
  --address-prefix 10.0.0.0/16 `
  --subnet-name myAGSubnet `
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create `
  --name myBackendSubnet `
  --resource-group myResourceGroupAG `
  --vnet-name myVNet   `
  --address-prefix 10.0.2.0/24
az network public-ip create `
  --resource-group myResourceGroupAG `
  --name myAGPublicIPAddress
```

### <a name="backend-servers"></a>后端服务器

后端可以包含 NIC、虚拟机规模集、公共 IP、内部 IP、完全限定的域名 (FQDN) 和多租户后端（例如 Azure 应用服务）。 在此示例中，将创建两个虚拟机，供 Azure 用作应用程序网关的后端服务器。 还可以在虚拟机上安装 IIS，以验证 Azure 是否已成功创建应用程序网关。

#### <a name="create-two-virtual-machines"></a>创建两个虚拟机

在虚拟机上安装 [NGINX Web 服务器](https://docs.nginx.com/nginx/)，验证是否已成功创建应用程序网关。 可使用 cloud-init 配置文件在 Linux 虚拟机上安装 NGINX 并运行“Hello World”Node.js 应用。 有关 cloud-init 的详细信息，请参阅 [cloud-init 对 Azure 中虚拟机的支持](/virtual-machines/linux/using-cloud-init)。

在当前的 Shell 中，将以下配置复制并粘贴到名为 *cloud-init.txt* 的文件中。 输入 *editor cloud-init.txt* 即可创建该文件。

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

使用 [az network nic create](/cli/network/nic#az-network-nic-create) 创建网络接口。 若要创建虚拟机，请使用 [az vm create](/cli/vm#az-vm-create)。

```azurecli
for i in `seq 1 2`; do
  az network nic create `
    --resource-group myResourceGroupAG `
    --name myNic$i `
    --vnet-name myVNet `
    --subnet myBackendSubnet
  az vm create `
    --resource-group myResourceGroupAG `
    --name myVM$i `
    --nics myNic$i `
    --image UbuntuLTS `
    --admin-username azureuser `
    --generate-ssh-keys `
    --custom-data cloud-init.txt
done
```

## <a name="create-the-application-gateway"></a>创建应用程序网关

使用 [az network application-gateway create](/cli/network/application-gateway) 创建应用程序网关。 使用 Azure CLI 创建应用程序网关时，请指定配置信息，例如容量、SKU 和 HTTP 设置。 然后，Azure 将添加网络接口的专用 IP 地址作为应用程序网关后端池中的服务器。

```azurecli
address1=$(az network nic show --name myNic1 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
address2=$(az network nic show --name myNic2 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
az network application-gateway create `
  --name myAppGateway `
  --location chinanorth `
  --resource-group myResourceGroupAG `
  --capacity 2 `
  --sku Standard_Medium `
  --http-settings-cookie-based-affinity Enabled `
  --public-ip-address myAGPublicIPAddress `
  --vnet-name myVNet `
  --subnet myAGSubnet `
  --servers "$address1" "$address2"
```

Azure 可能需要长达 30 分钟的时间来创建应用程序网关。 创建该网关以后，即可在“应用程序网关”页的“设置”部分查看以下设置：

- **appGatewayBackendPool**：位于“后端池”页。 它指定所需的后端池。
- **appGatewayBackendHttpSettings**：位于“HTTP设置”页。 它指定应用程序网关使用端口 80 和 HTTP 协议进行通信。
- **appGatewayHttpListener**：位于“侦听器”页。 它指定与 **appGatewayBackendPool** 关联的默认侦听器。
- **appGatewayFrontendIP**：位于“前端 IP 配置”页。 它将 *myAGPublicIPAddress* 分配到 **appGatewayHttpListener**。
- **rule1**：位于“规则”页。 它指定与 **appGatewayHttpListener** 关联的默认路由规则。

## <a name="test-the-application-gateway"></a>测试应用程序网关

虽然 Azure 不需 NGINX Web 服务器即可创建应用程序网关，但本快速入门中安装了它，用来验证 Azure 是否已成功创建应用程序网关。 若要获取新应用程序网关的公共 IP 地址，请使用 [az network public-ip show](/cli/network/public-ip#az-network-public-ip-show)。 

```azurepowershell
az network public-ip show `
  --resource-group myResourceGroupAG `
  --name myAGPublicIPAddress `
  --query [ipAddress] `
  --output tsv
```

复制该公共 IP 地址，并将其粘贴到浏览器的地址栏。
    
![测试应用程序网关](./media/quick-create-cli/application-gateway-nginxtest.png)

刷新浏览器时，会看到另一 VM 的名称。 有效的响应中会确认已成功创建应用程序网关，并且它可以成功连接到后端。

## <a name="clean-up-resources"></a>清理资源

如果不再需要通过应用程序网关创建的资源，请使用 [az group delete](/cli/group#az-group-delete) 命令删除资源组。 删除资源组时，也会删除应用程序网关和及其所有的相关资源。

```azurecli 
az group delete --name myResourceGroupAG
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [通过 Azure CLI 使用应用程序网关管理 Web 流量](./tutorial-manage-web-traffic-cli.md)


<!-- Update_Description: wording update -->