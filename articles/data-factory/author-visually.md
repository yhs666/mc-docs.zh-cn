---
title: 在 Azure 数据工厂中创作视觉对象 | Microsoft Docs
description: 了解如何在 Azure 数据工厂中使用视觉对象创作
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 01/09/2019
ms.date: 07/08/2019
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
ms.openlocfilehash: 1f0e198b58bf1184eed1f19b46ff81ae0837117b
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571450"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Azure 数据工厂中的视觉对象创作
Azure 数据工厂用户界面体验 (UX) 允许你以可视方式创作和部署资源为你的数据工厂而无需编写任何代码。 通过此无代码的界面，可将活动拖放到管道画布上、执行测试运行、以迭代方式进行调试，以及部署和监视管道运行。 有一种方法可以使用 UX 来执行视觉对象创作：

- 直接使用数据工厂服务
## <a name="author-directly-with-the-data-factory-service"></a>直接使用数据工厂服务

- 数据工厂服务不包括存储所做的更改的 JSON 实体的存储库。
- 数据工厂服务未优化协作或版本控制。

![配置数据工厂服务](media/author-visually/configure-data-factory.png)

当使用 UX 创作画布借助数据工厂服务直接进行创作时，只能使用“全部发布”模式   。 所做的任何更改都会直接发布到数据工厂服务。

![发布模式](media/author-visually/data-factory-publish.png)

## <a name="author-with-github-integration"></a>使用 GitHub 集成进行创作

