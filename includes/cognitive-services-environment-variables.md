---
author: aahill
ms.service: cognitive-services
ms.topic: include
origin.date: 06/24/2019
ms.date: 07/24/2019
ms.author: v-junlch
ms.openlocfilehash: 3e0be5eb9ee610c8f048415fb2a7d0d73cef1c53
ms.sourcegitcommit: 9a330fa5ee7445b98e4e157997e592a0d0f63f4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68439909"
---
## <a name="configure-an-environment-variable-for-authentication"></a>配置用于身份验证的环境变量

应用程序需要对它们使用的认知服务的访问进行身份验证。 若要进行身份验证，我们建议创建一个环境变量来存储 Azure 资源的密钥。 

获得密钥后，将其写入运行应用程序的本地计算机上的新环境变量。 若要设置环境变量，请打开控制台窗口，并遵照适用于操作系统的说明。 将 `your-key` 替换为资源的密钥之一。

* Windows

    ```console
    setx COGNITIVE_SERVICE_KEY "your-key"
    ```

    添加环境变量后，可能需要重启任何正在运行的、需要读取环境变量的程序（包括控制台窗口）。 例如，如果使用 Visual Studio 作为编辑器，请在运行示例之前重启 Visual Studio。

* Linux
    
    ```bash
    export COGNITIVE_SERVICE_KEY=your-key
    ```
    
    添加环境变量后，请从控制台窗口运行 `source ~/.bashrc`，使更改生效。
    
* macOS
    
    编辑 .bash_profile，然后添加环境变量：
    
    ```bash
    export COGNITIVE_SERVICE_KEY=your-key
    ```
    
    添加环境变量后，请从控制台窗口运行 `source .bash_profile`，使更改生效。

