---
title: 在 Azure IoT Central 应用程序中分析设备数据 | Microsoft Docs
description: 本文介绍如何使用查询和可视化效果在 Azure IoT Central 应用程序中分析设备数据。
author: lmasieri
ms.author: v-yiso
origin.date: 06/09/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 5259a6823d5bef470bdde94976384a87ca6a4412
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883158"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>如何使用分析功能来分析设备数据

*本文适用于操作员、构建者和管理员。*

Azure IoT Central 提供各种分析功能对来自设备的大量数据进行分析。 若要开始，请访问左侧窗格中的“Analytics”。 

## <a name="querying-your-data"></a>查询数据

若要开始，需要选择一个**设备集**，添加一个**筛选器**（可选），然后选择一个**时间段**。 完成后，选择“显示结果”开始可视化数据。 

* **设备集：** [设备集](howto-use-device-sets.md)是用户定义的设备组。 例如，奥克兰市的所有冷冻机，或所有版本 2.0 的风力涡轮机。

* **筛选器：** 可以选择性地在搜索中添加筛选器，以专注于分析数据。 一次最多可以添加 10 个筛选器。 例如，在奥克兰市的所有冷冻机中，找到温度超过 60 度的冷冻机。
* **时间段：** 默认会检索过去 10 分钟的数据。 可将此值更改为预定义的时间范围之一，或者选择自定义的时间段。

  ![Analytics 查询](media/howto-create-analytics/analytics-query.png)

## <a name="visualizing-your-data"></a>可视化数据

查询数据后，可以开始将数据可视化。 可以显示/隐藏度量、更改数据的聚合方式，以及按不同的设备属性进一步拆分数据。  

* **拆分依据：** 按设备属性拆分数据可以进一步向下钻取到数据。 例如，可按设备 ID 或位置拆分结果。

* **度量：** 一次可以选择显示/隐藏设备报告的最多 10 个不同遥测项。 度量是指温度和湿度等属性。

* **聚合：** 默认按平均值聚合数据，但可以根据需要更改为其他数据聚合方式。

   ![Analytics 可视化效果拆分依据](media/howto-create-analytics/analytics-splitby.png)

## <a name="interacting-with-your-data"></a>与数据交互

可通过多种方式更改查询结果，以满足可视化需求。 可以在图形视图与网格视图之间切换、放大和缩小、刷新数据集，以及更改线条的显示方式。

* **显示网格：** 以表格格式显示结果，这样可以查看每个数据点的特定值。 此视图也符合易访问性标准。
* **显示图表：** 将以线条格式显示结果，这样有助于确定上行或下行趋势和异常值。

  ![显示分析数据的网格视图](media/howto-create-analytics/analytics-showgrid.png)

使用缩放可以专注于数据。 如果你在结果集中发现了一个想要重点关注的时间段，可将光标移到想要放大的区域，然后使用可用的控件来执行以下操作之一：

* **放大：** 选择某个时间段后，将启用放大功能，使你能够放大数据。
* **缩小：** 使用此控件可在上次缩放大小的基础上缩小一个级别。 例如，如果你已放大数据三次，则每执行一次缩小操作，数据就会缩小一级。
* **缩放重置：** 执行不同级别的缩放后，可以使用缩放重置控件还原到原始结果集。

  ![对数据执行缩放](media/howto-create-analytics/analytics-zoom.png)

可根据需要更改线条样式。 有四个选项：

* **线条：** 每两个数据点之间的平缓线条。
* **平滑：** 在每两个点之间的曲线。
* **阶梯：** 图表上每两个点之间的线条是一个阶梯。
* **散点：** 在图表上绘制所有点，且不使用线条连接它们。

  ![Analytics 中提供的不同线条类型](media/howto-create-analytics/analytics-linetypes.png)

最后，可以通过从三种模式中选择一种，在 Y 轴上排列数据：

* **堆积：** 将每个度量的图堆积在一起，每个图都有自己的 Y 轴。 在选择多个度量的情况下，如果需要以独立的视图来呈现这些度量，则可使用堆积图。
* **非堆积：** 使用一个 Y 轴为每个度量值绘图，但 Y 轴的值会随突出显示的度量值而更改。 如果需要将多个度量值重叠在一起，并且需要跨这些度量值查看同一时间范围的模式，则可使用非堆积图。
* **共享 Y 轴：** 所有图共享同一 Y 轴，轴的值不会更改。 如果需要在使用拆分方式对数据进行切分时查看单个度量值，则可使用共享 Y 轴图。

  ![使用不同的可视化模式在 Y 轴上排列数据](media/howto-create-analytics/analytics-yaxis.png)

## <a name="next-steps"></a>后续步骤

了解如何创建适用于 Azure IoT Central 应用程序的自定义分析后，建议接下来执行以下步骤：

> [!div class="nextstepaction"]
> [准备和连接 Node.js 应用程序](howto-connect-nodejs.md)