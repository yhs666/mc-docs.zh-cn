---
title: 教程：模拟在 Azure 中访问读取访问冗余存储时出现的故障 | Microsoft Docs
description: 模拟在访问读取访问异地冗余存储时出现的错误
services: storage
author: WenJason
ms.service: storage
ms.topic: tutorial
origin.date: 01/03/2019
ms.date: 01/21/2019
ms.author: v-jay
ms.openlocfilehash: 278221ae223d30331930f095920f1c147660e7ec
ms.sourcegitcommit: 317ea7e3b2d307569d3bf7777bd3077013ae4df6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2019
ms.locfileid: "54334490"
---
# <a name="tutorial-simulate-a-failure-in-accessing-read-access-redundant-storage"></a>教程：模拟在访问读取访问冗余存储时出现的故障

本教程是一个系列中的第二部分。 本教程通过模拟一个故障，介绍[读取访问异地冗余](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) 的优点。

可以使用 [Fiddler](#simulate-a-failure-with-fiddler) 或[静态路由](#simulate-a-failure-with-an-invalid-static-route)来模拟一个故障。 可以使用任一方法来模拟这样一个故障：向[读取访问异地冗余](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) 存储帐户的主终结点发送请求失败，导致应用程序改从辅助终结点读取内容。

如果没有 Azure 订阅，可在开始前创建一个 [1 元人民币试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。

此系列的第二部分介绍如何：

> [!div class="checklist"]
> * 运行和暂停应用程序
> * 使用 [fiddler](#simulate-a-failure-with-fiddler) 或[无效的静态路由](#simulate-a-failure-with-an-invalid-static-route)模拟失败 
> * 模拟主终结点还原

## <a name="prerequisites"></a>先决条件

开始学习本教程之前，请完成前一教程：[使用 Azure 存储实现应用程序数据的高可用性][previous-tutorial]。

若要使用 Fiddler 模拟失败，请执行以下操作： 

* 下载并[安装 Fiddler](https://www.telerik.com/download/fiddler)

## <a name="simulate-a-failure-with-fiddler"></a>使用 Fiddler 模拟失败

若要使用 Fiddler 来模拟失败，请针对向 RA-GRS 存储帐户的主终结点发出的请求注入一个失败的响应。

以下部分介绍如何使用 fiddler 模拟失败和主终结点还原。

### <a name="launch-fiddler"></a>启动 Fiddler

打开 Fiddler，选择“规则”和“自定义规则”。

![自定义 Fiddler 规则](media/storage-simulate-failure-ragrs-account-app/figure1.png)

此时，Fiddler ScriptEditor 会启动，并显示 SampleRules.js 文件。 此文件用于自定义 Fiddler。

将以下代码示例粘贴到 `OnBeforeResponse` 函数中。 注释掉新代码是为了确保其创建的逻辑不会立即实现。

完成后，依次选择“文件”和“保存”，保存所做的更改。

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error. 
        // When you're ready to stop sending back errors, comment these lines of script out again 
        //     and save the changes.

        if ((oSession.hostname == "contosoragrs.blob.core.chinacloudapi.cn") 
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;  
        }
    */
```

![粘贴自定义的规则](media/storage-simulate-failure-ragrs-account-app/figure2.png)

### <a name="interrupting-the-application"></a>中断应用程序

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET、Python 和 Java v7] (#tab/dotnet-python-java-v7)

在 IDE 或 shell 中运行应用程序。

应用程序开始从主终结点读取数据以后，按控制台窗口中的任意键即可暂停应用程序。

![方案应用](media/storage-simulate-failure-ragrs-account-app/scenario.png)

# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

在 IDE 或 shell 中运行应用程序。

由于你可以控制示例，因此不需中断它即可模拟一个故障。 只需确保文件已上传到存储帐户即可，方法是：先运行示例，然后输入 **P**。

![方案应用](media/storage-simulate-failure-ragrs-account-app/Java-put-list-output.png)

---

### <a name="simulate-failure"></a>模拟故障

当应用程序暂停后，取消注释在 Fiddler 中保存的自定义规则。

此代码示例查找对 RA-GRS 存储帐户发出的请求，如果路径包含文件 `HelloWorld` 的名称，则返回响应代码 `503 - Service Unavailable`。

导航到 Fiddler，然后选择“规则” -> “自定义规则...”。

取消注释以下行，将 `STORAGEACCOUNTNAME` 替换为存储帐户名称。 选择“文件” -> “保存”，保存所做的更改。 

> [!NOTE]
> 如果是在 Linux 上运行示例应用程序，则需在编辑 **CustomRule.js** 文件时重启 Fiddler，以便 Fiddler 安装自定义逻辑。

```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.chinacloudapi.cn")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET、Python 和 Java v7] (#tab/dotnet-python-java-v7)

若要恢复应用程序，请按任意键。

应用程序重新开始运行以后，对主终结点的请求就会失败。 应用程序尝试重新连接到主终结点 5 次。 达到故障阈值（五次尝试）以后，就会从处于只读状态的辅助终结点请求图像。 从辅助终结点成功检索映像 20 次后，应用程序便会尝试连接到主终结点。 如果仍无法访问主终结点，应用程序会继续从辅助终结点读取数据。

![粘贴自定义的规则](media/storage-simulate-failure-ragrs-account-app/figure3.png)

# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

引入故障以后，请输入 **G** 来测试故障。

它会告知你，它使用的是辅助管道而不是主管道。

---

### <a name="simulate-primary-endpoint-restoration"></a>模拟主终结点还原

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET、Python 和 Java v7] (#tab/dotnet-python-java-v7)

由于在前面的步骤中已设置 Fiddler 自定义规则，因此对主终结点的请求失败。

若要模拟主终结点再次运行的情况，请删除用于注入 `503` 错误的逻辑。

若要暂停应用程序，请按任意键。

导航到 Fiddler，然后选择“规则”和“自定义规则...”。 

注释或删除 `OnBeforeResponse` 函数中的自定义逻辑，保留默认函数。

选择“文件”和“保存”，保存所做的更改。

![删除自定义的规则](media/storage-simulate-failure-ragrs-account-app/figure5.png)

完成后，按任意键即可恢复应用程序。 应用程序继续从主终结点读取数据，直到达到 999 次读取的限制。

![恢复应用程序](media/storage-simulate-failure-ragrs-account-app/figure4.png)

# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

由于在前面的步骤中已设置 Fiddler 自定义规则，因此对主终结点的请求失败。

若要模拟主终结点再次运行的情况，请删除用于注入 `503` 错误的逻辑。

导航到 Fiddler，然后选择“规则”和“自定义规则...”。注释或删除 `OnBeforeResponse` 函数中的自定义逻辑，保留默认函数。

选择“文件”和“保存”，保存所做的更改。

完成后，请输入 **G** 来测试下载。 应用程序会报告，它现在再次使用主管道。

---

## <a name="simulate-a-failure-with-an-invalid-static-route"></a>使用无效的静态路由模拟失败

对于向[读取访问异地冗余](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) 存储帐户的主终结点发出的所有请求，可以创建一个无效的静态路由。 本教程使用本地主机作为网关来路由向存储帐户发出的请求。 使用本地主机作为网关会导致向存储帐户主终结点发出的所有请求都以循环方式返回到主机内，随后导致请求失败。 执行以下步骤，使用无效的静态路由模拟失败和主终结点还原。 

### <a name="start-and-pause-the-application"></a>启动和暂停应用程序

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET、Python 和 Java v7] (#tab/dotnet-python-java-v7)

在 IDE 或 shell 中运行应用程序。 应用程序开始从主终结点读取数据以后，按控制台窗口中的任意键即可暂停应用程序。

# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

由于你可以控制示例，因此不需中断它即可测试故障。

确保文件已上传到存储帐户，方法是：先运行示例，然后输入 **P**。

---

### <a name="simulate-failure"></a>模拟故障

在应用程序暂停后，在 Windows 上以管理员身份启动命令提示符，或者在 Linux 上以根用户身份运行终端。

获取有关存储帐户主终结点域的信息，方法是在命令提示符处或终端上输入以下命令。

```
nslookup STORAGEACCOUNTNAME.blob.core.chinacloudapi.cn
``` 
 将 `STORAGEACCOUNTNAME` 替换为存储帐户的名称。 将存储帐户的 IP 地址复制到文本编辑器中，供以后使用。

若要获取本地主机的 IP 地址，请在 Windows 命令提示符处键入 `ipconfig`，或者在 Linux 终端上键入 `ifconfig`。 

若要添加目标主机的静态路由，请在 Windows 命令提示符处或 Linux 终端上键入以下命令。 

#### <a name="linux"></a>Linux

`route add <destination_ip> gw <gateway_ip>`

#### <a name="windows"></a>Windows

`route add <destination_ip> <gateway_ip>`

将 `<destination_ip>` 替换为存储帐户 IP 地址，将 `<gateway_ip>` 替换为本地主机 IP 地址。

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET、Python 和 Java v7] (#tab/dotnet-python-java-v7)

若要恢复应用程序，请按任意键。

应用程序重新开始运行以后，对主终结点的请求就会失败。 应用程序尝试重新连接到主终结点 5 次。 达到故障阈值（五次尝试）以后，就会从处于只读状态的辅助终结点请求图像。 成功地从辅助终结点检索图像 20 次以后，应用程序就会尝试连接到主终结点。 如果仍无法访问主终结点，应用程序会继续从辅助终结点读取数据。

# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

引入故障以后，请输入 **G** 来测试故障。 它会告知你，它使用的是辅助管道而不是主管道。

---

### <a name="simulate-primary-endpoint-restoration"></a>模拟主终结点还原

若要再次模拟主终结点功能，请从路由表中删除主终结点的静态路由。 这样就可以让发送到主终结点的所有请求通过默认网关来路由。

若要删除目标主机（存储帐户）的静态路由，请在 Windows 命令提示符处或 Linux 终端上键入以下命令。

#### <a name="linux"></a>Linux

`route del <destination_ip> gw <gateway_ip>`

#### <a name="windows"></a>Windows

`route delete <destination_ip>`

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET、Python 和 Java v7] (#tab/dotnet-python-java-v7)

若要恢复应用程序，请按任意键。 应用程序继续从主终结点读取数据，直到达到 999 次读取的限制。

![恢复应用程序](media/storage-simulate-failure-ragrs-account-app/figure4.png)


# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

输入 **G** 来测试下载。 应用程序会报告，它现在再次使用主管道。

![恢复应用程序](media/storage-simulate-failure-ragrs-account-app/java-get-pipeline-example-v10.png)

---

## <a name="next-steps"></a>后续步骤

本系列教程的第二部分介绍了如何模拟故障来测试读取访问异地冗余存储。

若要详细了解 RA-GRS 存储的工作原理以及相关风险，请阅读以下文章：

> [!div class="nextstepaction"]
> [使用 RA-GRS 设计 HA 应用](../common/storage-designing-ha-apps-with-ragrs.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md