---
title: Resource Manager 模板函数 | Azure
description: 介绍在 Azure Resource Manager 模板中检索值、处理字符串和数字以及检索部署信息时所用的函数。
services: azure-resource-manager
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: 0644abe1-abaa-443d-820d-1966d7d26bfd
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 03/05/2019
ms.date: 03/18/2019
ms.author: v-yeche
ms.openlocfilehash: 73f9fcbe0ea7cc9f74262804c277e364c4e51126
ms.sourcegitcommit: edce097f471b6e9427718f0641ee2b421e3c0ed2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348175"
---
# <a name="azure-resource-manager-template-functions"></a>Azure Resource Manager 模板函数
本文介绍可以在 Azure 资源管理器模板中使用的所有函数。 若要了解如何在模板中使用函数，请参阅[模板语法](resource-group-authoring-templates.md#syntax)。

若要创建自己的函数，请参阅[用户定义函数](resource-group-authoring-templates.md#functions)。

<a name="array" />
<a name="coalesce" />
<a name="concatarray" />
<a name="contains" />
<a name="createarray" />
<a name="empty" />
<a name="first" />
<a name="intersection" />
<a name="json" />
<a name="last" />
<a name="length" />
<a name="min" />
<a name="max" />
<a name="range" />
<a name="skip" />
<a name="take" />
<a name="union" />

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

<a name="equals" />
<a name="less" />
<a name="lessorequals" />
<a name="greater" />
<a name="greaterorequals" />

## <a name="comparison-functions"></a>比较函数
Resource Manager 提供了多个用于在模板中进行比较的函数。

* [equals](resource-group-template-functions-comparison.md#equals)
* [less](resource-group-template-functions-comparison.md#less)
* [lessOrEquals](resource-group-template-functions-comparison.md#lessorequals)
* [greater](resource-group-template-functions-comparison.md#greater)
* [greaterOrEquals](resource-group-template-functions-comparison.md#greaterorequals)

<a name="deployment" />
<a name="parameters" />
<a name="variables" />

## <a name="deployment-value-functions"></a>部署值函数
Resource Manager 提供以下函数，用于从与部署相关的模板和值部分获取值：

* [部署](resource-group-template-functions-deployment.md#deployment)
* [参数](resource-group-template-functions-deployment.md#parameters)
* [variables](resource-group-template-functions-deployment.md#variables)

<a name="and" />
<a name="bool" />
<a name="if" />
<a name="not" />
<a name="or" />

## <a name="logical-functions"></a>逻辑函数
资源管理器提供以下用于处理逻辑条件的函数：

* [and](resource-group-template-functions-logical.md#and)
* [bool](resource-group-template-functions-logical.md#bool)
* [if](resource-group-template-functions-logical.md#if)
* [not](resource-group-template-functions-logical.md#not)
* [or](resource-group-template-functions-logical.md#or)

<a name="add" />
<a name="copyindex" />
<a name="div" />
<a name="float" />
<a name="int" />
<a name="minint" />
<a name="maxint" />
<a name="mod" />
<a name="mul" />
<a name="sub" />

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

<a name="listkeys" />
<a name="list" />
<a name="providers" />
<a name="reference" />
<a name="resourcegroup" />
<a name="resourceid" />
<a name="subscription" />

## <a name="resource-functions"></a>Resource functions
Resource Manager 提供以下用于获取资源值的函数：

* [listAccountSas](resource-group-template-functions-resource.md#list)
* [listKeys](resource-group-template-functions-resource.md#listkeys)
* [listSecrets](resource-group-template-functions-resource.md#list)
* [list*](resource-group-template-functions-resource.md#list)
* [providers](resource-group-template-functions-resource.md#providers)
* [reference](resource-group-template-functions-resource.md#reference)
* [resourceGroup](resource-group-template-functions-resource.md#resourcegroup)
* [resourceId](resource-group-template-functions-resource.md#resourceid)
* [subscription](resource-group-template-functions-resource.md#subscription)

<a name="base64" />
<a name="base64tojson" />
<a name="base64tostring" />
<a name="concat" />
<a name="containsstring" />
<a name="datauri" />
<a name="datauritostring" />
<a name="emptystring" />
<a name="endswith" />
<a name="firststring" />
<a name="guid" />
<a name="indexof" />
<a name="laststring" />
<a name="lastindexof" />
<a name="lengthstring" />
<a name="padleft" />
<a name="replace" />
<a name="skipstring" />
<a name="split" />
<a name="startswith" />
<a name="string" />
<a name="substring" />
<a name="takestring" />
<a name="tolower" />
<a name="toupper" />
<a name="trim" />
<a name="uniquestring" />
<a name="uri" />
<a name="uricomponent" />
<a name="uricomponenttostring" />

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
* [字符串](resource-group-template-functions-string.md#string)
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