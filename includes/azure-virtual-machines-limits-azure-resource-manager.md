---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 11/09/2018
ms.date: 05/20/2019
ms.author: v-yeche
ms.openlocfilehash: f79508676c14a590033132999007228390d205db
ms.sourcegitcommit: bf4afcef846cc82005f06e6dfe8dd3b00f9d49f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66039218"
---
| Resource | 默认限制 |
| --- | --- |
| 每个可用性集的虚拟机数 | 200 |
| 每个订阅的证书数 |不受限制<sup>1</sup> |

<sup>1</sup>使用 Azure Resource Manager 时，证书存储在 Azure 密钥保管库中。 对于每个订阅，证书个数没有限制。 对于每个部署（包括单一 VM 或可用性集），证书的限制为 1-MB。