---
title: 'Üzleti folytonosság és vészhelyreállítás helyreállítási (BCDR): Azure párosított régiói |} A Microsoft Docs'
description: További információ az Azure régiónkénti párosítás, annak érdekében, hogy alkalmazásokat rugalmas adatközpontok meghibásodásának során.
author: rayne-wiselman
ms.service: multiple
ms.topic: article
ms.date: 07/03/2018
ms.author: raynew
ms.openlocfilehash: 13a2b78b50b1b10975a90c1da38810f1a62a6bb5
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436909"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Üzleti folytonosság és vészhelyreállítás helyreállítási (BCDR): Azure párosított régiói

## <a name="what-are-paired-regions"></a>Mik a párosított régiók?

Az Azure világszerte több földrajzi területeken működik. Egy Azure földrajzi területet adtunk a világ, amely tartalmaz legalább egy Azure-régióban. Egy Azure-régióban egy olyan terület, a földrajzi helyen tartalmazó egy vagy több adatközpont tartozhat.

Minden egyes Azure-régió párban áll egy regionális párokból érdemes együtt így azonos földrajzi helyen belül egy másik régióban. A kivétel, Dél-Brazília, amely kívül a földrajzi régió párban áll.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

1. ábra – az Azure regionális párok

| Földrajzi hely | Párosított régiók |  |
|:--- |:--- |:--- |
| Ázsia |Kelet-Ázsia |Délkelet-Ázsia |
| Ausztrália |Kelet-Ausztrália |Délkelet-Ausztrália |
| Ausztrália |Ausztrália középső régiója |Ausztrália 2. középső régiója |
| Brazília |Brazília 2. déli régiója |USA déli középső régiója |
| Kanada |Közép-Kanada |Kelet-Kanada |
| Kína |Észak-Kína |Kelet-Kína|
| Európa |Észak-Európa |Nyugat-Európa |
| Németország |Közép-Németország |Északkelet-Németország |
| India |Közép-India |Dél-India |
| India |Nyugat-India (1) |Dél-India |
| Japán |Kelet-Japán |Nyugat-Japán |
| Korea |Korea középső régiója |Korea déli régiója |
| Észak-Amerika |USA keleti régiója |USA nyugati régiója |
| Észak-Amerika |USA 2. keleti régiója |USA középső régiója |
| Észak-Amerika |USA északi középső régiója |USA déli középső régiója |
| Észak-Amerika |USA nyugati régiója, 2. |USA nyugati középső régiója 
| Egyesült Királyság |Az Egyesült Királyság nyugati régiója |Az Egyesült Királyság déli régiója |
| Védelmi Minisztérium, USA |US DoD – Kelet |US DoD – Középső régió |
| Az USA kormányzata |USA-beli államigazgatás – Arizona |USA-beli államigazgatás – Texas |
| Az USA kormányzata |USA-beli államigazgatás – Iowa (3) |USA-beli államigazgatás – Virginia |
| Az USA kormányzata |USA-beli államigazgatás – Virginia (4) |USA-beli államigazgatás – Texas |

1. táblázat – Azure regionális párok leképezése

- (1) Nyugat-Indiában nem egyezik, mert csak egy irányban egy másik régió párban áll. Nyugat-Indiát, a másodlagos régióba Dél-India, Dél-India másodlagos régióba azonban közép-India.
- (2) Dél-brazíliai régióban egy egyedülálló megoldás, mert kívül a saját földrajzi régió párban áll. Dél-Brazília másodlagos régió az USA déli középső Régiójában, de USA déli középső Régiójában a másodlagos régió nem Dél-brazíliai régióban.
- (3) másodlagos Egyesült Államokbeli beli államigazgatás – Iowa régióban US Gov – Virginia, de US Gov Virginia másodlagos régió nem US Gov – Iowa.
- (4) Egyesült Államok beli államigazgatás – Virginia másodlagos régióba US Gov Texas, de US Gov Texas másodlagos régió nem US Gov Virginia.


Azt javasoljuk, hogy számítási feladatok replikálhatja számára, hogy az Azure-elkülönítési és rendelkezésre állás házirendek regionális párokról. Ha például a tervezett Azure-rendszerfrissítések telepített egymás után (nem egy időben) párosított régiók között elosztva. Ez azt jelenti, hogy az esemény ritkán fordul elő egy hibás frissítés, még akkor is, mindkét régióban nem lesz hatással egyszerre. Továbbá egy széles körű leállás nem valószínű esetben minden párból legalább egyik régió helyreállítása előnyt élvez.

