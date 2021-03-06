---
title: Hozzon létre, és a BizTalk Services biztonsági másolatának visszaállítása |} A Microsoft Docs
description: A BizTalk Services biztonsági mentési és visszaállítási tartalmazza. Megtudhatja, hogyan hozhat létre, és a visszaállítás biztonsági másolatból, és döntse el, mi a mentendő. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 90cf2d0ddbba47a856bf1299a101c5185873b5d8
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214412"
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Biztonsági mentés és visszaállítás

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Az Azure BizTalk Services biztonsági mentési és visszaállítási képességeket is tartalmaz. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

> [!NOTE]
> Hibrid kapcsolatok nem készül biztonsági másolat, a kiadástól függetlenül. A hibrid kapcsolatokat újra létre kell hoznia.


## <a name="before-you-begin"></a>A telepítés megkezdése előtt
* Biztonsági mentés és visszaállítás nem lehet elérhető összes kiadása esetén. Lásd: [a BizTalk Services: kiadások diagramja](biztalk-editions-feature-chart.md).
* Biztonsági mentési tartalom visszaállíthatók, ugyanazt a BizTalk-szolgáltatás vagy egy új BizTalk szolgáltatást. A BizTalk-szolgáltatás nevének használatával visszaállításához törölni kell a meglévő BizTalk-szolgáltatás, és a név elérhetőnek kell lennie. Miután törli a BizTalk-szolgáltatás, ugyanazzal a névvel elérhető legyen az olyan hosszabb időt is igénybe vehet. Ha nem várja meg ugyanazzal a névvel elérhető legyen, majd állítsa vissza egy új BizTalk szolgáltatást.
* A BizTalk Services vissza tudja állítani a kiadást vagy magasabb kiadásra. A BizTalk Services alacsonyabb verzióra történő visszaállításához, amikor a biztonsági mentés készült, nem támogatott.
  
    Például az alapszintű kiadású használatával biztonsági másolatot a Premium Edition is lehet visszaállítani. A Premium Edition használata biztonsági másolat nem állítható vissza a Standard Edition kiadásra.
* Az EDI ellenőrzőszámok készül biztonsági másolat az ellenőrzőszámok fenntartásában. Üzenetek feldolgozása után az utolsó biztonsági mentés, visszaállítás, a biztonsági mentési tartalom okozhat ismétlődő ellenőrzőszámok.
* Ha egy kötegelt aktív üzenetek, dolgozza fel a batch **előtt** biztonsági másolat készítése. Biztonsági másolat létrehozása (a szükséges vagy ütemezett), ha üzeneteket, és kötegekben soha nem tárolja. 
  
    **Ha egy biztonsági mentés készül az aktív üzenetek találhatók egy kötegelt, ezeket az üzeneteket nem készül biztonsági másolat, és emiatt elvesznek.**
* Választható lehetőség: A BizTalk Services Portáljára, nem felügyeleti műveleteket sem.

