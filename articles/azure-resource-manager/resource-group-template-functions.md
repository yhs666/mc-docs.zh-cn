---
title: 模板函数
description: 介绍在 Azure Resource Manager 模板中检索值、处理字符串和数字以及检索部署信息时所用的函数。
ms.topic: conceptual
origin.date: 11/19/2019
ms.date: 12/09/2019
ms.openlocfilehash: 3fbff1eee2fe8d1b85b4451340712acccbef3343
ms.sourcegitcommit: cf73284534772acbe7a0b985a86a0202bfcc109e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74885000"
---
# <a name="azure-resource-manager-template-functions"></a>Azure Resource Manager 模板函数

本文介绍可以在 Azure 资源管理器模板中使用的所有函数。 若要了解如何在模板中使用函数，请参阅[模板语法](template-expressions.md)。

若要创建自己的函数，请参阅[用户定义函数](resource-group-authoring-templates.md#functions)。

<a name="array" aria-hidden="true" />
<a name="coalesce" aria-hidden="true" />
<a name="concatarray" aria-hidden="true" />
<a name="contains" aria-hidden="true" />
<a name="createarray" aria-hidden="true" />
<a name="empty" aria-hidden="true" />
<a name="first" aria-hidden="true" />
<a name="intersection" aria-hidden="true" />
<a name="json" aria-hidden="true" />
<a name="last" aria-hidden="true" />
<a name="length" aria-hidden="true" />
<a name="min" aria-hidden="true" />
<a name="max" aria-hidden="true" />
<a name="range" aria-hidden="true" />
<a name="skip" aria-hidden="true" />
<a name="take" aria-hidden="true" />
<a name="union" aria-hidden="true" />

## <a name="array-and-object-functions"></a>数组和对象函数
Resource Manager 提供以下用于处理数组和对象的函数。

* [array](resource-group-template-functions-array.md#array)
* [coalesce](resource-group-template-functions-array.md#coalesce)
* [concat](resource-group-template-functions-array.md#concat)
* [contains](resource-group-template-functions-array.md#contains)
* [createArray](resource-group-template-functions-array.md#createarray)
* [empty](resource-group-template-functions-array.md#empty)
* [first](resource-group-template-functions-array.md#first)
* [intersection](resource-group-template-functions-array.md#intersection)
* [json](resource-group-template-functions-array.md#json)
* [last](resource-group-template-functions-array.md#last)
* [length](resource-group-template-functions-array.md#length)
* [min](resource-group-template-functions-array.md#min)
* [max](resource-group-template-functions-array.md#max)
* [range](resource-group-template-functions-array.md#range)
* [skip](resource-group-template-functions-array.md#skip)
* [take](resource-group-template-functions-array.md#take)
* [union](resource-group-template-functions-array.md#union)

<a name="equals" aria-hidden="true" />
<a name="less" aria-hidden="true" />
<a name="lessorequals" aria-hidden="true" />
<a name="greater" aria-hidden="true" />
<a name="greaterorequals" aria-hidden="true" />

## <a name="comparison-functions"></a>比较函数
Resource Manager 提供了多个用于在模板中进行比较的函数。

* [equals](resource-group-template-functions-comparison.md#equals)
* [less](resource-group-template-functions-comparison.md#less)
* [lessOrEquals](resource-group-template-functions-comparison.md#lessorequals)
* [greater](resource-group-template-functions-comparison.md#greater)
* [greaterOrEquals](resource-group-template-functions-comparison.md#greaterorequals)

<a name="deployment" aria-hidden="true" />
<a name="parameters" aria-hidden="true" />
<a name="variables" aria-hidden="true" />

## <a name="deployment-value-functions"></a>部署值函数
Resource Manager 提供以下函数，用于从与部署相关的模板和值部分获取值：

* [部署](resource-group-template-functions-deployment.md#deployment)
* [环境](resource-group-template-functions-deployment.md#environment)
* [参数](resource-group-template-functions-deployment.md#parameters)
* [variables](resource-group-template-functions-deployment.md#variables)

<a name="and" aria-hidden="true" />
<a name="bool" aria-hidden="true" />
<a name="if" aria-hidden="true" />
<a name="not" aria-hidden="true" />
<a name="or" aria-hidden="true" />

## <a name="logical-functions"></a>逻辑函数
资源管理器提供以下用于处理逻辑条件的函数：

* [and](resource-group-template-functions-logical.md#and)
* [bool](resource-group-template-functions-logical.md#bool)
* [if](resource-group-template-functions-logical.md#if)
* [not](resource-group-template-functions-logical.md#not)
* [or](resource-group-template-functions-logical.md#or)

<a name="add" aria-hidden="true" />
<a name="copyindex" aria-hidden="true" />
<a name="div" aria-hidden="true" />
<a name="float" aria-hidden="true" />
<a name="int" aria-hidden="true" />
<a name="minint" aria-hidden="true" />
<a name="maxint" aria-hidden="true" />
<a name="mod" aria-hidden="true" />
<a name="mul" aria-hidden="true" />
<a name="sub" aria-hidden="true" />

## <a name="numeric-functions"></a>数值函数
Resource Manager 提供以下用于处理整数的函数：

* [添加](resource-group-template-functions-numeric.md#add)
* [copyIndex](resource-group-template-functions-numeric.md#copyindex)
* [div](resource-group-template-functions-numeric.md#div)
* [float](resource-group-template-functions-numeric.md#float)
* [int](resource-group-template-functions-numeric.md#int)
* [min](resource-group-template-functions-numeric.md#min)
* [max](resource-group-template-functions-numeric.md#max)
* [mod](resource-group-template-functions-numeric.md#mod)
* [mul](resource-group-template-functions-numeric.md#mul)
* [sub](resource-group-template-functions-numeric.md#sub)

<a name="extensionResourceId" aria-hidden="true" />
<a name="listkeys" aria-hidden="true" />
<a name="list" aria-hidden="true" />
<a name="providers" aria-hidden="true" />
<a name="reference" aria-hidden="true" />
<a name="resourcegroup" aria-hidden="true" />
<a name="resourceid" aria-hidden="true" />
<a name="subscription" aria-hidden="true" />
<a name="subscriptionResourceId" aria-hidden="true" />
<a name="tenantResourceId" aria-hidden="true" />

## <a name="resource-functions"></a>Resource functions
Resource Manager 提供以下用于获取资源值的函数：

* [extensionResourceId](resource-group-template-functions-resource.md#extensionresourceid)
* [listAccountSas](resource-group-template-functions-resource.md#list)
* [listKeys](resource-group-template-functions-resource.md#listkeys)
* [listSecrets](resource-group-template-functions-resource.md#list)
* [list*](resource-group-template-functions-resource.md#list)
* [providers](resource-group-template-functions-resource.md#providers)
* [reference](resource-group-template-functions-resource.md#reference)
* [resourceGroup](resource-group-template-functions-resource.md#resourcegroup)
* [resourceId](resource-group-template-functions-resource.md#resourceid)
* [subscription](resource-group-template-functions-resource.md#subscription)
* [subscriptionResourceId](resource-group-template-functions-resource.md#subscriptionresourceid)
* [tenantResourceId](resource-group-template-functions-resource.md#tenantresourceid)

<a name="base64" aria-hidden="true" />
<a name="base64tojson" aria-hidden="true" />
<a name="base64tostring" aria-hidden="true" />
<a name="concat" aria-hidden="true" />
<a name="containsstring" aria-hidden="true" />
<a name="datauri" aria-hidden="true" />
<a name="datauritostring" aria-hidden="true" />
<a name="emptystring" aria-hidden="true" />
<a name="endswith" aria-hidden="true" />
<a name="firststring" aria-hidden="true" />
<a name="guid" aria-hidden="true" />
<a name="indexof" aria-hidden="true" />
<a name="laststring" aria-hidden="true" />
<a name="lastindexof" aria-hidden="true" />
<a name="lengthstring" aria-hidden="true" />
<a name="padleft" aria-hidden="true" />
<a name="replace" aria-hidden="true" />
<a name="skipstring" aria-hidden="true" />
<a name="split" aria-hidden="true" />
<a name="startswith" aria-hidden="true" />
<a name="string" aria-hidden="true" />
<a name="substring" aria-hidden="true" />
<a name="takestring" aria-hidden="true" />
<a name="tolower" aria-hidden="true" />
<a name="toupper" aria-hidden="true" />
<a name="trim" aria-hidden="true" />
<a name="uniquestring" aria-hidden="true" />
<a name="uri" aria-hidden="true" />
<a name="uricomponent" aria-hidden="true" />
<a name="uricomponenttostring" aria-hidden="true" />

## <a name="string-functions"></a>字符串函数
Resource Manager 提供以下用于处理字符串的函数：

* [base64](resource-group-template-functions-string.md#base64)
* [base64ToJson](resource-group-template-functions-string.md#base64tojson)
* [base64ToString](resource-group-template-functions-string.md#base64tostring)
* [concat](resource-group-template-functions-string.md#concat)
* [contains](resource-group-template-functions-string.md#contains)
* [dataUri](resource-group-template-functions-string.md#datauri)
* [dataUriToString](resource-group-template-functions-string.md#datauritostring)
* [empty](resource-group-template-functions-string.md#empty)
* [endsWith](resource-group-template-functions-string.md#endswith)
* [first](resource-group-template-functions-string.md#first)
* [format](resource-group-template-functions-string.md#format)
* [guid](resource-group-template-functions-string.md#guid)
* [indexOf](resource-group-template-functions-string.md#indexof)
* [last](resource-group-template-functions-string.md#last)
* [lastIndexOf](resource-group-template-functions-string.md#lastindexof)
* [length](resource-group-template-functions-string.md#length)
* [newGuid](resource-group-template-functions-string.md#newguid)
* [padLeft](resource-group-template-functions-string.md#padleft)
* [replace](resource-group-template-functions-string.md#replace)
* [skip](resource-group-template-functions-string.md#skip)
* [split](resource-group-template-functions-string.md#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [string](resource-group-template-functions-string.md#string)
* [substring](resource-group-template-functions-string.md#substring)
* [take](resource-group-template-functions-string.md#take)
* [toLower](resource-group-template-functions-string.md#tolower)
* [toUpper](resource-group-template-functions-string.md#toupper)
* [trim](resource-group-template-functions-string.md#trim)
* [uniqueString](resource-group-template-functions-string.md#uniquestring)
* [uri](resource-group-template-functions-string.md#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)
* [utcNow](resource-group-template-functions-string.md#utcnow)

## <a name="next-steps"></a>后续步骤

* 有关 Azure Resource Manager 模板中各部分的说明，请参阅 [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md)（创作 Azure Resource Manager 模板）
* 要合并多个模板，请参阅[将链接的模板与 Azure 资源管理器配合使用](resource-group-linked-templates.md)
* 若要在创建资源类型时迭代指定的次数，请参阅 [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)（在 Azure Resource Manager 中创建多个资源实例）
* 若要查看如何部署已创建的模板，请参阅[使用 Azure 资源管理器模板部署应用程序](resource-group-template-deploy.md)

<!--Update_Description: update meta properties, update link, wording update -->