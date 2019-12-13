---
title: 在 Azure IoT Central 中使用应用程序模板 | Microsoft Docs
description: 如何以操作员的身份在 Azure IoT Central 应用程序中使用设备集。
author: dominicbetts
ms.author: v-yiso
origin.date: 05/30/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: fb44cf46bb24ae8de32d9a22d69c6c193e49c9d9
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883147"
---
# <a name="use-application-templates"></a>使用应用程序模板

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

本文介绍解决方案经理如何创建和使用应用程序模板。

创建 Azure IoT Central 应用程序时，可以选择内置的示例模板。 也可以从现有的 IoT Central 应用程序创建自己的应用程序模板。 然后，可以在创建新应用程序时使用自己的应用程序模板。

创建应用程序模板时，该模板将包含现有应用程序中的以下项：

- 默认应用程序仪表板，包括仪表板布局和定义的所有磁贴。
- 设备模板，包括度量、设置、属性、命令和仪表板。
- 规则。 包括所有规则定义。 但不包括电子邮件操作以外的其他操作。
- 设备集，包括其状态和仪表板。

> [!WARNING]
> 如果仪表板包含显示特定设备相关信息的磁贴，则这些磁贴会在新应用程序中显示“找不到请求的资源”。  必须重新配置这些磁贴才能显示有关新应用程序中设备的信息。

创建应用程序模板时，该模板不包含以下项：

- 设备
- 用户
- 作业定义
- 连续数据导出定义

需手动将这些项添加到从应用程序模板创建的任何应用程序。

## <a name="create-an-application-template"></a>创建应用程序模板

若要从现有的 IoT Central 应用程序创建应用程序模板：

1. 在应用程序中转到“管理”部分。 
1. 选择“应用程序模板导出”。 
1. 在“应用程序模板导出”页上，输入模板的名称和说明。 
1. 选择“导出”按钮创建应用程序模板。  现在可以复制“可共享的链接”，使用户能够从该模板创建新应用程序： 

![创建应用程序模板](media/howto-use-app-templates/create-template.png)

## <a name="use-an-application-template"></a>使用应用程序模板

若要使用应用程序模板创建新的 IoT Central 应用程序，需要事先创建一个**可共享的链接**。 将该**可共享的链接**粘贴到浏览器的地址栏中。 随后会显示“创建应用程序”页，其中已选择你的自定义应用程序模板： 

![从模板创建应用程序](media/howto-use-app-templates/create-app.png)

选择付款计划，并填写窗体中的其他字段。 然后选择“创建”，从应用程序模板创建新的 IoT Central 应用程序。 

## <a name="manage-application-templates"></a>管理应用程序模板

在“应用程序模板导出”页上，可以删除或更新应用程序模板。 

如果删除应用程序模板，则不再可以使用事先生成的可共享链接来创建新应用程序。

若要更新应用程序模板，请在“应用程序模板导出”页上更改模板名称或说明。  然后再次选择“导出”按钮。  此操作会生成新的**可共享链接**，并使以前的任何**可共享链接** URL 失效。

## <a name="next-steps"></a>后续步骤

了解如何使用应用程序模板后，建议接下来了解如何[在 Azure 门户中管理 IoT Central](howto-manage-iot-central-from-portal.md)
