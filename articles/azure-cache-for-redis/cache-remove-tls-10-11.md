---
title: 删除 Azure Cache for Redis 中使用的 TLS 1.0 和 1.1 | Microsoft Docs
description: 了解在与 Azure Cache for Redis 通信时如何从应用程序中删除 TLS 1.0 和 1.1
services: cache
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
origin.date: 10/22/2019
ms.date: 11/11/2019
ms.author: v-junlch
ms.openlocfilehash: da70910eacfab87ecb5b46ac32e39b14f4a751d4
ms.sourcegitcommit: 40a58a8b9be0c825c03725802e21ed47724aa7d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2019
ms.locfileid: "73934412"
---
# <a name="remove-use-of-tls-10-and-11-with-azure-cache-for-redis"></a>删除 Azure Cache for Redis 中使用的 TLS 1.0 和 1.1

整个行业正在大力推广 TLS 1.2 或更高版本。 TLS 版本 1.0 和 1.1 已知很容易遭到 BEAST 和 POODLE 之类的攻击，并且存在其他常见漏洞和披露 (CVE) 弱点。 此外，它们不支持 PCI 合规性标准推荐的新式加密法和加密套件。 这篇 [TLS 安全性博客文章](https://www.acunetix.com/blog/articles/tls-vulnerabilities-attacks-final-part/)详细说明了其中的一些漏洞。

尽管 TLS 1.0 和 1.1 眼前并未出现任何问题，但你仍应该考虑尽快从这两个版本迁移。 从 2020 年 3 月 31 日开始，Azure Cache for Redis 将停止支持这些 TLS 版本。 在此日期之后，应用程序至少需要使用 TLS 1.2 才能与缓存通信。

本文提供有关如何检测这些依赖项并从应用程序中将其删除的一般指导。

## <a name="check-if-your-application-is-already-compliant"></a>检查应用程序是否已合规

确定应用程序是否能够使用 TLS 1.2 的最简单方法是，在应用程序使用的测试或过渡缓存中将“最低 TLS 版本”设置为 TLS 1.2。 可以在 Azure 门户上的缓存实例的[高级设置](cache-configure.md#advanced-settings)中找到“最低 TLS 版本”设置。 如果做出此项更改后，应用程序可继续按预期方式运行，则应用程序很可能是合规的。 可能需要专门将应用程序使用的某些 Redis 客户端库配置为启用 TLS 1.2，才能通过该安全协议连接到 Azure Cache for Redis。

## <a name="configure-your-application-to-use-tls-12"></a>将应用程序配置为使用 TLS 1.2

大多数应用程序利用 Redis 客户端库来处理与缓存的通信。 下面说明了如何将以各种编程语言和框架编写的某些流行客户端库配置为使用 TLS 1.2。

### <a name="net-framework"></a>.NET framework

在 .NET Framework 4.5.2 或更低版本上，Redis .NET 客户端默认使用最低的 TLS 版本；在 4.6 或更高版本上，则使用最高的 TLS 版本。 如果使用较低版本的 .NET Framework，可以手动启用 TLS 1.2：

* StackExchange.Redis：在连接字符串中设置 `ssl=true` 和 `sslprotocls=tls12`。
* ServiceStack.Redis：根据[这些说明](https://github.com/ServiceStack/ServiceStack.Redis/pull/247)操作。

### <a name="net-core"></a>.NET Core

Redis .NET Core 客户端默认使用最高 TLS 版本。

### <a name="java"></a>Java

在 Java 6 或更低版本上，Redis Java 客户端使用 TLS 1.0。 如果在缓存中禁用了 TLS 1.0，则 Jedis、Lettuce 和 Radisson 无法连接到 Azure Cache for Redis。 目前没有已知的解决方法。

在 Java 7 或更高版本上，Redis 客户端默认不使用 TLS 1.2，但可以配置使用此版本。 Lettuce 和 Radisson 暂时不支持此版本。 如果缓存仅接受 TLS 1.2 连接，它们的工作将会中断。 Jedis 允许使用以下代码片段指定基础 TLS 设置：

``` Java
SSLSocketFactory sslSocketFactory = (SSLSocketFactory) SSLSocketFactory.getDefault();
SSLParameters sslParameters = new SSLParameters();
sslParameters.setEndpointIdentificationAlgorithm("HTTPS");
sslParameters.setProtocols(new String[]{"TLSv1", "TLSv1.1", "TLSv1.2"});
 
URI uri = URI.create("rediss://host:port");
JedisShardInfo shardInfo = new JedisShardInfo(uri, sslSocketFactory, sslParameters, null);
 
shardInfo.setPassword("cachePassword");
 
Jedis jedis = new Jedis(shardInfo);
```

### <a name="nodejs"></a>Node.js

Node Redis 和 IORedis 默认使用 TLS 1.2。

### <a name="php"></a>PHP

PHP 7 上的 Predis 无法正常工作，因为 PHP 7 仅支持 TLS 1.0。 在 PHP 7.2.1 或更低版本上，Predis 默认使用 TLS 1.0 或 1.1。 可以在实例化客户端时指定 TLS 1.2：

``` PHP
$redis=newPredis\Client([
    'scheme'=>'tls',
    'host'=>'host',
    'port'=>6380,
    'password'=>'password',
    'ssl'=>[
        'crypto_type'=>STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT,
    ],
]);
```

在 PHP 7.3 或更高版本上，Predis 使用最新的 TLS 版本。

PhpRedis 不支持任何 PHP 版本上的 TLS。

### <a name="python"></a>Python

Redis-py 默认使用 TLS 1.2。

### <a name="go"></a>前往

Redigo 默认使用 TLS 1.2。

## <a name="additional-information"></a>其他信息

- [如何配置 Azure Cache for Redis](cache-configure.md)

