---
title: "使用 Azure CLI (az.py) 创建 IoT 中心 | Azure"
description: "如何使用跨平台的 Azure CLI 2.0 (az.py) 创建 Azure IoT 中心。"
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 06/16/2017
ms.author: v-yiso
ms.date: 11/20/2017
ms.openlocfilehash: 2b18932bf5440d44682a1108086d30aab7b79516
ms.sourcegitcommit: 9a89fa2b33cbd84be4d8270628567bf0925ae11e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2017
---
# <a name="create-an-iot-hub-using-the-azure-cli-20"></a>使用 Azure CLI 2.0 创建 IoT 中心

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>介绍

可使用 Azure CLI 2.0 (az.py) 以编程方式创建和管理 Azure IoT 中心。 本文介绍如何使用 Azure CLI 2.0 (az.py) 创建 IoT 中心。

可以使用以下 CLI 版本之一完成任务：

* [Azure CLI (azure.js)](./iot-hub-create-using-cli-nodejs.md) - 适用于经典部署模型和资源管理部署模型的 CLI。
* Azure CLI 2.0 (az.py) - 适用于本文中所述的资源管理部署模型的下一代 CLI。

若要完成本教程，需要以下各项：

* 有效的 Azure 帐户。 如果没有帐户，只需几分钟即可创建一个[免费帐户][lnk-free-trial]。
* [Azure CLI 2.0][lnk-CLI-install]。

## <a name="sign-in-and-set-your-azure-account"></a>登录并设置 Azure 帐户

登录到 Azure 帐户，并选择订阅。

1. 在命令提示符中，运行 [login 命令][lnk-login-command]：
    
    ```azurecli
    az login
    ```

    按照说明使用代码进行身份验证，并通过 Web 浏览器登录 Azure 帐户。

2. 如果有多个 Azure 订阅，登录 Azure 可获得与凭据关联的所有 Azure 帐户的访问权限。 使用以下 [命令，列出可供使用的 Azure 帐户][lnk-az-account-command] ：
    
    ```azurecli
    az account list 
    ```

    使用以下命令，选择想要用于运行命令以创建 IoT 中心的订阅。 可使用上一命令输出中的订阅名称或 ID：

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a>创建 IoT 中心

使用 Azure CLI 创建资源组，并添加 IoT 中心。

1. 创建 IoT 中心时，必须在资源组中创建它。 使用现有资源组，或运行以下[命令创建资源组][lnk-az-resource-command]：

    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > 上一示例在美国西部位置创建资源组。 可运行 `az account list-locations -o table` 命令，查看可用位置的列表。
    >
    >

2. 运行以下[命令，使用 IoT 中心的全局唯一名称在资源组中创建 IoT 中心][lnk-az-iot-command]：
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> 上一命令在计费的 S1 定价层中创建 IoT 中心。 有关详细信息，请参阅 [Azure IoT 中心定价][lnk-iot-pricing]。
>
>

## <a name="remove-an-iot-hub"></a>删除 IoT 中心

可使用 Azure CLI [删除单个资源][lnk-az-resource-command]（例如 IoT 中心），或删除资源组及其所有资源（包括任何 IoT 中心）。

若要删除 IoT 中心，请运行以下命令：

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

若要删除资源组及其所有资源，请运行以下命令：

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>后续步骤
若要详细了解如何开发 IoT 中心，请参阅以下文章：

* [IoT 中心开发人员指南][lnk-devguide]

若要进一步探索 IoT 中心的功能，请参阅：

* [使用 Azure 门户管理 IoT 中心][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://www.azure.cn/pricing/1rmb-trial/
[lnk-CLI-install]: https://docs.azure.cn/zh-cn/cli/install-az-cli2?view=azure-cli-lastest
[lnk-login-command]: https://docs.azure.cn/zh-cn/cli/get-started-with-azure-cli?view=azure-cli-lastest
[lnk-az-account-command]: https://docs.azure.cn/zh-cn/cli/account?view=azure-cli-latest
[lnk-az-register-command]: https://docs.azure.cn/zh-cn/cli/provider?view=azure-cli-latest
[lnk-az-addcomponent-command]: https://docs.azure.cn/zh-cn/cli/component?view=azure-cli-latest
[lnk-az-resource-command]: https://docs.azure.cn/zh-cn/cli/resource?view=azure-cli-latest
[lnk-az-iot-command]: https://docs.azure.cn/zh-cn/cli/iot?view=azure-cli-latest
[lnk-iot-pricing]: https://www.azure.cn/pricing/details/iot-hub/
[lnk-devguide]: ./iot-hub-devguide.md
[lnk-portal]: ./iot-hub-create-through-portal.md

<!--Update_Description: update meta data only-->