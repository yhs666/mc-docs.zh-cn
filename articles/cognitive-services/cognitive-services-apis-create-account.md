---
title: 在 Azure 门户中创建认知服务 API 帐户 | Microsoft 文档
description: 如何在 Azure 门户中创建 Microsoft 认知服务 API 帐户。
services: machine-learning
documentationcenter: ''
author: alexchen2016
manager: digimobile
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/09/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 021f089a44c2d1563e2bde32169d8ccf6eada5aa
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
ms.locfileid: "23407591"
---
# <a name="create-a-cognitive-services-apis-account-in-the-azure-portal"></a>在 Azure 门户中创建认知服务 API 帐户

若要使用 Microsoft 认知服务 API，首先需要在 Azure 门户中创建帐户。

1. 登录到 [Azure 门户](http://portal.azure.cn)。

2. 单击“+ 新建”。

3. 选择“AI + 认知服务”，然后可以看到可用 API 的列表。 单击“全部查看”，查看认知服务 API 的完整列表。 单击所选的 API 以继续。

    ![选择认知服务 API](./media/cognitive-services-apis-create-account/select-cognitive-services-apis.png)

4. 在“创建”页中提供以下信息： 

   - **帐户名：** 帐户的名称。 我们建议使用描述性的名称，例如 *AFaceAPIAccount*。

   - **订阅：** 选择一个可用的至少具有参与者权限的 Azure 订阅。

   - **API 类型：** 选择想要使用的认知服务 API。 有关可用的各种认知服务 API 的详细信息，请参考[认知服务](/cognitive-services/)站点。

        ![选择 API 类型](./media/cognitive-services-apis-create-account/list-of-apis.png)

   - **定价层：** 认知服务帐户的费用取决于实际用量和所选的选项。 有关每个 API 的定价的详细信息，请参考[定价页](https://www.azure.cn/pricing/details/cognitive-services/)。

   - **资源组：** 资源组是具有共同生命周期、权限和策略的资源的集合。 若要了解有关资源组的详细信息，请参阅[通过门户管理 Azure 资源](/azure-resource-manager/resource-group-portal)。

   - **资源组位置：** 仅当所选 API 是全局的（未绑定到一个位置），才需要填写此信息。 但是，如果 API 是全局的且未绑定到一个位置，则必须为资源组指定一个位置，与认知服务 API 帐户关联的元数据将驻留于此位置。 此位置对帐户的运行时可用性没有任何影响。 若要了解有关资源组的详细信息，请参阅[通过门户管理 Azure 资源](/azure-resource-manager/resource-group-portal)。

   - **明确接受联机服务条款：** 若要创建帐户，订阅所有者或参与者（由 [Azure 基于角色的访问控制](/active-directory/role-based-access-control-what-is/)定义）需要明确接受[联机服务条款](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx)中适用于认知服务的条款。 

     订阅所有者可以遵循[使用 Azure 门户分配和管理资源策略](/azure-resource-manager/resource-manager-policy-portal)一文、分配“不允许的资源类型”策略定义并将 **Microsoft.CognitiveServices/accounts** 指定为目标资源类型，通过[资源策略](/azure-resource-manager/resource-manager-policy)来禁用为特定的资源组或订阅创建认知服务帐户。

     如果已禁用帐户创建，则创建帐户时会显示以下错误：

     ![帐户创建错误](./media/cognitive-services-apis-create-account/error-message.png)

5. 要将帐户固定到 Azure 门户仪表板，请单击“固定到仪表板”。

6. 单击“创建”  以创建帐户。

成功部署认知服务帐户后，单击仪表板中的磁贴以查看帐户信息。

可以使用“概览”部分的“终结点 URL”和“密钥”部分的密钥在应用程序中进行 API 调用。

![显示帐户信息](./media/cognitive-services-apis-create-account/display-account.png)

![显示帐户密钥](./media/cognitive-services-apis-create-account/account-keys.png)

### <a name="next-steps"></a>后续步骤

- 有关所有可用的 Microsoft 认知服务的详细信息，请参阅[认知服务](/cognitive-services/)。
