---
ms.assetid: ''
title: Azure Key Vault 安全体系
ms.service: key-vault
author: bryanla
ms.author: v-biyu
manager: mbaldwin
origin.date: 07/03/2017
ms.date: 10/22/2017
ms.openlocfilehash: e7ed7f5154c16e518bdb81862a64a1b5020474f8
ms.sourcegitcommit: 2fdf25eb4b978855ff2832bcdcca093c141be261
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "49120608"
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure Key Vault 安全体系和地理边界

## <a name="back-up-and-restore-behavior"></a>备份和还原行为

只要满足以下两个条件，从某一 Azure 位置中的 Key Vault 对密钥所执行的备份可以还原到另一 Azure 位置中的 Key Vault：

- 两个 Azure 位置属于相同的地理位置
- 两个 Key Vault 属于同一个 Azure 订阅

例如，在印度西部的 Key Vault 中由给定订阅对密钥执行的备份，只能还原到相同订阅和地理位置（印度西部、印度中部或印度南部）中的另一个 Key Vault。

## <a name="regions-and-products"></a>区域和产品

- [Azure 区域](https://azure.microsoft.com/regions/)
- [Microsoft 产品（按区域）](https://azure.microsoft.com/regions/services/)

区域映射到安全体系，如表中的主要标题所示：

例如，在产品（按区域）文章中，“美国”标签包含美国东部、美国中部、美国西部，这些都会映射到美国区域。 

>[!NOTE]
>例外情况是“美国 DOD 东部”和“美国 DOD 中部”有其自己的安全体系。 

同样，在“欧洲”标签上，欧洲北部和欧洲西部都映射到欧洲区域。 “亚太区”标签也是如此。



<!-- Update_Description: wording update -->
