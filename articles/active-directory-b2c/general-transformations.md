---
title: Azure Active Directory B2C 的 Identity Experience Framework 架构的常规声明转换示例
description: Azure Active Directory B2C 的 Identity Experience Framework 架构的常规声明转换示例。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
origin.date: 08/27/2019
ms.date: 09/17/2019
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 9c73d89a285e3acef978291d32a922f7dfb7309b
ms.sourcegitcommit: b47a38443d77d11fa5c100d5b13b27ae349709de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2019
ms.locfileid: "71083231"
---
# <a name="general-claims-transformations"></a>常规声明转换

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

本文提供了有关在 Azure Active Directory B2C (Azure AD B2C) 中使用 Identity Experience Framework 架构的常规声明转换的示例。 有关详细信息，请参阅 [ClaimsTransformations](claimstransformations.md)。

## <a name="doesclaimexist"></a>DoesClaimExist

检查 inputClaim  是否存在并将 outputClaim  相应地设置为 true 或 false。

| 项目 | TransformationClaimType | 数据类型 | 注释 |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |任意 | 需要验证是否存在的输入声明。 |
| OutputClaim | outputClaim | 布尔值 | 调用此 ClaimsTransformation 后生成的 ClaimType。 |

使用此声明转换检查声明是否存在或是否包含任何值。 返回值是指示声明是否存在的布尔值。 以下示例检查电子邮件地址是否存在。

```XML
<ClaimsTransformation Id="CheckIfEmailPresent" TransformationMethod="DoesClaimExist">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isEmailPresent" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>示例

- 输入声明：
  - **inputClaim**: someone@contoso.com
- 输出声明：
  - **outputClaim**: true

## <a name="hash"></a>哈希

使用加密盐和机密对提供的纯文本执行哈希。 使用的哈希算法是 SHA-256。

| 项目 | TransformationClaimType | 数据类型 | 注释 |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | 纯文本 | string | 要加密的输入声明 |
| InputClaim | 加密盐 | string | 加密盐参数。 可以使用 `CreateRandomString` 声明转换创建随机值。 |
| InputParameter | randomizerSecret | string | 指向现有的 Azure AD B2C **策略密钥**。 若要创建新策略密钥，请执行以下操作：在 Azure AD B2C 租户的**管理**下，选择 **Identity Experience Framework**。 选择“策略密钥”，以查看租户中的可用密钥。  选择“设置”  （应用程序对象和服务主体对象）。 对于“选项”，请选择“手动”   。 提供名称（可能会自动添加前缀 B2C_1A_  ）。 在“机密”  文本框中，输入要使用的任何机密，如 1234567890。 对于“密钥用法”，请选择“签名”   。 选择“创建”  。 |
| OutputClaim | hash | string | 调用此声明转换后生成的 ClaimType。 在 `plaintext` inputClaim 中配置的声明。 |

```XML
<ClaimsTransformation Id="HashPasswordWithEmail" TransformationMethod="Hash">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="password" TransformationClaimType="plaintext" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="salt" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="randomizerSecret" DataType="string" Value="B2C_1A_AccountTransformSecret" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hashedPassword" TransformationClaimType="hash" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>示例

- 输入声明：
  - **plaintext**: MyPass@word1
  - **加密盐**：487624568
  - **randomizerSecret**：B2C_1A_AccountTransformSecret
- 输出声明：
  - **outputClaim**：CdMNb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U=

<!-- Update_Description: wording update -->