---
title: Resource Manager-sablonok függvényei |} A Microsoft Docs
description: Ismerteti a functions az Azure Resource Manager-sablon használatával értékeket beolvasni, karakterláncok és numerics és telepítési információk lekéréséhez.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0644abe1-abaa-443d-820d-1966d7d26bfd
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/03/2018
ms.author: tomfitz
ms.openlocfilehash: a4a86576b8f9f842c54cfa195305a3e0d0ff4724
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39527619"
---
# <a name="azure-resource-manager-template-functions"></a>Az Azure Resource Manager-sablonfüggvények
Ez a cikk ismerteti az Azure Resource Manager-sablon használható összes funkció áttekintésével.

Funkciók hozzáadása zárójelben kötegfájlokban azokat a sablonokat: `[` és `]`, illetve. A kifejezés lesz kiértékelve, üzembe helyezés során. Ír egy szöveges karakterlánc, míg a során a kifejezés eredménye egy másik JSON típusú, például egy tömb, objektumot vagy egész szám lehet. Csak, például a JavaScript, függvényhívások formázott `functionName(arg1,arg2,arg3)`. A pont és a [index] operátorok használatával tulajdonságok hivatkozik.

Egy kifejezés nem lehet 24,576 karakternél.

Sablon függvényeket és paramétereket és nagybetűk nincsenek megkülönböztetve. Például, oldja fel az erőforrás-kezelő **variables('var1')** és **VARIABLES('VAR1')** ugyanaz, mint. Kiértékelésekor, hacsak a függvény kifejezetten módosítja (például toUpper vagy toLower) eset, a függvény megőrzi az az eset. Előfordulhat, hogy az egyes erőforrástípusok használatieset-követelmények attól függetlenül, hogyan értékeli ki a funkciók.

