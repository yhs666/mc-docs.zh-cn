---
title: 使用 Azure 门户创建和部署 Azure 资源管理器模板 | Azure
description: 了解如何使用 Azure 门户创建第一个 Azure 资源管理器模板，以及如何部署该模板。
services: azure-resource-manager
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
origin.date: 09/07/2018
ms.date: 09/24/2018
ms.topic: quickstart
ms.author: v-yeche
ms.openlocfilehash: f1fba4b3dba76c216057b152e7237bc2e0b554ab
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52663217"
---
<!-- Verify successfully-->
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-azure-portal"></a>快速入门：使用 Azure 门户创建和部署 Azure 资源管理器模板

了解如何使用 Azure 门户生成第一个 Azure 资源管理器模板，以及如何从门户编辑和部署该模板。

Resource Manager 模板为 JSON 文件，用于定义针对解决方案进行部署时所需的资源。 若要创建模板，不一定总要从头开始。 本教程将介绍如何从 Azure 门户生成模板。 然后，可以自定义模板并将其部署。

本教程中的说明将创建一个 Azure 存储帐户。 可以使用相同的过程来创建其他 Azure 资源。

如果没有 Azure 订阅，请在开始前[创建一个试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="generate-a-template-using-the-portal"></a>使用门户生成模板

在本部分，我们将使用 Azure 门户创建一个存储帐户。 在部署存储帐户之前，可以使用相应的选项浏览门户根据配置生成的模板。 可以保存模板，便于将来重复使用。

1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 选择“创建资源” > “存储” > “存储帐户 - Blob、文件、表、队列”。

    ![使用 Azure 门户创建 Azure 存储帐户](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
3. 输入以下信息。 在下一步骤请务必选择“自动化选项”而不是“创建”，以便在部署模板之前可以查看它。

    - **名称**：为存储帐户指定唯一的名称。 在屏幕截图中，名称为 *mystorage0626*。
    - **资源组**：使用所选的名称创建新的 Azure 资源组。 在屏幕截图中，资源组名称为 *mystorage0626rg*。

    可对剩余的属性使用默认值。

    ![使用 Azure 门户创建 Azure 存储帐户配置](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > 某些导出的模板需要经过一些编辑才能部署。

4. 选择屏幕底部的“自动化选项”。 门户在“模板”选项卡上上显示该模板：

    ![通过门户生成模板](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    主窗格会显示该模板。 它是包含四个顶级元素的 JSON 文件。 有关详细信息，请参阅[了解 Azure 资源管理器模板的结构和语法](./resource-group-authoring-templates.md)

    **Parameter** 元素下面定义了五个参数。 若要查看在部署期间提供的值，请选择“参数”选项卡。

    ![通过门户生成模板](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    这些值是在上一部分配置的。 使用模板和 parameters 文件可以创建一个 Azure 存储帐户。

5. 选项卡的顶部有两个菜单项：<!-- Mooncake contain two menu instead-->
    
    - **下载**：将模板和 parameters 文件下载到本地计算机。
    <!-- Not Available on - **Add to library**: Add the template to the library to be reused in the future.-->
    - **部署**：将 Azure 存储帐户部署到 Azure。

    <!-- Not Available on In this tutorial, you use the **Add to library** option.-->

<!-- Not Available on 6. Select **Add to library**.-->
<!-- Not Available on 7. Enter **Name** and **Description**, and then select **Save**.-->

> [!NOTE]
>  大部分人会选择将模板保存到本地计算机，或者 Github 等公共存储。  
<！-- 未在“模板库功能为预览版”中提供。-->

<！-- 未在##“编辑和部署模板”中提供-->

## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。

1. 在 Azure 门户上的左侧菜单中选择“资源组”。
2. 在“按名称筛选”字段中输入资源组名称。
3. 选择资源组名称。  应会看到资源组中的存储帐户。
4. 在顶部菜单中选择“删除资源组”。

## <a name="next-steps"></a>后续步骤

本教程已介绍如何通过 Azure 门户生成模板，以及如何使用门户部署模板。 本快速入门中使用的模板是包含一个 Azure 资源的简单模板。 如果模板较为复杂，使用 Visual Studio Code 或 Visual Studio 开发模板会更方便。 下一快速入门还介绍如何使用 Azure PowerShell 和 Azure 命令行界面 (CLI) 来部署模板。

> [!div class="nextstepaction"]
> [使用 Visual Studio Code 创建模板](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)

<!-- Update_Description: update meta properties, wording update -->
