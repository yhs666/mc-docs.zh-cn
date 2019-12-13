---
title: 在 Azure IoT Central 应用程序中查看帐单并将试用转换为即用即付 | Microsoft Docs
description: 作为管理员，了解如何在 Azure IoT Central 应用程序中查看帐单以及如何从试用转换为即用即付
author: v-krghan
ms.author: v-yiso
origin.date: 07/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: caf3d6a2f32cafffacccd5417ace506ce204a9ea
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883807"
---
# <a name="view-your-bill-in-iot-central-application"></a>在 IoT Central 应用程序中查看帐单

本文介绍了如何以管理员身份在 Azure IoT Central 应用程序中查看帐单，以及如何将试用版转换为即用即付版。

只有 Azure IoT Central 应用程序的“管理员”  角色才能访问和使用“管理”  部分。 如果你创建了 Azure IoT Central 应用程序，则会自动分配到该应用程序的“管理员”  角色。

## <a name="view-your-bill"></a>查看帐单

若要查看帐单，请在“管理”  部分转到“计费”  页。 Azure 计费页将在新选项卡中打开，可以在此处看到每个 Azure IoT Central 应用程序的帐单。

## <a name="convert-your-trial-to-pay-as-you-go"></a>将试用版转换为即用即付版

- **试用版**应用程序免费 7 天，然后过期。 它们可以在到期之前随时转换为即用即付。
- **即用即付**应用程序按设备收费，前 5 台设备按订阅免费。

可在 [Azure IoT Central 定价页](htps://www.azure.cn/pricing/details/iot-central/)上了解定价详细信息。

在“计费”部分中，可以将试用应用程序转换为即用即付应用程序。

若要完成此自助服务过程，请遵循以下步骤：

1. 在“管理”部分转到“计费”页。  

    ![试用状态](media/howto-administer/freetrialbilling.png)

1. 选择“转换为即用即付”  。

    ![转换试用版](media/howto-administer/convert.png)

1. 选择相应的 Azure Active Directory，然后选择要对即用即付应用程序使用的 Azure 订阅。

1. 选择“转换”后，应用程序随即会变成即用即付应用程序，并开始计费。 

## <a name="next-steps"></a>后续步骤

现在，你已了解如何在 Azure IoT Central 应用程序中查看帐单，建议下一步是了解如何在 Azure IoT Central 中[自定义应用程序 UI](howto-customize-ui.md)。