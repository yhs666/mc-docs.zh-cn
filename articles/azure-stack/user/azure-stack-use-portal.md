---
title: 使用 Azure Stack 门户 | Microsoft Docs
description: 了解如何访问和使用 Azure Stack 中的用户门户。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 5aa00123-5b87-45e0-a671-4165e66bfbc6
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/25/2017
ms.date: 03/09/2018
ms.author: v-junlch
ms.openlocfilehash: 1fa3ee7e9defb11ec9aa75114b91d4bd6c01f2a6
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="using-the-azure-stack-portal"></a>使用 Azure Stack 门户

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

Azure Stack 服务的使用者可以使用 Azure Stack 门户来订阅公共产品，以及使用通过这些产品提供的服务。 如果你以前用过 Azure 门户，则很熟悉该用户界面。

## <a name="access-the-portal"></a>访问门户

Azure Stack 操作员（服务提供商或组织中的管理员）将会告知门户的正确 URL。 

- 对于集成系统，URL 根据操作员所在的区域和外部域名的不同而异，格式为 https://portal.&lt;*区域*&gt;.&lt;*FQDN*&gt;。
- 如果使用 Azure Stack 开发工具包，则门户地址为 https://portal.local.azurestack.external。

![Azure Stack 用户门户的屏幕截图](./media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>自定义仪表板

仪表板包含一组默认磁贴。 可以单击“编辑仪表板”来修改默认仪表板，或者单击“新建仪表板”来添加自定义仪表板。 可以轻松地将磁贴添加到仪表板中。 例如，可以单击“新建”，右键单击“计算”，然后单击“固定到仪表板”。

## <a name="create-subscription-and-browse-available-resources"></a>创建订阅和浏览可用资源
 
如果没有订阅，则首先需要订阅某个产品。 然后，便可以浏览可用的资源。 若要浏览和创建资源，请执行以下任一操作：

- 单击仪表板上的“Marketplace”磁贴。 
- 在“所有资源”磁贴上，单击“创建资源”。
- 在左侧导航窗格上，单击“新建”。

## <a name="learn-how-to-use-available-services"></a>了解如何使用可用服务

如需有关如何使用可用服务的指导，可以使用不同的选项。

- 你的组织或服务提供商可能提供了自己的文档， 尤其是他们提供自定义的服务或应用时。
- 第三方应用提供自身的文档。
- 为保持服务的 Azure 一致性，我们强烈建议先查看 Azure Stack 文档。 若要访问 Azure Stack 用户文档，请单击“帮助”图标，然后单击“帮助 + 支持”。
 
    ![UI 中“帮助 + 支持”选项的屏幕截图](./media/azure-stack-use-portal/HelpAndSupport.png)

    具体而言，我们建议查看以下入门文章：

    - [关键注意事项：使用 Azure Stack 的服务或为其生成应用](azure-stack-considerations.md)
    - 在此文档的“使用服务”部分，列出了每个 Azure 一致性服务。 每个服务有一个“注意事项”主题，其中描述了 Azure 中提供的服务与 Azure Stack 中提供的相同服务之间的差异。 有关示例，请参阅 [VM 注意事项](azure-stack-vm-considerations.md)。 “使用服务”部分可能包含特定于 Azure Stack 的其他信息。 
     
      可以使用 Azure 文档来大致了解服务，但必须注意这些差异。 请注意，“快速入门教程”磁贴中的文档链接指向 Azure 文档。

## <a name="get-support"></a>获取支持

如需更多支持，请联系你的组织或服务提供商。 

如果使用 Azure Stack 开发工具包，则只能通过 [Azure Stack 论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)获取支持。

## <a name="next-steps"></a>后续步骤

[关键注意事项：使用 Azure Stack 的服务或为其生成应用](azure-stack-considerations.md)

