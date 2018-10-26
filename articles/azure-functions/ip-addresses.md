---
title: Azure Functions 中的 IP 地址
description: 了解如何查找函数应用的入站和出站 IP 地址，以及这些地址发生更改的原因。
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
origin.date: 07/18/2018
ms.date: 10/19/2018
ms.author: v-junlch
ms.openlocfilehash: 6d879fa1587c4227163323d4247936165953fb74
ms.sourcegitcommit: 2d33477aeb0f2610c23e01eb38272a060142c85d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49453644"
---
# <a name="ip-addresses-in-azure-functions"></a>Azure Functions 中的 IP 地址

本文介绍与函数应用的 IP 地址相关的以下主题：

- 如何查找函数应用当前正在使用 IP 地址。
- 哪些因素导致函数应用的 IP 地址发生更改。
- 如何限制可访问函数应用的 IP 地址。
- 如何获取函数应用的专用 IP 地址。

IP 地址与函数应用而不是单个函数相关联。 传入的 HTTP 请求不能使用入站 IP 地址来调用单个函数；它们必须使用默认域名 (functionappname.chinacloudsites.cn) 或自定义域名。

## <a name="function-app-inbound-ip-address"></a>函数应用的入站 IP 地址

每个函数应用具有单个入站 IP 地址。 查找该 IP 地址：

1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 导航到函数应用。
3. 选择“平台功能”。
4. 选择“属性”，然后选择“虚拟 IP 地址”下面显示的入站 IP 地址。

## <a name="find-outbound-ip-addresses"></a>函数应用的出站 IP 地址

每个函数应用具有一组可用的出站 IP 地址。 从某个函数发起的任何出站连接（例如，与后端数据库的连接）使用某个可用的出站 IP 地址作为源 IP 地址。 无法事先知道给定的连接要使用哪个 IP 地址。 因此，后端服务必须向函数应用的所有出站 IP 地址开放其防火墙。

可以通过 PowerShell cmdlet 查找可用的出站 IP 地址：

```azurecli
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

## <a name="data-center-outbound-ip-addresses"></a>数据中心出站 IP 地址

如果需要将函数应用使用的出站 IP 地址加入白名单，另一种做法是将函数应用的数据中心（Azure 区域）加入白名单。 可以[下载列出所有 Azure 数据中心 IP 地址的 JSON 文件](https://www.microsoft.com/en-us/download/details.aspx?id=56519)。 然后，找到应用于运行函数应用的区域的 JSON 片段。

例如，中国北部区域的 JSON 片段可能如下所示：

```
{
  "name": "AzureChinaCloud.chinanorth",
  "id": "AzureChinaCloud.chinanorth",
  "properties": {
    "changeNumber": 9,
    "region": "chinanorth",
    "platform": "Azure",
    "systemService": "",
    "addressPrefixes": [
      "13.69.0.0/17",
      "13.73.128.0/18",
      ... Some IP addresses not shown here
     "213.199.180.192/27",
     "213.199.183.0/24"
    ]
  }
}
```

 有关此文件何时更新以及 IP 地址何时更改的信息，请展开[下载中心页](https://www.microsoft.com/en-us/download/details.aspx?id=56519)的“详细信息”部分。

## <a name="inbound-ip-address-changes"></a>入站 IP 地址更改

 如果执行以下操作，入站 IP 地址**可能**会更改：

- 删除函数应用，然后在不同的资源组中重新创建它。
- 删除资源组和区域组合中的最后一个函数应用，然后重新创建它。
- 删除 SSL 绑定（例如，在[证书续订](../app-service/app-service-web-tutorial-custom-ssl.md#renew-certificates)期间）。

如果未执行上面所列的操作，入站 IP 地址也可能会更改。

## <a name="outbound-ip-address-changes"></a>出站 IP 地址更改

如果执行以下操作，函数应用可用的出站 IP 地址集可能会更改：

- 执行可能更改入站 IP 地址的任何操作。
- 更改应用服务计划的定价层。 应用可在所有定价层中使用的所有可能出站 IP 地址列表在 `possibleOutboundIPAddresses` 属性中指定。 请参阅[查找出站 IP](#find-outbound-ip-addresses)。

如果未执行上面所列的操作，入站 IP 地址也可能会更改。

有意强制出站 IP 地址更改：

1. 在标准和高级 v2 定价层之间纵向缩放应用服务计划。
2. 等待 10 分钟。
3. 缩放回到最初的层。

## <a name="ip-address-restrictions"></a>IP 地址限制

可以配置允许或拒绝其访问函数应用的 IP 地址列表。 有关详细信息，请参阅 [Azure 应用服务静态 IP 限制](../app-service/app-service-ip-restrictions.md)。

## <a name="dedicated-ip-addresses"></a>专用 IP 地址

确定函数应用是否在应用服务环境中运行：

1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 导航到函数应用。
3. 选择“概述”选项卡。
4. 应用服务计划层显示在“应用服务计划/定价层”下面。 应用服务环境定价层为“隔离”。
 
或者，可以使用 PowerShell cmdlet：

```azurecli
az webapp show --resource-group <group_name> --name <app_name> --query sku --output tsv
```

应用服务环境的 `sku` 为“`Isolated`”。

## <a name="next-steps"></a>后续步骤

IP 发生更改的常见原因之一是函数应用的规模发生更改。 [详细了解函数应用的缩放](functions-scale.md)。

<!-- Update_Description: wording update -->