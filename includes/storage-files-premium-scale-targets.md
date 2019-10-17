---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 06/07/2019
ms.date: 10/14/2019
ms.author: v-jay
ms.openlocfilehash: c3da4843dd5cd0a40fcaac35a19c120896fdbc7e
ms.sourcegitcommit: aea45739ba114a6b069f782074a70e5dded8a490
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275170"
---
#### <a name="additional-premium-file-share-level-limits"></a>其他高级文件共享级别限制

|区域  |目标  |
|---------|---------|
|最小大小增加/减少    |1 GiB      |
|基线 IOPS    |每个 GiB 1 IOPS，最高 100,000|
|IOPS 突发    |每个 GiB 3倍 IOPS，最高 100,000|
|出口速率         |60 MiB/秒 + 0.06 * 预配 GiB        |
|入口速率| 40 MiB/秒 + 0.04 * 预配 GiB |

#### <a name="file-level-limits"></a>文件级别限制

|区域  |高级文件  |标准文件 |
|---------|---------|---------|
|大小                  |1 TiB         |1 TiB|
|每个文件的最大 IOPS     |5,000         |1,000|
|并发句柄数    |2,000         |2,000|
|流入量  |300 MiB/秒|      查看标准文件吞吐量值|
|流出量   |200 Mib/秒| 查看标准文件吞吐量值|
|吞吐量| 查看高级文件流入量/流出量值| 最多 60 MiB/秒|