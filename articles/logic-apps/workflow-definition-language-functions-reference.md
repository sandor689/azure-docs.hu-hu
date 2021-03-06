---
title: A munkafolyamat-definíciós nyelv – Azure Logic Apps-hivatkozási függvények |} A Microsoft Docs
description: További információ a logikai alkalmazások létrehozásával a munkafolyamat-definíciós nyelv függvényei
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: reference
ms.date: 04/25/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 46ccf9484b76ec5f24dba470a194b5b83c32f013
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263776"
---
# <a name="functions-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Az Azure Logic Apps munkafolyamat-definíciós nyelv-funkciók dokumentációja

Ez a cikk bemutatja a munkafolyamatok létrehozása automatikus használható függvények [Azure Logic Apps](../logic-apps/logic-apps-overview.md). Logikaialkalmazás-definíciók függvényei kapcsolatos további információkért lásd: [Azure Logic Apps munkafolyamat-definíciós nyelv](../logic-apps/logic-apps-workflow-definition-language.md#functions). 

> [!NOTE]
> A szintaxist a paraméter-definíciók egy kérdőjelet (?), amely akkor jelenik meg, miután egy paraméter azt jelenti, hogy a paraméter nem kötelező megadni. Lásd a [getFutureTime()](#getFutureTime).

<a name="action"></a>

## <a name="action"></a>művelet

Vissza a *aktuális* futtatókörnyezet, illetve értékét más JSON név-érték párok, hozzárendelheti egy kifejezés, amely a következő kimeneti művelet. Alapértelmezés szerint ez a függvény a teljes művelet objektumra hivatkozik, de igény szerint megadhat egy tulajdonság, melynek az értéke. Lásd még: [actions()](../logic-apps/workflow-definition-language-functions-reference.md#actions).

Használhatja a `action()` függvény csak ezen a helyen: 

* A `unsubscribe` tulajdonsága egy webhook művelettel úgy az eredmény az eredeti `subscribe` kérelem
* A `trackedProperties` tulajdonsága egy műveletet
* A `do-until` hurok egy művelet feltételét

```
action()
action().outputs.body.<property> 
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*A tulajdonság*> | Nem | Sztring | A neve, melynek az értéke. a műveleti objektum tulajdonság: **neve**, **startTime**, **endTime**, **bemenetek**,  **outputs**, **állapot**, **kód**, **trackingId**, és **clientTrackingId**. Az Azure Portalon keresse meg ezeket a tulajdonságokat egy adott futtatási előzmények részletes áttekintésével. További információkért lásd: [REST API - munkafolyamat-Futtatás műveletek](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | -----| ----------- | 
| <*művelet – kimenet*> | Sztring | Az aktuális művelet vagy tulajdonság kimenete | 
|||| 

<a name="actionBody"></a>

## <a name="actionbody"></a>actionBody

Egy művelet visszaadandó `body` kimeneti futásidőben. A gyorsírás `actions('<actionName>').outputs.body`. Lásd: [body()](#body) és [actions()](#actions).

```
actionBody('<actionName>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Műveletnév*> | Igen | Sztring | A művelet nevét `body` meg a kívánt kimeneti | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | -----| ----------- | 
| <*művelet – törzs-kimenet*> | Sztring | A `body` a megadott művelet kimenete | 
|||| 

*Példa*

Ez a példa lekéri a `body` a Twitter-művelet kimenete `Get user`: 

```
actionBody('Get_user')
```

És ezt az eredményt adja vissza:

```json
"body": {
  "FullName": "Contoso Corporation",
  "Location": "Generic Town, USA",
  "Id": 283541717,
  "UserName": "ContosoInc",
  "FollowersCount": 172,
  "Description": "Leading the way in transforming the digital workplace.",
  "StatusesCount": 93,
  "FriendsCount": 126,
  "FavouritesCount": 46,
  "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="actionOutputs"></a>

## <a name="actionoutputs"></a>actionOutputs

Futásidőben egy műveletet a hibaüzenettel reagál. A gyorsírás `actions('<actionName>').outputs`. Lásd: [actions()](#actions).

```
actionOutputs('<actionName>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Műveletnév*> | Igen | Sztring | A művelet a nevét, amelyet az kimeneti | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | -----| ----------- | 
| <*Kimenet*> | Sztring | A megadott művelet kimenete | 
|||| 

*Példa*

Ebben a példában a kimeneti olvas be a Twitter-művelet `Get user`: 

```
actionOutputs('Get_user')
```

És ezt az eredményt adja vissza:

```json
{ 
  "statusCode": 200,
  "headers": {
    "Pragma": "no-cache",
    "Vary": "Accept-Encoding",
    "x-ms-request-id": "a916ec8f52211265d98159adde2efe0b",
    "X-Content-Type-Options": "nosniff",
    "Timing-Allow-Origin": "*",
    "Cache-Control": "no-cache",
    "Date": "Mon, 09 Apr 2018 18:47:12 GMT",
    "Set-Cookie": "ARRAffinity=b9400932367ab5e3b6802e3d6158afffb12fcde8666715f5a5fbd4142d0f0b7d;Path=/;HttpOnly;Domain=twitter-wus.azconn-wus.p.azurewebsites.net",
    "X-AspNet-Version": "4.0.30319",
    "X-Powered-By": "ASP.NET",
    "Content-Type": "application/json; charset=utf-8",
    "Expires": "-1",
    "Content-Length": "339"
  },
  "body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
  }
}
```

<a name="actions"></a>

## <a name="actions"></a>műveletek

Egyéb JSON név-érték párok, hozzárendelheti egy kifejezés, amely egy műveleti kimenet runtime vagy az értékek visszaadásához. Alapértelmezés szerint a függvény a teljes művelet objektumra hivatkozik, de igény szerint adjon meg egy tulajdonságot, amely azt szeretné, amelynek értékét. Gyorsírás verziók, lásd: [actionBody()](#actionBody), [actionOutputs()](#actionOutputs), és [body()](#body). Lásd: az aktuális művelet [action()](#action).

> [!NOTE] 
> Korábban, használhatja a `actions()` függvény vagy a `conditions` elem, hogy egy művelet futott alapján a kimenet egy másik műveletet a megadásakor. Explicit módon deklarálni műveletek közötti függőségeket, kell most már használhatja azonban a függő művelet `runAfter` tulajdonság. További információkat talál a `runAfter` tulajdonságot használja, lásd: [feltárhatja és runAfter tulajdonság hibáinak a kezelése](../logic-apps/logic-apps-workflow-definition-language.md).

```
actions('<actionName>')
actions('<actionName>').outputs.body.<property> 
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Műveletnév*> | Igen | Sztring | A művelet kimenete kívánt objektum nevét  | 
| <*A tulajdonság*> | Nem | Sztring | A neve, melynek az értéke. a műveleti objektum tulajdonság: **neve**, **startTime**, **endTime**, **bemenetek**,  **outputs**, **állapot**, **kód**, **trackingId**, és **clientTrackingId**. Az Azure Portalon keresse meg ezeket a tulajdonságokat egy adott futtatási előzmények részletes áttekintésével. További információkért lásd: [REST API - munkafolyamat-Futtatás műveletek](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | -----| ----------- | 
| <*művelet – kimenet*> | Sztring | A megadott művelet nebo vlastnost kimenete | 
|||| 

*Példa*

Ez a példa lekéri a `status` tulajdonság értékét a Twitter-művelet `Get user` futtatáskor: 

```
actions('Get_user').outputs.body.status 
```

És ezt az eredményt adja vissza: `"Succeeded"`

<a name="add"></a>

## <a name="add"></a>hozzáadás

Két szám összeadásának az eredmény visszaadása.

```
add(<summand_1>, <summand_2>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*summand_1*>, <*summand_2*> | Igen | Egész szám, lebegőpontos, vagy a vegyes | A számok hozzáadása | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | -----| ----------- | 
| <*eredmény – összeg*> | Egész vagy lebegőpontos szám | A megadott szám összeadásának eredménye | 
|||| 

*Példa*

Ez a példa hozzáadja a megadott számok:

```
add(1, 1.5)
```

És ezt az eredményt adja vissza: `2.5`

<a name="addDays"></a>

## <a name="adddays"></a>napokHozzaadasa

Adja hozzá a napok számát időbélyeghez.

```
addDays('<timestamp>', <days>, '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*nap*> | Igen | Egész szám | A hozzáadandó napok pozitív vagy negatív szám | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | Az időbélyeg plusz a megadott számú nap  | 
|||| 

*1. példa*

Ebben a példában 10 napot ad hozzá a megadott időbélyeg:

```
addDays('2018-03-15T13:00:00Z', 10)
```

És ezt az eredményt adja vissza: `"2018-03-25T00:00:0000000Z"`

*2. példa*

Ebben a példában kivonja a megadott időbélyeg 5 napos:

```
addDays('2018-03-15T00:00:00Z', -5)
```

És ezt az eredményt adja vissza: `"2018-03-10T00:00:0000000Z"`

<a name="addHours"></a>

## <a name="addhours"></a>addHours

Adja hozzá a órák száma időbélyeget.

```
addHours('<timestamp>', <hours>, '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*Óra*> | Igen | Egész szám | A hozzáadandó órák pozitív vagy negatív szám | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | Az időbélyeg plusz a megadott számú órák  | 
|||| 

*1. példa*

Ebben a példában a megadott időbélyeg 10 órát ad hozzá:

```
addHours('2018-03-15T00:00:00Z', 10)
```

És ezt az eredményt adja vissza: `"2018-03-15T10:00:0000000Z"`

*2. példa*

Ebben a példában kivonja a megadott időbélyeg öt órák:

```
addHours('2018-03-15T15:00:00Z', -5)
```

És ezt az eredményt adja vissza: `"2018-03-15T10:00:0000000Z"`

<a name="addMinutes"></a>

## <a name="addminutes"></a>addMinutes

Adja hozzá a percek száma időbélyeget.

```
addMinutes('<timestamp>', <minutes>, '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*perc*> | Igen | Egész szám | Perc alatt adhatja hozzá az a pozitív vagy negatív szám | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | Az időbélyeg plusz a megadott számú perc | 
|||| 

*1. példa*

Ebben a példában 10 perc hozzáadja a megadott időbélyeg:

```
addMinutes('2018-03-15T00:10:00Z', 10)
```

És ezt az eredményt adja vissza: `"2018-03-15T00:20:00.0000000Z"`

*2. példa*

Ebben a példában öt perc alatt az a megadott időbélyeg kivonja:

```
addMinutes('2018-03-15T00:20:00Z', -5)
```

És ezt az eredményt adja vissza: `"2018-03-15T00:15:00.0000000Z"`

<a name="addProperty"></a>

## <a name="addproperty"></a>addProperty

Egy tulajdonság és az érték, vagy a név-érték párhoz, ad hozzá egy JSON-objektumot, és a frissített objektumot ad vissza. Ha az objektum már létezik futásidőben, a függvény hibát jelez.

```
addProperty(<object>, '<property>', <value>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Objektum*> | Igen | Objektum | A JSON-objektum, amelyen kívánt tulajdonság hozzáadása | 
| <*A tulajdonság*> | Igen | Sztring | A hozzáadandó tulajdonság nevét | 
| <*Érték*> | Igen | Bármelyik | A tulajdonság értéke |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*frissített objektum*> | Objektum | A frissített JSON-objektum és a megadott tulajdonság | 
|||| 

*Példa*

Ebben a példában a `accountNumber` tulajdonságot a `customerProfile` objektum, amely a JSON-t alakítja át a [JSON()](#json) függvény. A függvény által létrehozott értéket rendeli hozzá a [guid()](#guid) függvényt, és a frissített objektumot ad vissza:

```
addProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="addSeconds"></a>

## <a name="addseconds"></a>masodpercekHozzaadasa

Adja hozzá a másodpercek számát egy időbélyegző.

```
addSeconds('<timestamp>', <seconds>, '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*Másodperc*> | Igen | Egész szám | A hozzáadandó másodpercek pozitív vagy negatív szám | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | Az időbélyeg plusz a megadott számú másodperc  | 
|||| 

*1. példa*

Ebben a példában 10 másodperc hozzáadja a megadott időbélyeg:

```
addSeconds('2018-03-15T00:00:00Z', 10)
```

És ezt az eredményt adja vissza: `"2018-03-15T00:00:10.0000000Z"`

*2. példa*

Ebben a példában kivonja a megadott időbélyeghez öt másodperc:

```
addSeconds('2018-03-15T00:00:30Z', -5)
```

És ezt az eredményt adja vissza: `"2018-03-15T00:00:25.0000000Z"`

<a name="addToTime"></a>

## <a name="addtotime"></a>addToTime

Adja hozzá az időegységek számos időbélyeghez. Lásd még: [getFutureTime()](#getFutureTime).

```
addToTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*időköz*> | Igen | Egész szám | A megadott időegység hozzáadandó száma | 
| <*timeUnit*> | Igen | Sztring | Az időegység használata *időköz*: "A második", "Minute", "Hour", "Day", "Week", "Month", "Year" | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | Az időbélyeg plusz a megadott számú alkalommal egységek  | 
|||| 

*1. példa*

Ebben a példában egy napot ad hozzá a megadott időbélyeg:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day') 
```

És ezt az eredményt adja vissza: `"2018-01-02T00:00:00:0000000Z"`

*2. példa*

Ebben a példában egy napot ad hozzá a megadott időbélyeg:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day', 'D')
```

És a nem kötelező "D" formátumban adja vissza: `"Tuesday, January 2, 2018"`

<a name="and"></a>

## <a name="and"></a>és

Ellenőrzése, hogy az összes kifejezés igaz. Igaz értéket ad vissza az összes kifejezések teljesülése esetén, vagy vissza false (hamis), ha legalább egy kifejezés false (hamis).

```
and(<expression1>, <expression2>, ...)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Kifejezés1*>, <*Kifejezés2*>,... | Igen | Logikai | Ellenőrizze a kifejezések | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | -----| ----------- | 
| IGAZ vagy hamis | Logikai | Igaz értéket ad vissza ha összes kifejezés igaz. Vissza a False (hamis), ha legalább egy kifejezés false (hamis). | 
|||| 

*1. példa*

Ezekben a példákban ellenőrizze, hogy a megadott logikai értékek a következők mindegyike teljesül:

```
and(true, true)
and(false, true)
and(false, false)
```

És ezeket az eredményeket adja vissza:

* Első példa: mindkét kifejezés igaz, ezért adja vissza `true`. 
* Második példa: egy kifejezés false (hamis), ezért adja vissza `false`.
* Harmadik példában: mindkét kifejezés false (hamis), az így adja vissza `false`.

*2. példa*

Ezekben a példákban ellenőrzése, hogy a megadott kifejezések mindegyike teljesül:

```
and(equals(1, 1), equals(2, 2))
and(equals(1, 1), equals(1, 2))
and(equals(1, 2), equals(1, 3))
```

És ezeket az eredményeket adja vissza:

* Első példa: mindkét kifejezés igaz, ezért adja vissza `true`. 
* Második példa: egy kifejezés false (hamis), ezért adja vissza `false`.
* Harmadik példában: mindkét kifejezés false (hamis), az így adja vissza `false`.

<a name="array"></a>

## <a name="array"></a>tömb

A megadott egyetlen tömböt adjon vissza. Több bemenet, lásd: [createArray()](#createArray). 

```
array('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | A karakterlánc a tömb létrehozása | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| [<*érték*>] | Tömb | Egy tömb, amely tartalmazza a egyetlen megadott bemeneti adatok | 
|||| 

*Példa*

Ez a példa létrehoz egy tömböt a "hello" karakterlánc:

```
array('hello')
```

És ezt az eredményt adja vissza: `["hello"]`

<a name="base64"></a>

## <a name="base64"></a>Base64

A base64-kódolású verzióját egy karakterláncot ad vissza.

```
base64('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | A bemeneti karakterlánc | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*Base64-karakterlánc*> | Sztring | A bemeneti karakterláncot Base-64 kódolású verzió | 
|||| 

*Példa*

Ebben a példában a "hello" karakterlánc alakítja a base64-kódolású karakterlánc:

```
base64('hello')
```

És ezt az eredményt adja vissza: `"aGVsbG8="`

<a name="base64ToBinary"></a>

## <a name="base64tobinary"></a>base64ToBinary

A bináris verziójú a base64-kódolású karakterláncként adja vissza.

```
base64ToBinary('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az átalakítandó Base-64 kódolású sztring | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*bináris a Base64 kódolású karakterlánc*> | Sztring | A bináris verzió a base64-kódolású karakterlánc | 
|||| 

*Példa*

Ebben a példában alakítja át a "aGVsbG8 =" base64-kódolású karakterláncot bináris karakterlánc:

```
base64ToBinary('aGVsbG8=')
```

És ezt az eredményt adja vissza: 

`"0110000101000111010101100111001101100010010001110011100000111101"`

<a name="base64ToString"></a>

## <a name="base64tostring"></a>base64ToString

A base64-kódolású karakterlánchoz, hatékonyan a base64-karakterlánc dekódolása karakterlánc verziót adja vissza. Ez a függvény helyett [decodeBase64()](#decodeBase64). Mindkét függvény a ugyanúgy működnek, de `base64ToString()` részesíti előnyben.

```
base64ToString('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | A dekódolandó base64-kódolású karakterlánc | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*dekódovat base64-karakterlánc*> | Sztring | A karakterlánc-verzió base64-kódolású karakterlánc | 
|||| 

*Példa*

Ebben a példában alakítja át a "aGVsbG8 =" base64-kódolású karakterlánc csak egy karakterlánc:

```
base64ToString('aGVsbG8=')
```

És ezt az eredményt adja vissza: `"hello"`

<a name="binary"></a>

## <a name="binary"></a>Bináris 

A bináris verzió egy karakterláncot ad vissza.

```
binary('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az átalakítandó sztring | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*bináris fájlt a bemeneti érték*> | Sztring | A bináris verzióra a megadott karakterlánc | 
|||| 

*Példa*

Ebben a példában a "hello" karakterlánc bináris karakterlánccá alakítja át:

```
binary('hello')
```

És ezt az eredményt adja vissza: 

`"0110100001100101011011000110110001101111"`

<a name="body"></a>

## <a name="body"></a>törzs

Egy művelet visszaadandó `body` kimeneti futásidőben. A gyorsírás `actions('<actionName>').outputs.body`. Lásd: [actionBody()](#actionBody) és [actions()](#actions).

```
body('<actionName>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Műveletnév*> | Igen | Sztring | A művelet nevét `body` meg a kívánt kimeneti | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | -----| ----------- | 
| <*művelet – törzs-kimenet*> | Sztring | A `body` a megadott művelet kimenete | 
|||| 

*Példa*

Ez a példa lekéri a `body` kimenete a `Get user` Twitter-művelet: 

```
body('Get_user')
```

És ezt az eredményt adja vissza: 

```json
"body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="bool"></a>

## <a name="bool"></a>Logikai

A logikai értéket verziót adja vissza.

```
bool(<value>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Bármelyik | Az átalakítandó érték | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis | Logikai | A megadott értéket a logikai verziójára | 
|||| 

*Példa*

Ezekben a példákban a megadott érték átalakítása logikai értékek: 

```
bool(1)
bool(0)
```

És ezeket az eredményeket adja vissza: 

* Első. példa: `true` 
* Második példa: `false`

<a name="coalesce"></a>

## <a name="coalesce"></a>Coalesce

Az első nem üres érték visszaadása egy vagy több paramétert. Üres karakterláncok, üres tömbök és üres objektumok ne legyenek.

```
coalesce(<object_1>, <object_2>, ...)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*object_1*>, <*object_2*>,... | Igen | Bármely, kombinálhatja típusok | Egy vagy több elem NULL értékű keresése | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*első-nem-null-elem*> | Bármelyik | Az első elem vagy nem null értéket. Ha minden paraméter null értékű, a függvény null értéket ad vissza. | 
|||| 

*Példa*

Ezekben a példákban az első nem üres értéket a megadott értékeket, vagy null értékű adja vissza, amikor null értékek:

```
coalesce(null, true, false)
coalesce(null, 'hello', 'world')
coalesce(null, null, null)
```

És ezeket az eredményeket adja vissza: 

* Első. példa: `true` 
* Második példa: `"hello"`
* Harmadik. példa: `null`

<a name="concat"></a>

## <a name="concat"></a>Concat

Két vagy több karakterláncok egyesítése, és a kombinált karakterláncot ad vissza. 

```
concat('<text1>', '<text2>', ...)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*TEXT1*>, <*szöveg2*>,... | Igen | Sztring | Úgy, hogy legalább két karakterlánc | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*text1text2...*> | Sztring | A karakterlánc a kombinált bemeneti karakterláncokból létrehozva | 
|||| 

*Példa*

Ebben a példában a "Hello" és "World" karakterlánc kombinálja:

```
concat('Hello', 'World')
```

És ezt az eredményt adja vissza: `"HelloWorld"`

<a name="contains"></a>

## <a name="contains"></a>tartalmazza a következőt:

Ellenőrizze, hogy egy gyűjtemény rendelkezik-e egy adott elemet. Igaz értéket ad vissza, ha az elem található, vagy visszatérhet false (hamis) Ha nem található. Ez a funkció akkor kis-és nagybetűket.

```
contains('<collection>', '<value>')
contains([<collection>], '<value>')
```

Ez a függvény kifejezetten, a gyűjtemény típusaival működik: 

* A *karakterlánc* található egy *karakterláncrészletet*
* Egy *tömb* található egy *érték*
* A *szótár* található egy *kulcs*

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Gyűjtemény*> | Igen | Karakterlánc, tömböt vagy szótár | Ellenőrizze a gyűjtemény | 
| <*Érték*> | Igen | Karakterlánc, tömböt vagy szótár, illetve | Az elem keresése | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis | Logikai | Igaz értéket ad vissza, ha az elem található. Vissza false (hamis) Ha nem található. |
|||| 

*1. példa*

Ebben a példában a karakterlánc a "world" substring "hello world" ellenőrzi, és IGAZ értéket ad vissza:

```
contains('hello world', 'world')
```

*2. példa*

Ebben a példában a karakterlánc a "universe" substring "hello world" ellenőrzi, és hamis értéket ad vissza:

```
contains('hello world', 'universe')
```

<a name="convertFromUtc"></a>

## <a name="convertfromutc"></a>convertFromUtc

Konvertálja az időbélyegző az univerzális idő egyezményes (UTC) időzónában.

```
convertFromUtc('<timestamp>', '<destinationTimeZone>', '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*destinationTimeZone*> | Igen | Sztring | A célként megadott időzóna neve. További információkért lásd: [időzóna-azonosítói](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*konvertált időbélyeg*> | Sztring | Az időbélyeg időzónában alakítani. | 
|||| 

*1. példa*

Ebben a példában egy időbélyeg alakítja át a megadott időzóna: 

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time')
```

És ezt az eredményt adja vissza: `"2018-01-01T00:00:00.0000000"`

*2. példa*

Ebben a példában egy időbélyeg alakítja át a megadott időzónát és formátumot:

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time', 'D')
```

És ezt az eredményt adja vissza: `"Monday, January 1, 2018"`

<a name="convertTimeZone"></a>

## <a name="converttimezone"></a>convertTimeZone

A forrásidőzóna időbélyeg konvertálása időzónában.

```
convertTimeZone('<timestamp>', '<sourceTimeZone>', '<destinationTimeZone>', '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*sourceTimeZone*> | Igen | Sztring | A forrásidőzóna nevét. További információkért lásd: [időzóna-azonosítói](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*destinationTimeZone*> | Igen | Sztring | A célként megadott időzóna neve. További információkért lásd: [időzóna-azonosítói](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*konvertált időbélyeg*> | Sztring | Az időbélyeg időzónában alakítani. | 
|||| 

*1. példa*

Ebben a példában a forrásidőzóna alakítja át a célidőzónát: 

```
convertTimeZone('2018-01-01T08:00:00.0000000Z', 'UTC', 'Pacific Standard Time')
```

És ezt az eredményt adja vissza: `"2018-01-01T00:00:00.0000000"`

*2. példa*

Ebben a példában egy időzóna alakítja át a megadott időzónát és formátumot:

```
convertTimeZone('2018-01-01T80:00:00.0000000Z', 'UTC', 'Pacific Standard Time', 'D')
```

És ezt az eredményt adja vissza: `"Monday, January 1, 2018"`

<a name="convertToUtc"></a>

## <a name="converttoutc"></a>convertToUtc

A próbaverzióról időbélyeg a forrásidőzóna univerzális idő egyezményes (UTC).

```
convertToUtc('<timestamp>', '<sourceTimeZone>', '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*sourceTimeZone*> | Igen | Sztring | A forrásidőzóna nevét. További információkért lásd: [időzóna-azonosítói](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*konvertált időbélyeg*> | Sztring | Az UTC szerint konvertálja időbélyeg | 
|||| 

*1. példa*

Ebben a példában időbélyeg konvertálása UTC: 

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time')
```

És ezt az eredményt adja vissza: `"2018-01-01T08:00:00.0000000Z"`

*2. példa*

Ebben a példában időbélyeg konvertálása UTC:

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time', 'D')
```

És ezt az eredményt adja vissza: `"Monday, January 1, 2018"`

<a name="createArray"></a>

## <a name="createarray"></a>createArray

Származó több bemenet egy tömböt adnak vissza. Egyetlen bemeneti tömbök, lásd: [array()](#array).

```
createArray('<object1>', '<object2>', ...)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Object1*>, <*object2*>,... | Igen | Bármely, de nem vegyes | Legalább két elemet a tömb létrehozása | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| [<*object1*>, <*object2*>,...] | Tömb | A tömb összes bemeneti cikk alapján létrehozott | 
|||| 

*Példa*

Ebben a példában az alábbi ráfordítások létrehoz egy tömböt:

```
createArray('h', 'e', 'l', 'l', 'o')
```

És ezt az eredményt adja vissza: `["h", "e", "l", "l", "o"]`

<a name="dataUri"></a>

## <a name="datauri"></a>dataUri

Egy adatok egységes erőforrás-azonosítója (URI) egy karakterláncot ad vissza. 

```
dataUri('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az átalakítandó sztring | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*adat-uri*> | Sztring | Az adat-URI a bemeneti karakterlánc | 
|||| 

*Példa*

Ez a példa létrehoz egy adat-URI a "hello" karakterlánc:

```
dataUri('hello') 
```

És ezt az eredményt adja vissza: `"data:text/plain;charset=utf-8;base64,aGVsbG8="`

<a name="dataUriToBinary"></a>

## <a name="datauritobinary"></a>dataUriToBinary

Vissza a bináris adatok egységes erőforrás-azonosítója (URI) verziójára. Ez a függvény helyett [decodeDataUri()](#decodeDataUri). Mindkét függvény a ugyanúgy működnek, de `decodeDataUri()` részesíti előnyben.

```
dataUriToBinary('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az adat-URI konvertálása | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*bináris-az-adat-uri*> | Sztring | A bináris verziójára az adat-URI | 
|||| 

*Példa*

Ez a példa létrehoz egy bináris verziót az adat-URI:

```
dataUriToBinary('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

És ezt az eredményt adja vissza: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="dataUriToString"></a>

## <a name="datauritostring"></a>dataUriToString

Ez a karakterlánc verzió adatok egységes erőforrás-azonosítója (URI) visszaadása.

```
dataUriToString('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az adat-URI konvertálása | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*karakterlánc-az-adatok – uri*> | Sztring | Ez a karakterlánc verzió az adat-URI | 
|||| 

*Példa*

Ez a példa létrehoz egy az adat-URI karakterláncát:

```
dataUriToString('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

És ezt az eredményt adja vissza: `"hello"`

<a name="dayOfMonth"></a>

## <a name="dayofmonth"></a>dayOfMonth

A hónap napjának visszaadása a időbélyeg. 

```
dayOfMonth('<timestamp>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*hónap napja*> | Egész szám | A megadott időbélyeg a hónap napja | 
|||| 

*Példa*

Ebben a példában a közötti számot ad vissza a hónap napját az időbélyeg:

```
dayOfMonth('2018-03-15T13:27:36Z')
```

És ezt az eredményt adja vissza: `15`

<a name="dayOfWeek"></a>

## <a name="dayofweek"></a>dayOfWeek

A hét napjának visszaadása egy időbélyegző.  

```
dayOfWeek('<timestamp>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*– hét napja*> | Egész szám | A megadott időbélyeg, ahol vasárnap értéke 0, hétfőn, a hét napja az 1, és így tovább | 
|||| 

*Példa*

Ebben a példában a közötti számot ad vissza, ha a hét napon az időbélyeg:

```
dayOfWeek('2018-03-15T13:27:36Z')
```

És ezt az eredményt adja vissza: `3`

<a name="dayOfYear"></a>

## <a name="dayofyear"></a>dayOfYear

Az év napjának visszaadása egy időbélyegző. 

```
dayOfYear('<timestamp>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*év napja*> | Egész szám | A megadott időbélyeg az év napját | 
|||| 

*Példa*

Ebben a példában az év napjának száma az időbélyeget adja vissza:

```
dayOfYear('2018-03-15T13:27:36Z')
```

És ezt az eredményt adja vissza: `74`

<a name="decodeBase64"></a>

## <a name="decodebase64"></a>decodeBase64

A base64-kódolású karakterlánchoz, hatékonyan a base64-karakterlánc dekódolása karakterlánc verziót adja vissza. Fontolja meg [base64ToString()](#base64ToString) helyett `decodeBase64()`. Mindkét függvény a ugyanúgy működnek, de `base64ToString()` részesíti előnyben.

```
decodeBase64('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | A dekódolandó base64-kódolású karakterlánc | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*dekódovat base64-karakterlánc*> | Sztring | A karakterlánc-verzió base64-kódolású karakterlánc | 
|||| 

*Példa*

Ez a példa létrehoz egy Base-64 kódolású karakterláncot karakterláncként:

```
decodeBase64('aGVsbG8=')
```

És ezt az eredményt adja vissza: `"hello"`

<a name="decodeDataUri"></a>

## <a name="decodedatauri"></a>decodeDataUri

Vissza a bináris adatok egységes erőforrás-azonosítója (URI) verziójára. Fontolja meg [dataUriToBinary()](#dataUriToBinary), helyett `decodeDataUri()`. Mindkét függvény a ugyanúgy működnek, de `dataUriToBinary()` részesíti előnyben.

```
decodeDataUri('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az adatok a URI karakterlánc dekódolása | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*bináris-az-adat-uri*> | Sztring | A bináris adat URI karakterlánc verziójára | 
|||| 

*Példa*

Ebben a példában a az adat-URI bináris verziót adja vissza:

```
decodeDataUri('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

És ezt az eredményt adja vissza: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="decodeUriComponent"></a>

## <a name="decodeuricomponent"></a>decodeUriComponent

Lépjen vissza, hogy lecseréli escape-karakter a dekódolt verziókkal karakterláncot. 

```
decodeUriComponent('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | A karakterlánc való dekódolandó escape-karakterrel | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*uri dekódovat.*> | Sztring | A dekódolt escape-karakterekkel a frissített karakterlánc | 
|||| 

*Példa*

Ebben a példában a dekódolt verziókkal váltja fel az escape-karaktereket a következő karakterláncot:

```
decodeUriComponent('http%3A%2F%2Fcontoso.com')
```

És ezt az eredményt adja vissza: `"https://contoso.com"`

<a name="div"></a>

## <a name="div"></a>DIV

Két szám hányadosát adja vissza az egész típusú eredményként. A fennmaradó eredményének beolvasása, lásd: [mod()](#mod).

```
div(<dividend>, <divisor>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*osztandó*> | Igen | Egész vagy lebegőpontos szám | A szám, mellyel osztani a *osztó* | 
| <*osztó*> | Igen | Egész vagy lebegőpontos szám | A szám, amely elosztja a *osztandó*, de nem lehet 0 | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*hányadosát-eredmény*> | Egész szám | Az egész típusú eredményként a második szám szerint az első szám hányadosát | 
|||| 

*Példa*

Mindkét példa osztani a második szám szerint az első szám:

```
div(10, 5)
div(11, 5)
```

És ezt az eredményt adja vissza: `2`

<a name="encodeUriComponent"></a>

## <a name="encodeuricomponent"></a>encodeUriComponent

Egy karakterláncot egy egységes erőforrás-azonosító (URI) kódolású verzió vissza lecserélésével URL-címekben nem biztonságos karaktereket escape-karaktereket. Fontolja meg [uriComponent()](#uriComponent), helyett `encodeUriComponent()`. Mindkét függvény a ugyanúgy működnek, de `uriComponent()` részesíti előnyben.

```
encodeUriComponent('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az URI-ként kódolt formában alakítandó karakterláncot | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*uri-ként kódolt*> | Sztring | Az URI-ként kódolt karakterlánc escape-karakterrel | 
|||| 

*Példa*

Ez a példa létrehoz egy URI-ként kódolt verzió a következő karakterláncot:

```
encodeUriComponent('https://contoso.com')
```

És ezt az eredményt adja vissza: `"http%3A%2F%2Fcontoso.com"`

<a name="empty"></a>

## <a name="empty"></a>üres

Ellenőrizze, hogy egy gyűjtemény üres. Igaz értéket ad vissza üres a gyűjtemény esetén, vagy visszatérhet a hamis értéket, ha nem üres.

```
empty('<collection>')
empty([<collection>])
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Gyűjtemény*> | Igen | Karakterlánc, tömb vagy objektum | Ellenőrizze a gyűjtemény | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis | Logikai | Igaz értéket ad vissza üres a gyűjtemény esetén. Vissza a hamis értéket, ha nem üres. | 
|||| 

*Példa* 

Ezekben a példákban ellenőrzése, hogy a megadott gyűjteményekkel üres:

```
empty('')
empty('abc')
```

És ezeket az eredményeket adja vissza: 

* Első példa: üres karakterlánc, továbbítja, így a függvény `true`. 
* Második példa: továbbítja a "abc", a karakterlánc, a függvény `false`. 

<a name="endswith"></a>

## <a name="endswith"></a>endsWith

Ellenőrizze, hogy e karakterlánc végződik-e egy adott karakterláncrészletet. Igaz értéket ad vissza, ha a substring található, vagy visszatérhet false (hamis) Ha nem található. Ez a funkció nem áll kis-és nagybetűket.

```
endsWith('<text>', '<searchText>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szöveg*> | Igen | Sztring | A karakterlánc | 
| <*Keresettszöveg*> | Igen | Sztring | A befejezési karakterláncrész keresése | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis  | Logikai | Igaz értéket ad vissza, ha a befejezési karakterláncrészletet található. Vissza false (hamis) Ha nem található. | 
|||| 

*1. példa* 

Ebben a példában ellenőrzi, hogy a "hello world" karakterlánc a "world" karakterlánccal végződik:

```
endsWith('hello world', 'world')
```

És ezt az eredményt adja vissza: `true`

*2. példa*

Ebben a példában ellenőrzi, hogy a "hello world" karakterlánc a "universe" karakterlánccal végződik:

```
endsWith('hello world', 'universe')
```

És ezt az eredményt adja vissza: `false`

<a name="equals"></a>

## <a name="equals"></a>egyenlő

Ellenőrizze, hogy e egyaránt értékek, kifejezések vagy objektumok egyenértékűek. Igaz értéket ad vissza is egyenértékű, vagy adja vissza, ha azok még nem egyenértékű false (hamis).

```
equals('<object1>', '<object2>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Object1*>, <*object2*> | Igen | Különböző | Az értékeket, kifejezések vagy objektumok összehasonlítása | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis | Logikai | Igaz értéket ad vissza, ha mindkét egyenértékűek. Vissza a hamis értéket, ha nem megfelelő. | 
|||| 

*Példa*

Ezekben a példákban ellenőrizze, hogy a megadott bemeneti adatok egyenértékűek. 

```
equals(true, 1)
equals('abc', 'abcd')
```

És ezeket az eredményeket adja vissza: 

* Első példa: mindkét értéket egyenértékűek, így a függvény `true`.
* Második példában: mindkét értéket nem egyenértékű, így a függvény `false`.

<a name="first"></a>

## <a name="first"></a>első

Az első elem visszaadása egy karakterlánc vagy a tömb.

```
first('<collection>')
first([<collection>])
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Gyűjtemény*> | Igen | Karakterlánc- vagy tömb | A gyűjtemény, hogy hol található az első elem |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*első gyűjteményelemet*> | Bármelyik | Az első elem a gyűjteményben | 
|||| 

*Példa*

Ezekben a példákban az első elem a gyűjteményekben lévő keresése:

```
first('hello')
first([0, 1, 2])
```

És ezeket az eredményeket adja vissza: 

* Első. példa: `"h"`
* Második példában: `0`

<a name="float"></a>

## <a name="float"></a>lebegőpontos

Egy karakterlánc verziót lebegőpontos szám átalakítása egy tényleges lebegőpontos számot. Ez a funkció csak akkor, ha egyéni paraméterek átadása egy alkalmazáshoz, például a logikai alkalmazás használható.

```
float('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | A karakterlánc, amely rendelkezik egy érvényes lebegőpontos szám konvertálása |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*lebegőpontos-érték*> | lebegőpontos | A lebegőpontos szám a megadott karakterlánc | 
|||| 

*Példa*

Ez a példa létrehoz egy karakterlánc verziót a lebegőpontos szám:

```
float('10.333')
```

És ezt az eredményt adja vissza: `10.333`

<a name="formatDateTime"></a>

## <a name="formatdatetime"></a>formatDateTime

A megadott formátumban adja vissza egy időbélyegző.

```
formatDateTime('<timestamp>', '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*újraformázta időbélyeg*> | Sztring | A frissített időbélyeg formátuma | 
|||| 

*Példa*

Ebben a példában egy időbélyeg alakítja át a megadott formátumban:

```
formatDateTime('03/15/2018 12:00:00', 'yyyy-MM-ddTHH:mm:ss')
```

És ezt az eredményt adja vissza: `"2018-03-15T12:00:00"`

<a name="formDataMultiValues"></a>

## <a name="formdatamultivalues"></a>formDataMultiValues

A kulcs nevét, egy műveletet a megfelelő értékekkel egy tömböt adnak vissza *űrlapadatokból* vagy *űrlapként* kimeneti. 

```
formDataMultiValues('<actionName>', '<key>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Műveletnév*> | Igen | Sztring | A művelet kimenete rendelkezik a kívánt kulcs értéke | 
| <*Kulcs*> | Igen | Sztring | A használni kívánt kulcs neve | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| [<*tömb a kulcs értékeit*>] | Tömb | Egy tömb azokra az értékekre, amelyek megfelelnek a megadott kulcs | 
|||| 

*Példa* 

Ebben a példában a "Tárgy" kulcs értéket a megadott művelet űrlapadatokból vagy űrlapként kimeneti tömbben hoz létre:  

```
formDataMultiValues('Send_an_email', 'Subject')
```

És a tulajdonos a szöveget adja vissza egy tömb, például: `["Hello world"]`

<a name="formDataValue"></a>

## <a name="formdatavalue"></a>formDataValue

Ad vissza, amely megfelel egy műveletet a kulcs nevét egyetlen értéket *űrlapadatokból* vagy *űrlapként* kimeneti. Ha a függvény legalább egy egyezést talál, a függvény hibát jelez.

```
formDataValue('<actionName>', '<key>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Műveletnév*> | Igen | Sztring | A művelet kimenete rendelkezik a kívánt kulcs értéke | 
| <*Kulcs*> | Igen | Sztring | A használni kívánt kulcs neve |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*kulcs-érték*> | Sztring | A megadott kulcs értéke  | 
|||| 

*Példa* 

Ebben a példában a karakterlánc a "Tárgy" kulcs értéke a megadott művelet űrlapadatokból vagy a kimeneti űrlapként hoz létre:  

```
formDataValue('Send_an_email', 'Subject')
```

Például egy karakterláncként adja vissza a tárgy szövegét, és: `"Hello world"`

<a name="getFutureTime"></a>

## <a name="getfuturetime"></a>getFutureTime

Vissza az aktuális timestamp plusz a megadott időegység.

```
getFutureTime(<interval>, <timeUnit>, <format>?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*időköz*> | Igen | Egész szám | A megadott időegység kivonandó száma | 
| <*timeUnit*> | Igen | Sztring | Az időegység használata *időköz*: "A második", "Minute", "Hour", "Day", "Week", "Month", "Year" | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | Az aktuális timestamp plusz a megadott számú alkalommal egységek | 
|||| 

*1. példa*

Tegyük fel, hogy az aktuális Timestamp "2018-03-01T00:00:00.0000000Z". Ebben a példában öt napot ad hozzá az időbélyeg:

```
getFutureTime(5, 'Day')
```

És ezt az eredményt adja vissza: `"2018-03-06T00:00:00.0000000Z"`

*2. példa*

Tegyük fel, hogy az aktuális Timestamp "2018-03-01T00:00:00.0000000Z". Ebben a példában öt napot ad, és a "D" formátumra alakítja át az eredmény:

```
getFutureTime(5, 'Day', 'D')
```

És ezt az eredményt adja vissza: `"Tuesday, March 6, 2018"`

<a name="getPastTime"></a>

## <a name="getpasttime"></a>getPastTime

A megadott időegység mínusz az aktuális időbélyeget adja vissza.

```
getPastTime(<interval>, <timeUnit>, <format>?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*időköz*> | Igen | Egész szám | A megadott időegység kivonandó száma | 
| <*timeUnit*> | Igen | Sztring | Az időegység használata *időköz*: "A második", "Minute", "Hour", "Day", "Week", "Month", "Year" | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | Az aktuális timestamp mínusz a megadott számú alkalommal egységek | 
|||| 

*1. példa*

Tegyük fel, hogy az aktuális Timestamp "2018-02-01T00:00:00.0000000Z". Ebben a példában az időbélyeg 5 napos kivonja:

```
getPastTime(5, 'Day')
```

És ezt az eredményt adja vissza: `"2018-01-27T00:00:00.0000000Z"`

*2. példa*

Tegyük fel, hogy az aktuális Timestamp "2018-02-01T00:00:00.0000000Z". Ebben a példában öt nappal levonja, és a "D" formátumra alakítja át az eredmény:

```
getPastTime(5, 'Day', 'D')
```

És ezt az eredményt adja vissza: `"Saturday, January 27, 2018"`

<a name="greater"></a>

## <a name="greater"></a>nagyobb

Ellenőrizze, hogy az első érték nagyobb, mint a második érték. Igaz értéket ad vissza az első érték további esetén, vagy visszatérhet hamis mikor kevesebb.

```
greater(<value>, <compareTo>)
greater('<value>', '<compareTo>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Egész szám, lebegőpontos vagy karakterlánc | Ellenőrizze, hogy nagyobb, mint a második érték az első érték | 
| <*compareto metódus végrehajtása*> | Igen | Egész szám, lebegőpontos vagy karakterlánc, illetve | Az összehasonlító érték | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis | Logikai | Igaz értéket ad vissza az első érték nagyobb, mint a második érték esetén. Vissza a False (hamis), amikor az első érték egyenlő vagy kisebb, mint a második érték. | 
|||| 

*Példa*

Ezekben a példákban ellenőrizze, hogy az első érték nagyobb, mint a második érték:

```
greater(10, 5)
greater('apple', 'banana')
```

És ezeket az eredményeket adja vissza: 

* Első. példa: `true`
* Második példa: `false`

<a name="greaterOrEquals"></a>

## <a name="greaterorequals"></a>greaterOrEquals

Ellenőrizze, hogy az első érték kisebb, mint a második érték egyenlő.
Igaz értéket ad vissza, ha az első érték nagyobb vagy egyenlő, vagy adja vissza, amikor az első érték kisebb false (hamis).

```
greaterOrEquals(<value>, <compareTo>)
greaterOrEquals('<value>', '<compareTo>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Egész szám, lebegőpontos vagy karakterlánc | Az első érték kisebb, mint a második érték egyenlő-e ellenőrizni | 
| <*compareto metódus végrehajtása*> | Igen | Egész szám, lebegőpontos vagy karakterlánc, illetve | Az összehasonlító érték | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis | Logikai | Igaz értéket ad vissza, ha az első érték kisebb, mint a második érték egyenlő. Hamis értéket, ha az első érték kisebb, mint a második érték visszaadása. | 
|||| 

*Példa*

Ezekben a példákban ellenőrizze, hogy az első érték nagyobb vagy egyenlő, mint a második érték:

```
greaterOrEquals(5, 5)
greaterOrEquals('apple', 'banana')
```

És ezeket az eredményeket adja vissza: 

* Első. példa: `true`
* Második példa: `false`

<a name="guid"></a>

## <a name="guid"></a>GUID azonosítója

Egy karakterlánc, például a "c2ecc88d-88c8-4096-912c-d6f2e2b138ce" hozzon létre egy globálisan egyedi azonosítóját (GUID): 

```
guid()
```

Emellett a GUID nem az alapértelmezett formátum, a "D", amely 32 számjegy kötőjeleket elválasztva más formátumba is megadhat.

```
guid('<format>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Formátum*> | Nem | Sztring | Egyetlen [specifikátor formázása](https://msdn.microsoft.com/library/97af8hh4) a visszaadott GUID. Alapértelmezés szerint a formátum a "D", de használhatja a "N", "D", "B", "P" vagy "X". | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*GUID-érték*> | Sztring | Egy véletlenszerűen előállított GUID Azonosítóhoz | 
|||| 

*Példa* 

Ebben a példában ugyanaz a GUID állít elő, de 32 számjegy választják kötőjeleket tartalmazhat, és zárójelek között: 

```
guid('P')
```

És ezt az eredményt adja vissza: `"(c2ecc88d-88c8-4096-912c-d6f2e2b138ce)"`

<a name="if"></a>

## <a name="if"></a>Ha

Ellenőrizze, hogy egy kifejezés true vagy FALSE (hamis). Az eredmény alapján a megadott érték visszaadása.

```
if(<expression>, <valueIfTrue>, <valueIfFalse>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*kifejezés*> | Igen | Logikai | Ellenőrizze, hogy a kifejezés | 
| <*valueIfTrue*> | Igen | Bármelyik | A visszatérési érték, ha a kifejezés igaz | 
| <*valueIfFalse*> | Igen | Bármelyik | A visszatérési érték, amikor a kifejezés értéke FALSE (hamis) | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*a megadott visszatérési-érték*> | Bármelyik | A megadott, hogy alapján ad vissza a kifejezés értéke true vagy FALSE (hamis) | 
|||| 

*Példa* 

Ebben a példában adja vissza `"yes"` , mert a megadott kifejezés igaz értéket ad vissza. Ellenkező esetben adja vissza a példa `"no"`:

```
if(equals(1, 1), 'yes', 'no')
```

<a name="indexof"></a>

## <a name="indexof"></a>indexOf

A kezdő pozíció vagy index értéke egy karakterláncrészt adja vissza. Ez a funkció nem kis-és nagybetűket, és az indexek kezdje a számot 0. 

```
indexOf('<text>', '<searchText>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szöveg*> | Igen | Sztring | A karakterlánc, amely rendelkezik a karakterláncrész keresése | 
| <*Keresettszöveg*> | Igen | Sztring | Keresse meg a keresendő karakterláncrészletet | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*index-érték*>| Egész szám | A megadott karakterláncrészlet kezdő pozíció vagy index értéket. <p>Ha a karakterlánc nem található, -1 számának visszaadása. </br>Ha a karakterlánc üres, a 0 számának visszaadása. | 
|||| 

*Példa* 

Ebben a példában a "world" substring "hello world" karakterláncban megkeresi a kezdő indexet érték:

```
indexOf('hello world', 'world')
```

És ezt az eredményt adja vissza: `6`

<a name="int"></a>

## <a name="int"></a>int

Az egész verziója egy karakterláncot ad vissza.

```
int('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az átalakítandó sztring | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*eredmény nélküli egész szám*> | Egész szám | A megadott karakterlánc az egész verziója | 
|||| 

*Példa* 

Ez a példa létrehoz egy egész verziója a "10" karakterlánc:

```
int('10')
```

És ezt az eredményt adja vissza: `10`

<a name="item"></a>

## <a name="item"></a>Elem

Egy ismétlődő műveletet használat keresztül egy tömb, vissza az aktuális elem a tömbben a művelet aktuális iteráció során. Az értékeket is kérhet, hogy elem tulajdonságai. 

```
item()
```

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*jelenlegi-tömb-elem*> | Bármelyik | A művelet aktuális iteráció a tömbben az aktuális elem | 
|||| 

*Példa* 

Ez a példa lekéri a `body` az aktuális üzenetet, a "Send_an_email" művelethez egy for-each ciklusban az aktuális iteráció belül elemet:

```
item().body
```

<a name="items"></a>

## <a name="items"></a>elem

A for-each ciklusban minden ciklusban az aktuális elem visszaadása. A for-each ciklusban belül e funkció használatához.

```
items('<loopName>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*loopName*> | Igen | Sztring | A for-each ciklus neve | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*Elem*> | Bármelyik | Az elem található a megadott for-each ciklusban az aktuális ciklus | 
|||| 

*Példa* 

Ebben a példában az aktuális elem olvas be a megadott for-each ciklus:

```
items('myForEachLoopName')
```

<a name="json"></a>

## <a name="json"></a>JSON-ban

A JavaScript Object Notation (JSON) típusú érték, vagy egy karakterláncot vagy XML-objektumot ad vissza.

```
json('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Karakterláncot vagy XML | Az átalakítandó karakterláncot vagy XML | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*JSON-eredmény*> | Nativní typ JSON vagy az objektum | A JSON natív típusú értékké. vagy a megadott karakterlánc vagy XML-objektumot. Ha a karakterlánc értéke null, a függvény egy üres objektumot ad vissza. | 
|||| 

*1. példa* 

Ebben a példában ez a karakterlánc konvertálja a JSON-érték:

```
json('[1, 2, 3]')
```

És ezt az eredményt adja vissza: `[1, 2, 3]`

*2. példa*

Ebben a példában ez a karakterlánc konvertálja a JSON-ná: 

```
json('{"fullName": "Sophia Owen"}')
```

És ezt az eredményt adja vissza:

```
{
  "fullName": "Sophia Owen"
}
```

*3. példa*

Ebben a példában ez XML konvertálja JSON: 

```
json(xml('<?xml version="1.0"?> <root> <person id='1'> <name>Sophia Owen</name> <occupation>Engineer</occupation> </person> </root>'))
```

És ezt az eredményt adja vissza:

```json
{ 
   "?xml": { "@version": "1.0" }, 
   "root": { 
      "person": [ { 
         "@id": "1", 
         "name": "Sophia Owen", 
         "occupation": "Engineer" 
       } ] 
   } 
}
```

<a name="intersection"></a>

## <a name="intersection"></a>Metszet

Vissza, amely rendelkezik *csak* a gyakori elemek a megadott gyűjtemények között. Az eredmény jelenik meg, hogy egy elem szerepelnie kell a függvénynek átadott összes gyűjteményt. Ha egy vagy több elemet ugyanazzal a névvel rendelkezik, az eredmény ilyen nevű legutóbbi elem jelenik meg.

```
intersection([<collection1>], [<collection2>], ...)
intersection('<collection1>', '<collection2>', ...)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*collection1*>, <*2. gyűjtemény*>,... | Igen | Tömb vagy objektum, de nem mindkettőt | A gyűjtemények, ahonnan csak szeretné *csak* a gyakori elemek | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*közös-elemek*> | Olyan tömböt vagy objektumot, illetve | Egy gyűjteményt, amely csak a gyakori elem van a megadott gyűjtemények között | 
|||| 

*Példa* 

Ebben a példában a közös elemeket talál, ezek a tárolótömbök között:  

```
intersection([1, 2, 3], [101, 2, 1, 10], [6, 8, 1, 2])
```

És a egy tömböt ad vissza, *csak* ezeket az elemeket: `[1, 2]`

<a name="join"></a>

## <a name="join"></a>csatlakozás

Lépjen vissza a karakterlánc, amely tartalmaz egy tömb összes elemét és minden egyes karakter választja el egy *elválasztó*.

```
join([<collection>], '<delimiter>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Gyűjtemény*> | Igen | Tömb | A tömb, amely rendelkezik a cikkek való csatlakozásra |  
| <*elválasztó karakter*> | Igen | Sztring | Az elválasztó, amely megjelenik az eredményül kapott karakterláncban szereplő karakterek közé | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*char1*><*elválasztó*><*char2*><*elválasztó*>... | Sztring | Az eredményül kapott karakterláncot a megadott tömb összes eleme alapján létrehozott |
|||| 

*Példa* 

Ebben a példában hoz létre egy karakterláncot a tömb a megadott karaktert az összes eleme elválasztóként:

```
join([a, b, c], '.')
```

És ezt az eredményt adja vissza: `"a.b.c"`

<a name="last"></a>

## <a name="last"></a>utolsó

Az utolsó elem visszaadása egy gyűjteményt.

```
last('<collection>')
last([<collection>])
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Gyűjtemény*> | Igen | Karakterlánc- vagy tömb | A gyűjtemény utolsó elemének megkeresése | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*utolsó gyűjteményelemet*> | Karakterlánc- vagy tömböt, illetve | Az utolsó elem a gyűjteményben | 
|||| 

*Példa* 

Ezekben a példákban az utolsó elem a gyűjteményekben lévő keresése:

```
last('abcd')
last([0, 1, 2, 3])
```

És ezeket az eredményeket adja vissza: 

* Első. példa: `"d"`
* Második példa: `3`

<a name="lastindexof"></a>

## <a name="lastindexof"></a>lastIndexOf

Egy karakterláncrészletet befejező pozíció vagy index értéket adja vissza. Ez a funkció nem kis-és nagybetűket, és az indexek kezdje a számot 0.

```
lastIndexOf('<text>', '<searchText>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szöveg*> | Igen | Sztring | A karakterlánc, amely rendelkezik a karakterláncrész keresése | 
| <*Keresettszöveg*> | Igen | Sztring | Keresse meg a keresendő karakterláncrészletet | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*a befejező-index-érték*> | Egész szám | A megadott karakterláncrészlet befejező pozíció vagy az index értéke. <p>Ha a karakterlánc nem található, -1 számának visszaadása. </br>Ha a karakterlánc üres, a 0 számának visszaadása. | 
|||| 

*Példa* 

Ebben a példában a "world" substring "hello world" karakterláncban megkeresi a záró értéket:

```
lastIndexOf('hello world', 'world')
```

És ezt az eredményt adja vissza: `10`

<a name="length"></a>

## <a name="length"></a>Hossza

Egy gyűjteményben lévő elemek számának visszaadása.

```
length('<collection>')
length([<collection>])
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Gyűjtemény*> | Igen | Karakterlánc- vagy tömb | A gyűjteményhez az elemek száma | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*hossza vagy száma*> | Egész szám | A gyűjteményben lévő elemek száma. | 
|||| 

*Példa*

Ezekben a példákban a gyűjteményekben lévő elemek száma: 

```
length('abcd')
length([0, 1, 2, 3])
```

És ezt az eredményt adja vissza: `4`

<a name="less"></a>

## <a name="less"></a>kevesebb

Ellenőrizze, hogy van-e az első érték kisebb, mint a második érték.
Igaz értéket ad vissza, ha az első érték kisebb, vagy adja vissza, ha az első érték több false (hamis).

```
less(<value>, <compareTo>)
less('<value>', '<compareTo>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Egész szám, lebegőpontos vagy karakterlánc | Az első értéket, ellenőrizze, hogy kevesebb, mint a második érték | 
| <*compareto metódus végrehajtása*> | Igen | Egész szám, lebegőpontos vagy karakterlánc, illetve | Az összehasonlítás elem | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis | Logikai | IGAZ, ha az első érték kisebb, mint a második érték visszaadása. Vissza a False (hamis), amikor az első érték egyenlő vagy nagyobb, mint a második érték. | 
|||| 

*Példa*

Ezekben a példákban ellenőrizze, hogy van-e az első érték kisebb, mint a második érték.

```
less(5, 10)
less('banana', 'apple')
```

És ezeket az eredményeket adja vissza: 

* Első. példa: `true`
* Második példa: `false`

<a name="lessOrEquals"></a>

## <a name="lessorequals"></a>lessOrEquals

Ellenőrizze, hogy az első érték kisebb vagy egyenlő a második érték.
Igaz értéket ad vissza, ha az első értéke kisebb vagy egyenlő, vagy adja vissza, ha az első érték több false (hamis).

```
lessOrEquals(<value>, <compareTo>)
lessOrEquals('<value>', '<compareTo>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Egész szám, lebegőpontos vagy karakterlánc | Ellenőrizheti, hogy kevesebb, mint az első értéket vagy a második érték egyenlő | 
| <*compareto metódus végrehajtása*> | Igen | Egész szám, lebegőpontos vagy karakterlánc, illetve | Az összehasonlítás elem | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis  | Logikai | Igaz értéket ad vissza, ha az első érték kisebb vagy egyenlő a második érték. Vissza a False (hamis), ha az első érték nagyobb, mint a második érték. |  
|||| 

*Példa*

Ezekben a példákban ellenőrizze, hogy az első érték kisebb vagy egyenlő, mint a második érték.

```
lessOrEquals(10, 10)
lessOrEquals('apply', 'apple')
```

És ezeket az eredményeket adja vissza: 

* Első. példa: `true`
* Második példa: `false`

<a name="listCallbackUrl"></a>

## <a name="listcallbackurl"></a>listCallbackUrl

A "visszahívási URL-címe", amely meghívja az eseményindítók vagy műveletek visszaadása. Ez a függvény csak az eseményindítók és műveletek esetében működik a **HttpWebhook** és **ApiConnectionWebhook** összekötő típusai, de nem a **manuális**,  **Ismétlődési**, **HTTP**, és **APIConnection** típusokat. 

```
listCallbackUrl()
```

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*visszahívási URL-címet*> | Sztring | A visszahívási URL-Címének egy trigger vagy művelet esetén |  
|||| 

*Példa*

Ez a példa bemutatja, hogy ez a függvény vissza egy minta visszahívási URL-címe:

`"https://prod-01.westus.logic.azure.com:443/workflows/<*workflow-ID*>/triggers/manual/run?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<*signature-ID*>"`

<a name="max"></a>

## <a name="max"></a>max.

A legmagasabb érték visszaadása egy listából vagy a tömb számokat, amelyek mindkét végén is beleértve. 

```
max(<number1>, <number2>, ...)
max([<number1>, <number2>, ...])
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szám1*>, <*szám2*>,... | Igen | Egész szám, lebegőpontos vagy mindkettő | A számok, ahonnan a legmagasabb érték beállítása | 
| [<*szám1*>, <*szám2*>,...] | Igen | Tömb - egész szám, lebegőpontos vagy mindkettő | A számtömbből, ahonnan a legmagasabb érték. | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*Max-érték*> | Egész vagy lebegőpontos szám | A megadott tömbben, vagy számokat a legmagasabb érték | 
|||| 

*Példa* 

Ezekben a példákban a legmagasabb érték lekérése a készletből, számokat és a tömb:

```
max(1, 2, 3)
max([1, 2, 3])
```

És ezt az eredményt adja vissza: `3`

<a name="min"></a>

## <a name="min"></a>perc

A legkisebb érték visszaadása számokat vagy a tömböt.

```
min(<number1>, <number2>, ...)
min([<number1>, <number2>, ...])
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szám1*>, <*szám2*>,... | Igen | Egész szám, lebegőpontos vagy mindkettő | A készlet kívánt a legalacsonyabbnál számok | 
| [<*szám1*>, <*szám2*>,...] | Igen | Tömb - egész szám, lebegőpontos vagy mindkettő | A kívánt a legalacsonyabbnál számból álló tömböt | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*Min-érték*> | Egész vagy lebegőpontos szám | A legkisebb érték a megadott készlet számokat vagy a megadott tömb | 
|||| 

*Példa* 

Ezekben a példákban a legkisebb érték a készlet, számokat és a tömb első:

```
min(1, 2, 3)
min([1, 2, 3])
```

És ezt az eredményt adja vissza: `1`

<a name="mod"></a>

## <a name="mod"></a>MOD

Két szám hányadosát adja vissza a maradékot. Az egész típusú eredményként lekéréséhez lásd: [div()](#div).

```
mod(<dividend>, <divisor>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*osztandó*> | Igen | Egész vagy lebegőpontos szám | A szám, mellyel osztani a *osztó* | 
| <*osztó*> | Igen | Egész vagy lebegőpontos szám | A szám, amely elosztja a *osztandó*, de nem lehet 0. | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*Maradékos osztás eredménye*> | Egész vagy lebegőpontos szám | A második szám szerint az első szám osztásával kapott maradékot | 
|||| 

*Példa* 

Ebben a példában az első szám, a második szám alapján osztja fel:

```
mod(3, 2)
```

És ezt az eredményt adja vissza: `1`

<a name="mul"></a>

## <a name="mul"></a>MUL számú

A termék vissza a két szám szorzásának.

```
mul(<multiplicand1>, <multiplicand2>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*multiplicand1*> | Igen | Egész vagy lebegőpontos szám | A szám szorzása a következővel *multiplicand2* | 
| <*multiplicand2*> | Igen | Egész vagy lebegőpontos szám | A szám, amely több *multiplicand1* | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*a termék-eredmény*> | Egész vagy lebegőpontos szám | A termék, a második szám szerint az első szám szorzásának | 
|||| 

*Példa* 

Ezekben a példákban több az első szám, a második szám szerint:

```
mul(1, 2)
mul(1.5, 2)
```

És ezeket az eredményeket adja vissza:

* Első. példa: `2`
* Második példa `3`

<a name="multipartBody"></a>

## <a name="multipartbody"></a>multipartBody

Egy adott rész törzsét visszaadása egy művelet kimenete, amely több részből áll.

```
multipartBody('<actionName>', <index>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Műveletnév*> | Igen | Sztring | A művelet, amely rendelkezik a több részből kimeneti neve | 
| <*Index*> | Igen | Egész szám | Az index értéke a kívánt részt | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*Törzs*> | Sztring | A megadott rész törzsét | 
|||| 

<a name="not"></a>

## <a name="not"></a>nem

Ellenőrizze, hogy egy kifejezés false (hamis). Igaz értéket ad vissza, ha a kifejezés értéke HAMIS, vagy visszatérhet a hamis értéket, ha az értéke igaz.

```
not(<expression>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*kifejezés*> | Igen | Logikai | Ellenőrizze, hogy a kifejezés | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis | Logikai | Igaz értéket ad vissza, ha a kifejezés értéke hamis. Vissza a False (hamis), ha a kifejezés igaz. |  
|||| 

*1. példa*

Ezekben a példákban ellenőrzése, hogy a megadott kifejezés false (hamis): 

```
not(false)
not(true)
```

És ezeket az eredményeket adja vissza:

* Első példa: A kifejezés hamis, nem, így a függvény `true`.
* Második példa: A kifejezés értéke igaz, hogy vissza a függvény `false`.

*2. példa*

Ezekben a példákban ellenőrzése, hogy a megadott kifejezés false (hamis): 

```
not(equals(1, 2))
not(equals(1, 1))
```

És ezeket az eredményeket adja vissza:

* Első példa: A kifejezés hamis, nem, így a függvény `true`.
* Második példa: A kifejezés értéke igaz, hogy vissza a függvény `false`.

<a name="or"></a>

## <a name="or"></a>vagy

Ellenőrizze, hogy legalább egy kifejezés igaz. Igaz értéket ad vissza, ha legalább egy kifejezés értéke true, vagy vissza false (hamis), amikor az összes false (hamis).

```
or(<expression1>, <expression2>, ...)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Kifejezés1*>, <*Kifejezés2*>,... | Igen | Logikai | Ellenőrizze a kifejezések | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis | Logikai | Igaz értéket ad vissza, ha legalább egy kifejezés értéke true. Vissza a False (hamis), amikor összes kifejezés false (hamis). |  
|||| 

*1. példa*

Ezekben a példákban ellenőrizze, hogy legalább egy kifejezés igaz:

```
or(true, false)
or(false, false)
```

És ezeket az eredményeket adja vissza:

* Első példa: legalább egy kifejezés értéke igaz, hogy vissza a függvény `true`.
* Második példa: mindkét kifejezés false (hamis),, így a függvény `false`.

*2. példa*

Ezekben a példákban ellenőrizze, hogy legalább egy kifejezés igaz:

```
or(equals(1, 1), equals(1, 2))
or(equals(1, 2), equals(1, 3))
```

És ezeket az eredményeket adja vissza:

* Első példa: legalább egy kifejezés értéke igaz, hogy vissza a függvény `true`.
* Második példa: mindkét kifejezés false (hamis),, így a függvény `false`.

<a name="parameters"></a>

## <a name="parameters"></a>paraméterek

A logikai alkalmazás definíciójában leírt paraméter értékének visszaadása. 

```
parameters('<parameterName>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*parameterName*> | Igen | Sztring | A nevet a paraméterhez, melynek az értéke | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*a paraméter-érték*> | Bármelyik | A megadott paraméter értéke | 
|||| 

*Példa* 

Tegyük fel, hogy a JSON-értéket:

```json
{
  "fullName": "Sophia Owen"
}
```

Ez a példa lekérdezi a megadott paraméter értékét:

```
parameters('fullName')
```

És ezt az eredményt adja vissza: `"Sophia Owen"`

<a name="rand"></a>

## <a name="rand"></a>rand

Véletlenszerű egész szám visszaadása egy megadott tartományt, amely csak a kezdő végén is beleértve.

```
rand(<minValue>, <maxValue>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*a minValue*> | Igen | Egész szám | A legkisebb egész szám | 
| <*maxValue*> | Igen | Egész szám | A legnagyobb egész szám, amely a függvénynek a következő egész szám | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*véletlenszerű eredménye*> | Egész szám | A véletlenszerű egész számot adja vissza a megadott tartomány |  
|||| 

*Példa*

Ebben a példában egy véletlenszerű egész számot olvas be a megadott tartomány, kivéve a maximális érték: 

```
rand(1, 5)
```

És számot adja vissza az eredményt: `1`, `2`, `3`, vagy `4` 

<a name="range"></a>

## <a name="range"></a>Címtartomány

Adja vissza, amely elindítja a megadott egész számnak egész számok tömbje.

```
range(<startIndex>, <count>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*startIndex*> | Igen | Egész szám | Az egész szám, amely a indítja el a tömb első eleme | 
| <*Száma*> | Igen | Egész szám | A tömbben található egész számok száma | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| [<*tartomány-eredmény*>] | Tömb | Az egész számok, a megadott index a tömb |  
|||| 

*Példa*

Ez a példa létrehoz egy egész számok tömbje, amely a megadott index indul, és rendelkezik a megadott számú egész számokat:

```
range(1, 4)
```

És ezt az eredményt adja vissza: `[1, 2, 3, 4]`

<a name="replace"></a>

## <a name="replace"></a>cserélje le

Cserélje le a megadott karakterlánc részkarakterláncot, és az eredményül kapott karakterláncban adja vissza. Ez a funkció akkor kis-és nagybetűket.

```
replace('<text>', '<oldText>', '<newText>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szöveg*> | Igen | Sztring | A karakterlánc, amely rendelkezik a karakterláncrészletet | 
| <*oldText*> | Igen | Sztring | A karakterláncrészletet | 
| <*newText*> | Igen | Sztring | A behelyettesítendő karakterlánc | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*frissített szöveg*> | Sztring | A frissített karakterláncot a substring lecserélése után <p>Ha a substring nem található, adja vissza az eredeti karakterláncot. | 
|||| 

*Példa* 

Ebben a példában megkeresi a "régi" substring "old string", és lecseréli az "új" a "régi": 

```
replace('the old string', 'old', 'new')
```

És ezt az eredményt adja vissza: `"the new string"`

<a name="removeProperty"></a>

## <a name="removeproperty"></a>removeProperty

Tulajdonság eltávolítása egy objektumot, és a frissített objektumot ad vissza.

```
removeProperty(<object>, '<property>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Objektum*> | Igen | Objektum | Ha el kívánja távolítani a tulajdonságot a JSON-objektum | 
| <*A tulajdonság*> | Igen | Sztring | Az eltávolítandó tulajdonság nevét | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*frissített objektum*> | Objektum | A frissített JSON-objektum anélkül, hogy a megadott tulajdonság | 
|||| 

*Példa*

Ebben a példában eltávolítjuk a `"accountLocation"` tulajdonságot egy `"customerProfile"` objektum, amely a JSON-t alakítja át a [JSON()](#json) függvényt, és a frissített objektumot ad vissza:

```
removeProperty(json('customerProfile'), 'accountLocation')
```

<a name="setProperty"></a>

## <a name="setproperty"></a>setProperty

Egy objektum tulajdonság értékét, és a frissített objektumot ad vissza. Adjon hozzá egy új tulajdonság, használhatja ezt a funkciót, vagy a [addProperty()](#addProperty) függvény.

```
setProperty(<object>, '<property>', <value>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Objektum*> | Igen | Objektum | A JSON-objektum szeretné állítani, amelynek tulajdonsága | 
| <*A tulajdonság*> | Igen | Sztring | A meglévő vagy új beállítandó tulajdonság nevét | 
| <*Érték*> | Igen | Bármelyik | Az érték a megadott tulajdonság beállítása |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*frissített objektum*> | Objektum | A frissített JSON-objektum, amelynek tulajdonsága | 
|||| 

*Példa*

Ebben a példában beállítja a `"accountNumber"` tulajdonsága egy `"customerProfile"` objektum, amely a JSON-t alakítja át a [JSON()](#json) függvény. A függvény által létrehozott értéket rendel [guid()](#guid) függvényt, és a frissített JSON-objektumot ad vissza:

```
setProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="skip"></a>

## <a name="skip"></a>Kihagyás

A gyűjtemény elejéről eltávolítandó elemek, és vissza *összes többi* elemek.

```
skip([<collection>], <count>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Gyűjtemény*> | Igen | Tömb | A gyűjtemény, amelynek el kívánja távolítani elemek | 
| <*Száma*> | Igen | Egész szám | Távolítsa el elöl az elemek számának pozitív egész szám | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| [<*frissített gyűjtemény*>] | Tömb | A megadott elemek eltávolítása után a frissített gyűjtemény | 
|||| 

*Példa*

Ebben a példában egy elem, a szám 0, a megadott tömb elejéről távolítja el: 

```
skip([0, 1, 2, 3], 1)
```

Ez a fennmaradó elemek tömböt ad vissza, és: `[1,2,3]`

<a name="split"></a>

## <a name="split"></a>felosztás

A visszaadandó egy tömb, amely egy karakterlánc összes karakter pedig minden karakter választja el egy *elválasztó*.

```
split('<text>', '<separator>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szöveg*> | Igen | Sztring | A karakterlánc, amely rendelkezik felosztása a karakterek |  
| <*elválasztó*> | Igen | Sztring | Az elválasztó, amely megjelenik az eredményül kapott tömbben található karakterek közé | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| [<*char1*><*elválasztó*><*char2*><*elválasztó*>...] | Tömb | Az eredményül kapott tömb a megadott karakterlánc megtalálható összes elemet alapján létrehozott |
|||| 

*Példa* 

Ebben a példában egy tömb a megadott karakterlánc, és megadhat minden karakter, mint az elválasztó vessző hoz létre:

```
split('abc', ',')
```

És ezt az eredményt adja vissza: `[a, b, c]`

<a name="startOfDay"></a>

## <a name="startofday"></a>startOfDay

Időbélyeg nap visszaadása. 

```
startOfDay('<timestamp>', '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | A megadott időbélyeg, de az óra jel napi díjért | 
|||| 

*Példa* 

Ebben a példában találja az időbélyegző nap kezdete:

```
startOfDay('2018-03-15T13:30:30Z')
```

És ezt az eredményt adja vissza: `"2018-03-15T00:00:00.0000000Z"`

<a name="startOfHour"></a>

## <a name="startofhour"></a>startOfHour

Az időbélyeg az óra kezdetét adja vissza. 

```
startOfHour('<timestamp>', '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | A megadott időbélyeg, de a nulla perces be van jelölve, az óra díjért | 
|||| 

*Példa* 

Ebben a példában az időbélyeg az óra kezdetét talál:

```
startOfHour('2018-03-15T13:30:30Z')
```

És ezt az eredményt adja vissza: `"2018-03-15T13:00:00.0000000Z"`

<a name="startOfMonth"></a>

## <a name="startofmonth"></a>startOfMonth

Az időbélyeg a hónap kezdetét adja vissza. 

```
startOfMonth('<timestamp>', '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | A megadott időbélyeg, de az óra be van jelölve, a hónap első napjától kezdve | 
|||| 

*Példa* 

Ebben a példában az időbélyegző a hónap kezdetét adja vissza:

```
startOfMonth('2018-03-15T13:30:30Z')
```

És ezt az eredményt adja vissza: `"2018-03-01T00:00:00.0000000Z"`

<a name="startswith"></a>

## <a name="startswith"></a>startsWith

Annak ellenőrzése, hogy e karakterlánc kezdődik-e egy adott karakterláncrészletet. Igaz értéket ad vissza, ha a substring található, vagy visszatérhet false (hamis) Ha nem található. Ez a funkció nem áll kis-és nagybetűket.

```
startsWith('<text>', '<searchText>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szöveg*> | Igen | Sztring | A karakterlánc | 
| <*Keresettszöveg*> | Igen | Sztring | A kezdő karakterlánc keresése | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| IGAZ vagy hamis  | Logikai | Igaz értéket ad vissza, ha a kiindulási karakterláncrészletet található. Vissza false (hamis) Ha nem található. | 
|||| 

*1. példa* 

Ebben a példában ellenőrzi, hogy a "hello world" karakterlánc a "hello" karakterláncrészletet indításakor:

```
startsWith('hello world', 'hello')
```

És ezt az eredményt adja vissza: `true`

*2. példa*

Ebben a példában ellenőrzi, hogy a "hello world" karakterlánc a "hónap" karakterláncrészletet indításakor:

```
startsWith('hello world', 'greetings')
```

És ezt az eredményt adja vissza: `false`

<a name="string"></a>

## <a name="string"></a>sztring

A karakterlánc-verziót egy értéket ad vissza.

```
string(<value>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Bármelyik | Az átalakítandó érték | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*karakterlánc-érték*> | Sztring | A megadott érték a karakterlánc verziójára | 
|||| 

*1. példa* 

Ebben a példában ez a szám a karakterlánc verziót hoz létre:

```
string(10)
```

És ezt az eredményt adja vissza: `"10"`

*2. példa*

Ebben a példában egy karakterláncot a megadott JSON-objektumot hoz létre, illetve a fordított perjel karaktert (\\) a dupla idézőjel (") helyettesítő karakterek.

```
string( { "name": "Sophie Owen" } )
```

És ezt az eredményt adja vissza: `"{ \\"name\\": \\"Sophie Owen\\" }"`

<a name="sub"></a>

## <a name="sub"></a>Sub

Az első szám, a második szám kivonásának az eredmény visszaadása.

```
sub(<minuend>, <subtrahend>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*kisebbítendőt*> | Igen | Egész vagy lebegőpontos szám | A szám, amelyből ki szeretnénk vonni a *kivonandót* | 
| <*kivonandót*> | Igen | Egész vagy lebegőpontos szám | A kivonni kívánt a szám a *kisebbítendőt* | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*Eredmény*> | Egész vagy lebegőpontos szám | Az első szám, a második szám kivonásának eredménye | 
|||| 

*Példa* 

Ebben a példában a második szám az első szám az kivonja:

```
sub(10.3, .3)
```

És ezt az eredményt adja vissza: `10`

<a name="substring"></a>

## <a name="substring"></a>karakterláncrészlet

Karaktert adja vissza egy karakterláncból, kezdve a megadott pozíciónál, vagy az index. Értékek start index 0 számmal. 

```
substring('<text>', <startIndex>, <length>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szöveg*> | Igen | Sztring | A karakterláncot, amelynek kívánt karakterek | 
| <*startIndex*> | Igen | Egész szám | A kezdő pozíció és az index értéke pozitív szám | 
| <*Hossza*> | Igen | Egész szám | Pozitív szám, amelyet szeretne a substring karakter | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*karakterláncrészlet-eredmény*> | Sztring | A megadott számú karaktert, a forrás karakterláncban megadott pozíciótól kezdődően a karakterláncrész | 
|||| 

*Példa* 

Ebben a példában öt karakterből álló karakterláncrész hoz létre a megadott karakterlánc, az index értéke 6 kezdve:

```
substring('hello world', 6, 5)
```

És ezt az eredményt adja vissza: `"world"`

<a name="subtractFromTime"></a>

## <a name="subtractfromtime"></a>subtractFromTime

Az időbélyeg mértékegységét számos kivonása. Lásd még: [getPastTime](#getPastTime).

```
subtractFromTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | A karakterlánc, amely tartalmazza az időbélyeg | 
| <*időköz*> | Igen | Egész szám | A megadott időegység kivonandó száma | 
| <*timeUnit*> | Igen | Sztring | Az időegység használata *időköz*: "A második", "Minute", "Hour", "Day", "Week", "Month", "Year" | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*időbélyeg frissítése*> | Sztring | A mínusz a megadott számú alkalommal egység időbélyegző | 
|||| 

*1. példa*

Ebben a példában kivonja az időbélyegző egy nap:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day') 
```

És ezt az eredményt adja vissza: `"2018-01-01T00:00:00:0000000Z"`

*2. példa*

Ebben a példában kivonja az időbélyegző egy nap:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day', 'D') 
```

És ez nem kötelező "D" formátumban eredményt adja vissza: `"Monday, January, 1, 2018"`

<a name="take"></a>

## <a name="take"></a>hajtsa végre a megfelelő

Az első gyűjtemény elemek visszaadása. 

```
take('<collection>', <count>)
take([<collection>], <count>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Gyűjtemény*> | Igen | Karakterlánc- vagy tömb | A gyűjtemény, amelynek kívánt elemeket | 
| <*Száma*> | Igen | Egész szám | Az előtérben lévő elemek számának pozitív egész szám | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*részhalmazát*> vagy [<*részhalmazát*>] | Karakterlánc- vagy tömböt, illetve | Egy karakterlánc vagy egy tömb, amely rendelkezik a megadott számú elemet az eredeti gyűjtemény elejéről hozott | 
|||| 

*Példa*

Ezekben a példákban a megadott számú elemet le ezeket a gyűjteményeket elejéhez:

```
take('abcde`, 3)
take([0, 1, 2, 3, 4], 3)
```

És ezeket az eredményeket adja vissza:

* Első. példa: `"abc"`
* Második példa: `[0, 1, 2]`

<a name="ticks"></a>

## <a name="ticks"></a>órajel

Vissza a `ticks` tulajdonság értéke a megadott időbélyeg. A *osztásjelek* van egy 100 nanoszekundumos időszak.

```
ticks('<timestamp>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Időbélyeg*> | Igen | Sztring | Az időbélyeg karakterlánc | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*órajel során végbemenő-szám*> | Egész szám | A megadott időbélyeg óta számát | 
|||| 

<a name="toLower"></a>

## <a name="tolower"></a>toLower

Kis formátumban adja vissza. Egy karaktert a karakterlánc nem rendelkezik egy kis verziójával, ha adott karaktert a visszaadott karakterláncban változatlan marad.

```
toLower('<text>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szöveg*> | Igen | Sztring | A karakterlánc kis formátumban adja vissza | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*kisbetűk-szöveg*> | Sztring | Az eredeti karakterláncot kisbetűssé formátumban | 
|||| 

*Példa* 

Ebben a példában ez a karakterlánc kisbetűssé alakítja át: 

```
toLower('Hello World')
```

És ezt az eredményt adja vissza: `"hello world"`

<a name="toUpper"></a>

## <a name="toupper"></a>toUpper

Nagybetűk formátumban adja vissza. Ha egy karaktert a karakterlánc nem tartalmaz egy nagybetűs verzióját, adott karaktert a visszaadott karakterláncban változatlan marad.

```
toUpper('<text>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szöveg*> | Igen | Sztring | A karakterlánc nagybetűssé formátumban adja vissza | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*nagybetűk szöveg*> | Sztring | Az eredeti karakterláncot nagybetűssé formátumban | 
|||| 

*Példa* 

Ebben a példában ez a karakterlánc nagybetűssé alakítja át:

```
toUpper('Hello World')
```

És ezt az eredményt adja vissza: `"HELLO WORLD"`

<a name="trigger"></a>

## <a name="trigger"></a>Az eseményindító

Egyéb JSON név-érték párok, hozzárendelheti egy kifejezés, amely egy trigger kimenetéből runtime vagy az értékek visszaadásához. 

* Belül egy trigger bemenetei között ez a függvény kimenete az előző végrehajtás adja vissza. 

* Belül egy indítási feltétel a függvény kimenete az aktuális végrehajtási adja vissza. 

Alapértelmezés szerint a függvény a teljes eseményindító objektumra hivatkozik, de igény szerint adjon meg egy tulajdonságot, amely azt szeretné, amelynek értékét. Is ez a függvény rendelkezik gyorsírás verzióiban elérhető, lásd: [triggerOutputs()](#triggerOutputs) és [triggerBody()](#triggerBody). 

```
trigger()
```

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*eseményindító-kimenet*> | Sztring | Futásidőben trigger kimenete | 
|||| 

<a name="triggerBody"></a>

## <a name="triggerbody"></a>triggerBody

Egy eseményindító vissza `body` kimeneti futásidőben. A gyorsírás `trigger().outputs.body`. Lásd: [trigger()](#trigger). 

```
triggerBody()
```

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*eseményindító-törzs-kimenet*> | Sztring | A `body` a trigger kimenete | 
|||| 

<a name="triggerFormDataMultiValues"></a>

## <a name="triggerformdatamultivalues"></a>triggerFormDataMultiValues

A kulcs nevét, egy eseményindítót a megfelelő értékekkel egy tömböt adnak vissza *űrlapadatokból* vagy *űrlapként* kimeneti. 

```
triggerFormDataMultiValues('<key>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Kulcs*> | Igen | Sztring | A használni kívánt kulcs neve | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| [<*tömb a kulcs értékeit*>] | Tömb | Egy tömb azokra az értékekre, amelyek megfelelnek a megadott kulcs | 
|||| 

*Példa* 

Ez a példa létrehoz egy tömböt egy RSS-trigger űrlapadatokból vagy a kimeneti űrlapként "feedUrl" kulcs értékét: 

```
triggerFormDataMultiValues('feedUrl')
```

És ezt a tömböt egy példa eredményt adja vissza: `["http://feeds.reuters.com/reuters/topNews"]`

<a name="triggerFormDataValue"></a>

## <a name="triggerformdatavalue"></a>triggerFormDataValue

Az egyetlen érték, amely megfelel egy eseményindítót a kulcs nevét adja vissza *űrlapadatokból* vagy *űrlapként* kimeneti. Ha a függvény legalább egy egyezést talál, a függvény hibát jelez.

```
triggerFormDataValue('<key>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Kulcs*> | Igen | Sztring | A használni kívánt kulcs neve |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*kulcs-érték*> | Sztring | A megadott kulcs értéke | 
|||| 

*Példa* 

Ebben a példában a karakterlánc a "feedUrl" kulcs értékét egy RSS-trigger űrlapadatokból vagy a kimeneti űrlapként hoz létre:

```
triggerFormDataValue('feedUrl')
```

És ez a karakterlánc egy példa eredményt adja vissza: `"http://feeds.reuters.com/reuters/topNews"` 

<a name="triggerMultipartBody"></a>

Egy adott rész törzsét visszaadása egy trigger kimenete, amely több részből áll. 

```
triggerMultipartBody(<index>)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Index*> | Igen | Egész szám | Az index értéke a kívánt részt |
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*Törzs*> | Sztring | A trigger többrészes kimenetéből az a megadott rész törzsét | 
|||| 

<a name="triggerOutputs"></a>

## <a name="triggeroutputs"></a>triggerOutputs

A trigger kimenetéből runtime vagy az értékek visszaadásához más JSON-név és érték párokat. A gyorsírás `trigger().outputs`. Lásd: [trigger()](#trigger). 

```
triggerOutputs()
```

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*eseményindító-kimenet*> | Sztring | Futásidőben trigger kimenete  | 
|||| 

<a name="trim"></a>

## <a name="trim"></a>Trim

Kezdő és záró szóközök eltávolítása a karakterláncot, és a frissített karakterláncot ad vissza.

```
trim('<text>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Szöveg*> | Igen | Sztring | A karakterlánc, amely rendelkezik a kezdő és záró szóközök eltávolítása | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*updatedText*> | Sztring | Az eredeti karakterláncot kezdő vagy záró szóközök nélkül a frissített verzió | 
|||| 

*Példa* 

Ebben a példában a kezdő és záró szóközök eltávolítása a "Hello World" karakterlánc:  

```
trim(' Hello World  ')
```

És ezt az eredményt adja vissza: `"Hello World"`

<a name="union"></a>

## <a name="union"></a>Union

Vissza, amely rendelkezik *összes* elemet a megadott gyűjteményekkel a. Az eredmény jelenik meg, hogy egy elem egy gyűjteményt a függvénynek átadott is megjelennek. Ha egy vagy több elemet ugyanazzal a névvel rendelkezik, az eredmény ilyen nevű legutóbbi elem jelenik meg. 

```
union('<collection1>', '<collection2>', ...)
union([<collection1>], [<collection2>], ...)
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*collection1*>, <*2. gyűjtemény*>,...  | Igen | Tömb vagy objektum, de nem mindkettőt | A gyűjtemények, ahonnan csak szeretné *összes* elemek | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*updatedCollection*> | Olyan tömböt vagy objektumot, illetve | A megadott gyűjteményekkel – nincsenek ismétlődések elemeivel rendelkező gyűjtemények | 
|||| 

*Példa* 

Ez a példa lekéri *összes* elemet a következő gyűjteményekhez: 

```
union([1, 2, 3], [1, 2, 10, 101])
```

És ezt az eredményt adja vissza: `[1, 2, 3, 10, 101]`

<a name="uriComponent"></a>

## <a name="uricomponent"></a>uriComponent

Egy karakterláncot egy egységes erőforrás-azonosító (URI) kódolású verzió vissza lecserélésével URL-címekben nem biztonságos karaktereket escape-karaktereket. Ez a függvény helyett [encodeUriComponent()](#encodeUriComponent). Mindkét függvény a ugyanúgy működnek, de `uriComponent()` részesíti előnyben.

```
uriComponent('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az URI-ként kódolt formában alakítandó karakterláncot | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*uri-ként kódolt*> | Sztring | Az URI-ként kódolt karakterlánc escape-karakterrel | 
|||| 

*Példa*

Ez a példa létrehoz egy URI-ként kódolt verzió a következő karakterláncot:

```
uriComponent('https://contoso.com')
```

És ezt az eredményt adja vissza: `"http%3A%2F%2Fcontoso.com"`

<a name="uriComponentToBinary"></a>

## <a name="uricomponenttobinary"></a>uriComponentToBinary

A bináris verzió egységes erőforrás-azonosító (URI) összetevő visszaadása.

```
uriComponentToBinary('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az URI-ként kódolt karakterlánc konvertálása | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*bináris-az-kódolású – uri*> | Sztring | Az URI-ként kódolt karakterlánc bináris verziószáma. A bináris tartalmat a base64-kódolású és által képviselt `$content`. | 
|||| 

*Példa*

Ebben a példában a ez URI-ként kódolt karakterlánc bináris verziót hoz létre: 

```
uriComponentToBinary('http%3A%2F%2Fcontoso.com')
```

És ezt az eredményt adja vissza: 

`"001000100110100001110100011101000111000000100101001100
11010000010010010100110010010001100010010100110010010001
10011000110110111101101110011101000110111101110011011011
110010111001100011011011110110110100100010"`

<a name="uriComponentToString"></a>

## <a name="uricomponenttostring"></a>uriComponentToString

Visszaadja a karakterláncot egy egységes erőforrás-azonosítója (URI) verziót-ként kódolt karakterlánc, hatékonyan az URI-ként kódolt karakterlánc dekódolása.

```
uriComponentToString('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | Az URI-ként kódolt karakterlánc dekódolása | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*uri dekódovat.*> | Sztring | A dekódolt verzió az URI-ként kódolt karakterlánc | 
|||| 

*Példa*

Ebben a példában a dekódolt karakterlánc verzió a következő URI-ként kódolt karakterláncot hoz létre: 

```
uriComponentToString('http%3A%2F%2Fcontoso.com')
```

És ezt az eredményt adja vissza: `"https://contoso.com"` 

<a name="uriHost"></a>

## <a name="urihost"></a>uriHost

Vissza a `host` egy egységes erőforrás-azonosítója (URI) értékét.

```
uriHost('<uri>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*URI-t*> | Igen | Sztring | Az URI-t amelynek `host` kívánt érték | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*gazdagép-érték*> | Sztring | A `host` a megadott URI azonosító értékét | 
|||| 

*Példa*

Ebben a példában megkeresi a `host` ezt az URI értékét: 

```
uriHost('https://www.localhost.com:8080')
```

És ezt az eredményt adja vissza: `"www.localhost.com"`

<a name="uriPath"></a>

## <a name="uripath"></a>uriPath

Vissza a `path` egy egységes erőforrás-azonosítója (URI) értékét. 

```
uriPath('<uri>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*URI-t*> | Igen | Sztring | Az URI-t amelynek `path` kívánt érték | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*elérési út-érték*> | Sztring | A `path` a megadott URI azonosító értékét. Ha `path` nem rendelkezik értékkel, a "/" karaktert adja vissza. | 
|||| 

*Példa*

Ebben a példában megkeresi a `path` ezt az URI értékét: 

```
uriPath('http://www.contoso.com/catalog/shownew.htm?date=today')
```

És ezt az eredményt adja vissza: `"/catalog/shownew.htm"`

<a name="uriPathAndQuery"></a>

## <a name="uripathandquery"></a>uriPathAndQuery

Vissza a `path` és `query` egy egységes erőforrás-azonosítója (URI) értékeket.

```
uriPathAndQuery('<uri>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*URI-t*> | Igen | Sztring | Az URI-t amelynek `path` és `query` kívánt értékek | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*érték lekérdezése elérési útja*> | Sztring | A `path` és `query` értékeket a megadott URI azonosító. Ha `path` nem adjon meg egy értéket, a "/" karaktert adja vissza. | 
|||| 

*Példa*

Ebben a példában megkeresi a `path` és `query` ezt az URI értékét:

```
uriPathAndQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

És ezt az eredményt adja vissza: `"/catalog/shownew.htm?date=today"`

<a name="uriPort"></a>

## <a name="uriport"></a>uriPort

Vissza a `port` egy egységes erőforrás-azonosítója (URI) értékét.

```
uriPort('<uri>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*URI-t*> | Igen | Sztring | Az URI-t amelynek `port` kívánt érték | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*port-érték*> | Egész szám | A `port` a megadott URI azonosító értékét. Ha `port` nem adjon meg egy értéket, az alapértelmezett port a protokoll adja vissza. | 
|||| 

*Példa*

Ebben a példában adja vissza a `port` ezt az URI értékét:

```
uriPort('http://www.localhost:8080')
```

És ezt az eredményt adja vissza: `8080`

<a name="uriQuery"></a>

## <a name="uriquery"></a>uriQuery

Vissza a `query` egy egységes erőforrás-azonosítója (URI) értékét.

```
uriQuery('<uri>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*URI-t*> | Igen | Sztring | Az URI-t amelynek `query` kívánt érték | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*érték lekérdezése*> | Sztring | A `query` a megadott URI azonosító értékét | 
|||| 

*Példa*

Ebben a példában adja vissza a `query` ezt az URI értékét: 

```
uriQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

És ezt az eredményt adja vissza: `"?date=today"`

<a name="uriScheme"></a>

## <a name="urischeme"></a>uriScheme

Vissza a `scheme` egy egységes erőforrás-azonosítója (URI) értékét.

```
uriScheme('<uri>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*URI-t*> | Igen | Sztring | Az URI-t amelynek `scheme` kívánt érték | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*séma-érték*> | Sztring | A `scheme` a megadott URI azonosító értékét | 
|||| 

*Példa*

Ebben a példában adja vissza a `scheme` ezt az URI értékét:

```
uriScheme('http://www.contoso.com/catalog/shownew.htm?date=today')
```

És ezt az eredményt adja vissza: `"http"`

<a name="utcNow"></a>

## <a name="utcnow"></a>utcNow

Az aktuális időbélyeget adja vissza. 

```
utcNow('<format>')
```

Szükség esetén megadhat más formátumba való a <*formátum*> paraméter. 


| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Formátum*> | Nem | Sztring | Vagy egy [egyetlen formátummegadó](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) vagy egy [egyéni Formátumminta](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Az alapértelmezett az időbélyeg formátuma ["ó"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (éééé-hh-ddT:mm:ss:fffffffK), amely megfelel az [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) és megőrzi az időzóna-információkat. | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*jelenlegi-időbélyeg*> | Sztring | Az aktuális dátum és idő | 
|||| 

*1. példa*

Tegyük fel, hogy ma 2018. április 15. 1:00:00 PM. Ez a példa lekérdezi az aktuális timestamp: 

```
utcNow()
```

És ezt az eredményt adja vissza: `"2018-04-15T13:00:00.0000000Z"`

*2. példa*

Tegyük fel, hogy ma 2018. április 15. 1:00:00 PM. Ez a példa lekérdezi az aktuális időbélyeget, a nem kötelező "D" formátumban:

```
utcNow('D')
```

És ezt az eredményt adja vissza: `"Sunday, April 15, 2018"`

<a name="variables"></a>

## <a name="variables"></a>Változók

A megadott változó értékének visszaadása. 

```
variables('<variableName>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*variableName*> | Igen | Sztring | A használni kívánt változó nevét | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*változó-érték*> | Bármelyik | A megadott változó értékét | 
|||| 

*Példa*

Tegyük fel, hogy egy "numItems" változó aktuális értéke 20. Ez a példa lekérdezi az egész értéket a változót:

```
variables('numItems')
```

És ezt az eredményt adja vissza: `20`

<a name="workflow"></a>

## <a name="workflow"></a>munkafolyamat

Magáról a munkafolyamatról részleteinek vissza a futtatási idő alatt. 

```
workflow().<property>
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*A tulajdonság*> | Nem | Sztring | A munkafolyamat tulajdonság, melynek az értéke neve <p>Egy munkafolyamat-objektum rendelkezik, ezeket a tulajdonságokat: **neve**, **típus**, **azonosító**, **hely**, és **futtatása**. A **futtatása** tulajdonság értéke is olyan objektum, amely rendelkezik, ezeket a tulajdonságokat: **neve**, **típus**, és **azonosító**. | 
||||| 

*Példa*

Ebben a példában az aktuális egy munkafolyamat-Futtatás nevét adja vissza:

```
workflow().run.name
```

<a name="xml"></a>

## <a name="xml"></a>xml

Az XML-verzió, a JSON-objektum tartalmazó karakterláncot ad vissza. 

```
xml('<value>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*Érték*> | Igen | Sztring | A karakterlánc, konvertálja a JSON-objektum <p>A JSON-objektum csak egy legfelső szintű tulajdonsággal kell rendelkeznie. <br>Használja a fordított perjel karaktert (\\) a dupla idézőjel (") helyettesítő karakterek. | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*XML-verzió*> | Objektum | A megadott karakterlánc vagy JSON-objektum a kódolt XML | 
|||| 

*1. példa*

Ebben a példában az XML-verzió, a ezt a karakterláncot, amely tartalmaz egy JSON-objektumot hoz létre: 

`xml( '{ \"name\": \"Sophia Owen\" }' )`

És ez XML eredményt adja vissza: 

```xml
<name>Sophia Owen</name>
```

*2. példa*

Tegyük fel, a JSON-objektum:

```json
{ 
  "person": { 
    "name": "Sophia Owen", 
    "city": "Seattle" 
  } 
}
```

Ebben a példában a karakterlánc, amely tartalmazza a JSON-objektum XML hoz létre:

`xml( '{ \"person\": { \"name\": \"Sophia Owen\", \"city\": \"Seattle\" } }' )`

És ez XML eredményt adja vissza: 

```xml
<person>
  <name>Sophia Owen</name>
  <city>Seattle</city>
<person>
```

<a name="xpath"></a>

## <a name="xpath"></a>XPath

XML ellenőrizze csomópontok és a egy (XML Path Language) XPath kifejezés megfelelő értékeket, és a megfelelő csomópontok vagy értékeket adnak vissza. Az XPath-kifejezés, vagy csak "XPath", segítségével keresse meg egy XML-dokumentum struktúra, így választhat, hogy milyen csomópontok vagy számítási értékek XML-tartalomban található.

```
xpath('<xml>', '<xpath>')
```

| Paraméter | Szükséges | Típus | Leírás | 
| --------- | -------- | ---- | ----------- | 
| <*XML*> | Igen | Bármelyik | Az XML-karakterlánc csomópontok és a egy XPath kifejezés értéke megfelelő értékeket keresése | 
| <*XPath*> | Igen | Bármelyik | A megfelelő XML-csomópontnak vagy értékek kereséséhez használt XPath-kifejezés | 
||||| 

| Vrácená hodnota | Típus | Leírás | 
| ------------ | ---- | ----------- | 
| <*XML-csomópont*> | XML | XML-csomópontot csak egyetlen csomópont megegyezik a megadott XPath-kifejezés | 
| <*Érték*> | Bármelyik | Az XML-csomópontot csak egyetlen érték megegyezik a megadott XPath-kifejezés értékét | 
| [<*xml-csomópont1*>, <*xml-csomópont2*>,...] </br>– vagy – </br>[<*érték1*>, <*value2*>,...] | Tömb | Olyan tömböt vagy XML-csomópontnak, amelyek megfelelnek a megadott XPath-kifejezésnek | 
|||| 

*1. példa*

Ebben a példában a megfelelő csomópontok megkeresi a `<name></name>` a megadott argumentumok csomópontja és az adott csomópont értékei tömböt ad vissza: 

`xpath(xml(parameters('items')), '/produce/item/name')`

Az alábbiakban az argumentumok:

* Az "elem" karakterlánc, amely tartalmazza az XML:

  `"<?xml version="1.0"?> <produce> <item> <name>Gala</name> <type>apple</type> <count>20</count> </item> <item> <name>Honeycrisp</name> <type>apple</type> <count>10</count> </item> </produce>"`

  A példában a [parameters()](#parameters) az XML-karakterlánc beolvasása "elem" argumentum a funkciót, de kell is konvertálása a karakterlánc XML formátum használatával az [xml()](#xml) függvény. 

* Az XPath-kifejezés, amelyet karakterláncként:

  `"/produce/item/name"`

Az eredmény tömb azokkal a csomópontokkal, amelyek megfelelnek a következő `<name></name`:

`[ <name>Gala</name>, <name>Honeycrisp</name> ]`

*2. példa*

1. példa az alábbi, ebben a példában megkeresi a megfelelő csomópontok a `<count></count>` csomópontot, és hozzáadja az adott csomópont értékei a `sum()` függvény:

`xpath(xml(parameters('items')), 'sum(/produce/item/count)')`

És ezt az eredményt adja vissza: `30`

*3. példa*

Ebben a példában mindkét kifejezés a megfelelő csomópontok keresse meg a `<location></location>` csomópont található a megadott argumentumok, többek között XML névtér esetében. A kifejezések használata a fordított perjel karaktert (\\) a dupla idézőjel (") helyettesítő karakterek.

* *1. kifejezés*

  `xpath(xml(body('Http')), '/*[name()=\"file\"]/*[name()=\"location\"]')`

* *2 kifejezés* 

  `xpath(xml(body('Http')), '/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]')`

Az alábbiakban az argumentumok:

* Az XML, beleértve az XML-dokumentum névtér, `xmlns="http://contoso.com"`: 

  ```xml
  <?xml version="1.0"?> <file xmlns="http://contoso.com"> <location>Paris</location> </file>
  ```

* Vagy XPath kifejezés itt:

  * `/*[name()=\"file\"]/*[name()=\"location\"]`

  * `/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]`

Íme a eredmény csomópont, amely megfelel a `<location></location` csomópont:

```xml
<location xmlns="https://contoso.com">Paris</location>
```

*4. példa*

A következő példa 3, ebben a példában szereplő érték megkeresi a `<location></location>` csomópont: 

`xpath(xml(body('Http')), 'string(/*[name()=\"file\"]/*[name()=\"location\"])')`

És ezt az eredményt adja vissza: `"Paris"`

## <a name="next-steps"></a>További lépések

További információ a [munkafolyamat-definíciós nyelv](../logic-apps/logic-apps-workflow-definition-language.md)
