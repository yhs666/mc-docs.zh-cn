---
author: rockboyfor
ms.service: site-recovery
ms.topic: include
origin.date: 08/06/2018
ms.date: 09/17/2018
ms.author: v-yeche
ms.openlocfilehash: ff38bba3f745fd143b9d96a955c77b36c788705a
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52643534"
---
注销进程服务器的步骤各不相同，具体取决于其与配置服务器的连接状态。

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>注销处于“已连接”状态的进程服务器

1. 以管理员身份远程进入进程服务器。
2. 启动“控制面板”，然后打开“程序”>“卸载程序”
3. 卸载名为“Azure Site Recovery 配置/进程服务器”的程序
4. 步骤 3 完成后，可以卸载 **Azure Site Recovery 配置/进程服务器依赖项**

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>注销处于“已断开连接 ”状态的进程服务器

> [!WARNING]
> 如果无法恢复安装了进程服务器的虚拟机，则应使用以下步骤。

1. 以管理员身份登录到配置服务器。
2. 打开管理命令提示符，并浏览到目录 `%ProgramData%\ASR\home\svsystems\bin`。
3. 现在，运行命令。

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. 上面的命令将为进程服务器列表（在条目重复的情况下，进程服务器可能为多个）提供序列号（简称 S.No）、IP 地址（简称 IP）、部署进程服务器时所在 VM 的名称（简称“名称”）、VM 的检测信号（简称“检测信号”），如下所示。
    ![Unregister-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)
5. 现在，请输入要注销的进程服务器的序列号。
6. 此时会清除系统提供的进程服务器的详细信息，并会显示消息：**成功注销了 server-name> (server-IP-address)**
<!--Update_Description: wording update-->