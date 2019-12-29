---
ms.openlocfilehash: ba04dbb410c5dd0f4126c6b351681036f4d2bf3a
ms.sourcegitcommit: 676e2c676414ded74b980a1da9eb0de30817afbe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/27/2019
ms.locfileid: "75500408"
---
资源|默认限制|最大限制
---|---|---
[每个部署的 Web/辅助角色数](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup>|25|25
[实例输入终结点数](https://msdn.microsoft.com/library/gg557552.aspx#InstanceInputEndpoint)|25|25
[输入终结点数](https://msdn.microsoft.com/library/gg557552.aspx#InputEndpoint)|25|25
[内部终结点数](https://msdn.microsoft.com/library/gg557552.aspx#InternalEndpoint)|25|25

<sup>1</sup>包含 Web/辅助角色的每个云服务可以有两个部署，一个用于生产，一个用于过渡。 另请注意，此限制与不同的角色数目（配置）相关，而不是与每个角色的实例数目（缩放）相关。