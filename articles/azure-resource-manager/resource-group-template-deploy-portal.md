---
title: 使用 Azure 门户部署 Azure 资源 | Azure
description: 使用 Azure 门户和 Azure Resource Manager 来部署资源。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: conceptual
origin.date: 06/27/2019
ms.date: 07/22/2019
ms.author: v-yeche
ms.openlocfilehash: 1d560359a214713201715228f17c35c4772dddf6
ms.sourcegitcommit: 5fea6210f7456215f75a9b093393390d47c3c78d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68337473"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>使用 Resource Manager 模板和 Azure 门户部署资源

了解如何将 [Azure 门户](https://portal.azure.cn)与 [Azure 资源管理器](resource-group-overview.md)配合使用来部署 Azure 资源。 若要了解有关管理资源的信息，请参阅[通过 Azure 门户管理 Azure 资源](manage-resources-portal.md)。

使用 Azure 门户部署 Azure 资源通常涉及两个步骤：

- 创建资源组。
- 将资源部署到资源组。

此外，还可以部署 Azure 资源管理器模板以创建 Azure 资源。

本文介绍了这两种方法。

## <a name="create-a-resource-group"></a>创建资源组

1. 若要创建新资源组，请从 [Azure 门户](https://portal.azure.cn)中选择“资源组”  。

    ![选择资源组](./media/resource-group-template-deploy-portal/select-resource-groups.png)

1. 在“资源组”下，选择“添加”  。

    ![添加资源组](./media/resource-group-template-deploy-portal/add-resource-group.png)

1. 选择或输入以下属性值：

    - **订阅**：选择 Azure 订阅。
    - **资源组**：为资源组指定名称。
    - **区域**：指定 Azure 位置。 资源组在此处存储有关资源的元数据。 出于合规性原因，你可能会想要指定该元数据的存储位置。 一般情况下，建议指定大部分资源的驻留位置。 使用相同位置可简化模板。

    ![设置组的值](./media/resource-group-template-deploy-portal/set-group-properties.png)

1. 选择“查看 + 创建”  。
1. 查看值，然后选择“创建”  。
1. 选择“刷新”  ，然后才能在列表中看到新资源组。

## <a name="deploy-resources-to-a-resource-group"></a>将资源部署到资源组

创建资源组后，可以从市场将资源部署到该组。 市场为常见方案提供预定义的解决方案。

1. 若要开始部署，请从 [Azure 门户](https://portal.azure.cn)中选择“创建资源”  。

    ![新建资源](./media/resource-group-template-deploy-portal/new-resources.png)

1. 查找希望部署的资源类型。 资源将按类别进行组织。 如果看不到想要部署的特定解决方案，可以在市场中搜索。 以下屏幕截图显示已选择“Ubuntu Server”。

    ![选择资源类型](./media/resource-group-template-deploy-portal/select-resource-type.png)

1. 根据所选资源的类型，需要在部署之前设置相关属性的集合。 对于所有类型，必须选择目标资源组。 下图显示了如何创建 Linux 虚拟机并将其部署到所创建的资源组。

    ![创建资源组](./media/resource-group-template-deploy-portal/select-existing-group.png)

    也可以在部署资源时创建资源组。 选择“新建”  并为资源组指定名称。

1. 部署开始。 部署可能需要几分钟的时间。 有些资源比其他资源需要更长的时间。 完成部署后，会看到一条通知。 选择“转到资源”  以将其打开

    ![查看通知](./media/resource-group-template-deploy-portal/view-notification.png)

1. 部署资源后，可以通过选择“添加”  将更多资源添加到资源组。

    ![添加资源](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>从自定义模板部署资源

如果想要执行部署，但不使用市场中的任何模板，可以创建自定义模板来针对解决方案定义基础结构。 若要了解如何创建模板，请参阅[了解 Azure 资源管理器模板的结构和语法](resource-group-authoring-templates.md)。

> [!NOTE]
> 门户界面不支持引用[来自 Key Vault 的密码](resource-manager-keyvault-parameter.md)。 请改用 [PowerShell](resource-group-template-deploy.md) 或 [Azure CLI](resource-group-template-deploy-cli.md)，从本地或从外部 URI 部署模板。


1. 若要通过门户部署自定义模板，请选择“创建资源”  ，搜索“模板”  ， 然后选择“模板部署”  。

    ![搜索模板部署](./media/resource-group-template-deploy-portal/search-template.png)
    
    <!--MOONCAKE: CUSTOMIZE, EDIT CAREFULLY-->
    
1. 选择“创建”  。

    ![选择“创建”](./media/resource-group-template-deploy-portal/show-template-option.png)
    
   <!--MOONCAKE: NOT VALID ON ![View options](./media/resource-group-template-deploy-portal/see-options.png)-->

1. 选择“编辑模板”。  你已具有可用于自定义的空白模板。

    ![创建模板](./media/resource-group-template-deploy-portal/blank-template.png)

1. 可以手动编辑 JSON 语法，也可以从[模板库快速入门](https://github.com/Azure/azure-quickstart-templates/)中选择预建模板。 但是，本文使用的是“添加资源”  选项。

    ![编辑模板](./media/resource-group-template-deploy-portal/select-add-resource.png)

1. 选择“存储帐户”  并为其提供名称。 完成提供值后，选择“确定”  。

    ![选择存储帐户](./media/resource-group-template-deploy-portal/add-storage-account.png)

1. 编辑器会自动为资源类型添加 JSON。 请注意，它包含用于定义存储帐户类型的参数。 选择“其他安全性验证”  。

    ![显示模板](./media/resource-group-template-deploy-portal/show-json.png)

1. 现在，可以选择部署模板中定义的资源。 若要部署，请同意条款和条件，然后选择“购买”  。

    ![部署模板](./media/resource-group-template-deploy-portal/provide-custom-template-values.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>从保存到帐户中的模板部署资源

该门户允许用户将模板保存到 Azure 帐户，以便以后重新部署它。 有关模板的详细信息，请参阅[创建和部署第一个 Azure 资源管理器模板](resource-manager-create-first-template.md)。

1. 若要查找已保存模板，请选择“更多服务”  。

    ![其他服务](./media/resource-group-template-deploy-portal/more-services.png)

1. 搜索模板  并选择该选项。

    ![搜索模板](./media/resource-group-template-deploy-portal/find-templates.png)

1. 从已保存到帐户的模板列表中，选择要使用的模板。

    ![已保存模板](./media/resource-group-template-deploy-portal/saved-templates.png)

1. 选择“部署”  以重新部署该已保存模板。

    ![部署已保存模板](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

<!--MOONCAKE: CUSTOMIZE, EDIT CAREFULLY-->

## <a name="next-steps"></a>后续步骤

- 若要查看审核日志，请参阅[使用资源管理器审核操作](./resource-group-audit.md)。
- 若要排查部署错误，请参阅[查看部署操作](./resource-manager-deployment-operations.md)。
- 若要从部署或资源组中导出模板，请参阅[导出 Azure 资源管理器模板](./manage-resource-groups-portal.md#export-resource-groups-to-templates)。

<!-- Not Available on [Azure Deployment Manager](deployment-manager-overview.md)-->
<!--Update_Description: update meta properties, update link -->