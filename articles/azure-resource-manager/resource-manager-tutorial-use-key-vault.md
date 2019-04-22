---
title: 在资源管理器模板部署中集成 Azure Key Vault | Azure
description: 了解如何在资源管理器模板部署期间使用 Azure Key Vault 来传递安全参数值
services: azure-resource-manager
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
origin.date: 03/04/2019
ms.date: 04/15/2019
ms.topic: tutorial
ms.author: v-yeche
ms.custom: seodec18
ms.openlocfilehash: f8e47c47b564aac927e21b2cea6c8cc0789bd186
ms.sourcegitcommit: 9f7a4bec190376815fa21167d90820b423da87e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "59529353"
---
<!-- Verify successfully-->
# <a name="tutorial-integrate-azure-key-vault-in-resource-manager-template-deployment"></a>教程：在资源管理器模板部署中集成 Azure Key Vault

了解如何在资源管理器部署期间从 Azure Key Vault 检索机密，并将机密作为参数传递。 值永远不会公开，因为仅引用其密钥保管库 ID。 有关详细信息，请参阅[在部署过程中使用 Azure Key Vault 传递安全参数值](./resource-manager-keyvault-parameter.md)。

[设置资源部署顺序](./resource-manager-tutorial-create-templates-with-dependent-resources.md)教程介绍如何创建虚拟机、虚拟网络以及其他一些依赖资源。 在本教程中，我们将自定义模板，以便从 Key Vault 检索虚拟机管理员密码。

