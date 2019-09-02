---
title: 为 Azure API 管理实例配置自定义域名
description: 本主题介绍了如何为 Azure API 管理实例配置自定义域名。
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
origin.date: 07/01/2019
ms.date: 07/15/2019
ms.author: v-yiso
ms.openlocfilehash: 5cf571aece22ffffae59574406909d81532f8782
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569647"
---
# <a name="configure-a-custom-domain-name"></a>配置自定义域名 

创建 Azure API 管理服务实例时，Azure 会为其分配 azure-api.net 的子域（例如，`apim-service-name.azure-api.cn`）。 不过，你可以使用自己的自定义域名（例如 **contoso.com**）公开 API 管理终结点。 本教程演示了如何将现有的自定义 DNS 名称映射到 API 管理实例公开的终结点。

> [!WARNING]
> 想要使用证书固定改进其应用程序安全性的客户必须使用自定义域名和他们管理的证书，而不是使用默认证书。 改为固定默认证书的客户将硬依赖于他们不控制的证书属性，建议不要这样做。
>
>

## <a name="prerequisites"></a>先决条件

若要执行本文中所述的步骤，必须具有：

+ 一个有效的 Azure 订阅。

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ API 管理实例。 有关详细信息，请参阅[创建 Azure API 管理实例](get-started-create-service-instance.md)。
+ 一个由你拥有的自定义域名。 必须单独获取要使用的自定义域名并将其托管在 DNS 服务器上。 本主题没有说明如何托管自定义域名。
+ 必须具有有效的带有公钥和私钥 (.PFX) 的证书。 使用者或使用者可选名称 (SAN) 必须与域名匹配（这使得 API 管理实例可以通过 SSL 安全地公开 URL）。

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>使用 Azure 门户设置自定义域名

1. 在 [Azure 门户](https://portal.azure.cn/)中导航到自己的 API 管理实例。
2. 选择“自定义域和 SSL”  。
    
    可以为许多终结点分配自定义域名。 当前有以下终结点可用： 
   + **代理**（默认值为：`<apim-service-name>.azure-api.cn`）， 
   + **门户**（默认值为：`<apim-service-name>.portal.azure-api.cn`），     
   + **管理**（默认值为：`<apim-service-name>.management.azure-api.cn`）， 
   + **SCM**（默认值为：`<apim-service-name>.scm.azure-api.cn`）。

     >[!NOTE]
     > 可以更新所有终结点或者更新其中的一部分。 通常情况下，客户会更新**代理**（此 URL 用来调用通过 API 管理公开的 API）和**门户**（开发人员门户 URL）。 **管理**和 **SCM** 终结点由 APIM 客户在内部使用，因此很少会为其分配自定义域名。
    
3. 选择要更新的终结点。 
4. 在右侧窗口中，单击“自定义”  。

   + 在“自定义域名”  中，指定要使用的名称。 例如，`api.contoso.com`。 还支持通配符域名（例如 *.domain.com）。
   + 在**证书**中，从密钥保管库中选择证书。 如果证书受密码保护，你还可以上传有效的 .PFX 文件并提供其**密码**。

    > [!TIP]
    > 我们建议使用 Azure Key Vault 来管理证书并将其设置为“自动轮换”。
    > 如果使用 Azure 密钥保管库来管理自定义域 SSL 证书，请确保该证书[作为证书  ](https://docs.microsoft.com/rest/api/keyvault/CreateCertificate/CreateCertificate)而不是机密  插入到密钥保管库中。
    >
    > 若要提取 SSL 证书，API 管理必须使列表对包含证书的 Azure Key Vault 具有“获取机密”权限。 使用 Azure 门户时，所有必要的配置步骤都将自动完成。 使用命令行工具或管理 API 时，必须手动授予这些权限。 此过程分为两个步骤。 首先，使用 API 管理实例上的“托管标识”页确保已启用“托管标识”并记下该页上显示的主体 ID。 其次，在包含证书的 Azure Key Vault 上为此主体 ID 提供权限列表并获取机密权限。
    >
    > 如果证书设置为“自动轮换”，API 管理将自动选取最新版本，而不会对服务造成任何停机（如果你的 API 管理层具有SLA - 即在除了开发人员层之外的所有层中）。

5. 单击“应用”。

    >[!NOTE]
    >分配证书的过程可能需要 15 分钟或更久，这取决于部署规模。 开发人员 SKU 有故障时间，基本和更高版本的 SKU 没有故障时间。

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>后续步骤

[升级和缩放你的服务](upgrade-and-scale.md)