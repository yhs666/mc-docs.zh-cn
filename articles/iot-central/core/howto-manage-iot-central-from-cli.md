---
title: 从 Azure CLI 管理 IoT Central | Microsoft Docs
description: 本文介绍如何使用 CLI 创建和管理 IoT Central 应用程序。 可以使用 CLI 查看、修改和删除应用程序。
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: v-yiso
origin.date: 08/23/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 8c8b488bf03031efe91f96d9ec61167377c745ed
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883818"
---
# <a name="manage-iot-central-from-azure-cli"></a>从 Azure CLI 管理 IoT Central

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

如果不在 [Azure IoT Central 应用程序管理器](https://aka.ms/iotcentral)网站上创建和管理 IoT Central 应用程序，可以使用 [Azure CLI](/cli/azure/) 来管理应用程序。

## <a name="prerequisites"></a>先决条件

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

如果你偏好于在本地计算机上运行 Azure CLI，请参阅[安装 Azure CLI](/cli/azure/install-azure-cli)。 在本地运行 Azure CLI 时，请在尝试运行本文所述的命令之前，使用 **az login** 命令登录到 Azure。

## <a name="create-an-application"></a>创建应用程序

使用 [az iotcentral app create](/cli/azure/iotcentral/app#az-iotcentral-app-create) cmdlet 在 Azure 订阅中创建一个 IoT Central 应用程序。 例如：

```azurecli-interactive
# Create a resource group for the IoT Central application
az group create --location "East US" \
    --name "MyIoTCentralResourceGroup"
```

```azurecli-interactive
# Create an IoT Central application
az iotcentral app create \
  --resource-group "MyIoTCentralResourceGroup" \
  --name "myiotcentralapp" --subdomain "mysubdomain" \
  --sku S1 --template "iotc-demo@1.0.0" \
  --display-name "My Custom Display Name"
```

这些命令首先在美国东部位置为应用程序创建一个资源组。 下表描述了与 **az iotcentral app create** 命令结合使用的参数：

| 参数         | 说明 |
| ----------------- | ----------- |
| resource-group    | 包含该应用程序的资源组。 此资源组必须已存在于订阅中。 |
| location          | 此命令默认使用资源组中的位置。 目前可以在“美国”、“澳大利亚”、“亚太”或“欧洲”位置创建 IoT Central 应用程序。     |
| name              | 应用程序在 Azure 门户中的名称。 |
| subdomain         | 应用程序 URL 中的子域。 在该示例中，应用程序 URL 为 https://mysubdomain.azureiotcentral.com 。 |
| sku               | 目前，唯一的值是 **S1**（标准层）。 请参阅 [Azure IoT Central 定价](htps://www.azure.cn/pricing/details/iot-central/)。 |
| template          | 要使用的应用程序模板。 有关详细信息，请参阅下表： |
| display-name      | UI 中显示的应用程序名称。 |

**包含正式推出的功能的应用程序模板**

| 模板名称            | 说明 |
| ------------------------ | ----------- |
| iotc-default@1.0.0       | 创建一个空的应用程序，以便在其中填充你自己的设备模板和设备。 |
| iotc-demo@1.0.0          | 创建一个应用程序，其中包含已为冷藏食品贩卖机创建的设备模板。 通过此模板来完成 Azure IoT Central 的入门。 |
| iotc-devkit-sample@1.0.0 | 创建一个应用程序，其中的设备模板可以用来连接 MXChip 或 Raspberry Pi 设备。 如果你是在其中任一设备上进行试验的设备开发人员，请使用此模板。 |


**包含公共预览版功能的应用程序模板**

| 模板名称            | 说明 |
| ------------------------ | ----------- |
| iotc-pnp-preview@1.0.0   | 创建一个空的即插即用预览版应用程序，以便在其中填充你自己的设备模板和设备。 |
| iotc-condition@1.0.0     | 使用“店内分析 - 状况监视”模板创建应用程序。 使用此模板连接和监视商店环境。 |
| iotc-consumption@1.0.0   | 使用水消耗量监视模板创建应用程序。 使用此模板监视和控制水流。 |
| iotc-distribution@1.0.0  | 使用数字分发模板创建应用程序。 使用此模板，可以通过将密钥资产和操作数字化来提高仓库输出效率。 |
| iotc-inventory@1.0.0     | 使用智能库存管理模板创建应用程序。 使用此模板自动执行传感器的接收、产品移动、周期计数和跟踪操作。 |
| iotc-logistics@1.0.0     | 使用互联物流模板创建应用程序。 使用此模板，可以通过位置和状况监视实时跟踪空中、水上和陆地上的运输情况。 |
| iotc-meter@1.0.0         | 使用智能计量监视模板创建应用程序。 使用此模板，可以监视能源消耗、网络状态并确定改进客户支持和智能计量管理的趋势。  |
| iotc-patient@1.0.0       | 使用患者持续监视模板创建应用程序。 使用此模板，可以扩大患者护理范围、减少再次入院并控制疾病。 |
| iotc-power@1.0.0         | 使用太阳能电池板监视模板创建应用程序。 使用此模板，可以监视太阳能电池板状态和能源生成趋势。 |
| iotc-quality@1.0.0       | 使用水质监视模板创建应用程序。 使用此模板以数字方式监视水质。|
| iotc-store@1.0.0         | 使用“店内分析 - 结帐”模板创建应用程序。 使用此模板，可以监视和管理商店中的结帐流程。 |
| iotc-waste@1.0.0         | 使用互联废物管理模板创建应用程序。 使用此模板，可以监视废物箱和调度现场操作员。 |

> [!NOTE]
> 预览应用程序模板目前仅在“欧洲”和“美国”   位置可用。

## <a name="view-your-applications"></a>查看应用程序

使用 [az iotcentral app list](/cli/azure/iotcentral/app#az-iotcentral-app-list) 命令可列出 IoT Central 应用程序和查看元数据。

## <a name="modify-an-application"></a>修改应用程序

使用 [az iotcentral app update](/cli/azure/iotcentral/app#az-iotcentral-app-update) 命令可更新 IoT Central 应用程序的元数据。 例如，若要更改应用程序的显示名称：

```azurecli-interactive
az iotcentral app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>删除应用程序

使用 [az iotcentral app delete](/cli/azure/iotcentral/app#az-iotcentral-app-delete) 命令可删除 IoT Central 应用程序。 例如：

```azurecli-interactive
az iotcentral app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>后续步骤

了解如何从 Azure CLI 管理 Azure IoT Central 应用程序后，建议接下来学习：

> [!div class="nextstepaction"]
> [管理应用程序](howto-administer.md)
