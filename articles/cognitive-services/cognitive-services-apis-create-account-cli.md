---
title: 使用 Azure CLI 创建认知服务资源
titleSuffix: Azure Cognitive Services
description: 通过使用 Azure 命令行接口创建和订阅资源，开始使用 Azure 认知服务。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
origin.date: 06/26/2019
ms.date: 07/17/2019
ms.author: v-junlch
ms.openlocfilehash: e3afffcfd732e505848f4539cdbd74691389cd4e
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70104133"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-command-line-interfacecli"></a>使用 Azure 命令行接口 (CLI) 创建认知服务资源

使用本快速入门可通过 [Azure 命令行接口 (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 开始使用 Azure 认知服务。 认知服务由你在 Azure 订阅中创建的 Azure [资源](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)表示。 创建资源后，请使用生成的密钥和终结点对应用程序进行身份验证。 


本快速入门介绍如何使用 [Azure 命令行接口 (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 注册 Azure 认知服务以及创建包含单服务或多服务订阅的帐户。 这些服务由 Azure [资源](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)表示，可用于连接到一个或多个 Azure 认知服务 API。

## <a name="prerequisites"></a>先决条件

* 有效的 Azure 订阅。 免费[创建一个帐户](https://www.azure.cn/pricing/1rmb-trial/)。
* [Azure 命令行接口 (CLI)](/cli/install-azure-cli?view=azure-cli-latest)

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="install-the-azure-cli-and-sign-in"></a>安装 Azure CLI 并登录 

安装 [Azure CLI](/cli/install-azure-cli?view=azure-cli-latest)。 若要登录到本地安装的 CLI，请运行 [az login](/cli/reference-index#az-login) 命令：

```console
az login
```

也可以使用绿色的“尝试”按钮在浏览器中运行这些命令。 
 
## <a name="create-a-new-azure-cognitive-services-resource-group"></a>创建新的 Azure 认知服务资源组

在创建认知服务资源之前，必须具有 Azure 资源组才能包含该资源。 在创建新资源时，可以选择新建资源组，或使用现有的资源组。 本文介绍如何创建新资源组。

### <a name="choose-your-resource-group-location"></a>选择资源组位置

若要创建资源，需要为订阅提供一个可用的 Azure 位置。 可以使用 [az account list-locations](/cli/account#az-account-list-locations) 命令检索可用位置的列表。 可以从多个位置访问大部分认知服务。 选择离你最近的位置，或查看哪些位置可供服务使用。

> [!IMPORTANT]
> * 请记住选择的 Azure 位置，因为在调用 Azure 认知服务时需要用到。
> * 某些认知服务的可用性因区域而异。 有关详细信息，请参阅 [Azure 产品在各区域中的推出情况](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)。  

```azurecli
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

选择 Azure 位置后，在 Azure CLI 中使用 [az group create](/cli/group#az-group-create) 命令创建新的资源组。

在以下示例中，请将 Azure 位置 `chinanorth` 替换为你的订阅可用的某个 Azure 位置。

```azurecli
az group create \
    --name cognitive-services-resource-group \
    --location chinanorth
```

## <a name="create-a-cognitive-services-resource"></a>创建认知服务资源

### <a name="choose-a-cognitive-service-and-pricing-tier"></a>选择认知服务和定价层

创建新资源时，需要知道你要使用哪种服务，以及所需的[定价层](https://www.azure.cn/pricing/details/cognitive-services/)（或 SKU）。 创建资源时，需要使用此信息和其他信息作为参数。

> [!NOTE]
> 许多认知服务提供免费层让客户试用相应的服务。 若要使用免费层，请使用 `F0` 作为资源的 SKU。

### <a name="vision"></a>影像

| 服务                    | 种类                      |
|----------------------------|---------------------------|
| 计算机视觉            | `ComputerVision`          |
| 人脸 API                   | `Face`                    |

### <a name="speech"></a>语音

| 服务            | 种类                 |
|--------------------|----------------------|
| 语音服务    | `SpeechServices`     |

### <a name="language"></a>语言

| 服务            | 种类                |
|--------------------|---------------------|
| LUIS               | `LUIS`              |
| 文本分析     | `TextAnalytics`     |
| 文本翻译   | `TextTranslation`   |

### <a name="decision"></a>决策

| 服务           | 种类               |
|-------------------|--------------------|
| 内容审查器 | `ContentModerator` |

可以使用 [az cognitiveservices account list-kinds](/cli/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-kinds) 命令查找可用认知服务“种类”的列表：

```azurecli
az cognitiveservices account list-kinds
```

### <a name="add-a-new-resource-to-your-resource-group"></a>将新资源添加到资源组

若要创建并订阅新的认知服务资源，请使用 [az cognitiveservices account create](/cli/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create) 命令。 此命令会将新的可计费资源添加到前面创建的资源组。 创建新资源时，需要知道你要使用哪种服务，及其定价层（或 SKU）和 Azure 位置：

可以使用以下命令为异常检测器创建名为 `anomaly-detector-resource` 的 F0（免费）资源。

```azurecli
az cognitiveservices account create \
    --name anomaly-detector-resource \
    --group cognitive-services-resource-group \
    --kind AnomalyDetector \
    --sku F0 \
    --location chinanorth \
    --yes
```

## <a name="get-the-keys-for-your-subscription"></a>获取订阅的密钥

若要登录到本地安装的命令行接口 (CLI)，请使用 [az login](/cli/reference-index?view=azure-cli-latest#az-login) 命令。

```console
az login
```

使用 [az cognitiveservices account keys list](/cli/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list) 命令获取认知服务资源的密钥。

```azurecli
    az cognitiveservices account keys list \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group
```

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>清理资源

如果想要清理并删除认知服务订阅，可以删除资源或资源组。 删除资源组同时也会删除与资源组相关联的任何其他资源。

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 az group delete 命令。

```azurecli
az group delete --name storage-resource-group
```

## <a name="see-also"></a>另请参阅

* [什么是 Azure 认知服务？](Welcome.md)
* [自然语言支持](language-support.md)

<!-- Update_Description: link update -->
