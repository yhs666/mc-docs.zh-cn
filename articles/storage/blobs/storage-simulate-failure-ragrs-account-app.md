---
title: "模拟在 Azure 中访问读取访问冗余存储时出现的故障 | Microsoft Docs"
description: "模拟在访问读取访问异地冗余存储时出现的错误"
services: storage
documentationcenter: 
author: forester123
manager: digimobile
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
origin.date: 12/05/2017
ms.date: 01/01/2018
ms.author: v-johch
ms.custom: mvc
ms.openlocfilehash: eff2af121653950ccc1c4ec04c75ed4e1742b334
ms.sourcegitcommit: 469a0ce3979408a4919a45c1eb485263f506f900
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2017
---
# <a name="simulate-a-failure-in-accessing-read-access-redundant-storage"></a>模拟在访问读取访问冗余存储时出现的故障

本教程是一个系列中的第二部分。 在本教程中，你需要将 Fiddler 对请求的故障响应注入[读取访问异地冗余](../common/storage-redundancy.md#read-access-geo-redundant-storage) (RA-GRS) 存储帐户，以便模拟一个故障，让应用程序从辅助终结点读取数据。

![方案应用](media/storage-simulate-failure-ragrs-account-app/scenario.png)

此系列的第二部分介绍如何：

> [!div class="checklist"]
> * 运行和暂停应用程序
> * 模拟故障
> * 模拟主终结点还原

## <a name="prerequisites"></a>先决条件

完成本教程：

* 下载并安装 [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

完成本教程的前提是完成前面的存储教程：[使应用程序数据在 Azure 存储中高度可用][previous-tutorial]。

## <a name="launch-fiddler"></a>启动 Fiddler

打开 Fiddler，选择“规则”和“自定义规则”。

![自定义 Fiddler 规则](media/storage-simulate-failure-ragrs-account-app/figure1.png)

此时会启动 Fiddler ScriptEditor，显示 SampleRules.js 文件。 此文件用于自定义 Fiddler。 将以下代码示例粘贴到 `OnBeforeResponse` 函数中。 注释掉新代码是为了确保其创建的逻辑不会立即实现。 完成后，选择“文件”和“保存”以保存所做的更改。

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

## <a name="start-and-pause-the-application"></a>启动和暂停应用程序

在 Visual Studio 中，按 F5 或选择“启动”，开始调试应用程序。 应用程序开始从主终结点读取数据以后，按控制台窗口中的任意键即可暂停应用程序。

## <a name="simulate-failure"></a>模拟故障

暂停应用程序以后，即可取消注释在前面的步骤中在 Fiddler 中保存的自定义规则。 此代码示例查找对 RA-GRS 存储帐户的请求。如果路径包含图像的名称 `HelloWorld`，则会返回响应代码 `503 - Service Unavailable`。

导航到 Fiddler，然后选择“规则” -> “自定义规则...”。取消注释以下行，将 `STORAGEACCOUNTNAME` 替换为你的存储帐户的名称。 选择“文件” -> “保存”，保存所做的更改。

```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.chinacloudapi.cn")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

若要恢复应用程序，请按任意键。

应用程序重新开始运行以后，对主终结点的请求就会失败。 应用程序尝试重新连接到主终结点 5 次。 达到故障阈值（五次尝试）以后，就会从处于只读状态的辅助终结点请求图像。 成功地从辅助终结点检索图像 20 次以后，应用程序就会尝试连接到主终结点。 如果仍无法访问主终结点，应用程序会继续从辅助终结点读取数据。 此模式为[断路器](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker)模式，在前一教程中已介绍过。

![粘贴自定义规则](media/storage-simulate-failure-ragrs-account-app/figure3.png)

## <a name="simulate-primary-endpoint-restoration"></a>模拟主终结点还原

由于在前面的步骤中已设置 Fiddler 自定义规则，因此对主终结点的请求失败。 若要模拟主终结点再次运行的情况，请删除用于注入 `503` 错误的逻辑。

若要暂停应用程序，请按任意键。

### <a name="remove-the-custom-rule"></a>删除自定义规则

导航到 Fiddler，然后选择“规则”和“自定义规则...”。注释或删除 `OnBeforeResponse` 函数中的自定义逻辑，保留默认函数。 选择“文件”和“保存”，保存所做的更改。

![删除自定义的规则](media/storage-simulate-failure-ragrs-account-app/figure5.png)

完成后，按任意键即可恢复应用程序。 应用程序继续从主终结点读取数据，直到达到 999 次读取的限制。

![恢复应用程序](media/storage-simulate-failure-ragrs-account-app/figure4.png)

## <a name="next-steps"></a>后续步骤

此系列的第二部分介绍了如何模拟一个故障，以便测试读取访问异地冗余存储，例如，如何：

> [!div class="checklist"]
> * 运行和暂停应用程序
> * 模拟故障
> * 模拟主终结点还原

请访问以下链接，查看预先生成的存储示例。

> [!div class="nextstepaction"]
> [Azure 存储脚本示例](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md