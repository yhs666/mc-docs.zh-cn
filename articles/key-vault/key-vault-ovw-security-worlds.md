---
ms.assetid: 
title: "Azure Key Vault 安全体系 | Microsoft Docs"
ms.service: key-vault
author: alexchen2016
ms.author: v-junlch
manager: digimobile
origin.date: 07/03/2017
ms.date: 08/02/2017
ms.openlocfilehash: b9ddb517831f65c07f8eda288bcdafb7b1ef29c6
ms.sourcegitcommit: 20d1c4603e06c8e8253855ba402b6885b468a08a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure Key Vault 安全体系和地理边界

## <a name="back-up-and-restore-behavior"></a>备份和还原行为

只要满足以下两个条件，从某一 Azure 位置中的 Key Vault 对密钥所执行的备份可以还原到另一 Azure 位置中的 Key Vault：

- 两个 Azure 位置属于相同的地理位置
- 两个 Key Vault 属于同一个 Azure 订阅

例如，在印度西部的 Key Vault 中由给定订阅对密钥执行的备份，只能还原到相同订阅和地理位置（印度西部、印度中部或印度南部）中的另一个 Key Vault。

## <a name="regions-and-products"></a>区域和产品

- [Azure 区域](https://azure.microsoft.com/regions/)
- [按区域列出的 Microsoft 产品](https://azure.microsoft.com/regions/services/)

区域映射到安全体系，如表中的主要标题所示：

例如，在“按区域列出的产品”一文中，“美国”选项卡包含“美国东部”、“美国中部”、“美国西部”，所有这些都映射到“美国”区域。 

>[!NOTE]
>例外情况是“美国 DOD 东部”和“美国 DOD 中部”有其自己的安全体系。 

同样，在“欧洲”选项卡上，“欧洲北部”和“欧洲西部”都映射到“欧洲”区域。 “亚太区”标签也是如此。



<!-- Update_Description: wording update -->
