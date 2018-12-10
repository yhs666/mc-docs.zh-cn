---
title: 使用 Visual Studio Code 创建 Azure 资源管理器模板 | Azure
description: 使用 Visual Studio Code 和可在资源管理器模板上运行的 Azure 资源管理器工具扩展。
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
ms.date: 11/19/2018
ms.topic: quickstart
ms.author: v-yeche
ms.openlocfilehash: 24523af62ba175a846b2bcaaf2deb72e5af0a04d
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52663216"
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

可通过多种方法来部署模板。  在本快速入门中，使用 Azure 本地 Shell 或 Azure PowerShell 部署模板。

<!--Not Available on Cloud Shell-->
1. 从 Azure 本地 shell 运行以下命令。 选择用于显示 PowerShell 代码或 CLI 代码的选项卡。

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```cli
    az cloud set -n AzureChinaCloud
    az login
    az group create --name <ResourceGroupName> --location <AzureLocation>

    az group deployment create --name <DeploymentName> --resource-group <ResourceGroupName> --template-file <TemplateFileName>
    ```

    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    ```powershell
    Connect-AzureRmAccount -Environment AzureChinaCloud
    New-AzureRmResourceGroup -Name <ResourceGroupName> -Location <AzureLocation>

    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroupName> -TemplateFile <TemplateFileName>
    ```

    ---

    以下屏幕截图显示了示例部署：

    # <a name="clitabcli"></a>[CLI](#tab/CLI)

    ![Azure CLI 部署模板](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure PowerShell 部署模板](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template-powershell.png)

    ---

    屏幕截图中使用了以下值：

    - **&lt;ResourceGroupName>**：myresourcegroup0709。 参数有两种形式。  请确保使用相同的值。
    - **&lt;AzureLocation>**：chinaeast2
    - **&lt;DeployName>**：mydeployment0709
    - **&lt;TemplateFile>**：azuredeploy.json

    在屏幕截图上的输出中，存储帐户名称为 *3tqebj3slyfyestandardsa*。 

7. 运行以下 CLI 或 PowerShell 命令，列出新建的存储帐户：

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```cli
    az storage account show --resource-group <ResourceGroupName> --name <StorageAccountName>
    ```

    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

    ```powershell
    Get-AzureRmStorageAccount -ResourceGroupName <ResourceGroupName> -Name <StorageAccountName>
    ```

    ---

## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。

1. 在 Azure 门户上的左侧菜单中选择“资源组”。
2. 在“按名称筛选”字段中输入资源组名称。
3. 选择资源组名称。  应会看到，该资源组中总共有六个资源。
4. 在顶部菜单中选择“删除资源组”。

## <a name="next-steps"></a>后续步骤

本快速入门的主要关注点是如何使用 Visual Studio Code 编辑 Azure 快速入门模板中的现有模板。 此外还介绍了如何通过本地电脑使用 CLI 或 PowerShell 部署模板。 Azure 快速入门模板中的模板可能并未提供你所需的一切。 下一教程介绍如何从模板参考中查找信息，以便创建加密的 Azure 存储帐户。
<!--Not Available on Cloud Shell-->

> [!div class="nextstepaction"]
> [创建加密的存储帐户](./resource-manager-tutorial-create-encrypted-storage-accounts.md)

<!-- Update_Description: update meta properties, wording update -->
