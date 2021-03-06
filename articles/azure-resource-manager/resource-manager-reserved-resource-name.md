---
title: 保留资源名称错误
description: 描述了如何解决提供的资源名称包括保留字的错误。
ms.topic: troubleshooting
origin.date: 11/08/2017
ms.date: 11/25/2019
ms.openlocfilehash: fb07ed3ecac3fbb2e2d5bba645cc1c969c2447ac
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389569"
---
# <a name="resolve-reserved-resource-name-errors"></a>解决保留资源名称错误

本文描述了在部署其名称中包含保留字的资源时遇到的错误。

## <a name="symptom"></a>症状

在部署可通过公共终结点访问的资源时，可能会收到以下错误：

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>原因

具有公共终结点的资源的名称中不能使用保留字或商标。

以下为保留字：

* ACCESS
* AZURE
* BING
* BIZSPARK
* BIZTALK
* CORTANA
* DIRECTX
* DOTNET
* DYNAMICS
* EXCEL
* EXCHANGE
* FOREFRONT
* GROOVE
* HOLOLENS
* HYPERV
* KINECT
* LYNC
* MSDN
* O365
* OFFICE
* OFFICE365
* ONEDRIVE
* ONENOTE
* OUTLOOK
* POWERPOINT
* SHAREPOINT
* SKYPE
* VISIO
* VISUALSTUDIO

以下字词不能作为全字或子字符串在名称中使用：

* LOGIN
* MICROSOFT
* WINDOWS
* XBOX

## <a name="solution"></a>解决方案

请提供一个不使用其中任一保留字的名称。

<!-- Update_Description: update meta properties, wording update, update link -->