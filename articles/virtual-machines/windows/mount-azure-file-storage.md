---
title: 从 Azure Windows VM 装载 Azure 文件存储 | Azure
description: 使用 Azure 文件存储在云中存储文件，并从 Azure 虚拟机 (VM) 装载你的云文件共享。
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
origin.date: 01/02/2018
ms.date: 02/05/2018
ms.author: v-yeche
ms.openlocfilehash: e1606422d3afb127f08b1dfbc48264d778f79054
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52660841"
---
# <a name="use-azure-file-shares-with-windows-vms"></a>将 Azure 文件共享用于 Windows VM 

可将 Azure 文件共享用作一种从 VM 中存储和访问文件的方式。 例如，你可以存储一个要与所有 VM 共享的脚本或应用程序配置文件。 本文介绍如何创建和装载 Azure 文件共享，以及如何上传和下载文件。

## <a name="connect-to-a-file-share-from-a-vm"></a>从 VM 连接到文件共享

本部分假设你有想要连接的文件共享。 如果需要创建一个，请参阅本文后面的[创建文件共享](#create-a-file-share)。

1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 在左侧菜单中，单击“存储帐户”。
3. 选择存储帐户。
4. 在“概述”页面中的“服务”下，选择“文件”。
5. 选择文件共享或单击“+ 文件共享”创建要使用的新文件共享。
6. 单击“连接”，打开显示命令行语法的页面，这些语法用于从 Windows 或 Linux 装载文件共享。
7. 在“驱动器号”中，选择想要用于标识驱动器的字母。
8. 选择要使用的语法，然后选择右侧的复制按钮将它复制到剪贴板。 再将它粘贴到可以轻松访问的位置。 
8. 连接到 VM，然后打开命令提示符。
9. 在其中粘贴经过编辑的连接语法，然后点击“Enter”。
10. 创建连接后，将出现消息“命令已成功完成”。
11. 通过键入驱动器号切换到该驱动器，检查连接，然后键入“dir”，查看文件共享的内容。

## <a name="create-a-file-share"></a>创建文件共享 
1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 在左侧菜单中，单击“存储帐户”。
3. 选择存储帐户。
4. 在“概述”页面中的“服务”下，选择“文件”。
5. 在“文件服务”页面中，单击“+ 文件共享”。
6. 填写文件共享名。 文件共享名可以使用小写字母、数字和单个连字符。 该名称不能以连字符开头，并且不能使用多个连续的连字符。 
7. 填写文件的大小限制，最大为 5120 GB。
8. 单击“确定”以创建文件共享。

## <a name="upload-files"></a>上传文件
1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 在左侧菜单中，单击“存储帐户”。
3. 选择存储帐户。
4. 在“概述”页面中的“服务”下，选择“文件”。
5. 选择文件共享。
6. 单击“上传”打开“上传文件”页。
7. 单击文件夹图标，浏览要上传的文件的本地文件系统。   
8. 单击“上传”将文件上传到文件共享。

## <a name="download-files"></a>下载文件
1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 在左侧菜单中，单击“存储帐户”。
3. 选择存储帐户。
4. 在“概述”页面中的“服务”下，选择“文件”。
5. 选择文件共享。
6. 右键单击文件，然后选择“下载”将其下载到本地计算机。

## <a name="next-steps"></a>后续步骤

还可以通过 PowerShell 创建和管理文件共享。 有关详细信息，请参阅[在 Windows 上开始使用 Azure 文件存储](../../storage/files/storage-dotnet-how-to-use-files.md)。

<!-- Update_Description: update meta properties, wording update -->