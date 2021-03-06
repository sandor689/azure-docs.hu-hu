---
title: A SELECT INTO Azure Stream Analytics lekérdezések hibakeresése
description: A cikk ismerteti a közepes adatlekérdezés Azure Stream Analytics-feladat a mintavételhez. a lekérdezés szintaxisa a SELECT INTO utasítás használatával.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: ccaa6203e4bfe52758e26416646f9152ac5378ea
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30907955"
---
# <a name="debug-queries-by-using-select-into-statements"></a>A SELECT INTO utasítás használatával lekérdezések hibakeresése

A valós idejű adatfeldolgozással az adatok néz közepén a lekérdezés ismerete hasznos lehet. Bemeneti adatok vagy a lépéseket az Azure Stream Analytics-feladat több alkalommal olvashatók, mert külön SELECT INTO utasítások is írhat. Ezzel a közbülső adatok kiírja a tárolóba, és lehetővé teszi, hogy ellenőrizze a helyességét az adatokat, ugyanúgy, mint *változók bemutató* tegye amikor hibakeresése a program.

## <a name="use-select-into-to-check-the-data-stream"></a>A SELECT INTO használatával ellenőrizze az adatfolyam

Egy Azure Stream Analytics-feladat a következő példa lekérdezést egy adatfolyam-bemenet, a két hivatkozás adatok bemeneti és a Azure Table Storage kimenettel rendelkezik. A lekérdezés az event hubs és a két hivatkozás blobok a nevét és kategóriáját adatainak megszerzése adatokból csatlakozik:

![Példa a SELECT INTO lekérdezést](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

Vegye figyelembe, hogy a feladat fut, de nincsenek események a kimenetben alatt előállítása. A a **figyelés** csempe látható itt, láthatja, hogy a bemeneti van olyan adatokat, de nem tudja, melyik lépésében a **csatlakozás** elvet összes esemény miatt.

![A figyelés csempe](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
Ez a helyzet néhány további SELECT INTO utasítások a "" a köztes ILLESZTÉS eredményei és bemenetről beolvasott adatok adhat hozzá.

Ebben a példában fel lett véve a két új "ideiglenes kimenetek." A fogadó tetszés el. Azure Storage itt példaként használjuk:

![Extra SELECT INTO utasítások hozzáadása](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

Majd módosíthatja a lekérdezés ehhez hasonló:

![Egy átírt SELECT INTO lekérdezést](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Most indítsa el újra a feladatot, és azt a Futtatás néhány percig. Majd lekérdezés temp1 és a Visual Studio Cloud Explorer létrehozásához az alábbi táblázatok temp2:

**temp1 tábla**
![SELECT INTO temp1 tábla](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**temp2 tábla**
![SELECT INTO temp2 tábla](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Ahogy látja, temp1 és temp2 mindkét és adatokat, és a Név oszlopban nem üres megfelelően temp2. Azonban mivel a még nincsenek adatok kimenet, valami probléma:

![A SELECT INTO output1 tábla adatot nem tartalmazó](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Az adatok mintavétele, biztos lehet majdnem biztos, hogy a probléma van a második való CSATLAKOZÁST. A referenciaadatok letöltését a blob, és tekintse meg:

![A SELECT INTO ref tábla](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Ahogy látja, a referenciaadatok a GUID formátuma formátuma eltér az [a] oszlopában temp2. Ezért az adatok nem érkeznek output1 a várt módon.

Hárítsa el a adatformátum, töltse fel az blob hivatkoznak, és próbálkozzon újra:

![A SELECT INTO ideiglenes tábla](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Most, az adatok a kimenetben formázott és feltölti a várt módon.

![A SELECT INTO végső tábla](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a>Segítségkérés

Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>További lépések

* [Az Azure Stream Analytics bemutatása](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

