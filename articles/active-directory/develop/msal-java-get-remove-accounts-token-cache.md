---
title: 使用 MSAL for Java (MSAL4j) 在令牌缓存中获取和删除帐户
titleSuffix: Microsoft identity platform
description: 了解如何使用适用于 Java 的 Microsoft 身份验证库查看和删除令牌缓存中的帐户。
services: active-directory
documentationcenter: dev-center-name
author: sangonzal
manager: henrikm
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 11/07/2019
ms.date: 11/25/2019
ms.author: v-junlch
ms.reviewer: navyasri.canumalla
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 263deaf41378ef439e0c903f5c6cfdf2c2c9576c
ms.sourcegitcommit: 9597d4da8af58009f9cef148a027ccb7b32ed8cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2019
ms.locfileid: "74655472"
---
# <a name="get-and-remove-accounts-from-the-token-cache-using-msal-for-java-msal4j"></a>使用 MSAL for Java (MSAL4j) 在令牌缓存中获取和删除帐户

MSAL4J 默认提供内存中令牌缓存。 内存中令牌缓存的持续时间与应用程序实例的持续时间相同。

## <a name="see-which-accounts-are-in-the-cache"></a>查看哪些帐户在缓存中

如以下示例所示，可以通过调用 `PublicClientApplication.getAccounts()` 查看哪些帐户在缓存中：

```java
PublicClientApplication pca = new PublicClientApplication.Builder(
                labResponse.getAppId()).
                authority(TestConstants.ORGANIZATIONS_AUTHORITY).
                build();

Set<IAccount> accounts = pca.getAccounts().join();
```

## <a name="remove-accounts-from-the-cache"></a>从缓存中删除帐户

如以下示例所示，若要从缓存中删除帐户，请找到需要删除的帐户，然后调用 `PublicClientApplicatoin.removeAccount()`：

```java
Set<IAccount> accounts = pca.getAccounts().join();

IAccount accountToBeRemoved = accounts.stream().filter(
                x -> x.username().equalsIgnoreCase(
                        UPN_OF_USER_TO_BE_REMOVED)).findFirst().orElse(null);

pca.removeAccount(accountToBeRemoved).join();
```

## <a name="learn-more"></a>了解详细信息

如果使用的是适用于 Java 的 MSAL，请了解[适用于 Java 的 MSAL 中的自定义令牌缓存序列化](msal-java-token-cache-serialization.md)。
