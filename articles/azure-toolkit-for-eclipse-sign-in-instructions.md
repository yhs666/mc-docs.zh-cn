---
title: "用于 Eclipse 的 Azure 工具包的登录说明 | Microsoft Docs"
description: "了解如何使用用于 Eclipse 的 Azure 工具包登录到 Azure。"
services: 
documentationcenter: java
author: alexchen2016
manager: digimobile
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
origin.date: 04/14/2017
ms.date: 08/25/2017
ms.author: v-junlch
ms.openlocfilehash: 2c2bae89dcd672ec18302fd1b9ff6ef899e057e9
ms.sourcegitcommit: 98dcd975d4c18b9f2b125edf5d9760914ef8136c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2017
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>用于 Eclipse 的 Azure 工具包的 Azure 登录说明

用于 Eclipse 的 Azure 工具包提供了两种用于登录到 Azure 帐户的方法：

  - 交互式 - 如果使用此方法，每次登录 Azure 帐户时，都需要输入 Azure 凭据。 目前此方式不支持 Azure 中国区帐户。
  - 自动 - 如果使用此方法，将创建一个包含服务主体数据的凭据文件，然后即可使用该凭据文件自动登录到 Azure 帐户。

以下部分中的步骤介绍如何使用**自动**方法。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## 如何创建 Azure 帐户的凭据文件 <a name="create-a-credentials-file"></a>

[!INCLUDE [azure-eclipse-login-guide](../includes/azure-eclipse-login-guide.md)]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>使用已创建的凭据文件自动登录到 Azure 帐户

如果在使用 Eclipse 时从 Azure 中注销，则需要重新配置用于 Eclipse 的 Azure 工具包以使用已创建的凭据文件，才能自动登录到 Azure 帐户。 以下步骤介绍如何配置 Azure 工具包以使用现有凭据文件。

1. 使用 Eclipse 打开项目。

1. 依次单击“工具”、“Azure”、“登录”。

   ![用于 Azure 登录的 Eclipse 菜单][A01]

1. 显示“Azure 登录”对话框时，选择“自动”，并单击“浏览”。

   ![“登录”对话框][A02]

1. 显示“选择已验证文件”对话框时，选择先前创建的凭据文件，并单击“选择”。

   ![“登录”对话框][A08]

1. 显示“Azure 登录”对话框时，单击“登录”。

   ![“Azure 登录”对话框][A06]

1. 显示“选择订阅”对话框时，选择要使用的订阅，并单击“确定”。

   ![“选择订阅”对话框][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>已自动登录时，从 Azure 帐户中注销

按照上一部分中的步骤配置后，每次重启 Eclipse 时，Azure 工具包都会将你自动登录到 Azure 帐户。 但是，若要注销 Azure 帐户并禁止 Azure 工具包将你自动登录，请使用以下步骤。

1. 在 Eclipse 中，依次单击“工具”、“Azure”、“注销”。

   ![用于 Azure 注销的 Eclipse 菜单][L01]

1. 显示“Azure 注销”对话框时，单击“是”。

   ![“注销”对话框][L03]

## <a name="see-also"></a>另请参阅
有关 Azure Toolkits for Java IDE 的详细信息，请参阅以下链接：

- [适用于 Eclipse 的 Azure 工具包]
  - [用于 Eclipse 的 Azure 工具包的新增功能]
  - [安装用于 Eclipse 的 Azure 工具包]
  - *用于 Eclipse 的 Azure 工具包的登录说明（本文）*
  - [在 Eclipse 中创建 Azure 的 Hello World Web 应用]
- [用于 IntelliJ 的 Azure 工具包]
  - [用于 IntelliJ 的 Azure 工具包的新增功能]
  - [安装用于 IntelliJ 的 Azure 工具包]
  - [用于 IntelliJ 的 Azure 工具包的登录说明]
  - [在 IntelliJ 中创建 Azure 的 Hello World Web 应用]

有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心]和[用于 Visual Studio Team Services 的 Java 工具]。

<!-- URL List -->

[用于 Eclipse 的 Azure 工具包]: ./azure-toolkit-for-eclipse.md
[适用于 IntelliJ 的 Azure 工具包]: ./azure-toolkit-for-intellij.md
[在 Eclipse 中创建 Azure 的 Hello World Web 应用]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[在 IntelliJ 中创建 Azure 的 Hello World Web 应用]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[安装 Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[安装用于 IntelliJ 的 Azure 工具包]: ./azure-toolkit-for-intellij-installation.md
[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[用于 IntelliJ 的 Azure 工具包的登录说明]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[用于 Eclipse 的 Azure 工具包的新增功能]: ./azure-toolkit-for-eclipse-whats-new.md
[用于 IntelliJ 的 Azure 工具包的新增功能]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java 开发人员中心]: /develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png

