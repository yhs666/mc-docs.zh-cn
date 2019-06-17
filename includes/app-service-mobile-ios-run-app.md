---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
origin.date: 08/23/2018
ms.date: 06/17/2019
ms.author: v-biyu
ms.openlocfilehash: bcc79b9367fbb00562cfa47fbbd290ac732ebd4c
ms.sourcegitcommit: d7db02d1b62c7b4deebd5989be97326b4425d1d3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687465"
---
1. 使用 Xcode 打开下载的客户端项目。

2. 转到 [Azure 门户](https://portal.azure.cn/)，并导航到已创建的移动应用。 在 `Overview` 边栏选项卡上，查找作为移动应用公共终结点的 URL。 示例 - 我的应用名称“test123”的站点名将为 https://test123.chinacloudsites.cn 。

3. 在 Swift 项目中，打开此文件夹中的文件 `ToDoTableViewController.swift` - ZUMOAPPNAME/ZUMOAPPNAME/ToDoTableViewController.swift。 应用程序名称为 `ZUMOAPPNAME`。

4. 在 `viewDidLoad()` 方法中，将 `ZUMOAPPURL` 参数替换为上面的公共终结点。

    `let client = MSClient(applicationURLString: "ZUMOAPPURL")`

    变为
    
    `let client = MSClient(applicationURLString: "https://test123.chinacloudsites.cn")`
    
5. 在 Objective-C 项目中，打开此文件夹（ZUMOAPPNAME/ZUMOAPPNAME）中的文件 `QSTodoService.m`。 应用程序名称为 `ZUMOAPPNAME`。

6. 在 `init` 方法中，将 `ZUMOAPPURL` 参数替换为上面的公共终结点。

    `self.client = [MSClient clientWithApplicationURLString:@"ZUMOAPPURL"];`

    变为
    
    `self.client = [MSClient clientWithApplicationURLString:@"https://test123.chinacloudsites.cn"];`

7. 按“运行”  按钮生成项目，并在 iOS 模拟器中启动应用。

8. 在应用中，  单击加号 (+) 图标，键入有意义的文本（例如 Complete the tutorial）  ，然后单击“保存”按钮。 这会将一个 POST 请求发送到之前部署的 Azure 后端。 后端将请求中的数据插入到 TodoItem SQL 表中，并将有关新存储的项的信息返回给移动应用。 移动应用会在列表中显示此数据。

   ![在 iOS 上运行的快速启动应用](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure portal]: https://portal.azure.cn/
