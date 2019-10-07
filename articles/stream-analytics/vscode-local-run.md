---
title: 使用 Visual Studio Code（预览版）在本地测试 Azure 流分析查询
description: 本文介绍如何使用针对 Visual Studio Code 的 Azure 流分析工具在本地测试查询。
ms.service: stream-analytics
author: lingliw
ms.author: v-lingwu
manager: digimobile
ms.reviewer: jasonh
origin.date: 05/15/2019
ms.date: 08/09/2019
ms.topic: conceptual
ms.openlocfilehash: 80e4ad36148e1165ba92e5664ee49bd7fe831ee4
ms.sourcegitcommit: c72fba1cacef1444eb12e828161ad103da338bb1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71674689"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio-code"></a>使用 Visual Studio Code 在本地测试流分析查询

可使用针对 Visual Studio Code 的 Azure 流分析工具通过示例数据在本地测试流分析作业。

使用此[快速入门](quick-create-vs-code.md)了解如何使用 Visual Studio Code 创建流分析作业。

## <a name="run-queries-locally"></a>在本地运行查询

可使用适用于 Visual Studio Code 的 Azure 流分析扩展通过示例数据在本地测试流分析作业。

1. 创建流分析作业后，使用 **Ctrl+Shift+P** 打开命令面板。 然后键入并选择 **ASA:Add Input**。

    ![在 Visual Studio code 中添加 ASA 输入](./media/vscode-local-run/add-input.png)

2. 选择“本地输入”  。

    ![在 Visual Studio code 中添加 ASA 本地输入](./media/vscode-local-run/add-local-input.png)

3. 选择“+ 新建本地输入”  。

    ![在 Visual Studio code 中添加新的 ASA 本地输入](./media/vscode-local-run/add-new-local-input.png)

4. 输入在查询中使用的相同输入别名。

    ![添加新的 ASA 本地输入别名](./media/vscode-local-run/new-local-input-alias.png)

5. 在 **LocalInput_DefaultLocalStream.json** 文件中，输入本地数据文件所在的文件路径。

    ![在 Visual Studio 中输入本地文件路径](./media/vscode-local-run/local-file-path.png)

6. 返回到查询编辑器，并选择“在本地运行”  。

    ![在查询编辑器中选择“在本地运行”](./media/vscode-local-run/run-locally.png)

## <a name="next-steps"></a>后续步骤

* [在 Visual Studio Code（预览版）中创建 Azure 流分析云作业](quick-create-vs-code.md)

* [使用 Visual Studio Code（预览版）浏览 Azure 流分析作业](vscode-explore-jobs.md)
