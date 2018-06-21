---
title: 要求进行用户分配 - Azure AD | Microsoft Docs
description: 如何要求对 Azure 应用程序进行用户分配。
services: active-directory
documentationcenter: ''
author: kgremban
manager: femila
editor: ''
ms.assetid: 30b78cba-1e0f-472f-8314-f2250a9b91c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/16/2017
ms.date: 06/21/2017
ms.author: v-junlch
robots: noindex
ms.openlocfilehash: 26ac62c2ace24bc0c1e2b3b07bf49f4b5bfb2fae
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
ms.locfileid: "20186726"
---
# <a name="azure-ad-and-applications-require-user-assignment"></a>Azure AD 和应用程序：要求进行用户分配
## <a name="requiring-user-assignment"></a>要求进行用户分配
1. 使用管理员帐户登录到 Azure 门户。
2. 在主菜单中单击“所有项”项。
3. 选择要用于应用程序的目录。
4. 单击“应用程序”选项卡。
5. 从与此目录关联的应用程序列表中选择应用程序。
6. 单击“配置”选项卡  。
7. 将“访问应用需要进行用户分配”切换为“是”。
8. 单击屏幕底部的“保存”按钮。

现在，必须将用户和/或组分配到应用程序。 请参阅[将用户分配到应用程序](./active-directory-applications-guiding-developers-assigning-users.md)。

## <a name="next-steps"></a>后续步骤
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]