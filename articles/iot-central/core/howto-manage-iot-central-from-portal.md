---
title: 通过 Azure 门户管理 IoT Central | Microsoft Docs
description: 本文介绍如何通过 Azure 门户创建和管理 IoT Central 应用程序。
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: v-yiso
origin.date: 10/02/2019
ms.date: 12/16/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: d4a1d589f79c5c4666351b3cae8a4471301e5905
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883328"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>通过 Azure 门户管理 IoT Central

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

如果不在 [Azure IoT Central 应用程序管理器](https://aka.ms/iotcentral)网站上创建和管理 IoT Central 应用程序，可以使用 [Azure 门户](https://portal.azure.cn)来管理应用程序。

## <a name="create-iot-central-applications"></a>创建 IoT Central 应用程序

若要创建应用程序，请导航到 [Azure 门户](https://ms.portal.azure.cn)，选择左侧主窗格中的“创建资源”  。

![管理门户：导航菜单](media/howto-manage-iot-central-from-portal/image0.png)

在搜索栏中，键入“IoT Central”  。

![管理门户：搜索](media/howto-manage-iot-central-from-portal/image0a1.png)

选择搜索结果中的“IoT Central 应用程序”行项  。

![管理门户：搜索结果](media/howto-manage-iot-central-from-portal/image0b1.png)

现在选择“创建”  。

![管理门户：IoT Central 资源](media/howto-manage-iot-central-from-portal/image0c1.png)

填写窗体中的所有字段。 此窗体与在 [Azure IoT Central 应用程序管理器](https://aka.ms/iotcentral)网站创建应用程序时需要填写的窗体类似。 有关详细信息，请参阅[创建 IoT Central 应用程序](quick-deploy-iot-central.md)快速入门。

可以创建包含通用功能的 IoT Central 应用程序，方法是：选择“示例 Contoso”、“自定义应用程序”和“示例 Devkits”作为应用程序模板。所有其他应用程序模板使用公共预览版功能。   

![创建 IoT Central 窗体](media/howto-manage-iot-central-from-portal/image6a.png)

“位置”是你想要创建应用程序的[地理位置](https://azure.microsoft.com/global-infrastructure/geographies/)  。 通常，应选择物理上离设备最近的位置以获得最佳性能。 目前，美国、澳大利亚、亚太地区或欧洲提供 Azure IoT Central     。  选择一个位置后，之后便不能将应用程序移到其他位置。

> [!NOTE]
> 预览应用程序模板目前仅在“欧洲”和“美国”   位置可用。

![管理门户：创建 IoT Central 资源](media/howto-manage-iot-central-from-portal/image1a.png)  

填写所有字段后，选择“创建”  。

## <a name="manage-existing-iot-central-applications"></a>管理现有 IoT Central 应用程序

如果已有 Azure IoT Central 应用程序，可将其删除或移动到 Azure 门户中的其他订阅或资源组。

> [!NOTE]
> Azure 门户中不会显示试用版应用程序，因为它们与订阅无关。

要想开始，请选择左侧主窗格中的“所有资源”  。 在搜索框中键入应用程序的名称以在资源列表中将其找到。 然后选择想要管理的 IoT Central 应用程序。

![管理门户：资源管理](media/howto-manage-iot-central-from-portal/image2a.png)

要想导航到应用程序，请选择“IoT Central 应用程序 URL”。

![管理门户：资源管理](media/howto-manage-iot-central-from-portal/image3.png)

若要将应用程序移动到其他资源组，请选择资源组旁边的“更改”  。 在“移动资源”页上，选取要将此应用程序迁移到其中的资源组  。

![管理门户：资源管理](media/howto-manage-iot-central-from-portal/image4a.png)

若要将应用程序移动到不同的订阅，请选择订阅旁的“更改”  。 在显示的对话框中，选取要将此应用程序迁移到其中的订阅。

![管理门户：资源管理](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>后续步骤

了解如何通过 Azure 门户管理 Azure IoT Central 应用程序后，建议接下来执行以下步骤：

> [!div class="nextstepaction"]
> [管理应用程序](howto-administer.md)