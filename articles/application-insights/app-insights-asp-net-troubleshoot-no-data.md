---
title: Adathiány hibaelhárítása – Application Insights .NET-hez
description: Nem jelennek meg adatok az Azure Application insights szolgáltatásban? Próbálja meg itt.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: e231569f-1b38-48f8-a744-6329f41d91d3
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 07/23/2018
ms.author: mbullwin
ms.openlocfilehash: 1a46564c324edb1999a2e1b1d482817685df2893
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205986"
---
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Adathiány hibaelhárítása – Application Insights .NET-hez
## <a name="some-of-my-telemetry-is-missing"></a>Láthatók a telemetriai adatok némelyike hiányzik
*Az Application Insights csak látható az alkalmazás által generált események töredékéért.*

* Az azonos töredék következetesen jelennek meg, esetén valószínűleg miatt adaptív [mintavételi](app-insights-sampling.md). Ennek ellenőrzéséhez nyissa meg a keresési (az Áttekintés panelről), és tekintse meg a kérelem vagy más esemény egy példányát. A Tulajdonságok szakaszának alján kattintson a "...", teljes tulajdonság részletes. Ha a Count > 1 kérelem, majd a mintavétel van folyamatban. 
* Ellenkező esetben lehetséges, hogy állt egy [sávszélesség-korlátjának](app-insights-pricing.md#limits-summary) a díjcsomagra. Ezek a korlátok percenkénti érvényesek.

## <a name="no-data-from-my-server"></a>Nincsenek a kiszolgálón lévő adatok
*Az alkalmazás telepítése a webkiszolgálón, és be van kapcsolva, nem látható semmilyen telemetriai adatot belőle. A fejlesztői gépen, dolgoztam OK.*

* Valószínűleg tűzfal probléma. [Az Application Insights adatforgalma számára tűzfalkivételek beállítása](app-insights-ip-addresses.md).
* Az IIS-kiszolgálón hiányoznak egyes Előfeltételek: .NET-bővíthetőség 4.5 és az ASP.NET 4.5-ös verzióját.

*E [telepített állapotfigyelő](app-insights-monitor-performance-live-website-now.md) a meglévő alkalmazások figyelése a webkiszolgálón. Nem látható az eredményt.*

* Lásd: [Állapotfigyelőt hibaelhárítási](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights). 

## <a name="q01"></a>Nincs "Application Insights hozzáadása" lehetőség a Visual studióban
*Ha a jobb gombbal egy meglévő projektben a Megoldáskezelőre, az Application Insights-beállításokat nem látható.*

* Az eszközök által támogatott .NET-projekt nem minden típusú. Webes és WCF-projektek támogatottak. Minden olyan projekt esetében, mint például az asztali vagy service-alkalmazások, akkor is [manuálisan az Application Insights SDK hozzáadása a projekthez](app-insights-windows-desktop.md).
* Ellenőrizze, hogy [Visual Studio 2013 Update 3 vagy újabb](https://docs.microsoft.com/visualstudio/releasenotes/vs2013-update3-rtm-vs). Developer Analytics eszközök, amelyeket az Application Insights SDK-t adja meg az előre telepített származik.
* Válassza ki **eszközök**, **bővítmények és frissítések** , és ellenőrizze, hogy **Developer Analytics Tools** telepítve és engedélyezve van. Ha igen, kattintson a **frissítések** , hogy van egy frissítés.
* Az új projekt párbeszédpanel megnyitásához, és válassza ki az ASP.NET-webalkalmazás. Ha látja az Application Insights lehetőség van, az eszközök vannak telepítve. Ha nem, kísérelje meg eltávolítani, majd telepítse újra az Application Insights Tools.

## <a name="q02"></a>Nem sikerült hozzáadni az Application Insights
*Amikor az Application Insights hozzáadása egy meglévő projektjébe, egy hibaüzenet látható.*

Valószínű okai a következők:

* Nem sikerült az Application Insights portálon kommunikációt; vagy
* Probléma lép fel az Azure-fiókkal;
* Csak [olvasási hozzáférést az előfizetés vagy a csoport, ahol próbált létrehozni az új erőforrás](app-insights-resources-roles-access-control.md).

Javítás:

* Ellenőrizze, hogy a megfelelő Azure-fiókot a megadott bejelentkezési hitelesítő adataival. 
* A böngészőben, ellenőrizze, hogy van-e a hozzáférést a [az Azure portal](https://portal.azure.com). Nyissa meg a beállításokat, majd e korlátozás.
* [Application Insights hozzáadása a meglévő projekt](app-insights-asp-net.md): A Megoldáskezelőben kattintson a jobb gombbal a projekt, és válassza az "Application Insights hozzáadása".
* Ha ez még mindig nem működik, kövesse a [manuális eljárást](app-insights-windows-services.md) , adjon hozzá egy erőforrást a portálon, majd adja hozzá az SDK-t a projekthez. 

## <a name="emptykey"></a>"Kialakítási kulcsot nem lehet üres" hibaüzenetet kapok
Úgy tűnik, probléma merült fel, míg az Application Insights vagy talán egy naplózási adapter telepíti.

A Megoldáskezelőben kattintson jobb gombbal a projektre, és válassza a **Application Insights > Application Insights konfigurálása**. Kap egy párbeszédpanel, amely felkéri, hogy jelentkezzen be az Azure-ba, és hozzon létre egy Application Insights-erőforrást, vagy használja újra egy már meglévőt.

## <a name="NuGetBuild"></a> "NuGet-csomagok hiányoznak a" saját buildelési kiszolgáló
*Minden tekintetben OK I vagyok hibakeresése a saját fejlesztői gépen, de a NuGet-hiba jelenik meg a build-kiszolgálón.*

Lásd: [NuGet-csomagok visszaállításának](http://docs.nuget.org/Consume/Package-Restore) és [automatikus csomagok visszaállításának](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Hiányzó menüparancs a Visual Studio Application Insights megnyitása
*Ha jobb gombbal a projekt Megoldáskezelőben, az Application Insights parancsok nem látható, vagy egy nyissa meg az Application Insights-parancs nem látható.*

Valószínű okai a következők:

* Ha manuálisan hozott létre az Application Insights-erőforrást, vagy ha a projekt, amely az Application Insights eszközök által nem támogatott típusú.
* A Developer Analytics tools le vannak tiltva, a Visual studióban. 
* A Visual Studio 2013 Update 3-nál régebbi.

Javítás:

* Győződjön meg arról, hogy az Ön Visual Studio 2013 update 3 vagy újabb.
* Válassza ki **eszközök**, **bővítmények és frissítések** , és ellenőrizze, hogy **Developer Analytics tools** telepítve és engedélyezve van. Ha igen, kattintson a **frissítések** , hogy van egy frissítés.
* Kattintson a jobb gombbal a projektre a Megoldáskezelőben. Ha a parancs látható **Application Insights > Application Insights konfigurálása**,-projekt csatlakoztatása az erőforrás az Application Insights szolgáltatás segítségével.

Ellenkező esetben a projekt típusa nem támogatja közvetlenül az Application Insights-eszközökről. A telemetriai adatok megtekintéséhez jelentkezzen be a [az Azure portal](https://portal.azure.com)Application Insights válassza a bal oldali navigációs sávon, majd válassza ki az alkalmazását.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>"Hozzáférés megtagadva" a Visual Studio Application Insights megnyitása
*A "Megnyitás az Application Insights" parancs tart velem az Azure Portalra, de "hozzáférés megtagadva" hibaüzenetet kapok.*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

A Microsoft bejelentkezési a legutóbb használt az alapértelmezett böngésző nem fér hozzá a [jött létre, amikor az alkalmazás hozzá lett adva az Application Insights erőforrás](app-insights-asp-net.md). Két valószínű oka van: 

* Egynél több Microsoft-fiók – talán egy munkahelyi és személyes Microsoft-fiókkal van? A legutóbb használt az alapértelmezett böngészőben a bejelentkezési volt-e egy másik fiókot, amely hozzáféréssel rendelkezik [az Application Insights hozzáadása a projekthez](app-insights-asp-net.md). 
  
  * Javítás: A nevét, kattintson a böngészőablak jobb oldalán, és jelentkezzen ki. Majd jelentkezzen be azzal a fiókkal, amely hozzáféréssel rendelkezik. Ezután a bal oldali navigációs sávon kattintson az Application Insights, és válassza ki az alkalmazást.
* Valaki más Application Insights hozzáadja a projekthez, és azokat, hogy elfelejtette [hozzáférés az erőforráscsoporthoz](app-insights-resources-roles-access-control.md) található, amely lett létrehozva. 
  
  * Javítás: Ha szervezeti fiókkal használják, azokat is hozzáadhatja a csapat; akkor is, az egyes való hozzáférés engedélyezése vagy az erőforráscsoportot.

## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>"Objektum nem található" a Visual Studio Application Insights megnyitása
*A "Megnyitás az Application Insights" parancs tart velem az Azure Portalra, de "objektum nem található" hibaüzenet.*

Valószínű okai a következők:

* Az Application Insights-erőforrást az alkalmazás törölve lett; vagy
* A kialakítási kulcsot lett beállítva, vagy módosíthatja az applicationinsights.config fájlban, a Szerkesztés, közvetlenül a projekt fájl frissítése nélkül. 

A kialakítási kulcs az ApplicationInsights.config vezérlők, ahol a telemetriai adatok küldése. Egy sort a következő projektfájlban szabályozza, hogy melyik erőforrás megnyitása a Visual Studióban a parancs használatakor. 

Javítás:

* A Megoldáskezelőben kattintson a jobb gombbal a projektre, és válassza az Application Insights, az Application Insights konfigurálása. A párbeszédpanelen vagy kiválaszthatja telemetria küldése egy meglévő erőforrásnak, vagy hozzon létre egy újat. Vagy:
* Nyissa meg közvetlenül az erőforrást. Jelentkezzen be a [az Azure Portalon](https://portal.azure.com), és a bal oldali navigációs sávon kattintson az Application Insights, majd válassza ki az alkalmazást.

## <a name="where-do-i-find-my-telemetry"></a>Hol találom meg a telemetriát?
*A bejelentkezésem a [Microsoft Azure-portálon](https://portal.azure.com), és az Azure otthoni irányítópultjánál keresek. Ezért hol találhatók az Application Insights-adataim?*

* A bal oldali navigációs sávon kattintson az Application Insights, akkor az alkalmazás nevére. Ha ott nem rendelkezik projektekkel, akkor [hozzáadása vagy az Application Insights beállítása a webes projekt](app-insights-asp-net.md).
  
    Ott találja majd néhány összegző diagramok. Kattinthat, ezeket a további részletek megtekintéséhez.
* A Visual Studióban amíg az alkalmazás hibakeresése, kattintson az Application Insights gomb.

## <a name="q03"></a> Kiszolgálói adatok nem (vagy egyáltalán nincs adat)
*E alkalmazásom futott, és majd megnyitni az Application Insights szolgáltatás a Microsoft Azure-ban, de a diagramok megjelenítése "Megtudhatja, hogyan gyűjtheti az..." vagy "Nincs konfigurálva."* Másik lehetőségként *csak oldal nézet és a felhasználói adatok, de nem a server-adatok.*

* Futtassa az alkalmazást hibakeresési módban, a Visual Studióban (F5). Használja az alkalmazást úgy, hogy létrejöjjön valamennyi telemetria. Ellenőrizze, hogy látja-e az események naplózása a Visual Studio kimeneti ablakában. 
  
    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)
* Nyissa meg az Application Insights portálon [diagnosztikai keresés](app-insights-diagnostic-search.md). Adatok általában itt jelenik meg először.
* A frissítés gombra. A panel rendszeres időközönként frissíti magát, de manuálisan is megteheti. A frissítési időköz hosszabb ideig nagyobb időintervallumok.
* Ellenőrizze, hogy a rendszerállapot-kulcsok megegyezzenek. A az alkalmazáshoz az Application Insights portálon, a fő panelen a a **Essentials** legördülő, tekintse meg **kialakítási kulcs**. Ezután a projekt a Visual Studióban, nyissa meg az applicationinsights.config fájlt, és keresse meg a `<instrumentationkey>`. Ellenőrizze, hogy a két kulcs egyenlő. Ha nem:
  
  * A portálon kattintson az Application Insights és a megfelelő kulccsal; keresse meg az alkalmazás-erőforrást vagy
  * A Visual Studio Solution Explorerben kattintson jobb gombbal a projektre, és válassza az Application Insights konfigurálása. Állítsa vissza az alkalmazást, hogy küldjön telemetriát a megfelelő erőforrás.
  * Ha nem találja a megfelelő kulcsokkal, ellenőrizze, hogy használ azonos bejelentkezési hitelesítő adatok, a Visual studióban a portálra.
    
    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
* Az a [Microsoft Azure irányítópult](https://portal.azure.com), tekintse meg a Service Health-leképezés. Ha egyes riasztási jelzések, várjon, amíg azok OK vissza majd zárja be és nyissa meg ismét az Application Insights-alkalmazás paneljén.
* Ellenőrizze azt is [állapot blogunkat](http://blogs.msdn.com/b/applicationinsights-status/).
* Felelt meg a kód írása a [kiszolgálóoldali SDK](app-insights-api-custom-events-metrics.md) , előfordulhat, hogy módosítsa a kialakítási kulcsot az `TelemetryClient` példányok vagy a `TelemetryContext`? Adta meg írni vagy egy [szűrő vagy mintavételi konfiguráció](app-insights-api-filtering-sampling.md) előfordulhat, hogy szűrése, amely túl sok ki?
* Ha szerkesztette az applicationinsights.config fájlt, gondosan ellenőrizze konfigurációját [TelemetryInitializers és TelemetryProcessors](app-insights-api-filtering-sampling.md). Egy nem megfelelően nevű típus vagy paraméter okozhat adatot nem küld az SDK-t.

## <a name="q04"></a>Nincs adat a böngészőkben, Oldalmegtekintések, használat
*Oldal nézet Lapbetöltési idő, illetve a böngészőben vagy a használati paneleket kiszolgálói válaszidő-és kiszolgálói kérelmek adatainak, de az adatok nem látható.*

Az adatok a weboldalakon parancsfájlok származik. 

* Ha hozzáadta az Application Insights egy meglévő webes projektet [kell manuálisan adja hozzá a parancsfájlok](app-insights-javascript.md).
* Ellenőrizze, hogy az Internet Explorer nem jelennek meg a hely kompatibilitási módban.
* A böngésző hibakereső szolgáltatás (F12 az egyes böngészőkben, majd válassza a hálózati) használatával győződjön meg arról, hogy adatokat küld a `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Nincs függőség vagy kivétel adat
Lásd: [– függőségi telemetria](app-insights-asp-net-dependencies.md) és [kivételtelemetria](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Teljesítményadatok
Teljesítményadatok (CPU, i/o-forgalom, és így tovább) érhető el [Java webes szolgáltatások](app-insights-java-collectd.md), [Windows asztali alkalmazások](app-insights-windows-desktop.md), [IIS webalkalmazások és szolgáltatások Ha telepítse az állapotfigyelőt](app-insights-monitor-performance-live-website-now.md), és [Az azure Cloud Services](app-insights-azure.md). a beállítások kiszolgálók találhat.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Nincs (kiszolgáló) adat, mivel az alkalmazás közzétett kiszolgálóhoz
* Ellenőrizze, hogy ténylegesen kimásolt minden Microsoft. A kiszolgálóval együtt Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll ApplicationInsights dll-EK
* A tűzfalat, előfordulhat, hogy kell [nyissa meg az egyes TCP-portok](app-insights-ip-addresses.md).
* Ha szeretne küldeni a vállalati hálózaton kívül, állítsa be a proxy használatát [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) a Web.config fájlban
* A Windows Server 2008: Ellenőrizze, hogy telepítette a következő frissítéseket: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).

## <a name="i-used-to-see-data-but-it-has-stopped"></a>Használt adatokat, de leállt
* Ellenőrizze a [állapot blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Elérte a havi kvóta adatpontok? Nyissa meg a beállítások/kvóta és díjszabás megtudhatja. Ha igen, váltson magasabb szintű csomagra, vagy további kapacitást kell fizetnie. Tekintse meg a [díjszabási séma](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="i-dont-see-all-the-data-im-expecting"></a>Felfedezhetem minden adat nem látható
Ha az alkalmazás nagy mennyiségű adatot küld el, és használja az Application Insights SDK-t az ASP.NET 2.0.0-beta3 vagy újabb, illetve a [adaptív mintavételezés](app-insights-sampling.md) funkció is működnek, és csak a telemetria százalékában küldjön. 

Bármikor letilthatja azt, de ez nem ajánlott. Mintavételi célja, hogy a kapcsolódó telemetriai adatokat megfelelően továbbítani, diagnosztikai célokra. 

## <a name="client-ip-address-is-0000"></a>Ügyfél IP-cím 0.0.0.0 
A 2018 február azt [bejelentett](https://blogs.msdn.microsoft.com/applicationinsights-status/2018/02/01/all-octets-of-ip-address-will-be-set-to-zero/) , hogy az ügyfél IP-cím naplózása eltávolítottuk. Ez nincs hatással a földrajzi helyet.

## <a name="wrong-geographical-data-in-user-telemetry"></a>Nem megfelelő földrajzi adatokat a felhasználó telemetria
Az városa, régió és ország dimenziók IP-címek vannak származtatva, és nem minden esetben pontosak. Ezen IP-címek hely elsőként feldolgozva, és a 0.0.0.0 kell tárolni, majd módosítani.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>„A metódus nem található” kivétel az Azure Cloud Servicesben futó rendszeren
A .NET 4.6-os verziójára készítette el az alkalmazást? Az Azure Cloud Services szerepkörei nem támogatják automatikusan a 4.6-os verziót. [Telepítse a 4.6-os verziót mindegyik szerepkörön](../cloud-services/cloud-services-dotnet-install-dotnet.md), mielőtt futtatná az alkalmazást.

## <a name="still-not-working"></a>Még mindig nem működik...
* [Application Insights-fórum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

