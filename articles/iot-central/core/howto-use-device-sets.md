---
title: 在 Azure IoT Central 应用程序中使用设备集 | Microsoft Docs
description: 如何以操作员的身份在 Azure IoT Central 应用程序中使用设备集。
author: ellenfosborne
ms.author: elfarber
origin.date: 06/09/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 2fc2750c57319da78c3e47362abf0a0ecfa79cce
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883164"
---
# <a name="use-device-sets-in-your-azure-iot-central-application"></a>在 Azure IoT Central 应用程序中使用设备集

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

本文介绍如何以操作员的身份在 Azure IoT Central 应用程序中使用设备集。

设备集是由于与某个指定的条件匹配而分组到一起的一系列设备。 设备集通过将设备分组到较小的逻辑组，来帮助大规模管理、可视化和分析设备。 例如，可以针对西雅图的所有空调设备创建一个设备集，使技术人员能够找到他们所要负责的设备。 本文介绍如何创建和配置设备集。

## <a name="create-a-device-set"></a>创建设备集

若要创建设备集：

1. 在左侧窗格中选择“设备集”  。

1. 选择“+ 新建”  。

    ![新建设备集](media/howto-use-device-sets/image1.png)

1. 为设备集指定一个在整个应用程序中唯一的名称。 还可以添加说明。 设备集只能包含单个设备模板中的设备。 选择用于此集的设备模板。

1. 通过选择属性、比较运算符和值，创建查询来识别设备集的设备。 可以添加满足设备集中指定的**所有**条件的多个查询和设备。 有权访问该应用程序的任何人都可以访问创建的设备集，因此，任何人都可以查看、修改或删除该设备集。

    ![设备集查询](media/howto-use-device-sets/image2.png)

    > [!NOTE]
    > 设备集是动态查询。 每当查看设备列表时，列表中都可能显示不同的设备。 列表内容取决于哪些设备当前满足查询的条件。

1. 选择“保存”  。

## <a name="configure-the-dashboard-for-your-device-set"></a>配置设备集的仪表板

创建设备集后，可以配置其**仪表板**。 **仪表板**是一个主页，在其中放置图像和链接。 还可以添加网格，用于列出设备集中的设备。

1. 在左侧窗格中选择“设备集”  。

1. 选择设备集。

1. 选择“仪表板”选项卡  。

1. 选择“编辑”  。

    ![设计模式已打开](media/howto-use-device-sets/image3.png)

1. 有关添加图像的详细信息，请参阅[准备图像并将其上传到 Azure IoT Central 应用程序](howto-prepare-images.md)。

1. 添加链接磁贴：
    1. 在右窗格中选择“链接”。 
    1. 为链接指定**标题**。
    1. 选择选中该链接时要打开的 URL。
    1. 为链接提供说明，该说明将显示在**标题**下面。
    1. 选择“保存”  。

        ![保存链接](media/howto-use-device-sets/image7.png)

    1. 可以在**仪表板**上移动链接磁贴并并调整其大小。

1. 添加网格。 网格是设备集中的设备表格，其中包含所选的列。
    1. 在右窗格中选择“网格”。 
    1. 为网格指定**标题**。
    1. 选择按下“添加/删除”  按钮时要显示的列。 在弹出的面板中，选择要显示的列，并使用右箭头将其选中。
    1. 选择“确定”。 
    1. 选择“保存”  。

        ![保存网格](media/howto-use-device-sets/image9.png)

    1. 将网格拖放到**仪表板**。

        > [!NOTE]
        > 可以添加多个图像、链接和网格。
  
    1. 选择“完成”  。

若要详细了解如何在 Azure IoT Central 中使用磁贴，请参阅[使用仪表板磁贴](howto-use-tiles.md)。

### <a name="configure-a-location-map-in-your-device-sets-dashboard"></a>在设备集仪表板中配置位置地图

可以添加地图，直观呈现设备集中设备的位置。

若要向设备集仪表板添加地图，必须在设备模板中配置位置度量或位置属性。 若要了解详细信息，请参阅[创建位置度量](howto-set-up-template.md)或[创建位置属性](howto-set-up-template.md)。

1. 在设备集**仪表板**中，选择库中的“地图”。 
2. 添加标题，选择之前配置的位置度量或属性。
3. 选择“保存”，此时地图图块会显示设备集中设备的上次已知位置。 
4. 操作者在查看设备集仪表板时，可以看到你已配置的所有磁贴（包括位置地图）。

可以在仪表板上调整地图磁贴的大小。 选择地图中的图钉会显示设备信息、名称和位置。 选择弹出项，以转到设备属性页。

## <a name="configure-the-list-for-your-device-set"></a>配置设备集的列表

创建设备集后，可以配置**列表**。 **列表**在包含所选列的表格中显示设备集中的所有设备。

1. 在左侧窗格中选择“设备集”  。

1. 选择“列表”选项卡。 

1. 选择“列选项”。 

    ![列选项](media/howto-use-device-sets/image11.png)

1. 选择要显示的列，并使用右箭头将其选中。

    ![选择列](media/howto-use-device-sets/image12.png)

1. 选择“确定”。 

## <a name="analytics"></a>分析

设备集中的分析功能与左侧窗格中的主要分析选项卡相同。 可以在有关[如何创建分析](howto-use-device-sets.md)的文章中详细了解分析。

## <a name="next-steps"></a>后续步骤

了解如何在 Azure IoT Central 应用程序中使用设备集后，建议接下来执行以下步骤：

> [!div class="nextstepaction"]
> [如何创建遥测规则](howto-create-telemetry-rules.md)
