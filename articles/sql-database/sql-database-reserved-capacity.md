---
title: Fizessen elő az Azure SQL Database virtuális magot kapnak, pénzt takaríthat meg |} A Microsoft Docs
description: Ismerje meg, hogyan vásárolhat Azure SQL Database szolgáltatás számára fenntartott kapacitás a számítási költségek mentéséhez.
services: sql-database
documentationcenter: ''
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.devlang: na
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: carlrab
ms.openlocfilehash: 1d1cad4b614eb35be9235a721368db8048be050a
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39634293"
---
# <a name="prepay-for-sql-database-compute-resources-with-azure-sql-database-reserved-capacity"></a>Fizessen elő az SQL-adatbázis számítási erőforrásokat, hogy az Azure SQL Database szolgáltatás számára fenntartott kapacitás

Pénzt takaríthat meg az Azure SQL Database az Azure SQL Database számítási erőforrások a használatalapú fizetéshez képest előre. Azure SQL Database szolgáltatás számára fenntartott kapacitás esetén módosítani egy előzetes kötelezettségvállalás az SQL Database egy vagy három évig, jelentős árkedvezményeket lekérni a számítási költségek. SQL Database szolgáltatás számára fenntartott kapacitás vásárlása, meg kell adnia az Azure-régió, a központi telepítési típus, a szolgáltatás és a kifejezés. 

