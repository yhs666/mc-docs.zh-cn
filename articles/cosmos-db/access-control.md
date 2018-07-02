---
title: 设置 Azure Cosmos DB 中的访问控制 | Azure
description: 了解如何设置 Azure Cosmos DB 中的访问控制。
services: cosmos-db
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
origin.date: 02/06/2018
ms.date: 07/02/2018
ms.author: v-yeche
ms.openlocfilehash: 1471661aa1e3ef571e69bd0a76e3b8b2ccb97ac6
ms.sourcegitcommit: 4ce5b9d72bde652b0807e0f7ccb8963fef5fc45a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2018
ms.locfileid: "37070209"
---
# <a name="access-control-in-azure-cosmos-db"></a>Azure Cosmos DB 中的访问控制

若要将 Azure Cosmos DB 帐户读者访问权限添加到用户帐户，请让订阅所有者在 Azure 门户执行以下步骤。

1. 打开 Azure 门户，并选择 Azure Cosmos DB 帐户。
2. 单击“访问控制(IAM)”选项卡，然后单击“+ 添加”。
3. 在“添加权限”窗格中的“角色”框中，选择“Cosmos DB 帐户读者角色”。
<!-- Not Available in MC on 4. In the **Assign access to box**, select **Azure AD user, group, or application**. -->
5. 在你想要授予访问权限的目录中选择用户、组或应用程序。  可以通过显示名称、电子邮件地址或对象标识符搜索目录。
    所选用户、组或应用程序会显示在所选成员列表中。
6. 单击“保存” 。

实体现在便可以读取 Azure Cosmos DB 资源。

<!-- Update_Description: Update meta properties -->