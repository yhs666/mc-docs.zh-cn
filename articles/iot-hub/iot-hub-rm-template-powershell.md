---
title: 使用模板创建 Azure IoT 中心 (PowerShell) | Azure
description: 如何使用 Azure Resource Manager 模板和 PowerShell 创建 IoT 中心。
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 7eade855-c289-4ffb-b5ef-02be8c5f670f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 04/02/2019
ms.author: v-yiso
ms.date: 05/06/2019
ms.openlocfilehash: 98bc4355010dfe0e3192aca072ef9ab335b5dac7
ms.sourcegitcommit: a4b88888b83bf080752c3ebf370b8650731b01d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2019
ms.locfileid: "74179004"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>使用 Azure 资源管理器模板创建 IoT 中心 (PowerShell)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

了解如何使用 Azure 资源管理器模板创建 IoT 中心和使用者组。 Resource Manager 模板为 JSON 文件，用于定义针对解决方案进行部署时所需的资源。 有关开发资源管理器模板的详细信息，请参阅 [Azure 资源管理器文档](/azure-resource-manager/)。

如果没有 Azure 订阅，请在开始前[创建一个试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="create-an-iot-hub"></a>创建 IoT 中心

本快速入门中使用的资源管理器模板来自 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/101-iothub-with-consumergroup-create/)。 下面是该模板的副本：

<!--[!code-json[iothub-creation](~/quickstart-templates/101-iothub-with-consumergroup-create/azuredeploy.json)]-->

该模板创建一个具有三个终结点（eventhub、cloud-to-device 和 messaging）的 Azure Iot 中心和一个使用者组。 有关更多模板示例，请参阅 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Devices&pageNumber=1&sort=Popular)。 可在[此处](https://docs.microsoft.com/azure/templates/microsoft.devices/iothub-allversions)找到 Iot 中心模板架构。

可通过多种方法来部署模板。  在本教程中，将使用 Azure PowerShell。
```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$iotHubName = Read-Host -Prompt "Enter the IoT Hub name"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-iothub-with-consumergroup-create/azuredeploy.json" `
    -iotHubName $iotHubName
```

正如你在 PowerShell 脚本中所见，使用的模板来自 Azure 快速入门模板。 若要使用你自己的模板，你需要先上传模板文件，然后使用 `-TemplateFile` 开关来指定文件名。  有关示例，请参阅[部署模板](../azure-resource-manager/resource-manager-quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell#deploy-the-template)。

## <a name="next-steps"></a>后续步骤

现在，你已使用 Azure 资源管理器模板部署了一个 IoT 中心，你可能希望进一步进行探索：

* 阅读了解 [IoT 中心资源提供程序 REST API][lnk-rest-api] 的相关功能。
* 若要详细了解 Azure 资源管理器功能，请阅读 [Azure 资源管理器概述][lnk-azure-rm-overview]。
* 有关要在模板中使用的 JSON 语法和属性，请参阅 [Microsoft.Devices 资源类型](https://docs.microsoft.com/en-us/azure/templates/microsoft.devices/iothub-allversions)。

若要详细了解如何开发 IoT 中心，请参阅以下文章：

- [C SDK 简介][lnk-c-sdk]
- [Azure IoT SDK][lnk-sdks]

若要进一步探索 IoT 中心的功能，请参阅：

* [使用 Azure IoT Edge 将 AI 部署到边缘设备][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://www.azure.cn/pricing/1rmb-trial/
[lnk-azure-portal]: https://portal.azure.cn/
[lnk-status]: https://status.azure.com/status
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/powershell-azure-resource-manager.md

[lnk-c-sdk]: ./iot-hub-device-sdk-c-intro.md
[lnk-sdks]: ./iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/quickstart-linux.md
