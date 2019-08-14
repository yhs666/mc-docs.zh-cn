---
author: msmbaldwin
ms.service: virtual-machines
ms.topic: include
origin.date: 07/10/2019
ms.date: 08/06/2019
ms.author: v-junlch
ms.openlocfilehash: 5a31cb8dbb1e0c9246a7c4a8ea08c086845c6b5b
ms.sourcegitcommit: 17cd5461e7d99f40b9b1fc5f1d579f82b2e27be9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819542"
---
## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 静态加密（例如服务器端加密、带客户托管密钥的服务器端加密，以及其他加密功能） | 是 | 请参阅[如何在 Azure 中加密 Linux 虚拟机](/virtual-machines/linux/encrypt-disks)和[加密 Windows VM 上的虚拟磁盘](/virtual-machines/windows/encrypt-disks)。 |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | Azure 虚拟机支持 [ExpressRoute](/expressroute) 和 VNET 加密。 |
| 加密密钥处理（CMK、BYOK 等）| 是 | 客户管理的密钥是受支持的 Azure 加密方案。|
| 列级加密（Azure 数据服务）| 不适用 | |
| 加密的 API 调用| 是 | 通过 HTTPS 和 SSL。 |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 是 | |
| VNet 注入支持| 是 | |
| 网络隔离和防火墙支持| 是 |  |
| 强制隧道支持| 是 | 请参阅[使用 Azure 资源管理器部署模型配置强制隧道](/vpn-gateway/vpn-gateway-forced-tunneling-rm)。 |

## <a name="detection"></a>检测

| 安全属性 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics 等）| 是 | 请参阅[在 Azure 中监视和更新 Linux 虚拟机](/virtual-machines/linux/tutorial-monitoring)和[在 Azure 中监视和更新 Windows 虚拟机](/virtual-machines/windows/tutorial-monitoring)。 |

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 |  |
| 授权| 是 |  |


## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 控制和管理平面日志记录和审核| 是 |  |
| 数据平面日志记录和审核 | 否 |  |

## <a name="configuration-management"></a>配置管理

| 安全属性 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 |  | 

