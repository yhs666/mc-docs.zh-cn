---
title: "在 Azure 自动化中测试 Runbook | Azure"
description: "在 Azure 自动化中发布某个 Runbook 之前，你可以对它进行测试，以确保它按预期工作。  本文介绍如何测试 Runbook 并查看其输出。"
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 7f7db785-52c0-4613-aa12-b02fd32a5182
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 09/12/2016
ms.date: 01/09/2017
ms.author: v-dazen
ms.openlocfilehash: d6487304dbf9526d9263d3903e7849f0a8a1d084
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="testing-a-runbook-in-azure-automation"></a>在 Azure 自动化中测试 Runbook
测试 Runbook 时，将执行[草稿版](automation-creating-importing-runbook.md#publishing-a-runbook)，并会完成其执行的任何操作。 不会创建作业历史记录，但会在“测试输出”窗格中显示[“输出”](automation-runbook-output-and-messages.md#output-stream)与[“警告和错误”](automation-runbook-output-and-messages.md#message-streams)流。 仅当 [$VerbosePreference 变量](automation-runbook-output-and-messages.md#preference-variables)设置为 Continue 时，才会在“输出”窗格中显示发送到[详细流](automation-runbook-output-and-messages.md#message-streams)的消息。

即使草稿版正在运行，该 Runbook 也仍会正常执行工作流，并针对环境中的资源执行任何操作。 因此，你只能在非生产资源中测试 Runbook。

## <a name="to-test-a-runbook-in-the-azure-classic-management-portal"></a>在 Azure 经典管理门户中测试 Runbook
1. [打开 Runbook 的草稿版](automation-edit-textual-runbook.md#to-edit-a-runbook-with-the-azure-portal)。
2. 单击“测试”按钮，开始测试。  如果 Runbook 具有参数，用户将收到一个对话框，可在其中提供用于测试的值。
3. 可以使用输出窗格下方的按钮停止或暂停正在测试中的 Runbook。 当你暂停 Runbook 时，该 Runbook 会完成它在被暂停之前正在进行的活动。 暂停 Runbook 后，你可以将它停止或重启。
7. 在输出窗格中检查 Runbook 的输出。

## <a name="next-steps"></a>后续步骤
* 若要了解如何创建或导入 Runbook，请参阅[在 Azure 自动化中创建或导入 Runbook](automation-creating-importing-runbook.md)
* 若要开始使用 PowerShell 工作流 Runbook，请参阅[我的第一个 PowerShell 工作流 Runbook](automation-first-runbook-textual.md)
* 若要详细了解如何配置 Runbook 以返回状态消息和错误（包括建议的做法），请参阅 [Azure 自动化中的 Runbook 输出和消息](automation-runbook-output-and-messages.md)