Nem kell a Foglalás hozzárendelése az SQL Database-példányokat. SQL Database-példány, amelyeken már fut, vagy azokat, amelyeket újonnan telepített, automatikusan megkapja a juttatás megfelelő. A Foglalás megvásárlásával Ön előre fizetés a számítási költségek a SQL Database-példányok esetében egy vagy három évig. Amint vásárol egy foglalást, a számítási díjakat, amelyek megfelelnek a Foglalás attribútumok már nem számoljuk el a használatalapú-as-, SQL Database díjszabása nyissa meg. A foglalás nem fedi le szoftvereket, hálózati vagy tárolási díjak az SQL Database-példányhoz társított. A Foglalás időtartamára végén a számlázási ellátás lejár, és az SQL-adatbázisok számlázása a használatalapú – mint-akkor lépjen díj ellenében. Foglalások nem automatikus megújítási. Díjszabási információkért tekintse meg a [SQL Database szolgáltatás számára fenntartott kapacitás ajánlat](https://azure.microsoft.com/pricing/details/sql-database/managed/).

Azure SQL Database szolgáltatás számára fenntartott kapacitás megvásárolhatja a [az Azure portal](https://portal.azure.com). Az SQL Database szolgáltatás számára fenntartott kapacitás vásárlása:
- A tulajdonos szerepkör legalább egy vállalati vagy használatalapú fizetéses előfizetésre kell lennie.
- Az Enterprise-előfizetések, a foglalást az Azure-beli vásárlásokra engedélyezve kell lennie a [a nagyvállalati szerződések portáljának](https://ea.azure.com).
-  A Cloud Solution Provider (CSP) program keretében csak a felügyeleti ügynökök vagy értékesítési ügynökök vásárolhatja meg az SQL Database szolgáltatás számára fenntartott kapacitás.

Hogyan vállalati ügyfelek és a használatalapú fizetéses ügyfelek számára számlázzuk foglalás vásárlásokra részleteiért foglalkozik [– gyakori kérdések](#frequently-asked-questions).

## <a name="determine-the-right-sql-size-before-purchase"></a>Határozza meg a megfelelő SQL-méretet vásárlás előtt

Foglalási mérete alapján kell a meglévő vagy hamarosan –--telepíteni az SQL által használt számítási teljes mennyisége önálló adatbázisok és/vagy egy adott régión belül a rugalmas készletek és a teljesítmény szint és a hardver-generation használatával. 

Például tegyük fel, hogy futtatja egy általános célú, Gen5 – 16 virtuális mag rugalmas készletet, és két üzletileg kritikus, Gen5 – 4 virtuális mag önálló adatbázisok. További nézzük lehet, hogy szeretné-e a következő hónap belüli üzembe helyezett egy további általános célú, a Gen5 – 16 virtuális mag rugalmas készletet, és a egy üzletileg kritikus, Gen5 – 32 virtuális mag a rugalmas készlet. Ezenkívül tegyük fel, hogy ismeri, hogy ezeket az erőforrásokat kell legalább 1 évig. Ebben az esetben meg kell vásárolnia egy 32 mag (2 x 16), SQL Database egyetlen/rugalmas készletbeli általános célú - Compute, Gen5 és a egy 40 (2 x 4 + 32) 1 év foglalás az SQL Database egyetlen/rugalmas készletbeli üzletileg kritikus - Compute, Gen5 virtuális mag 1 év foglalás.

## <a name="buy-sql-database-reserved-capacity"></a>Az SQL Database szolgáltatás számára fenntartott kapacitás vásárlása

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Válassza ki **minden szolgáltatás** > **foglalások**.
3. Válassza ki **Hozzáadás** és a termék típusának kiválasztása panelen válassza ki **SQL Database** , egy új foglalást vásárolhat SQL Database-hez.
4. Adja meg a kötelező mezőket. Meglévő vagy új önálló adatbázisokat vagy rugalmas készletek a kiválasztott attribútumok minősítéséhez beolvasni a fenntartott kapacitás kedvezményes megfelelő. SQL Database-példány, amely a kedvezmény a tényleges száma attól függ, a hatókör és a kiválasztott mennyiség.

   ![Képernyőkép, mielőtt beküldi az SQL Database szolgáltatás számára fenntartott kapacitás vásárlás](./media/sql-database-reserved-vcores/sql-reserved-vcores-purchase.png)

    | Mező      | Leírás|
    |:------------|:--------------|
    |Name (Név)        |A Foglalás neve.| 
    |Előfizetés|Az SQL Database szolgáltatás számára fenntartott kapacitás foglalási díjfizetéséhez használt előfizetés. A fizetési módot, az előfizetés fel van töltve az SQL Database szolgáltatás számára fenntartott kapacitás foglalás az előzetes költségek. Az előfizetés típusa kétféle lehet: nagyvállalati szerződés (ajánlatszám: MS-AZR-0017P) vagy használatalapú fizetéses (ajánlatszám: MS-AZR-0003P). Nagyvállalati előfizetésnél a díjak a regisztrációhoz tartozó keretek egyenlegeiből lesznek levonva, illetve túlhasználatként lesznek számlázva. Használatalapú fizetéses előfizetéseknél a díjakat az előfizetéshez tartozó hitelkártyára terheljük vagy a számlafizetési módnak megfelelően számlázzuk.|    
    |Hatókör       |A virtuális mag foglalás hatóköre egy előfizetés vagy több előfizetés (megosztott hatókör) is foglalkozik. Ha ki: <ul><li>Egyetlen előfizetés – a virtuális mag foglalási kedvezményt alkalmazza a rendszer SQL Database-példány ebben az előfizetésben. </li><li>Közös – a virtuális mag van alkalmazza a foglalási kedvezményt olyan előfizetéseket, a számlázási környezetben futó SQL Database-példányok. A vállalati ügyfelek a megosztott hatókörrel a regisztráció és előfizetéseken belül a regisztráció (kivéve a fejlesztési és tesztelési előfizetések) magában foglalja. Használatalapú fizetéses ügyfelek számára a megosztott hatókörrel a fiók rendszergazdája által létrehozott összes utólagos elszámolású előfizetések.</li></ul>|
    |Régió      |Az Azure-régióban, az SQL Database által fenntartott kapacitás foglalás.|    
    |Központi telepítés típusa|Az SQL központi telepítési típus, amely szeretné megvásárolni a foglalást.|
    |Szolgáltatási szint|Az SQL Database-példány szolgáltatási szintjei.
    |Időtartam        |Egy vagy három év.|
    |Mennyiség    |Az SQL Database megvásárolt példányainak számát fenntartott kapacitás foglalás. A mennyiség a futó kérheti le a számlázási kedvezményt SQL Database-példányok számát. Például ha 10 SQL Database-példányt futtat az USA keleti régiójában, majd kell megadni mennyiség 10 az összes futó gépek juttatása maximalizálása érdekében. |

5. Tekintse át az SQL-adatbázis költsége a szolgáltatás számára fenntartott kapacitás foglalása az **költségek** szakaszban.
6. Válassza a **Beszerzés** lehetőséget.
7. Válassza ki **megtekintése a Foglalás** a vásárlás állapotának megjelenítéséhez.

## <a name="next-steps"></a>További lépések 
A virtuális mag foglalási kedvezményt automatikusan alkalmazni, amelyek megfelelnek az SQL Database szolgáltatás számára fenntartott kapacitás foglalás hatóköre és attribútumok az SQL Database-példányok száma. Frissítheti a keresztül az SQL Database szolgáltatás számára fenntartott kapacitás foglalási hatókört [az Azure portal](https://portal.azure.com), PowerShell, CLI és az API-n keresztül. 

Megtudhatja, hogyan kezelheti az SQL Database szolgáltatás számára fenntartott kapacitás foglalás, lásd: [felügyelete az SQL Database szolgáltatás számára fenntartott kapacitás](../billing/billing-manage-reserved-vm-instance.md).

Azure foglalás kapcsolatos további információkért tekintse meg a következő cikkeket:

- [Mik az Azure-foglalásokat?](../billing/billing-save-compute-costs-reservations.md)
- [Az Azure-fenntartások kezelése](../billing/billing-manage-reserved-vm-instance.md)
- [A használatalapú fizetéses előfizetést foglalás használati adatai](../billing/billing-understand-reserved-instance-usage.md)
- [A nagyvállalati beléptetés foglalás használati adatai](../billing/billing-understand-reserved-instance-usage-ea.md)
- [A Partner Center Felhőszolgáltató (CSP) program Azure foglalások](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Segítség Kapcsolatfelvétel a támogatási szolgáltatással

Ha további kérdése van, [forduljon az ügyfélszolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma gyors megoldása érdekében.

