---
title: 使用 Visual Studio Code 创建 Azure 资源管理器模板 | Azure
description: 使用可在资源管理器模板上运行的 Azure 资源管理器工具扩展。
services: azure-resource-manager
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
origin.date: 07/17/2018
ms.date: 09/03/2018
ms.topic: quickstart
ms.author: v-yeche
ms.openlocfilehash: 042b1b1d8d1d9e95653d57bb2435089e5b949d68
ms.sourcegitcommit: aee279ed9192773de55e52e628bb9e0e9055120e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2018
ms.locfileid: "43171485"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>快速入门：使用 Visual Studio Code 创建 Azure 资源管理器模板

了解如何使用 Visual Studio Code 和 Azure 资源管理器工具扩展创建 Azure 资源管理器模板。 可以在 Visual Studio Code 中不使用扩展创建资源管理器模板，但是该扩展提供自动完成选项，可以简化模板开发。 若要了解与部署和管理 Azure 解决方案相关联的概念，请参阅 [Azure Resource Manager 概述](resource-group-overview.md)。

如果没有 Azure 订阅，请在开始前[创建一个试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="prerequisites"></a>先决条件

若要完成本文，需要做好以下准备：

- [Visual Studio Code](https://code.visualstudio.com/)。
- 资源管理器工具扩展。 若要安装此扩展，请使用以下步骤：

    1. 打开 Visual Studio Code。
    2. 按 **CTRL+SHIFT+X** 打开“扩展”窗格
    3. 搜索“Azure 资源管理器工具”，然后选择“安装”。
    4. 选择“重新加载”完成扩展安装。

## <a name="open-a-quickstart-template"></a>打开快速入门模板

无需从头开始创建模板，可以通过 [Azure 快速入门模板](https://github.com/Azure/azure-quickstart-templates/)打开一个模板。 Azure 快速入门模板是资源管理器模板的存储库。

本快速入门中使用的模板称为[创建标准存储帐户](https://github.com/Azure/azure-quickstart-templates/tree/master/101-storage-account-create/)。 该模板定义 Azure 存储帐户资源。

1. 在 Visual Studio Code 中，选择“文件”>“打开文件”。
2. 在“文件名”中粘贴以下 URL：

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. 选择“打开”以打开该文件。
4. 选择“文件”>“另存为”，将该文件作为 **azuredeploy.json** 保存到本地计算机。

## <a name="edit-the-template"></a>编辑模板

若要了解如何使用 Visual Studio Code 编辑模板，请将额外的一个元素添加到 outputs 节。

1. 在 Visual Studio Code 中，将额外的一个输出添加到导出的模板：

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    完成后，outputs 节如下所示：

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    如果在 Visual Studio Code 中复制并粘贴了代码，请尝试重新键入 **value** 元素，以体验资源管理器工具扩展的 intellisense 功能。

    ![资源管理器模板 - Visual Studio Code - intellisense](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. 选择“文件”>“保存”以保存文件。

## <a name="deploy-the-template"></a>部署模板

可通过多种方法来部署模板。  在本快速入门中，请使用 Azure CLI 来部署模板。

下面的示例使用 [az group create](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-create) 命令。 若要执行这些步骤，需要安装最新的 [Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-az-cli2?view=azure-cli-latest)，并使用 [az login](https://docs.azure.cn/zh-cn/cli/reference-index?view=azure-cli-latest#az-login) 登录到 Azure 帐户。

<!--Not Available on Cloud Shell-->
1. 在 Azure CLI 中运行以下命令：

    ```cli
    az group create --name <ResourceGroupName> --location <AzureLocation>

    az group deployment create --name <DeploymentName> --resource-group <ResourceGroupName> --template-file <TemplateFileNameWithPath>
    ```

    <!--Not Available on Cloud Shell-->

2. 运行以下 CLI 命令列出新建的存储帐户：

    ```cli
    az storage account show --resource-group <ResourceGroupName> --name <StorageAccountName>
    ```

## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。

1. 在 Azure 门户上的左侧菜单中选择“资源组”。
2. 在“按名称筛选”字段中输入资源组名称。
3. 选择资源组名称。  应会看到，该资源组中总共有六个资源。
4. 在顶部菜单中选择“删除资源组”。

## <a name="next-steps"></a>后续步骤

本教程介绍了如何使用 Visual Studio Code 创建模板，以及如何使用 Azure 本地 Shell 部署模板。 在下一教程中，你将详细了解如何开发模板，以及如何使用模板引用。
<!--Not Available on Cloud Shell-->

> [!div class="nextstepaction"]
> [创建加密的存储帐户](./resource-manager-tutorial-create-encrypted-storage-accounts.md)

<!-- Update_Description: new articles on resource manager quickstart create templates use visual studio code -->
<!--ms.date: 09/03/2018-->