---
title: Az Azure Functions – alkalmazásbeállítási referencia
description: Az Azure Functions-alkalmazás beállításai vagy a környezeti változók dokumentációja.
services: functions
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/26/2017
ms.author: glenga
ms.openlocfilehash: b5f4ce786371608b276e41f6881dcb1e0a91e303
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345055"
---
# <a name="app-settings-reference-for-azure-functions"></a>Az Azure Functions – alkalmazásbeállítási referencia

Alkalmazásbeállításokat a függvényalkalmazáshoz, amelyek befolyásolják, hogy a függvényalkalmazás a függvények globális konfigurációs beállításokat tartalmaznak. Ha helyileg futtatja, ezeket a beállításokat a környezeti változók találhatók. Ez a cikk az alkalmazásbeállításokat a függvényalkalmazások sorolja fel.

Egyéb globális konfigurációs lehetőségek a [host.json](functions-host-json.md) fájl és a [local.settings.json](functions-run-local.md#local-settings-file) fájlt.

## <a name="appinsightsinstrumentationkey"></a>ÁLLÍTANI AZ APPINSIGHTS_INSTRUMENTATIONKEY

Az Application Insights kialakítási kulcsot, az Application Insights használata. Lásd: [Azure Functions monitorozása](functions-monitoring.md).

|Kulcs|Mintaérték|
|---|------------|
|ÁLLÍTANI AZ APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Nem kötelező tárfiók kapcsolati sztringje naplók tárolására, és azokat megjelenítése a **figyelő** lapot a portálon. A storage-fiókot, amely támogatja a blobok, üzenetsorok és táblák általános célú egy kell lennie. Lásd: [tárfiók](functions-infrastructure-as-code.md#storage-account) és [Storage-fiókra vonatkozó követelmények](functions-create-function-app-portal.md#storage-account-requirements).

|Kulcs|Mintaérték|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [kulcs]|

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true` azt jelenti, hogy tiltsa le az alapértelmezett kezdőlap függvényalkalmazás gyökér URL-címe jelenik meg. Az alapértelmezett szint a `false`.

|Kulcs|Mintaérték|
|---|------------|
|AzureWebJobsDisableHomepage|true|

Ha ennek az alkalmazásbeállításnak nincs megadva, vagy állítsa `false`, az alábbi példához hasonló lap jelenik meg az URL-címet adott válaszként `<functionappname>.azurewebsites.net`.

![Függvény app kezdőlapja](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true` azt jelenti, hogy kiadási módban használja, ha a .NET-kód; fordítása `false` azt jelenti, hogy a hibakeresési módot használja. Az alapértelmezett szint a `true`.

|Kulcs|Mintaérték|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Bétaverzió funkciók engedélyezéséhez vesszővel tagolt listája. Ezek a jelölők által engedélyezett bétaverzió funkciói nem éles üzemre, de kísérleti használata esetén is engedélyezhető, mielőtt az élő esemény indításra.

|Kulcs|Mintaérték|
|---|------------|
|AzureWebJobsFeatureFlags|feature1, feature2|

## <a name="azurewebjobsscriptroot"></a>AzureWebJobsScriptRoot

A gyökérkönyvtár elérési útja, a *host.json* fájl- és függvény mappák találhatók. A függvényalkalmazás, az alapértelmezett érték `%HOME%\site\wwwroot`.

|Kulcs|Mintaérték|
|---|------------|
|AzureWebJobsScriptRoot|%Home%\site\wwwroot|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Megadja a tárházban vagy a szolgáltató kulcs tárolására. A támogatott adattárak jelenleg blob ("Blob") és a fájlrendszert ("tiltott"). Az alapértelmezett érték ("tiltott") fájlrendszer.

|Kulcs|Mintaérték|
|---|------------|
|AzureWebJobsSecretStorageType|letiltva|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

Az Azure Functions runtime a tárfiók kapcsolati sztringje HTTP által aktivált függvények kivételével az összes függvényt használja. A storage-fiókot, amely támogatja a blobok, üzenetsorok és táblák általános célú egy kell lennie. Lásd: [tárfiók](functions-infrastructure-as-code.md#storage-account) és [Storage-fiókra vonatkozó követelmények](functions-create-function-app-portal.md#storage-account-requirements).

|Kulcs|Mintaérték|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [kulcs]|

## <a name="azurewebjobstypescriptpath"></a>AzureWebJobs_TypeScriptPath

A használt TypeScript-fordítóprogram elérési útja. Ha szeretné az alapértelmezett beállítás felülbírálásával teszi lehetővé.

|Kulcs|Mintaérték|
|---|------------|
|AzureWebJobs_TypeScriptPath|%Home%\typescript|

## <a name="functionappeditmode"></a>FÜGGVÉNY\_ALKALMAZÁS\_SZERKESZTÉSE\_MÓD

Érvényes értékek: "readwrite" és "csak olvasható".

|Kulcs|Mintaérték|
|---|------------|
|FÜGGVÉNY\_ALKALMAZÁS\_SZERKESZTÉSE\_MÓD|csak olvasható|

## <a name="functionsextensionversion"></a>FÜGGVÉNYEK\_BŐVÍTMÉNY\_VERZIÓ

A függvényalkalmazás használata az Azure Functions futtatókörnyezettel verziója. A főverzió tilde azt jelenti, hogy a főverzió (például "~ 1") legújabb verzióját használja. Érhetők el ugyanazon új verziók, automatikusan települ a függvényalkalmazáshoz. Alkalmazás rögzítése a egy adott verziót, használja a teljes verziószám (például "1.0.12345"). Alapértelmezett érték a "~ 1".

|Kulcs|Mintaérték|
|---|------------|
|FÜGGVÉNYEK\_BŐVÍTMÉNY\_VERZIÓ|~1|

## <a name="websitecontentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

A használatalapú csomagok csak. A függvény kódját és konfigurációs tároló storage-fiókhoz tartozó kapcsolati karakterláncot. Lásd: [hozzon létre egy függvényalkalmazást](functions-infrastructure-as-code.md#create-a-function-app).

|Kulcs|Mintaérték|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [kulcs]|

## <a name="websitecontentshare"></a>WEBSITE_CONTENTSHARE

A használatalapú csomagok csak. A fájl elérési útja a függvény kódját és a konfiguráció. Az WEBSITE_CONTENTAZUREFILECONNECTIONSTRING használni. Alapértelmezés szerint egy egyedi karakterlánccá, amely a függvényalkalmazás neve kezdődik. Lásd: [hozzon létre egy függvényalkalmazást](functions-infrastructure-as-code.md#create-a-function-app).

|Kulcs|Mintaérték|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="websitemaxdynamicapplicationscaleout"></a>WEBHELY\_MAXIMÁLIS\_DINAMIKUS\_ALKALMAZÁS\_MÉRETEZÉSI\_KI

A függvényalkalmazás lehet horizontálisan-példányok maximális száma. Alapértelmezett érték a nincs korlátozva.

> [!NOTE]
> Ez a beállítás olyan előzetes verziójú funkció.

|Kulcs|Mintaérték|
|---|------------|
|WEBHELY\_MAXIMÁLIS\_DINAMIKUS\_ALKALMAZÁS\_MÉRETEZÉSI\_KI|10|

## <a name="websitenodedefaultversion"></a>WEBHELY\_CSOMÓPONT\_DEFAULT_VERSION

Az alapértelmezett érték "6.5.0".

|Kulcs|Mintaérték|
|---|------------|
|WEBHELY\_CSOMÓPONT\_DEFAULT_VERSION|6.5.0|

## <a name="next-steps"></a>További lépések

[Ismerje meg, hogyan-beállítások frissítése](functions-how-to-use-azure-function-app-settings.md#manage-app-service-settings)

[Tekintse meg a host.json fájl globális beállításai](functions-host-json.md)

[Tekintse meg az App Service-alkalmazások más app beállításait](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
