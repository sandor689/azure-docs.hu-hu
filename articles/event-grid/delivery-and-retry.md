---
title: Az Azure Event Grid teljesítés és újrapróbálkozás
description: Azure Event Grid piacról eseményeket, és hogyan kezeli az kézbesítetlen üzenetek ismerteti.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: tomfitz
ms.openlocfilehash: b34386a7b416d6f7d8b008a9cb5ef142948a370f
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005395"
---
# <a name="event-grid-message-delivery-and-retry"></a>Event Grid az üzenetek kézbesítését, és próbálkozzon újra 

Ez a cikk bemutatja, hogyan kezeli az Azure Event Grid a eseményeket, amikor a kézbesítési arra nem vonatkozik.

Event Grid biztosít a tartós kézbesítési. Minden üzenet legalább egyszer minden egyes előfizetés esetén kínál. Az események küldhetők a regisztrált webhook-előfizetések azonnal. Ha a webhook nem igazolhatom egy eseményt az első kézbesítési kísérlet 60 másodpercen belül, az Event Grid kézbesítési esemény újrapróbálkozik. 

Jelenleg Event Grid küld minden esemény külön-külön előfizetők számára. Az előfizető egy tömböt egyetlen eseményt kap.

## <a name="message-delivery-status"></a>Üzenet kézbesítési állapota

Event Grid igazolhatom az események HTTP-válaszkódot használ. 

### <a name="success-codes"></a>Sikeres kód

A következő HTTP-válaszkódot adja meg, hogy az esemény rendelkezik lett sikeresen kézbesítve, a webhook. Event Grid úgy ítéli meg kézbesítési befejeződött.

- 200 OK
- 202-es elfogadva

### <a name="failure-codes"></a>Sikertelen kód

A következő HTTP-válaszkódot adja meg, hogy az esemény kézbesítési kísérlet meghiúsult. 

- 400 Hibás kérés
- 401-es nem engedélyezett
- 404 – Nem található
- 408 kérés időtúllépése
- 414 URI túl hosszú
- 429 túl sok kérelem
- 500 Belső kiszolgálóhiba
- 503 A szolgáltatás nem érhető el
- 504 Időtúllépés az átjárón

Ha eseményrács hiba, amely azt jelzi, hogy a végpont átmenetileg nem érhető el, vagy egy jövőbeli kérés sikeres lehet, újrapróbálkozik az eseményt. Ha eseményrács egy hibaüzenetet, amely a szállítási soha nem fog sikerülni, és [kézbesíthetetlen levelek végpont konfigurálva van](manage-event-delivery.md), a kézbesíthetetlen levelek végpont elküldi az eseményt. 

## <a name="retry-intervals-and-duration"></a>Újrapróbálkozási időközök és időtartama

Event Grid egy exponenciális leállítási újrapróbálkozási házirend eseménykézbesítés használ. A webhook nem válaszol, vagy esetén hibakódot ad vissza, ha az Event Grid újrapróbálkozik a következő ütemezés kézbesítési:

1. 10 másodperc
2. 30 másodperc
3. 1 perc
4. 5 perc
5. 10 perc
6. 30 perc
7. 1 óra

Event Grid egy kis véletlenszerű ad hozzá minden újrapróbálkozási időközhöz. Egy óra elteltével eseménykézbesítés rendszer óránként egyszer.

Alapértelmezés szerint az Event Grid összes eseményt, amely nem biztosított 24 órán belül lejár. Is [testre szabhatja az újrapróbálkozási szabályzat](manage-event-delivery.md) egy esemény-előfizetés létrehozásakor. Biztosítanak a maximális számát (alapértelmezés: 30) a kézbesítési kísérletek és az esemény élő idő (1440 perc az alapértelmezett érték).

## <a name="dead-letter-events"></a>Kézbesítetlen üzenetek esemény

Event Grid nem lehet kézbesíteni az egy eseményt, amikor azokat küldeni tudná, a kézbesítetlen esemény egy tárfiókba. Ez a folyamat az úgynevezett kézbesítetlen levelek kezelése. Kézbesítetlen események megtekintéséhez, kérheti le azokat a kézbesíthetetlen levelek helyről. További információkért lásd: [kapcsolat megszakadásának betűs, és ismételje meg a házirendek](manage-event-delivery.md).

## <a name="next-steps"></a>További lépések

* Esemény kézbesítések állapotának megtekintése: [figyelő Event Grid üzenetkézbesítése](monitor-event-delivery.md).
* Esemény a Kézbesítési beállítások testreszabásához tekintse meg a [kezelése Event Grid Kézbesítési beállítások](manage-event-delivery.md).
* Az Event Grid megismeréséhez tekintse meg [az Event Grid bevezetőjét](overview.md).
* Tekintse meg a gyors kezdéshez Event Grid használatával [az Azure Event Griddel egyéni események létrehozása és útvonal](custom-event-quickstart.md).