## <a name="create-a-backup"></a>Biztonsági mentés létrehozása
Biztonsági másolat tetszőleges időpontban lehet végrehajtani, és Ön teljes mértékben szabályozza. Biztonsági másolat létrehozásához használja a [REST API a BizTalk Services felügyelete az Azure-ban](https://msdn.microsoft.com/library/azure/dn232347.aspx).

## <a name="restore"></a>Visszaállítás
Biztonsági másolat visszaállításához használja a [REST API a BizTalk Services felügyelete az Azure-ban](https://msdn.microsoft.com/library/azure/dn232347.aspx).

### <a name="postrestore"></a>Biztonsági másolat visszaállítása után
A BizTalk-szolgáltatás mindig állítja vissza az egy **felfüggesztett** állapota. Ebben az állapotban végrehajtott bármilyen konfigurációs módosításokat előtt működési, beleértve az új környezet teheti meg:

* BizTalk-szolgáltatás alkalmazásait az Azure BizTalk Services SDK használatával hozta létre, ha lehetséges, hogy frissíteni szeretné ezeket az alkalmazásokat a visszaállított környezetben dolgozhat az Access Control (ACS) hitelesítő adatait.
* BizTalk-szolgáltatás replikálni egy meglévő BizTalk Service-környezet visszaállítása. Ebben a helyzetben az eredeti a BizTalk Services portálon konfigurált szerződések, amely egy forrásmappát FTP használata esetén előfordulhat, hogy frissíteni szeretné a megállapodások az újonnan visszaállított környezetben használni egy másik FTP-mappába. Ellenkező esetben előfordulhat, két különböző szerződések ugyanazon üzenet lekérése közben.
* Visszaállította az több BizTalk Service-környezetet, ellenőrizze, hogy a megfelelő környezetet a Visual Studio alkalmazást, a PowerShell-parancsmagok, a REST API-k vagy a Trading Partner Management OM API-k a célozhat meg.
* Tanácsos az újonnan visszaállított BizTalk Service-környezet automatikus biztonsági mentések konfigurálása.

## <a name="what-gets-backed-up"></a>Mi a mentendő
Biztonsági másolat létrehozásakor a következő elemek készül biztonsági másolat:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Elemek biztonsági mentése</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Az Azure BizTalk Services Portáljára</strong></td>
</tr> 
<tr>
<td>Konfiguráció és a futtatókörnyezet</td> 
<td>
<ul>
<li>Partner és a profil részletei</li>
<li>Egyezmények</li>
<li>Üzembe helyezett egyéni szerelvényeknek</li>
<li>Üzembe helyezett hidak</li>
<li>Tanúsítványok</li>
<li>Üzembe helyezett átalakítások</li>
<li>Adatcsatornák</li>
<li>Sablonok létrehozása és mentése a BizTalk Services portálon</li>
<li>X12 ST01 és GS01 leképezések</li>
<li>Ellenőrzőszámok (EDI)</li>
<li>AS2-üzenet MIC-értékek</li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2">
 <strong>Az Azure BizTalk-szolgáltatás</strong></td>
</tr> 
<tr>
<td>SSL-tanúsítvány</td> 
<td>
<ul>
<li>SSL-tanúsítvány adatait</li>
<li>SSL-tanúsítványának jelszava</li>
</ul>
</td>
</tr> 
<tr>
<td>A BizTalk-szolgáltatás beállításai</td> 
<td>
<ul>
<li>Skálázási egységek száma</li>
<li>Kiadás</li>
<li>Termékverzió</li>
<li>Régió/adatközpont</li>
<li>Access Control Service (ACS) névtér és a kulcs</li>
<li>Adatbázis-kapcsolati karakterlánc nyomon követése</li>
<li>Archiválás a Tárfiók kapcsolati sztringje</li>
<li>Figyelés a tárfiók kapcsolati sztringje</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>További elemek</strong></td>
</tr> 
<tr>
<td>Nyomkövető adatbázis</td> 
<td>Amikor létrejön a BizTalk-szolgáltatás, az adatbázis követési adatainak megadta, többek között az Azure SQL Database-kiszolgáló és a nyomkövetési adatbázis neve. A követés adatbázis nem automatikusan biztonsági másolat.
<br/><br/>
<strong>Fontos</strong><br/>
A követés adatbázisa törlődik, és az adatbázis igényeinek megfelelő helyre, ha egy korábbi biztonsági léteznie kell. Ha egy biztonsági másolat nem létezik, a változáskövetési adatbázis és az adatok nem lesznek helyreállítható. Ebben a helyzetben követési új adatbázis létrehozása az azonos nevű adatbázis. Georeplikáció ajánlott.</td>
</tr> 
</table>

## <a name="next"></a>Tovább
Azure BizTalk Services létrehozása, lépjen a [BizTalk Services: kiépítés](http://go.microsoft.com/fwlink/p/?LinkID=302280). Az alkalmazások létrehozásának megkezdéséhez ugorjon az [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197) című témakörre.

## <a name="see-also"></a>Lásd még:
* [Biztonsági mentési BizTalk-szolgáltatás](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk-szolgáltatás a biztonsági másolatból](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [A BizTalk Services: Fejlesztői, alapszintű, Standard és prémium kiadások diagramja](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [A BizTalk Services: kiépítés](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Kiépítési állapot diagramja](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Irányítópult, Figyelés és Méret lapok](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Szabályozás](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Kiállító neve és kiállító kulcsa](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Hogyan kezdhetem el az Azure BizTalk Services SDK használatát](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