使用 GitHub 集成进行创作允许在创作数据工厂管道时进行源代码管理和协作。 用户可以选择将数据工厂与 GitHub 帐户存储库相关联，以进行源代码管理、协作和版本控制。 单个的 GitHub 帐户可以有多个存储库，但 GitHub 存储库可以只有一个数据工厂与相关联。 如果没有 GitHub 帐户或存储库，请按照 [这些说明](https://github.com/join) 创建资源。

GitHub 与数据工厂的集成支持公共 GitHub（即 [https://github.com](https://github.com)）和 GitHub Enterprise。 只要你对 GitHub 中的存储库具有读写权限，就可以将公共和专用 GitHub 存储库与数据工厂一起使用。

若要配置 GitHub 存储库，必须对所用 Azure 订阅拥有管理员权限。

有关此功能的 9 分钟介绍和演示，请观看以下视频：

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="limitations"></a>限制

- 可在 GitHub 存储库中存储脚本和数据文件。 但是，必须手动将文件上传到 Azure 存储。 数据工厂管道不会自动将 GitHub 存储库中存储的脚本或数据文件上传到 Azure 存储。

- 2\.14.0 以下版本的 GitHub Enterprise 在 Microsoft Edge 浏览器中无法正常运行。

- 数据工厂视觉创作工具的 GitHub 集成仅适用于正式版数据工厂。

### <a name="configure-a-public-github-repository-with-azure-data-factory"></a>使用 Azure 数据工厂配置公共 GitHub 存储库

用户可通过两种方法使用数据工厂配置 GitHub 存储库。

**配置方法 1（公共存储库）：“开始使用”页**

在 Azure 数据工厂中，转到“开始使用” ****  页。 选择“配置代码存储库” **** ：

![数据工厂“入门”页](media/author-visually/github-integration-image1.png)

此时将显示“存储库设置” ****  配置窗格：

![GitHub 存储库设置](media/author-visually/github-integration-image2.png)

窗格将显示以下 Azure Repos 代码存储库设置：

| **设置**                                              | **说明**                                                                                                                                                                                                                                                                                                                                                                                                                   | **值**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **存储库类型**                                      | Azure Repos 代码存储库的类型。                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **GitHub 帐户**                                       | GitHub 帐户名。 可从 https:\//github.com/{account name}/{repository name} 找到此名称。 导航到此页时，系统会提示输入 GitHub 帐户的 GitHub OAuth 凭据。                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | GitHub 代码存储库名称。 GitHub 帐户包含用于管理源代码的 Git 存储库。 可以创建新存储库，或使用帐户中现有的存储库。                                                                                                                                                                                                                              |                    |
| **协作分支**                                 | 将用于发布的 GitHub 协作分支。 默认为主分支。 如果希望从其他分支发布资源，可更改此设置。                                                                                                                                                                                                                                                               |                    |
| **根文件夹**                                          | GitHub 协作分支中的根文件夹。                                                                                                                                                                                                                                                                                                                                                                             |                    |
| 确认选中“将现有的数据工厂资源导入存储库”选项。  | 指定是否将现有数据工厂资源从 UX  **创作画布** 导入到 GitHub 存储库中。 选择相应的框以将你的数据工厂资源导入 JSON 格式关联的 Git 存储库。 此操作单独导出每个资源（即，链接的服务和数据集导出到单独的 JSON）。 如果未选中此框，不能导入现有的资源。 | 已选择（默认） |
| **要将资源导入到的分支**                       | 指定要将数据工厂资源（管道、数据集、链接服务等）导入哪个分支。 可将资源导入以下分支之一：a. 协作；b. 新建；c. 使用现有项                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-public-repo-ux-authoring-canvas"></a>配置方法 2（公共存储库）：UX 创作画布

在 Azure 数据工厂 UX  **创作画布**中，找到数据工厂。 选择“数据工厂” ****  下拉菜单，然后选择“配置代码存储库” **** 。

此时将显示配置窗格。 有关配置设置的详细信息，请参阅前面 *配置方法 1* 中的说明。

### <a name="configure-a-github-enterprise-repository-with-azure-data-factory"></a>使用 Azure 数据工厂配置 GitHub Enterprise 存储库

可通过两种方法使用数据工厂配置 GitHub Enterprise 存储库。

#### <a name="configuration-method-1-enterprise-repo-lets-get-started-page"></a>配置方法 1（Enterprise 存储库）：“开始使用”页

在 Azure 数据工厂中，转到“开始使用” ****  页。 选择“配置代码存储库” **** ：

![数据工厂“入门”页](media/author-visually/github-integration-image1.png)

此时将显示“存储库设置” ****  配置窗格：

![GitHub 存储库设置](media/author-visually/github-integration-image3.png)

窗格将显示以下 Azure Repos 代码存储库设置：

| **设置**                                              | **说明**                                                                                                                                                                                                                                                                                                                                                                                                                   | **值**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **存储库类型**                                      | Azure Repos 代码存储库的类型。                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **使用 GitHub Enterprise**                                | 用于选择 GitHub Enterprise 的复选框                                                                                                                                                                                                                                                                                                                                                                                              |                    |
| **GitHub Enterprise URL**                                | GitHub Enterprise 根 URL。 例如： https://github.mydomain.com                                                                                                                                                                                                                                                                                                                                                          |                    |
| **GitHub 帐户**                                       | GitHub 帐户名。 可从 https:\//github.com/{account name}/{repository name} 找到此名称。 导航到此页时，系统会提示输入 GitHub 帐户的 GitHub OAuth 凭据。                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | GitHub 代码存储库名称。 GitHub 帐户包含用于管理源代码的 Git 存储库。 可以创建新存储库，或使用帐户中现有的存储库。                                                                                                                                                                                                                              |                    |
| **协作分支**                                 | 将用于发布的 GitHub 协作分支。 默认为主分支。 如果希望从其他分支发布资源，可更改此设置。                                                                                                                                                                                                                                                               |                    |
| **根文件夹**                                          | GitHub 协作分支中的根文件夹。                                                                                                                                                                                                                                                                                                                                                                             |                    |
| 确认选中“将现有的数据工厂资源导入存储库”选项。  | 指定是否将现有数据工厂资源从 UX  **创作画布** 导入到 GitHub 存储库中。 选择相应的框以将你的数据工厂资源导入 JSON 格式关联的 Git 存储库。 此操作单独导出每个资源（即，链接的服务和数据集导出到单独的 JSON）。 如果未选中此框，不能导入现有的资源。 | 已选择（默认） |
| **要将资源导入到的分支**                       | 指定要将数据工厂资源（管道、数据集、链接服务等）导入哪个分支。 可将资源导入以下分支之一：a. 协作；b. 新建；c. 使用现有项                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-enterprise-repo-ux-authoring-canvas"></a>配置方法 2（Enterprise 存储库）：UX 创作画布

在 Azure 数据工厂 UX  **创作画布**中，找到数据工厂。 选择“数据工厂” ****  下拉菜单，然后选择“配置代码存储库” **** 。

此时将显示配置窗格。 有关配置设置的详细信息，请参阅前面 *配置方法 1* 中的说明。

## <a name="use-the-expression-language"></a>使用表达式语言
可使用 Azure 数据工厂支持的表达式语言，在定义属性值过程中指定表达式。

通过选择“添加动态内容”  ，指定属性值的表达式：

![使用表达式语言](media/author-visually/dynamic-content-1.png)

## <a name="use-functions-and-parameters"></a>使用函数和参数

可以在数据工厂“表达式生成器”中使用函数或指定管道和数据集的参数  ：

有关受支持的表达式的更多信息，请参阅 [Azure 数据工厂中的表达式和函数](control-flow-expression-language-functions.md)。

![添加动态内容](media/author-visually/dynamic-content-2.png)

## <a name="provide-feedback"></a>提供反馈
联系 [Azure 支持](https://support.azure.cn/zh-cn/support/contact/)以评价功能或通知 Microsoft 有关工具的问题。

## <a name="next-steps"></a>后续步骤
若要了解有关监视和管理管道的信息，请参阅[以编程方式监视和管理管道](monitor-programmatically.md)。
