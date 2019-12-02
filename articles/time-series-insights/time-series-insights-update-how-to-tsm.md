---
title: 预览版环境中的数据建模 - Azure 时序见解 | Microsoft Docs
description: 了解 Azure 时序见解预览版中的数据建模。
author: deepakpalled
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
origin.date: 10/29/2019
ms.date: 12/02/2019
ms.custom: seodec18
ms.openlocfilehash: 50d0f19cf9d3db5cab614a41fbafc32c7cde7f55
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74388948"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Azure 时序见解预览版中的数据建模

本文介绍如何在 Azure 时序见解预览版中使用时序模型。 它详细介绍了几个常见数据方案。

若要详细了解如何使用更新，请阅读 [Azure 时序见解预览版资源管理器](./time-series-insights-update-explorer.md)。

## <a name="types"></a>类型

### <a name="create-a-single-type"></a>创建单个类型

1. 转到“时序模型”选择器面板，并从菜单中选择“类型”  。 折叠面板以集中于“时序模型”类型。

    [![“类型”窗格](media/v2-update-how-to-tsm/portal-one.png)](media/v2-update-how-to-tsm/portal-one.png#lightbox)

1. 选择“+ 添加”  。
1. 输入与类型相关的所有详细信息，然后选择“创建”。  此操作在环境中创建类型。

    [![适用于添加类型的选择](media/v2-update-how-to-tsm/portal-two.png)](media/v2-update-how-to-tsm/portal-two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>批量上传一个或多个类型

1. 选择“上传 JSON”。 
1. 选择包含类型有效负载的文件。
1. 选择“上传”。 

    [![适用于批量上传一个或多个类型的选择](media/v2-update-how-to-tsm/portal-three.png)](media/v2-update-how-to-tsm/portal-three.png#lightbox)

### <a name="edit-a-single-type"></a>编辑单个类型

1. 选择类型，然后选择“编辑”。  
1. 进行所需的更改，然后选择“保存”。 

    [![适用于编辑类型的选择](media/v2-update-how-to-tsm/portal-four.png)](media/v2-update-how-to-tsm/portal-four.png#lightbox)

### <a name="delete-a-type"></a>删除类型

1. 选择类型，然后选择“删除”。 
1. 如果没有实例与类型相关联，则它已被删除。

    [![“删除”按钮](media/v2-update-how-to-tsm/portal-five.png)](media/v2-update-how-to-tsm/portal-five.png#lightbox)

## <a name="hierarchies"></a>层次结构

### <a name="create-a-single-hierarchy"></a>创建单个层次结构

1. 转到“时序模型”选择器面板，并从菜单中选择“层次结构”  。 折叠面板以集中于“时序模型”层次结构。

    [![“层次结构”窗格](media/v2-update-how-to-tsm/portal-six.png)](media/v2-update-how-to-tsm/portal-six.png#lightbox)

1. 选择“+ 添加”  。

    [![“添加”按钮](media/v2-update-how-to-tsm/portal-seven.png)](media/v2-update-how-to-tsm/portal-seven.png#lightbox)

1. 在右窗格中选择“+ 添加级别”  。

    [![“添加级别”按钮](media/v2-update-how-to-tsm/portal-eight.png)](media/v2-update-how-to-tsm/portal-eight.png#lightbox)

1. 输入层次结构详细信息，然后选择“创建”  。

    [![层次结构详细信息和“创建”按钮](media/v2-update-how-to-tsm/portal-nine.png)](media/v2-update-how-to-tsm/portal-nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>批量上传一个或多个层次结构

1. 选择“上传 JSON”。 
1. 选择包含层次结构有效负载的文件。
1. 选择“上传”。 

    [![适用于批量上传层次结构的选择](media/v2-update-how-to-tsm/portal-ten.png)](media/v2-update-how-to-tsm/portal-ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>编辑单个层次结构

1. 选择层次结构，然后选择“编辑”  。
1. 进行所需的更改，然后选择“保存”。 

    [![适用于编辑单个层次结构的选择](media/v2-update-how-to-tsm/portal-eleven.png)](media/v2-update-how-to-tsm/portal-eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>删除层次结构

1. 选择层次结构，然后选择“删除”  。 
1. 如果没有实例与层次结构相关联，则它已被删除。

    [![“删除”按钮](media/v2-update-how-to-tsm/portal-twelve.png)](media/v2-update-how-to-tsm/portal-twelve.png#lightbox)

## <a name="instances"></a>Instances

### <a name="create-a-single-instance"></a>创建单个实例

1. 转到“时序模型”选择器面板，并从菜单中选择“实例”  。 折叠面板以集中于“时序模型”实例。

    [![“实例”窗格](media/v2-update-how-to-tsm/portal-thirteen.png)](media/v2-update-how-to-tsm/portal-thirteen.png#lightbox)

1. 选择“添加”   。

    [![适用于添加实例的选择](media/v2-update-how-to-tsm/portal-fourteen.png)](media/v2-update-how-to-tsm/portal-fourteen.png#lightbox)

1. 输入实例详细信息，选择类型和层次结构关联，然后选择“创建”  。

### <a name="bulk-upload-one-or-more-instances"></a>批量上传一个或多个实例

1. 选择“上传 JSON”。 
1. 选择包含实例有效负载的文件。

    [![适用于批量上传一个或多个实例的选择](media/v2-update-how-to-tsm/portal-fifteen.png)](media/v2-update-how-to-tsm/portal-fifteen.png#lightbox)

1. 选择“上传”。 

### <a name="edit-a-single-instance"></a>编辑单个实例

1. 选择实例，然后选择“编辑”。  
1. 进行所需的更改，然后选择“保存”。 

    [![适用于编辑单个实例的选择](media/v2-update-how-to-tsm/portal-sixteen.png)](media/v2-update-how-to-tsm/portal-sixteen.png#lightbox)

## <a name="next-steps"></a>后续步骤

- 有关时序模型的详细信息，请阅读[数据建模](./time-series-insights-update-tsm.md)。

- 若要详细了解预览版，请阅读[在 Azure 时序见解预览版资源管理器中可视化数据](./time-series-insights-update-explorer.md)。

- 若要了解支持的 JSON 形状，请阅读[支持的 JSON 形状](./time-series-insights-send-events.md#supported-json-shapes)。

<!-- Images -->
[1]: media/v2-update-how-to-tsm/portal_one.png
[2]: media/v2-update-how-to-tsm/portal_two.png
[3]: media/v2-update-how-to-tsm/portal_three.png
[4]: media/v2-update-how-to-tsm/portal_four.png
[5]: media/v2-update-how-to-tsm/portal_five.png
[6]: media/v2-update-how-to-tsm/portal_six.png
[7]: media/v2-update-how-to-tsm/portal_seven.png
[8]: media/v2-update-how-to-tsm/portal_eight.png
[9]: media/v2-update-how-to-tsm/portal_nine.png
[10]: media/v2-update-how-to-tsm/portal_ten.png
[11]: media/v2-update-how-to-tsm/portal_eleven.png
[12]: media/v2-update-how-to-tsm/portal_twelve.png
[13]: media/v2-update-how-to-tsm/portal_thirteen.png
[14]: media/v2-update-how-to-tsm/portal_fourteen.png
[15]: media/v2-update-how-to-tsm/portal_fifteen.png
[16]: media/v2-update-how-to-tsm/portal_sixteen.png