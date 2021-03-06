---
title: Infrastruktúra biztonsági mentési szolgáltatás ajánlott eljárások az Azure Stackhez |} A Microsoft Docs
description: Ajánlott eljárásokat is követheti, ha telepíti és kezeli az Azure Stack segítségével mérsékelhetik az adatvesztést egy Katasztrofális hiba esetén az adatközpontban.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 221FDE40-3EF8-4F54-A075-0C4D66EECE1A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/20/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 4a0166856cf4d3f91c6cadf3e71f93000fbf158d
ms.sourcegitcommit: d76d9e9d7749849f098b17712f5e327a76f8b95c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242971"
---
# <a name="infrastructure-backup-service-best-practices"></a>Infrastruktúra Backup szolgáltatás – ajánlott eljárások

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Ajánlott eljárásokat is követheti, ha telepíti és kezeli az Azure Stack segítségével mérsékelhetik az adatvesztést egy Katasztrofális hiba esetén az adatközpontban.

Az ajánlott eljárásokat, győződjön meg arról, hogy a telepítés még a megfelelőségi végrehajtott módosítások a művelet flow rendszeres időközönként tekintse át. A Microsoft Support kell problémát tapasztal az ajánlott eljárások végrehajtása közben, kérjen segítséget.

## <a name="configuration-best-practices"></a>Konfigurációs eljárások

### <a name="deployment"></a>Környezet

Infrastruktúra biztonsági mentésének engedélyezése az egyes Azure Stack-felhő üzembe helyezés után. Az operátor felügyeleti API-végpont a hozzáférést minden olyan ügyfél-kiszolgáló biztonsági mentések ütemezheti AzureStack-eszközök használatával.

### <a name="networking"></a>Hálózat

Az univerzális elnevezési konvenció (UNC) karakterlánc az elérési út egy teljesen minősített tartománynevét (FQDN) kell használnia. IP-cím akkor lehetséges, ha a névfeloldás ez nem lehetséges. Karakterláncnak UNC helyét adja meg az erőforrások, például megosztott fájlokhoz vagy eszközökön.

### <a name="encryption"></a>Titkosítás

A titkosítási kulcs beolvasása külső tárterület exportált adatok biztonsági másolatának titkosításához használatos. A kulcs létrehozásának részeként [a biztonsági mentés engedélyezése az Azure Stack a PowerShell-lel](azure-stack-backup-enable-backup-powershell.md).

A kulcs (például nyilvános Azure Key Vaultban titkos kódot) biztonságos helyen kell tárolni. Ezt a kulcsot kell használni az Azure Stack újbóli üzembe helyezés során. 

![A kulcsot biztonságos helyen tárolja.](media\azure-stack-backup\azure-stack-backup-encryption2.png)

## <a name="operational-best-practices"></a>Gyakorlati tanácsok az üzemeltetéshez

### <a name="backups"></a>Biztonsági másolatok

 - Infrastruktúra biztonsági mentést vezérlő kell igény szerinti aktiválását. Ajánljuk, hogy a biztonsági mentés naponta legalább kétszer.
 - Biztonsági mentési feladatok végrehajtása közben a rendszer fut, így a felhasználói élmény vagy a felhasználói alkalmazások üzemkimaradást sem okoz. A várt a biztonsági mentési feladatok ésszerű mértékű terhelés alatt álló megoldások 20 – 40 percet vesz igénybe.
 - OEM megadott utasítás használatával, manuálisan biztonsági mentési hálózati kapcsolók és a hardver életciklus gazdagép (HLH) kell tárolni a biztonsági mentési megosztáshoz, a biztonsági mentési infrastruktúra-vezérlő tárolók vezérlő adatsík a biztonsági mentési adatokat. Érdemes tárolni a kapcsoló- és HLH konfigurációk a régió mappában. Ha több Azure Stack-példányok ugyanabban a régióban, fontolja meg, az egyes konfigurációkhoz egy skálázási egység tartozó azonosító használatát.