## <a name="an-example-of-paired-regions"></a>Egy példa párosított régiók
2. ábra alább látható egy képzeletbeli alkalmazást, amely a regionális párokból érdemes használja a vész-helyreállítási. A zöld számok jelölje ki a régiók közötti tevékenységek három Azure-szolgáltatások (Azure számítási, tárolási és adatbázis-) és azok miként vannak konfigurálva a régiók közötti replikálására. Az üzembe helyezést több párosított régióra egyedi előnyeit a narancssárga számok ki vannak emelve.

![Párosított régió előnyt áttekintése](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

2. ábra – elméleti Azure regionális párokból érdemes

## <a name="cross-region-activities"></a>Régiók közötti tevékenységek
2. ábra az említett.

![PaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure Compute (PaaS)** – további számítási erőforrásokat előzetesen győződjön meg, hogy az erőforrások elérhetők egy másik régióban egy katasztrófa során kell kiépítenie. További információkért lásd: [műszaki útmutatást az Azure rugalmassága](resiliency/resiliency-technical-guidance.md).

![Tárolási](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage** -Georedundáns tárolás (GRS) alapértelmezés szerint konfigurálva, egy Azure Storage-fiók létrehozásakor. A grs Tárolással az adatok automatikus replikációja háromszor az elsődleges régióban, és három alkalommal a párosított régióban. További információkért lásd: [Azure Storage Redundanciabeállításainál](storage/common/storage-redundancy.md).

![Az Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL Database-adatbázisok** – az Azure SQL Standard Georeplikáció, konfigurálhatja a tranzakciók egy párosított régióba aszinkron replikáció. A prémium szintű georeplikáció konfigurálhat replikációt bármely régióba a világ; azt javasoljuk azonban, ezeket az erőforrásokat a legtöbb vész-helyreállítási helyzetekben a párosított régióban telepít. További információkért lásd: [Georeplikáció az Azure SQL Database](sql-database/sql-database-geo-replication-overview.md).

![Erőforrás-kezelő](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager** – Resource Manager természetüknél fogva biztosítják azok logikai elkülönítését, szolgáltatás-felügyeleti összetevők régiók között elosztva. Ez azt jelenti, hogy egy adott régióban logikai hibák kevésbé valószínű, hogy egy másik hatással.

## <a name="benefits-of-paired-regions"></a>Párosított régiók előnyei
2. ábra az említett.  

![Elkülönítés](./media/best-practices-availability-paired-regions/5Orange.png)
**fizikai elkülönítése** – Ha lehetséges, az Azure legalább 300 mérföld kellő mértékű elkülönítése a regionális párok adatközpontok részesíti előnyben, bár ez nem praktikus, vagy lehetséges az összes régióban. A fizikai adatközpontban működnek elkülönítése csökkenti a természeti katasztrófák, zavargások, áramkimaradások vagy fizikai hálózat fennakadásai mindkét régióban egyszerre valószínűségét. Elkülönítés a korlátok (földrajzi méret, teljesítmény és a hálózati infrastruktúra elérhetőségét, szabályzat, stb.) földrajzi helyen belül van.  

![Replikációs](./media/best-practices-availability-paired-regions/6Orange.png)
**Platform által biztosított replikáció** – bizonyos szolgáltatások, például a Georedundáns tárolás adja meg a replikálás automatikus, a párosított régióba.

![Helyreállítási](./media/best-practices-availability-paired-regions/7Orange.png)
**régió helyreállítási rendelés** – abban az esetben, ha egy széles körű leállás esetén az egyik régió helyreállítása előnyt élvez minden párból. Párosított régióban üzembe helyezett alkalmazások garantáltan rendelkezik egy helyre prioritással rendelkező régiót. Ha egy alkalmazás, amely a rendszer nem a párosított régióban üzemel, a helyreállítási késésével – a legrosszabb esetben a kiválasztott régióban lehet, hogy az utolsó két kell helyreállítani őket.

![Frissítések](./media/best-practices-availability-paired-regions/8Orange.png)
**egymást követő Adatfrissítés** – tervezett Azure-rendszerfrissítések jelennek meg a társított két régió bármelyikén egymás után (nem egy időben) állásidő, hibák, és a egy hibás, a ritka esemény logikai hibák hatásának minimálisra csökkentése érdekében frissítés.

![Adatok](./media/best-practices-availability-paired-regions/9Orange.png)
**adattárolás** – egy régióban található a pár (kivéve Dél-brazíliai), a megegyező területen található adatok tárolási helyére vonatkozó előírásoknak, ami adóügyi és törvényi végrehajtás szempontjából joghatósága betartása érdekében.