A saját függvény létrehozásával kapcsolatban lásd: [felhasználó által definiált függvények](resource-group-authoring-templates.md#functions).

<a id="array" />
<a id="coalesce" />
<a id="concatarray" />
<a id="contains" />
<a id="createarray" />
<a id="empty" />
<a id="first" />
<a id="intersection" />
<a id="json" />
<a id="last" />
<a id="length" />
<a id="min" />
<a id="max" />
<a id="range" />
<a id="skip" />
<a id="take" />
<a id="union" />

## <a name="array-and-object-functions"></a>Tömb- és objektumfüggvények
A Resource Manager-tömbök és objektumok használata a számos funkciót biztosít.

* [Pole](resource-group-template-functions-array.md#array)
* [Coalesce](resource-group-template-functions-array.md#coalesce)
* [concat](resource-group-template-functions-array.md#concat)
* [tartalmaz](resource-group-template-functions-array.md#contains)
* [createArray](resource-group-template-functions-array.md#createarray)
* [üres](resource-group-template-functions-array.md#empty)
* [első](resource-group-template-functions-array.md#first)
* [Metszet](resource-group-template-functions-array.md#intersection)
* [json](resource-group-template-functions-array.md#json)
* [utolsó](resource-group-template-functions-array.md#last)
* [Hossza](resource-group-template-functions-array.md#length)
* [Min](resource-group-template-functions-array.md#min)
* [max](resource-group-template-functions-array.md#max)
* [range](resource-group-template-functions-array.md#range)
* [skip](resource-group-template-functions-array.md#skip)
* [hajtsa végre a megfelelő](resource-group-template-functions-array.md#take)
* [Union](resource-group-template-functions-array.md#union)

<a id="equals" />
<a id="less" />
<a id="lessorequals" />
<a id="greater" />
<a id="greaterorequals" />

## <a name="comparison-functions"></a>Összehasonlító függvények
Resource Manager összehasonlítások végzett a sablonok számos funkciót biztosít.

* [egyenlő](resource-group-template-functions-comparison.md#equals)
* [kevesebb](resource-group-template-functions-comparison.md#less)
* [lessOrEquals](resource-group-template-functions-comparison.md#lessorequals)
* [greater](resource-group-template-functions-comparison.md#greater)
* [greaterOrEquals](resource-group-template-functions-comparison.md#greaterorequals)

<a id="deployment" />
<a id="parameters" />
<a id="variables" />

## <a name="deployment-value-functions"></a>Központi telepítési érték funkciók
Erőforrás-kezelő az alábbi funkciókat biztosít értékek lekérése a sablont és az értékek üzembe helyezésével kapcsolatos szakaszait:

* [Üzembe helyezés](resource-group-template-functions-deployment.md#deployment)
* [paraméterek](resource-group-template-functions-deployment.md#parameters)
* [Változók](resource-group-template-functions-deployment.md#variables)

<a id="and" />
<a id="bool" />
<a id="if" />
<a id="not" />
<a id="or" />

## <a name="logical-functions"></a>Logikai függvények
A Resource Manager az alábbi funkciókat biztosít a logikai feltételekkel használatához:

* [és](resource-group-template-functions-logical.md#and)
* [bool](resource-group-template-functions-logical.md#bool)
* [if](resource-group-template-functions-logical.md#if)
* [not](resource-group-template-functions-logical.md#not)
* [vagy](resource-group-template-functions-logical.md#or)

<a id="add" />
<a id="copyindex" />
<a id="div" />
<a id="float" />
<a id="int" />
<a id="minint" />
<a id="maxint" />
<a id="mod" />
<a id="mul" />
<a id="sub" />

## <a name="numeric-functions"></a>Numerikus függvények
A Resource Manager az alábbi funkciókat biztosít az egész számok használata:

* [Hozzáadása](resource-group-template-functions-numeric.md#add)
* [copyIndex](resource-group-template-functions-numeric.md#copyindex)
* [div](resource-group-template-functions-numeric.md#div)
* [float](resource-group-template-functions-numeric.md#float)
* [int](resource-group-template-functions-numeric.md#int)
* [Min](resource-group-template-functions-numeric.md#min)
* [max](resource-group-template-functions-numeric.md#max)
* [MOD](resource-group-template-functions-numeric.md#mod)
* [MUL számú](resource-group-template-functions-numeric.md#mul)
* [sub](resource-group-template-functions-numeric.md#sub)

<a id="listkeys" />
<a id="list" />
<a id="providers" />
<a id="reference" />
<a id="resourcegroup" />
<a id="resourceid" />
<a id="subscription" />

## <a name="resource-functions"></a>Erőforrásfüggvények
Resource Manager az alábbi funkciókat biztosít erőforrás-értékeinek beolvasása:

* [listAccountSas](resource-group-template-functions-resource.md#list)
* [listkeys műveletének](resource-group-template-functions-resource.md#listkeys)
* [listSecrets](resource-group-template-functions-resource.md#list)
* [lista *](resource-group-template-functions-resource.md#list)
* [Szolgáltatók](resource-group-template-functions-resource.md#providers)
* [reference](resource-group-template-functions-resource.md#reference)
* [resourceGroup](resource-group-template-functions-resource.md#resourcegroup)
* [resourceId](resource-group-template-functions-resource.md#resourceid)
* [előfizetést](resource-group-template-functions-resource.md#subscription)

<a id="base64" />
<a id="base64tojson" />
<a id="base64tostring" />
<a id="concat" />
<a id="containsstring" />
<a id="datauri" />
<a id="datauritostring" />
<a id="emptystring" />
<a id="endswith" />
<a id="firststring" />
<a id="guid" />
<a id="indexof" />
<a id="laststring" />
<a id="lastindexof" />
<a id="lengthstring" />
<a id="padleft" />
<a id="replace" />
<a id="skipstring" />
<a id="split" />
<a id="startswith" />
<a id="string" />
<a id="substring" />
<a id="takestring" />
<a id="tolower" />
<a id="toupper" />
<a id="trim" />
<a id="uniquestring" />
<a id="uri" />
<a id="uricomponent" />
<a id="uricomponenttostring" />

## <a name="string-functions"></a>Sztringfüggvények
A Resource Manager az alábbi funkciókat biztosít a karakterláncokkal való munka:

* [base64](resource-group-template-functions-string.md#base64)
* [base64ToJson](resource-group-template-functions-string.md#base64tojson)
* [base64ToString](resource-group-template-functions-string.md#base64tostring)
* [concat](resource-group-template-functions-string.md#concat)
* [tartalmaz](resource-group-template-functions-string.md#contains)
* [dataUri](resource-group-template-functions-string.md#datauri)
* [dataUriToString](resource-group-template-functions-string.md#datauritostring)
* [üres](resource-group-template-functions-string.md#empty)
* [endsWith](resource-group-template-functions-string.md#endswith)
* [első](resource-group-template-functions-string.md#first)
* [guid](resource-group-template-functions-string.md#guid)
* [indexOf](resource-group-template-functions-string.md#indexof)
* [utolsó](resource-group-template-functions-string.md#last)
* [lastIndexOf](resource-group-template-functions-string.md#lastindexof)
* [Hossza](resource-group-template-functions-string.md#length)
* [padLeft](resource-group-template-functions-string.md#padleft)
* [replace](resource-group-template-functions-string.md#replace)
* [skip](resource-group-template-functions-string.md#skip)
* [felosztás](resource-group-template-functions-string.md#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [string](resource-group-template-functions-string.md#string)
* [substring](resource-group-template-functions-string.md#substring)
* [hajtsa végre a megfelelő](resource-group-template-functions-string.md#take)
* [toLower](resource-group-template-functions-string.md#tolower)
* [toUpper](resource-group-template-functions-string.md#toupper)
* [trim](resource-group-template-functions-string.md#trim)
* [uniqueString](resource-group-template-functions-string.md#uniquestring)
* [uri](resource-group-template-functions-string.md#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

## <a name="next-steps"></a>További lépések
* A szakaszok az Azure Resource Manager-sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md)
* Több sablon egyesíteni, lásd: [kapcsolt sablonok használata az Azure Resource Managerrel](resource-group-linked-templates.md)
* A megadott számú alkalommal újrafuttathatja egy adott típusú erőforrás létrehozásakor, lásd: [több erőforráspéldány létrehozása az Azure Resource Manager](resource-group-create-multiple.md)
* Ellenőrizze, hogyan helyezheti üzembe a létrehozott sablont, tekintse meg a [alkalmazás üzembe helyezése Azure Resource Manager-sablonnal](resource-group-template-deploy.md)
