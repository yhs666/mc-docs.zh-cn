---
ms.openlocfilehash: e9dbd56fdfa86c0e6359fbdc40e5b1b146610c0f
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58627788"
---
1. 安装 dapl、rdmacm、ibverbs 和 mlx4

   ```bash
   sudo apt-get update

   sudo apt-get install libdapl2 libmlx4-1
   ```

2. 在 /etc/waagent.conf 中，通过取消注释以下配置行来启用 RDMA。 需要根访问权限才能编辑此文件。

   ```
   OS.EnableRDMA=y

   OS.UpdateRdmaDriver=y
   ```

3. 在 /etc/security/limits.conf 文件中，添加或更改 KB 中的以下内存设置。 需要根访问权限才能编辑此文件。 出于测试目的，可以将 memlock 设置为不受限制。 例如：`<User or group name>   hard    memlock   unlimited`。

   ```
   <User or group name> hard    memlock <memory required for your application in KB>

   <User or group name> soft    memlock <memory required for your application in KB>
   ```

4. 安装 Intel MPI 库。 从 Intel [购买和下载](https://software.intel.com/intel-mpi-library/)库或下载[免费评估版本](https://registrationcenter.intel.com/en/forms/?productid=1740)。

   ```bash
   wget http://registrationcenter-download.intel.com/akdlm/irc_nas/tec/9278/l_mpi_p_5.1.3.223.tgz
   ```

   仅支持 Intel MPI 5.x 运行时。

   有关安装步骤，请参阅 [Intel MPI 库安装指南](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html)。

5. 启用非根非调试器进程的 ptrace（为最新版本的 Intel MPI 所需）。

   ```bash
   echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
   ```
   <!--ms.date: 07/30/2018-->