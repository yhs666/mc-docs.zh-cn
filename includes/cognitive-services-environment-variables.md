---
author: aahill
ms.service: cognitive-services
ms.topic: include
origin.date: 06/24/2019
ms.date: 07/09/2019
ms.author: v-junlch
ms.openlocfilehash: dfd22c9c892b02594f8063da77b070c949975f80
ms.sourcegitcommit: 8f49da0084910bc97e4590fc1a8fe48dd4028e34
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844611"
---
## <a name="configure-an-environment-variable-for-authentication"></a>配置用于身份验证的环境变量

应用程序需要对它们使用的认知服务的访问进行身份验证。 若要进行身份验证，我们建议你创建一个环境变量，以便在 Azure 资源中存储密钥。 

获得密钥后，将其写入运行应用程序的本地计算机上的新环境变量。 若要设置环境变量，请打开控制台窗口，并遵照适用于操作系统的说明。 将 `your-key` 替换为异常检测器访问密钥：

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

