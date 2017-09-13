---
title: "Azure API 管理策略表达式 | Azure"
description: "了解 Azure API 管理中的策略表达式。"
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/09/2017
ms.author: v-yiso
ms.date: 
ms.openlocfilehash: 53743c605448049fdec2a28449b090ff391fd432
ms.sourcegitcommit: 81c9ff71879a72bc6ff58017867b3eaeb1ba7323
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2017
---
# <a name="api-management-policy-expressions"></a>API 管理策略表达式
策略表达式语法为 C# 6.0 版。 每个表达式都可以访问隐式提供的[上下文](./api-management-policy-expressions.md#ContextVariables)变量以及允许的 .NET Framework 类型[子集](./api-management-policy-expressions.md#CLRTypes)。  
  
 
>  -   10:30 - 了解如何应用 API 级策略，以便使用[设置查询字符串参数](./api-management-transformation-policies.md#SetQueryStringParameter)和[设置 HTTP 标头](./api-management-transformation-policies.md#SetHTTPheader)策略将上下文信息提供给后端服务。 在 12:10 处有一个演示，演示了如何在开发人员门户中调用操作，以便在其中查看这些生效的策略。  
> -   13:50 - 了解如何使用[验证 JWT](./api-management-access-restriction-policies.md#ValidateJWT) 策略根据令牌声明预先授予操作访问权限。 快进到 15:00，观看在策略编辑器中配置的策略，然后快进到 18:50，观看对使用和不使用所需授权令牌从开发人员门户调用操作的演示。  
> -   21:00 - 了解如何使用 [API 检查器](./api-management-howto-api-inspector.md)跟踪查看策略求值方法和求值结果。  
> -   25:25 - 了解如何针对[从缓存获取](./api-management-caching-policies.md#GetFromCache)和[存储到缓存](./api-management-caching-policies.md#StoreToCache)策略使用策略表达式，以便配置 API 管理响应缓存持续时间，使之匹配由后端服务的 `Cache-Control` 指令指定的后端服务响应缓存。  
> -   34:30 - 了解如何进行内容筛选，所用方法为删除从后端服务接收的响应中的数据元素，所用策略为[控制流](./api-management-advanced-policies.md#choose)和[设置正文](./api-management-transformation-policies.md#SetBody)。 若要大致了解用于此演示的 [Dark Sky Forecast API](https://developer.forecast.io/)，请从 31:50 开始观看。  
> -   若要下载此视频中使用的策略语句，请参阅 [api-management-samples/policies](https://github.com/Azure/api-management-samples/tree/master/policies) github 存储库。  
  
  
##  <a name="Syntax"></a> 语法  
 单一语句表达式括在 `@(expression)` 中，其中 `expression` 是格式正确的 C# 表达式语句。  
  
 多语句表达式括在 `@{expression}` 中。 多语句表达式中的所有代码路径必须以 `return` 语句结尾。  
  
##  <a name="PolicyExpressionsExamples"></a> 示例  
  
```  
@(true)  
  
@((1+1).ToString())  
  
@("Hi There".Length)  
  
@(Regex.Match(context.Response.Headers.GetValueOrDefault("Cache-Control",""), @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value)  
  
@(context.Variables.ContainsKey("maxAge") ? int.Parse((string)context.Variables["maxAge"]) : 3600)  
  
@{   
  string value;   
  if (context.Request.Headers.TryGetValue("Authorization", out value))   
  {   
    return Encoding.UTF8.GetString(Convert.FromBase64String(value));  
  }   
  else   
  {   
    return null;  
  }  
}  
```  
  
##  <a name="PolicyExpressionsUsage"></a> 用法  
 在任何 API 管理[策略](./api-management-policies.md)中，表达式都可以用作属性值或文本值，除非策略引用另行指定。  
  
> [!IMPORTANT]
>  请注意，在使用策略表达式对策略进行定义时，只能对策略表达式进行有限的验证。 由于表达式是在运行时的入站或出站管道中通过网关执行的，因此只要策略表达式生成了运行时异常，就会在 API 调用中出现运行时错误。  
  
##  <a name="CLRTypes"></a> 策略表达式中允许的 .NET Framework 类型  
 下表列出了策略表达式中允许的 .NET Framework 类型及其成员。  
  
|CLR 类型|支持的方法|  
|--------------|-----------------------|  
|Newtonsoft.Json.Linq.Extensions|支持所有方法|  
|Newtonsoft.Json.Linq.JArray|支持所有方法|  
|Newtonsoft.Json.Linq.JConstructor|支持所有方法|  
|Newtonsoft.Json.Linq.JContainer|支持所有方法|  
|Newtonsoft.Json.Linq.JObject|支持所有方法|  
|Newtonsoft.Json.Linq.JProperty|支持所有方法|  
|Newtonsoft.Json.Linq.JRaw|支持所有方法|  
|Newtonsoft.Json.Linq.JToken|支持所有方法|  
|Newtonsoft.Json.Linq.JTokenType|支持所有方法|  
|Newtonsoft.Json.Linq.JValue|支持所有方法|  
|System.Collections.Generic.IReadOnlyCollection<T\>|全部|  
|System.Collections.Generic.IReadOnlyDictionary<TKey,  TValue>|全部|  
|System.Collections.Generic.ISet<TKey, TValue>|全部|  
|System.Collections.Generic.KeyValuePair<TKey,  TValue>|键、值|  
|System.Collections.Generic.List<TKey, TValue>|全部|  
|System.Collections.Generic.Queue<TKey, TValue>|全部|  
|System.Collections.Generic.Stack<TKey, TValue>|全部|  
|System.Convert|全部|  
|System.DateTime|全部|  
|System.DateTimeKind|Utc|  
|System.DateTimeOffset|全部|  
|System.Decimal|全部|  
|System.Double|全部|  
|System.Guid|全部|  
|System.IEnumerable<T\>|全部|  
|System.IEnumerator<T\>|全部|  
|System.Int16|全部|  
|System.Int32|全部|  
|System.Int64|全部|  
|System.Linq.Enumerable<T\>|支持所有方法|  
|System.Math|全部|  
|System.MidpointRounding|全部|  
|System.Nullable<T\>|全部|  
|System.Random|全部|  
|System.SByte|全部|  
|System.Security.Cryptography. HMACSHA384|全部|  
|System.Security.Cryptography. HMACSHA512|全部|  
|System.Security.Cryptography.HashAlgorithm|全部|  
|System.Security.Cryptography.HMAC|全部|  
|System.Security.Cryptography.HMACMD5|全部|  
|System.Security.Cryptography.HMACSHA1|全部|  
|System.Security.Cryptography.HMACSHA256|全部|  
|System.Security.Cryptography.KeyedHashAlgorithm|全部|  
|System.Security.Cryptography.MD5|全部|  
|System.Security.Cryptography.RNGCryptoServiceProvider|全部|  
|System.Security.Cryptography.SHA1|全部|  
|System.Security.Cryptography.SHA1Managed|全部|  
|System.Security.Cryptography.SHA256|全部|  
|System.Security.Cryptography.SHA256Managed|全部|  
|System.Security.Cryptography.SHA384|全部|  
|System.Security.Cryptography.SHA384Managed|全部|  
|System.Security.Cryptography.SHA512|全部|  
|System.Security.Cryptography.SHA512Managed|全部|  
|System.Single|全部|  
|System.String|全部|  
|System.StringSplitOptions|全部|  
|System.Text.Encoding|全部|  
|System.Text.RegularExpressions.Capture|Index、Length、Value|  
|System.Text.RegularExpressions.CaptureCollection|Count、Item|  
|System.Text.RegularExpressions.Group|Captures、Success|  
|System.Text.RegularExpressions.GroupCollection|Count、Item|  
|System.Text.RegularExpressions.Match|Empty、Groups、Result|  
|System.Text.RegularExpressions.Regex|.ctor、IsMatch、Match、Matches、Replace|  
|System.Text.RegularExpressions.RegexOptions|Compiled、IgnoreCase、IgnorePatternWhitespace、Multiline、None、RightToLeft、Singleline|  
|System.TimeSpan|全部|  
|System.Tuple|全部|  
|System.UInt16|全部|  
|System.UInt32|全部|  
|System.UInt64|全部|  
|System.Uri|全部|  
|System.Xml.Linq.Extensions|支持所有方法|  
|System.Xml.Linq.XAttribute|支持所有方法|  
|System.Xml.Linq.XCData|支持所有方法|  
|System.Xml.Linq.XComment|支持所有方法|  
|System.Xml.Linq.XContainer|支持所有方法|  
|System.Xml.Linq.XDeclaration|支持所有方法|  
|System.Xml.Linq.XDocument|支持所有方法|  
|System.Xml.Linq.XDocumentType|支持所有方法|  
|System.Xml.Linq.XElement|支持所有方法|  
|System.Xml.Linq.XName|支持所有方法|  
|System.Xml.Linq.XNamespace|支持所有方法|  
|System.Xml.Linq.XNode|支持所有方法|  
|System.Xml.Linq.XNodeDocumentOrderComparer|支持所有方法|  
|System.Xml.Linq.XNodeEqualityComparer|支持所有方法|  
|System.Xml.Linq.XObject|支持所有方法|  
|System.Xml.Linq.XProcessingInstruction|支持所有方法|  
|System.Xml.Linq.XText|支持所有方法|  
|System.Xml.XmlNodeType|全部|  
  
##  <a name="ContextVariables"></a> 上下文变量  
 在每个策略[表达式](./api-management-policy-expressions.md#Syntax)中均可隐式使用名为 `context` 的变量。 其成员提供与 `\request` 相关的信息。 所有 `context` 成员均为只读的。  
  
|上下文变量|允许的方法、属性和参数值|  
|----------------------|-------------------------------------------------------|  
|context|Api：IApi<br /><br /> Deployment<br /><br /> LastError<br /><br /> Operation<br /><br /> Product<br /><br /> Request<br /><br /> RequestId：string<br /><br /> Response<br /><br /> Subscription<br /><br /> Tracing：bool<br /><br /> User<br /><br /> Variables:IReadOnlyDictionary<string, object><br /><br /> void Trace(message：string)|  
|context.Api|Id：string<br /><br /> Name：string<br /><br /> Path：string<br /><br /> ServiceUrl：IUrl|  
|context.Deployment|Region：string<br /><br /> ServiceName：string|  
|context.LastError|Source：string<br /><br /> Reason：string<br /><br /> Message：string<br /><br /> Scope：string<br /><br /> Section：string<br /><br /> Path：string<br /><br /> PolicyId：string<br /><br /> 有关 context.LastError 的详细信息，请参阅[错误处理](./api-management-error-handling-policies.md)。|  
|context.Operation|Id：string<br /><br /> Method：string<br /><br /> Name：string<br /><br /> UrlTemplate：string|  
|context.Product|Apis：IEnumerable<IApi\><br /><br /> ApprovalRequired：bool<br /><br /> Groups：IEnumerable<IGroup\><br /><br /> Id：string<br /><br /> Name：string<br /><br /> State：enum ProductState {NotPublished, Published}<br /><br /> SubscriptionLimit：int?<br /><br /> SubscriptionRequired：bool|  
|context.Request|Body：IMessageBody<br /><br /> Certificate：System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Headers：IReadOnlyDictionary<string, string[]><br /><br /> IpAddress：string<br /><br /> MatchedParameters：IReadOnlyDictionary<string, string><br /><br /> Method：string<br /><br /> OriginalUrl：IUrl<br /><br /> Url：IUrl|  
|string context.Request.Headers.GetValueOrDefault(headerName：string, defaultValue：string)|headerName：string<br /><br /> defaultValue：string<br /><br /> 如果找不到标头，则返回逗号分隔的请求标头值或 `defaultValue`。|  
|context.Response|Body：IMessageBody<br /><br /> Headers：IReadOnlyDictionary<string, string[]><br /><br /> StatusCode：int<br /><br /> StatusReason：string|  
|string context.Response.Headers.GetValueOrDefault(headerName：string, defaultValue：string)|headerName：string<br /><br /> defaultValue：string<br /><br /> 如果找不到标头，则返回逗号分隔的响应标头值或 `defaultValue`。|  
|context.Subscription|CreatedTime：DateTime<br /><br /> EndDate：DateTime?<br /><br /> Id：string<br /><br /> Key：string<br /><br /> Name：string<br /><br /> PrimaryKey：string<br /><br /> SecondaryKey：string<br /><br /> StartDate：DateTime?|  
|context.User|Email：string<br /><br /> FirstName：string<br /><br /> Groups：IEnumerable<IGroup\><br /><br /> Id：string<br /><br /> Identities：IEnumerable<IUserIdentity\><br /><br /> LastName：string<br /><br /> Note：string<br /><br /> RegistrationDate：DateTime|  
|IApi|Id：string<br /><br /> Name：string<br /><br /> Path：string<br /><br /> Protocols：IEnumerable<string\><br /><br /> ServiceUrl：IUrl<br /><br /> SubscriptionKeyParameterNames：ISubscriptionKeyParameterNames|  
|IGroup|Id：string<br /><br /> Name：string|  
|IMessageBody|As<T\>(preserveContent：bool = false)：其中 T 为：string、JObject、JToken、JArray、XNode、XElement、XDocument<br /><br /> `context.Request.Body.As<T>` 和 `context.Response.Body.As<T>` 方法用于以指定的类型 `T` 读取请求和响应消息正文。 该方法默认使用原始消息正文流，并在返回后将其呈现为不可用。 要通过让该方法在正文流的副本上执行操作而避免这种情况，请将 `preserveContent` 参数设置为 `true`。 请转到[此处](./api-management-transformation-policies.md#SetBody)查看示例。|  
|IUrl|Host：string<br /><br /> Path：string<br /><br /> Port：int<br /><br /> Query：IReadOnlyDictionary<string, string[]><br /><br /> QueryString：string<br /><br /> Scheme：string|  
|IUserIdentity|Id：string<br /><br /> Provider：string|  
|ISubscriptionKeyParameterNames|Header：string<br /><br /> Query：string|  
|string IUrl.Query.GetValueOrDefault(queryParameterName：string, defaultValue：string)|queryParameterName：string<br /><br /> defaultValue：string<br /><br /> 如果找不到参数，则会返回逗号分隔的查询参数值或 `defaultValue`。|  
|T context.Variables.GetValueOrDefault<T\>(variableName：string, defaultValue：T)|variableName：string<br /><br /> defaultValue：T<br /><br /> 如果找不到变量，则会返回强制转换为 `T` 或 `defaultValue` 类型的变量值。<br /><br /> 如果指定的类型与已返回变量的实际类型不符，此方法会引发异常。|  
|BasicAuthCredentials AsBasic(input：this string)|input：string<br /><br /> 如果输入参数包含有效的 HTTP Basic Authentication 授权请求标头值，此方法会返回类型为 `BasicAuthCredentials` 的对象；否则，此方法会返回 null。|  
|bool TryParseBasic(input：this string, result：out BasicAuthCredentials)|input：string<br /><br /> result：out BasicAuthCredentials<br /><br /> 如果输入参数包含有效的 HTTP Basic Authentication 授权请求标头值，此方法会返回 `true` 且结果参数会包含类型为 `BasicAuthCredentials` 的值；否则，此方法会返回 `false`。|  
|BasicAuthCredentials|Password：string<br /><br /> UserId：string|  
|Jwt AsJwt(input：this string)|input：string<br /><br /> 如果输入参数包含有效的 JWT 令牌值，此方法会返回类型为 `Jwt` 的对象；否则，此方法会返回 `null`。|  
|bool TryParseJwt(input：this string, result：out Jwt)|input：string<br /><br /> result：out Jwt<br /><br /> 如果输入参数包含有效的 JWT 令牌值，此方法会返回 `true` 且结果参数包含类型为 `Jwt` 的值；否则，此方法会返回 `false`。|  
|Jwt|Algorithm：string<br /><br /> Audience：IEnumerable<string\><br /><br /> Claims：IReadOnlyDictionary<string, string[]><br /><br /> ExpirationTime：DateTime?<br /><br /> Id：string<br /><br /> Issuer：string<br /><br /> NotBefore：DateTime?<br /><br /> Subject：string<br /><br /> Type：string|  
|string Jwt.Claims.GetValueOrDefault(claimName：string, defaultValue：string)|claimName：string<br /><br /> defaultValue：string<br /><br /> 如果找不到标头，则返回逗号分隔的声明值或 `defaultValue`。|

## <a name="next-steps"></a>后续步骤
有关如何使用策略的详细信息，请参阅 [API 管理中的策略](./api-management-howto-policies.md)。  