![资源管理器模板 Key Vault 集成关系图](./media/resource-manager-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

本教程涵盖以下任务：

> [!div class="checklist"]
> * 准备 Key Vault
> * 打开快速入门模板
> * 编辑参数文件
> * 部署模板
> * 验证部署
> * 清理资源

如果没有 Azure 订阅，请在开始前[创建一个试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>先决条件

若要完成本文，需要做好以下准备：

* 包含[资源管理器工具扩展](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)的 [Visual Studio Code](https://code.visualstudio.com/)。
* 若要提高安全性，请使用为虚拟机管理员帐户生成的密码。 以下是密码生成示例：

    ```azurecli
    openssl rand -base64 32
    ```
    验证生成的密码是否符合虚拟机密码要求。 每个 Azure 服务具有特定的密码要求。 有关 VM 密码要求，请参阅[创建 VM 时，密码有什么要求？](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm)。

## <a name="prepare-a-key-vault"></a>准备 Key Vault

在本部分，我们将使用资源管理器模板创建 Key Vault 和机密。 此模板：

* 创建启用了 `enabledForTemplateDeployment` 属性的 Key Vault。 此属性必须为 true，这样，模板部署过程才能访问此 Key Vault 中定义的机密。
* 将机密添加到 Key Vault。  该机密存储虚拟机管理员密码。

如果你（要部署虚拟机模板的用户）不是 Key Vault 的所有者或参与者，则 Key Vault 的所有者或参与者必须向你授予对 Key Vault 的 Microsoft.KeyVault/vaults/deploy/action 访问权限。 有关详细信息，请参阅[在部署过程中使用 Azure Key Vault 传递安全参数值](./resource-manager-keyvault-parameter.md)。

模板需要使用你的 Azure AD 用户对象 ID 来配置权限。 以下过程获取对象 ID (GUID)。

1. 运行以下 Azure PowerShell 或 Azure CLI 命令。  

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```azurecli
    echo "Enter your email address that is associated with your Azure subscription):" &&
    read upn &&
    az ad user show --upn-or-object-id $upn --query "objectId" &&
    ```   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    ```powershell
    $upn = Read-Host -Prompt "Enter your user principal name (email address) used to sign in to Azure"
    (Get-AzADUser -UserPrincipalName $upn).Id
    ```
    
    <!--MOONCAKE: correct on (Get-AzADUser -UserPrincipalName $upn).Id-->
    
    或
    ```powershell
    $displayName = Read-Host -Prompt "Enter your user display name (i.e. John Dole, see the upper right corner of the Azure portal)"
    (Get-AzADUser -DisplayName $displayName).Id
    ```
    ---
2. 请记下对象 ID， 稍后在本教程中需要用到。

创建 Key Vault：

1. 选择下图登录到 Azure 并打开一个模板。 该模板将创建 Key Vault 和机密。

    <a href="https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Farmtutorials.blob.core.windows.net%2Fcreatekeyvault%2FCreateKeyVault.json"><img src="http://azuredeploy.net/deploybutton.png" alt="deploy to azure"/></a>
    
    <!-- Notice: URL is correct on Farmtutorials.blob.core.windows.net-->
    <!--MOONCAKE CUSTOMIZE: ADD NEW PAGE due to Mooncake template is different with global-->
    
2. 从左窗格选择“编辑模板”，在第 93 行中将 **centralus** 替换为 **chinanorth**，然后单击“保存”。
    ![资源管理器模板 Key Vault 集成部署门户](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-key-vault-portal-chenye-edit-template.png)
    
    <!--MOONCAKE CUSTOMIZE: ADD NEW PAGE due to Mooncake template is different with global-->
    <!--MOONCAKE CUSTOMIZE: We update **Create** due to Mooncake template is different with global-->
    
3. 选择或输入以下值。  输入值后不要选择“创建”。
    
    <!--MOONCAKE CUSTOMIZE: We update **Create** due to Mooncake template is different with global-->
    
    ![资源管理器模板 Key Vault 集成部署门户](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-key-vault-portal.png)
    
    * “部署解决方案模板”部分。
        |名称|值| | **订阅**| 选择一个 Azure 订阅。| | **资源组**| 分配一个唯一名称。 记下此名称，因为在下一个会话中将使用同一资源组来部署虚拟机。 将密钥保管库和虚拟机放在同一资源组中可以在本教程结束时更轻松地清理资源。| | **资源组位置**| 选择一个位置。  默认位置为“中国北部”。|
    * 在“参数”部分选择“编辑参数”。
        |名称|值| |**密钥保管库名称**：分配一个唯一名称。 
        |**租户 Id**| 模板函数会自动检索租户 ID。不要更改默认值。| |**AD 用户 ID**| 输入你在上一过程中检索到的 Azure AD 用户对象 ID。| |**机密名称**| 默认名称为 **vmAdminPassword**。 如果你在此处更改机密名称，则在部署虚拟机时需要更新机密名称。| |**机密值**| 输入你的机密。  机密是用于登录虚拟机的密码。 建议使用在上一过程中创建的生成密码。|
    
3. 选择左窗格中的“编辑模板”以查看模板。

    <!-- Notice: We customized due to Mooncake is different with global site.-->
4. 浏览到模板 JSON 文件的第 28 行。 这是 Key Vault 资源的定义。
5. 浏览到第 35 行：

    ```json
    "enabledForTemplateDeployment": true,
    ```
    `enabledForTemplateDeployment` 是 Key Vault 属性。 此属性必须为 true，这样才能在部署期间从此 Key Vault 中检索机密。
6. 浏览到第 89 行。 这是 Key Vault 机密的定义。
7. 选择页面底部的“放弃”。 未进行任何更改。
    <!-- Notice: We ADD NEW PAGE due to Mooncake is different with global site.-->
8. 从左窗格选择“法律条款”，在确认参数值后单击“创建”。

    ![与法律条款对应的资源管理器模板 Key Vault](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-key-vault-portal-chenye-add-01.png)

8. 根据上面屏幕截图中所示检查是否已提供所有值，然后单击页面底部的“创建”。
    
    <!-- Notice: We ADD NEW PAGE due to Mooncake is different with global site.-->
    
9. 选择页面顶部的铃铛图标（通知）打开“通知”窗格。 等到出现资源已成功部署的消息。
10. 在“通知”窗格中选择“转到资源组”。 
11. 选择 Key Vault 名称将其打开。
12. 在左窗格中选择“机密”。 此处应列出 **vmAdminPassword**。
13. 在左窗格中选择“访问策略”。 此时应会列出你的名称 (Active Directory)，否则表示你无权访问 Key Vault。
14. 选择“单击以显示高级访问策略”。 注意“启用对 Azure 资源管理器的访问以进行模板部署”处于选中状态。 要正常进行 Key Vault 集成，也必须指定此设置。

    ![资源管理器模板 Key Vault 集成访问策略](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-key-vault-access-policies.png)
15. 在左窗格中选择“属性”。
16. 复制“资源 ID”。 部署虚拟机时需要此 ID。  资源 ID 格式为：

    ```json
    /subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
    ```

## <a name="open-a-quickstart-template"></a>打开快速入门模板

Azure 快速入门模板是资源管理器模板的存储库。 无需从头开始创建模板，只需找到一个示例模板并对其自定义即可。 本教程中使用的模板称为[部署简单的 Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/)。

1. 在 Visual Studio Code 中，选择“文件”>“打开文件”。
2. 在“文件名”中粘贴以下 URL：

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. 选择“打开”以打开该文件。 它是[教程：使用依赖资源创建 Azure 资源管理器模板](./resource-manager-tutorial-create-templates-with-dependent-resources.md)中所用的同一个方案。
4. 有五个通过此模板定义的资源：

    * `Microsoft.Storage/storageAccounts`。
    * `Microsoft.Network/publicIPAddresses`。
    * `Microsoft.Network/virtualNetworks`。
    * `Microsoft.Network/networkInterfaces`。
    * `Microsoft.Compute/virtualMachines`。

     <!-- Not Available on  [template reference](https://docs.microsoft.com/zh-cn/azure/templates/Microsoft.Storage/storageAccounts)-->
     <!-- Not Available on  [template reference](https://docs.microsoft.com/zh-cn/azure/templates/microsoft.network/publicipaddresses)-->
     <!-- Not Available on  [template reference](https://docs.microsoft.com/zh-cn/azure/templates/microsoft.network/virtualnetworks)-->
     <!-- Not Available on  [template reference](https://docs.microsoft.com/zh-cn/azure/templates/microsoft.network/networkinterfaces)-->
     <!-- Not Available on  [template reference](https://docs.microsoft.com/zh-cn/azure/templates/microsoft.compute/virtualmachines)-->
     在自定义模板之前，不妨对其进行一些基本的了解。
5. 选择“文件”>“另存为”，将该文件的副本保存到名为 **azuredeploy.json** 的本地计算机。
6. 重复步骤 1-4 打开以下 URL，然后将文件保存为 **azuredeploy.parameters.json**。

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>编辑参数文件

无需对模板文件进行任何更改。

1. 在 Visual Studio Code 中打开 **azuredeploy.parameters.json**（如果尚未打开）。
2. 将 **adminPassword** 参数更新为：

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```

    将 **id** 替换为在上一过程中创建的 Key Vault 的资源 ID。  

    ![集成 Key Vault 和资源管理器模板虚拟机部署参数文件](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. 指定以下值：

    * **adminUsername**：为虚拟机管理员帐户命名。
    * **dnsLabelPrefix**：为 dnsLabelPrefix 命名。
4. 保存更改。

## <a name="deploy-the-template"></a>部署模板

遵照[部署模板](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template)中的说明部署模板。 需在本地电脑上下载 **azuredeploy.json** 和 **azuredeploy.parameters.json**，然后使用以下 PowerShell 脚本来部署模板：

<!--Not Available on You need to upload both **azuredeploy.json** and **azuredeploy.parameters.json** to the Cloud shell-->

```powershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. chinaeast)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile azuredeploy.json `
    -TemplateParameterFile azuredeploy.parameters.json
```

部署模板时，请使用 Key Vault 所在的同一个资源组。 这样可以更轻松地清理资源。 只需删除一个资源组，而不用删除两个。

## <a name="valid-the-deployment"></a>验证部署

成功部署虚拟机后，使用 Key Vault 中存储的密码来测试登录。

1. 打开 [Azure 门户](https://portal.azure.cn)。
2. 选择“资源组”/**\<YourResourceGroupName\>**/**simpleWinVM**
3. 选择顶部的“连接”。
4. 选择“下载 RDP 文件”，然后遵照说明使用 Key Vault 中存储的密码登录到虚拟机。

## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。

1. 在 Azure 门户上的左侧菜单中选择“资源组”。
2. 在“按名称筛选”字段中输入资源组名称。
3. 选择资源组名称。  应会看到，该资源组中总共有六个资源。
4. 在顶部菜单中选择“删除资源组”。

## <a name="next-steps"></a>后续步骤

在本教程中，我们从 Azure Key Vault 检索了一个机密，并在模板部署中使用了该机密。  若要了解如何创建链接模板，请参阅：

> [!div class="nextstepaction"]
> [创建链接模板](./resource-manager-tutorial-create-linked-templates.md)

<!-- Update_Description: update meta properties, wording update, update link -->