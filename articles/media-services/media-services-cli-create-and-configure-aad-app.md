---
title: "使用 CLI 2.0 创建 Azure AD 应用，并将该应用配置为访问 Azure 媒体服务 API | Azure"
description: "本主题展示了如何使用 CLI 2.0 创建 Azure AD 应用，并将它配置为访问 Azure 媒体服务 API。"
services: media-services
documentationcenter: 
author: forester123
manager: digimobile
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/17/2017
ms.date: 11/06/2017
ms.author: v-johch
ms.openlocfilehash: 8851a23d743af37aba0794868c0079eb02cc01ac
ms.sourcegitcommit: c2be8d831d87f6a4d28c5950bebb2c7b8b6760bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2017
---
# <a name="use-cli-20-to-create-an-aad-app-and-configure-it-to-access-azure-media-services-api"></a>使用 CLI 2.0 创建 AAD 应用，并将它配置为访问 Azure 媒体服务 API

本主题展示了如何使用 CLI 2.0 创建 Azure Active Directory (Azure AD) 应用程序和服务主体，以便访问 Azure 媒体服务资源。

## <a name="prerequisites"></a>先决条件

- 一个 Azure 帐户。 有关详细信息，请参阅 [Azure 试用](https://www.azure.cn/pricing/1rmb-trial/)。
- 一个媒体服务帐户。 有关详细信息，请参阅[使用 Azure 门户创建 Azure 媒体服务帐户](media-services-portal-create-account.md)。

## <a name="create-an-azure-ad-app-and-configure-access-to-the-media-account-with-cli-20"></a>使用 CLI 2.0 创建 Azure AD 应用并配置对媒体帐户的访问权限

```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create -- assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

例如：

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

在此示例中，作用域是媒体服务帐户的完整资源路径。 不过，作用域可以为任何级别。

例如，作用域可以为下列级别之一：

- 订阅级别。
- 资源组级别。
- 资源级别（例如，媒体帐户）。

有关详细信息，请参阅[使用 Azure CLI 2.0 创建 Azure 服务主体](https://docs.azure.cn/cli/create-an-azure-service-principal-azure-cli)

另请参阅[使用 Azure 命令行接口管理基于角色的访问控制](../active-directory/role-based-access-control-manage-access-azure-cli.md)。

## <a name="next-steps"></a>后续步骤

开始[将文件上传到帐户](media-services-portal-upload-files.md)。