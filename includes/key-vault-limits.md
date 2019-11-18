---
author: rothja
ms.service: billing
ms.topic: include
origin.date: 11/09/2018
ms.date: 10/31/2019
ms.author: v-tawe
ms.openlocfilehash: 659c1a87fa275646b9a3724c2161d197dff04921
ms.sourcegitcommit: 642a4ad454db5631e4d4a43555abd9773cae8891
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425871"
---
#### <a name="key-transactions-maximum-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>密钥事务数（每个区域的每个保管库在 10 秒内允许的事务数上限<sup>1</sup>）：

|密钥类型|软件密钥<br>CREATE 密钥|Software-key<br>所有其他事务|
|:---|---:|---:|---:|---:|
|RSA 2,048 位|10 个|2,000|
|RSA 3,072 位|10 个|500|
|RSA 4,096 位|10 个|250|
|ECC P-256|10 个|2,000|
|ECC P-384|10 个|2,000|
|ECC P-521|10 个|2,000|
|ECC SECP256K1|10 个|2,000|


#### <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>机密、托管存储帐户密钥，以及保管库事务：
| 事务类型 | 每个区域的每个保管库在 10 秒内允许的事务数上限<sup>1</sup> |
| --- | --- |
| 所有事务 |2,000 |

有关超出这些限制时如何处理限制的信息，请参阅 [Azure Key Vault 限制指南](../articles/key-vault/key-vault-ovw-throttling.md)。

<sup>1</sup> 所有事务类型的订阅范围限制是每个密钥保管库限制的 5 倍。
