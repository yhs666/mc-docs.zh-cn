---
title: Azure AD Connect：声明性预配表达式 | Microsoft 文档
description: 说明声明性设置表达式
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/18/2017
ms.date: 06/26/2018
ms.component: hybrid
ms.author: v-junlch
ms.openlocfilehash: b2aeb501cec6d633a70ef312ea90ce6cca0db02c
ms.sourcegitcommit: 8b36b1e2464628fb8631b619a29a15288b710383
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2018
ms.locfileid: "36948029"
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect 同步：了解声明性设置表达式
Azure AD Connect 同步基于 Forefront Identity Manager 2010 中最先引入的声明性设置。 使用该功能可以实现完整的标识集成业务逻辑，而无需编写已编译的代码。

声明性设置的一个重要组成部分是属性流中使用的表达式语言。 所用的语言是 Microsoft® Visual Basic® for Applications (VBA) 的子集。 Microsoft Office 中使用了这种语言，具有 VBScript 经验的用户都认识该语言。 声明性设置表达式语言只使用函数，不属于结构化语言。 它不提供任何方法或语句。 函数嵌套在表达式程序流中。

有关详细信息，请参阅 [Welcome to the Visual Basic for Applications language reference for Office 2013](https://msdn.microsoft.com/library/gg264383.aspx)（欢迎使用适用于 Office 2013 的 Visual Basic 应用程序语言参考）。

属性属于强类型。 函数只接受正确类型的属性。 它还区分大小写。 函数名称和属性名称都必须具有正确的大小写，否则会引发错误。

## <a name="language-definitions-and-identifiers"></a>语言定义和标识符
- 函数名称后跟加括号的参数：FunctionName(argument 1, argument N)。
- 属性用方括号标识：[attributeName]
- 参数通过百分比符号标识：%ParameterName%
- 字符串常量放在引号中：例如 "Contoso"（注意：必须使用直引号 ""，而不能使用弯引号“”）
- 数字值表示不带引号，并且应为十进制。 十六进制值带有前缀 &H。 例如，98052, &HFF
- 布尔值以常量表示： True、 False。
- 内置常量和文本仅使用其名称表示：NULL、CRLF、IgnoreThisFlow

### <a name="functions"></a>函数
声明性设置使用许多函数来实现转换属性值的可能性。 这些函数可以嵌套，因此，一个函数的结果会传递到另一个函数。

`Function1(Function2(Function3()))`

函数的完整列表可在[函数引用](active-directory-aadconnectsync-functions-reference.md)中找到。

### <a name="parameters"></a>parameters
通过连接器或由管理员使用 PowerShell 定义参数。 参数通常包含因系统不同而各异的值，例如用户所在域的名称。 这些参数可在属性流中使用。

Active Directory 连接器为入站同步规则提供以下参数：

| 参数名称 | 注释 |
| --- | --- |
| Domain.Netbios |当前正在导入的域的 Netbios 格式，例如 FABRIKAMSALES |
| Domain.FQDN |当前正在导入的域的 FQDN 格式，例如 sales.fabrikam.com |
| Domain.LDAP |当前正在导入的域的 LDAP 格式，例如 DC=sales,DC=fabrikam,DC=com |
| Forest.Netbios |当前正在导入的林名称的 Netbios 格式，例如 FABRIKAMCORP |
| Forest.FQDN |当前正在导入的林名称的 FQDN 格式，例如 fabrikam.com |
| Forest.LDAP |当前正在导入的林名称的 LDAP 格式，例如 DC=fabrikam,DC=com |

系统提供以下参数用于获取当前正在运行的连接器的标识符：  
`Connector.ID`

以下示例使用用户所在域的 netbios 名称填充 Metaverse 属性域：  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>运算符
可以使用以下运算符：

- **比较**：<、<=、<>、=、>、>=
- **数学**：+、-、\*、-
- **字符串**：&（连接）
- **逻辑**：&&（和）、||（或）
- **计算顺序**：( )

运算符从左到右进行求值，并具有相同的求值优先级。 也就是说，\*（乘号）不会在 -（减号）之前求值。 2\*(5+3) 与 2\*5+3 不同。 如果从左到右的计算顺序不适当，则使用括号 () 来更改计算顺序。

## <a name="multi-valued-attributes"></a>多值属性
可对单值和多值属性运行函数。 对于多值属性，函数针对每个值运行，向每个值应用相同的函数。

例如：  
`Trim([proxyAddresses])` 对 proxyAddress 属性中的每个值执行 Trim。  
`Word([proxyAddresses],1,"@") & "@contoso.com"` 对于包含 @-sign 的每个值，将域替换为 @contoso.com。  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])` 查找 SIP 地址并从值中删除该地址。

## <a name="next-steps"></a>后续步骤
- 在[了解声明性预配](active-directory-aadconnectsync-understanding-declarative-provisioning.md)中阅读有关配置模型的详细信息。
- 在[了解默认配置](active-directory-aadconnectsync-understanding-default-configuration.md)中了解如何现成使用声明性设置。
- 在[如何更改默认配置](active-directory-aadconnectsync-change-the-configuration.md)中了解如何使用声明性预配进行实际更改。

**概述主题**

- [Azure AD Connect 同步：理解和自定义同步](active-directory-aadconnectsync-whatis.md)
- [将本地标识与 Azure Active Directory 集成](active-directory-aadconnect.md)

**参考主题**

- [Azure AD Connect 同步：函数引用](active-directory-aadconnectsync-functions-reference.md)


<!-- Update_Description: update metedata properties -->