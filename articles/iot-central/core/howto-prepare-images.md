---
title: 将图像上传到 Azure IoT Central 应用程序 | Microsoft Docs
description: 了解如何以开发者的身份准备图像并将其上传到 Azure IoT Central 应用程序。
author: dominicbetts
ms.author: v-yiso
origin.date: 07/11/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: c9b70db3d0a81c45e71d047a214e063b22b2f02f
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883157"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>准备图像并将其上传到 Azure IoT Central 应用程序

本文介绍如何以开发者的身份上传自定义图像，以便对 Azure IoT Central 应用程序进行自定义。 例如，可以使用设备图像自定义设备仪表板。

## <a name="before-you-begin"></a>准备阶段

若要完成本文中的步骤，需要做好以下准备：

1. Azure IoT Central 应用程序。 有关详细信息，请参阅[创建应用程序快速入门](quick-deploy-iot-central.md)。
1. 用于缩放图像文件和重设其大小的工具。

## <a name="choose-where-to-use-custom-images"></a>选择在何处使用自定义图像

可以将自定义图像添加到 Azure IoT Central 应用程序中的以下位置：

* “我的应用程序”  页

    ![“应用程序管理器”页上的图像](media/howto-prepare-images/applicationmanager.png)

* 应用程序仪表板

    ![应用程序仪表板上的图像](media/howto-prepare-images/homepage.png)

* 设备模板

    ![设备模板上的图像](media/howto-prepare-images/devicetemplate.png)

* 设备仪表板上的磁贴

    ![设备磁贴上的图像](media/howto-prepare-images/devicetile.png)

* 设备集仪表板上的磁贴

    ![设备集磁贴上的图像](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>准备图像

在所有四个位置中，可以使用 PNG、GIF 或 JPEG 图像。

下表汇总了可以使用的图像大小：

| 位置 | 大小 |
| -------- | ------ |
| 应用程序管理器 | 268x160 像素 |
| 设备模板 | 64x64 像素 |
| 仪表板磁贴 | 最小的磁贴为 200x200 像素，较大的磁贴可以是由多个小磁贴组成的正方形或矩形。 例如，200x400 像素、400x200 像素或 400x400 像素 |

若要确保在应用程序中的最佳显示效果，创建的图像应与上表中显示的尺寸匹配。

## <a name="upload-the-images"></a>上传图像

以下各部分介绍了如何在不同的位置上传图像：

### <a name="application-manager"></a>应用程序管理器

若要上传要在“我的应用程序”  页上使用的图像，请导航至“应用程序设置”  页的“管理”  部分。 必须是管理员才能完成此任务：

![上传应用程序图像](media/howto-prepare-images/uploadapplicationmanager.png)

选择“应用程序图像”  磁贴以从本地计算机上传图像（268x160 像素）。

### <a name="application-dashboard"></a>应用程序仪表板

若要在应用程序仪表板上上传图像，请导航到应用程序的“仪表板”  页，然后选择“编辑”  。 必须是开发者才能完成此任务：

![上传仪表板图像](media/howto-prepare-images/uploadhomepage.png)

在“配置图像”  下，选择“图像”  磁贴以从本地计算机上传图像。 最小的磁贴为 200x200 像素，较大的磁贴可以是由多个小磁贴组成的正方形或矩形。 例如，200x400 像素、400x200 像素或 400x400 像素。

**保存**所上传的图像。 你可以在编辑模式下调整图像大小。 完成后，选择“完成”  。

### <a name="device-template"></a>设备模板

若要将图像上传到设备模板中，请导航到“设备模板”，选择设备模板  。 必须是开发者才能完成此任务：

![上传设备模板图像](media/howto-prepare-images/uploaddevicetemplate.png)

选择“图像”磁贴以从本地计算机上传图像（64x64 像素）。

### <a name="device-dashboard"></a>设备仪表板

若要将图像上传到设备仪表板中，请导航到“设备模板”，选择设备模板  。 然后选择“仪表板”选项卡  。必须是开发者才能完成此任务：

![上传设备仪表板图像](media/howto-prepare-images/uploaddevicedashboard.png)

在“配置图像”  下，选择“图像”磁贴  ，然后选择要从本地计算机上传的文件。 最小的磁贴为 200x200 像素，较大的磁贴可以是由多个小磁贴组成的正方形或矩形。 例如，200x400 像素、400x200 像素或 400x400 像素。

**保存**所上传的图像。 你可以在编辑模式下调整图像大小及其位置。 完成后，选择“完成”  。

### <a name="device-set-dashboard"></a>设备集仪表板

若要在设备集仪表板中上传图像，请导航到“设备集”，选择设备集和设备。  然后选择“仪表板”  页并选择“编辑”  ：

![上传设备集仪表板图像](media/howto-prepare-images/uploaddevicesetdashboard.png)

在“配置图像”  下，选择“图像”  磁贴以从本地计算机上传图像。 最小的磁贴为 200x200 像素，较大的磁贴可以是由多个小磁贴组成的正方形或矩形。 例如，200x400 像素、400x200 像素或 400x400 像素。

**保存**所上传的图像。 你可以在编辑模式下调整图像大小及其位置。 完成后，选择“完成”  。

## <a name="next-steps"></a>后续步骤

了解如何准备图像并将其上传到 Azure IoT Central 应用程序后，建议接下来执行以下步骤：

* [自定义 Azure IoT Central UI](./howto-customize-ui.md)
* [向仪表板添加磁贴](./howto-add-tiles-to-your-dashboard.md)
* [在 Azure IoT Central 应用程序中管理设备](howto-manage-devices.md)
