---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
origin.date: 01/31/2019
ms.date: 03/04/2019
ms.author: v-biyu
ms.openlocfilehash: 773c6d426f58e0b1f67552ea025352ed38fd0cb9
ms.sourcegitcommit: b066ffa5ad735a6ea167044fe390cfd891d37df1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/19/2019
ms.locfileid: "56409087"
---
## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 静态加密：<ul><li>服务器端加密</li><li>使用客户托管密钥的服务器端加密</li><li>其他加密功能（例如客户端、始终加密等）</ul>| 是 | 加密所有对象。 |
| 传输中加密：<ul><li>快速路由加密</li><li>Vnet 中加密</li><li>VNet-VNet 加密</ul>| 是 | 所有通信都通过加密的 API 调用进行 |
| 加密密钥处理（CMK、BYOK 等）| 是 | 客户控制其密钥保管库中的所有密钥。|
| 列级加密（Azure 数据服务）| 不适用 |  |
| 加密的 API 调用| 是 | 使用 HTTPS。 |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 是 | 使用虚拟网络 (VNet) 服务终结点。 |
| vNET 注入支持| 否 |  |
| 网络隔离/防火墙支持| 是 | 使用 VNet 防火墙规则。 |
| 支持强制隧道 | 否 |  |


## <a name="iam-support"></a>IAM 支持

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 访问管理 - 身份验证| 是 | 身份验证通过 Azure Active Directory 来进行。 |
| 访问管理 - 授权| 是 | 使用密钥保管库访问策略。 |



## <a name="access-controls"></a>访问控制

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制/管理平面访问控制 | 是 | Azure Resource Manager 基于角色的访问控制 (RBAC) |
| 数据平面访问控制（在每个服务级别） | 是 | 密钥保管库访问策略 |
