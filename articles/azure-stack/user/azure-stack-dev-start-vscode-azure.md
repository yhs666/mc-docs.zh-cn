---
title: 使用 Visual Studio Code 中的 Azure 帐户扩展连接到 Azure Stack | Microsoft Docs
description: 以开发人员的身份使用 Visual Studio Code 中的 Azure 帐户扩展连接到 Azure Stack
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: Howto
origin.date: 06/25/2019
ms.date: 07/29/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 06/25/2019
ms.openlocfilehash: af77c789750fc4c0994377bfab5fe3797391bf18
ms.sourcegitcommit: 4d34571d65d908124039b734ddc51091122fa2bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68513532"
---
# <a name="connect-to-azure-stack-using-azure-account-extension-in-visual-studio-code"></a>使用 Visual Studio Code 中的 Azure 帐户扩展连接到 Azure Stack

本文逐步说明如何使用 Azure 帐户扩展连接到 Azure Stack。 需要更新 Visual Studio Code (VS Code) 设置。

VS Code 是用于生成和调试 Web 与云应用程序的轻型编辑器。 ASP.NET Core、Python、NodeJS、Go 和其他语言的开发人员都可以使用 VS Code。 使用 Azure 帐户扩展可以配合订阅筛选使用 Azure 单一登录，以获取更多的 Azure 扩展。 借助该扩展可以在与 VS Code 集成的终端中使用 Azure Cloud Shell。 通过该扩展，可以使用标识管理器的 Azure AD (Azure AD) 和 Active Directory 联合身份验证服务 (AD FS) 连接到 Azure Stack 订阅。 这样，你便可以登录到 Azure Stack，选择自己的订阅，并在 shell 中打开新的命令行。 

> [!Note]  
> 可对 Active Directory 联合身份验证服务 (AD FS) 环境使用本文所述的步骤。 使用 AD FS 凭据和终结点。

## <a name="pre-requisites-for-the-azure-account-extension"></a>Azure 帐户扩展的先决条件

1. Azure Stack 环境 1904 内部版本或更高版本
2. [Visual Studio Code](https://code.visualstudio.com/)
3. [Azure 帐户扩展](https://github.com/Microsoft/vscode-azure-account)
4. [Azure Stack 订阅](https://azure.microsoft.com/overview/azure-stack/)

## <a name="steps-to-connect-to-azure-stack"></a>连接到 Azure Stack 的步骤

1. 运行 GitHub 上的“Azure Stack 工具”中的“标识”脚本。 

    - 在运行该脚本之前，需要安装 PowerShell 并根据自己的环境配置 PowerShell。 有关说明，请参阅[安装适用于 Azure Stack 的 PowerShell](../operator/azure-stack-powershell-install.md)。

    - 有关“标识”脚本的说明和脚本内容，请参阅 **[AzureStack-Tools/Identity](https://github.com/Azure/AzureStack-Tools/tree/master/Identity)** 。

2. 打开 VS Code。

3. 选择左侧一角的“扩展”。 

3. 在搜索框中输入 `Azure Account`。

4. 依次选择“Azure 帐户”、“安装”。  

      ![Azure Stack Visual Studio Code](media/azure-stack-dev-start-vscode-azure/image1.png)

5. 重启 VS Code 以加载该扩展。

6. 检索用于连接到 Azure Stack 中的 Azure 资源管理器的元数据。 
    
    Azure 资源管理器是一种管理框架，可用于部署、管理和监视 Azure 资源。
    - Azure Stack 开发工具包 (ASDK) 的资源管理器 URL 为：`https://management.local.azurestack.external/`。 
    - 集成系统的资源管理器 URL 为：`https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`
    - 将以下文本添加到 URL，以访问元数据：`<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

    例如，用于检索 Azure 资源管理器终结点元数据的 URL 可能类似于：`https://management.local.azurestack.external/metadata/endpoints?api-version=1.0`

    记下返回 JSON。 稍后需要使用 `loginEndpoint` 和 `audiences` 属性的值。

7. 按下 **Ctrl+Shift+P**，然后选择“首选项:  打开用户设置(JSON)”。

8. 在代码编辑器中，使用环境的值更新以下 JSON 代码片段，然后将代码片段粘贴到设置块中。

    - 值：

        | 参数 | 说明 |
        | --- | --- |
        | `tenant-ID` | Azure Stack [租户 ID](../operator/azure-stack-identity-overview.md) 的值。 |
        | `activeDirectoryEndpointUrl` | 这是 loginEndpoint 属性中的 URL。 |
        | `activeDirectoryResourceId` | 这是 audiences 属性中的 URL。
        | `resourceManagerEndpointUrl` | 这是 Azure Stack 的 Azure 资源管理器的根 URL。 | 

    - JSON 代码片段：

      ```JSON  
      "azure.tenant": "tenant-ID",
      "azure.ppe": {
          "activeDirectoryEndpointUrl": "Login endpoint",
          "activeDirectoryResourceId": "This is the URL from the audiences property.",
          "resourceManagerEndpointUrl": "Aure Resource Management Endpoint",
      },
      "azure.cloud": "AzurePPE"
      ```

9. 保存用户设置并再次按下 **Ctrl+Shift+P**。 选择“Azure:  登录到 Azure 云”。 新选项“AzurePPE”将显示在目标列表中。 

10. 选择“AzurePPE”。  浏览器中会加载身份验证页。 登录到你的终结点。

11. 若要测试是否已成功登录到你的 Azure Stack 订阅，请按 **Ctrl+Shift+ P** 并选择“Azure:  选择订阅”，然后查看你的订阅是否可用。

## <a name="commands"></a>命令

| Azure：登录 | 登录到 Azure 订阅 |
| --- | --- |
| Azure：使用设备代码登录 | 使用设备代码登录到你的 Azure 订阅。 “登录”命令不起作用时，请在设置中使用此方法。 |
| Azure：登录到 Azure 云 | 登录到某个主权云中的 Azure 订阅。 |
| Azure：注销 | 从 Azure 订阅注销 |
| Azure：选择订阅 | 选择要使用的订阅集。 该扩展只显示筛选的订阅中的资源。 |
| Azure：创建帐户 | 如果你没有 Azure 帐户，可以立即[注册](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-azure-account&mktingSource=vscode-azure-account)一个帐户，并获得 \$200 的免费信用额度。 |
| Azure：在 Shell 中打开 Bash | 在 Shell 中打开运行 Bash 的新终端。 |
| Azure：在 Shell 中打开 PowerShell | 在 Shell 中打开运行 PowerShell 的新终端。 |
| Azure：上传到 Shell | 将文件上传到 Shell 存储帐户。 |

## <a name="next-steps"></a>后续步骤

[设置 Azure Stack 中的开发环境](azure-stack-dev-start.md)