### <a name="folder-names"></a>Mappanevek

 - Infrastruktúra automatikusan létrehozza a MASBACKUP mappát. Ez az a Microsoft által felügyelt fájlmegosztás. Megosztások MASBACKUP ugyanazon a szinten hozhat létre. Mappák vagy adatok a storage, amely nem hoz létre az Azure Stack MASBACKUP létrehozása nem ajánlott. 
 -  Felhasználó teljes Tartományneve és a mappa nevében régió megkülönböztetni a különböző felhőkben biztonsági mentési adatait. A teljesen minősített tartományneve (FQDN) az Azure Stack üzembe helyezés és a végpontok a régiót és a külső tartománynév paraméter együttes használata. További információkért lásd: [adatközpontban Azure Stack - integrációs DNS](azure-stack-integrate-dns.md).

Például a biztonsági másolatok megosztásáról az AzSBackups fileserver01.contoso.com üzemeltethetők. A fájlmegosztás egy mappát a külső tartománynév és a egy almappát, amely a régió nevét használja az Azure Stack üzemelő példányonként előfordulhatnak. 

Teljes tartománynév: contoso.com  
Régió: nyc


    \\fileserver01.contoso.com\AzSBackups
    \\fileserver01.contoso.com\AzSBackups\contoso.com
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\MASBackup

MASBackup mappában, ahol az Azure Stack tárolja a biztonsági mentési adatok. Ez a mappa nem használjon a saját adatok tárolására. OEM vagy bármely biztonsági mentési adatok tárolására ne használja ezt a mappát. 

OEM-ek javasolt, hogy az a régió mappában összetevők biztonsági mentési adatok tárolására. Minden egyes hálózati kapcsolók, hardver életciklus állomás (HLH), és így tovább a saját almappájába kell tárolni. Példa:

    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\HLH
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Switches
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\DeploymentData
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Registration

### <a name="monitoring"></a>Figyelés

A rendszer támogatja a következő riasztásokat:

| Riasztás                                                   | Leírás                                                                                     | Szervizelés                                                                                                                                |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Biztonsági mentés nem sikerült, mert a fájlmegosztás nincs elég kapacitás | Fájlmegosztás kívül esik a kapacitás és a biztonsági mentés vezérlő nem tudja exportálni biztonságimásolat-fájlok helyét. | Adja hozzá a további tárolókapacitás, és készítsen biztonsági másolatot próbálja újra. Szabadítson fel lemezterületet, törölje a meglévő biztonsági másolatok (kezdve Legrégebbi először).                    |
| Biztonsági mentés kapcsolati problémái miatt nem sikerült.             | Hálózat az Azure Stack és a fájl között megosztást hibásan.                          | A hálózati probléma, és próbálkozzon újra a biztonsági mentés.                                                                                            |
| Biztonsági mentés az elérési út egy hiba miatt nem sikerült                | A fájlmegosztás elérési útja nem oldható fel                                                          | Képezze le a megosztás egy másik számítógépről annak érdekében, hogy a megosztás érhető el. Szükség lehet frissíteni az elérési út, ha már nem érvényes.       |
| Biztonsági mentés hitelesítési probléma miatt nem sikerült               | A hitelesítő adatokkal probléma vagy hálózati probléma, amely hatással van a hitelesítés lehet.    | Képezze le a megosztás egy másik számítógépről annak érdekében, hogy a megosztás érhető el. Szükség lehet a hitelesítő adatok frissítése, ha azok már nem érvényesek. |
| Biztonsági mentés nem sikerült, mert egy általános hiba                    | A sikertelen kérelmek egy átmeneti probléma okozhatja. Próbálkozzon újra biztonsági mentése.                    | Telefonos támogatás                                                                                                                               |

## <a name="next-steps"></a>További lépések

 - Tekintse át a referenciaanyag az olyan a [infrastruktúra biztonsági mentési szolgáltatás](azure-stack-backup-reference.md).  
 - Engedélyezze a [infrastruktúra biztonsági mentési szolgáltatás](azure-stack-backup-enable-backup-console.md).