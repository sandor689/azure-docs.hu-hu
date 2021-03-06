---
title: Az Azure DNS használata saját tartományok |} A Microsoft Docs
description: Az üzemeltetési szolgáltatás a Microsoft Azure saját DNS áttekintése.
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: victorh
ms.openlocfilehash: 2ab7070a4cf46dae543af8d3e1d688e12ec1eb2a
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173642"
---
# <a name="use-azure-dns-for-private-domains"></a>Az Azure DNS használata saját tartományok
A Domain Name System, vagy a DNS-beli felelős fordítása (vagy feloldása) a szolgáltatás nevét annak IP-címét. DNS-tartományok, a üzemeltetési szolgáltatás az Azure DNS névfeloldása a Microsoft Azure-infrastruktúra használatával. Internetre irányuló DNS-tartományok támogatása esetén mellett az Azure DNS már támogatja a privát DNS-tartományok előzetes verzióként. 
 
Az Azure DNS kezeléséhez és a egy virtuális hálózati tartomány neveinek feloldásához anélkül, hogy egy egyéni DNS-megoldás hozzáadása megbízható és biztonságos DNS szolgáltatást nyújt. Privát DNS-zónák használatával még ma elérhető az Azure által biztosított nevek helyett saját egyéni tartománynevet is használhatja. Egyéni tartománynevek használata segít testre szabni a virtuális hálózati architektúra ajánlott megfeleljen a szervezet igényei szerint. Névfeloldás biztosítja a virtuális gépek (VM) egy virtuális hálózaton belül és virtuális hálózatok között. Ezenkívül konfigurálhatja zónák nevek split zónaneveket nézetet, amely lehetővé teszi a privát és a egy nyilvános DNS-zóna rendelkezik ugyanazzal a névvel.

![DNS áttekintése](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="benefits"></a>Előnyök

Az Azure DNS a következő előnyöket nyújtja:

* **Nincs szükség az egyéni DNS-megoldások**. Korábban a számos ügyfél létrehozott egyéni DNS-megoldások a virtuális hálózat DNS-zónák kezelése. DNS-zónák kezelése most már elvégezheti a natív Azure-infrastruktúra, amely eltávolítja a létrehozása és kezelése az egyéni DNS-megoldások terhe használatával.

* **Minden gyakori DNS rekordjait típust használja**. Az Azure DNS támogatja A, AAAA, CNAME, MX, NS, PTR, SOA, SRV és txt típusú rekordok.

* **Automatikus állomásnév-rekord-kezelésre**. Egyéni DNS-rekordjait üzemelteti, valamint az Azure automatikusan is kezeli gazdanév-rekordokat a megadott virtuális hálózatokat a virtuális gépek. Ebben a forgatókönyvben optimalizálhatja a tartománynevek egyéni DNS-megoldások létrehozása és alkalmazások módosítása nélkül használhatja.

* **Állomásnév-feloldási virtuális hálózatok közötti**. Ellentétben az Azure által biztosított állomásnevek privát DNS-zónák virtuális hálózatok között is megoszthatók. Ez a funkció egyszerűbbé teszi a kereszt-hálózat és a szolgáltatásészlelés forgatókönyvek, például a virtuális hálózatok közötti társviszony.

* **Ismerős eszközökkel és a felhasználói élmény**. Gyorsan elsajátítható csökkentése érdekében az új ajánlat jól bevált Azure DNS-eszközök (PowerShell, az Azure Resource Manager-sablonok és a REST API-t) használ.

* **Split zónaneveket DNS-támogatás**. Az Azure DNS használata esetén létrehozhat zónák ugyanazzal a névvel, hogy különböző választ az egy virtuális hálózaton belül, és a nyilvános interneten keresztül. Split zónaneveket DNS jellemzően olyan helyzetben, hogy a virtuális hálózaton belüli használatra a szolgáltatás egy dedikált verziója.

* **Az összes Azure-régióban elérhető**. Az Azure DNS saját zónák funkció az Azure nyilvános felhő összes Azure-régióban érhető el. 


## <a name="capabilities"></a>Funkciók

Az Azure DNS az alábbi képességeket biztosítja:
 
* **Egyetlen virtuális hálózattal, amely a regisztrációs virtuális hálózatként privát zónához van csatolva a virtuális gépek automatikus regisztráció**. A virtuális gépek, regisztrált (hozzáadott) a privát zónához a magánhálózati IP-címek mutató rögzíti. Ha egy virtuális géphez a virtuális hálózat törlése egy regisztrációt, az Azure automatikusan eltávolítja a megfelelő DNS rekord a csatolt titkos zónából. 

  > [!NOTE]
  > Alapértelmezés szerint a regisztrációs virtuális hálózatok szerepét is feloldási virtuális hálózatok, abban az értelemben, hogy ellen a zóna DNS-feloldás a regisztrációs virtuális hálózaton belüli virtuális gépek bármelyik működik. 

* **A privát zónához kapcsolódó feloldási virtuális hálózatok, virtuális hálózatok közötti továbbítás DNS-feloldás támogatott**. Közötti – virtuális hálózat DNS-feloldás nincs nincs explicit függőség úgy, hogy a virtuális hálózatok társviszonyban állnak egymással. Ügyfelek érdemes azonban más esetekben (például HTTP-forgalom) virtuális hálózatok társviszonyba állítása.

* **A virtuális hálózat hatókörén belül támogatott névkeresési DNS**. A privát zónák rendelt virtuális hálózaton belül egy privát IP-címet a DNS-névkeresési ad vissza, amely tartalmazza a állomásrekord/nevét, valamint a zónanevet utótagként teljes Tartománynevét. 


## <a name="limitations"></a>Korlátozások

Az Azure DNS van a következő korlátozások vonatkoznak:

* A regisztráció csak egy virtuális hálózat privát zónánként engedélyezett.
* Legfeljebb 10 feloldási virtuális hálózatok privát zónánként engedélyezett.
* Egy adott virtuális hálózaton egy regisztrációs virtuális hálózatként kapcsolhatók ki csak egy privát zónához.
* Egy adott virtuális hálózati feloldási virtuális hálózatként legfeljebb 10 privát zónák kapcsolható ki.
* Ha meg van adva a regisztrációs virtuális hálózatot, a virtuális gépek a kiválasztott virtuális hálózatban, amelyek a saját zóna a DNS-rekordok nincsenek megtekinthető vagy nem olvasható be az Azure Powershell és az Azure CLI API-k, de a virtuális gép rekordok valóban regisztrálva van, és lesz sikerült feloldani.
* Fordított DNS működését csak a privát IP-címteret a regisztrációs virtuális hálózatban.
* Fordított DNS egy magánhálózati IP-címet, amely nincs regisztrálva van (például egy virtuális hálózatot, amely kapcsolódik a feloldási virtuális hálózatot, egy privát zónához a virtuális gép magánhálózati IP-cím) a saját zóna visszaadja *internal.cloudapp.net* a DNS-utótagként. Ennek az utótagnak viszont nem oldható fel. 
* A virtuális hálózat üresnek kell lennie (azaz nincs virtuális gép rekordok léteznek) Ha azt kezdetben (azt jelenti, először) regisztrációs vagy feloldási virtuális hálózatként privát zónák mutató hivatkozásokat. Azonban a virtuális hálózat majd lehet nem üres jövőbeli kapcsolásának egy regisztrációs vagy feloldási virtuális hálózattal, az egyéb privát zónák. 
* Jelenleg a feltételes továbbítás (például a megoldás az Azure és a rendszert hálózatok közötti engedélyezése) nem támogatott. Hogyan ügyfelek is valósíthat meg ebben a forgatókönyvben más mechanizmusok használatával kapcsolatos információkért lásd: [névfeloldás virtuális gépek és szerepkörpéldányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

A gyakori kérdések és válaszok az Azure DNS privát zónái kapcsolatban, beleértve a DNS regisztráció és a megoldási viselkedést is várható bizonyos fajta műveleteket, lásd: [– gyakori kérdések](./dns-faq.md#private-dns).  


## <a name="pricing"></a>Díjszabás

A privát DNS-zónák funkció ingyenesen a nyilvános előzetes verzió ideje alatt. Általános rendelkezésre állásra a szolgáltatás szavatolnak használatalapú díjszabási modellt a meglévő Azure DNS-ben a hasonló kínál. 


## <a name="next-steps"></a>További lépések

Ismerje meg, hogyan hozhat létre a privát zónák az Azure DNS használatával [Azure PowerShell-lel](./private-dns-getstarted-powershell.md) vagy [Azure CLI-vel](./private-dns-getstarted-cli.md).

Olvassa el néhány gyakori [privát zónák – forgatókönyvek](./private-dns-scenarios.md) , amelyek megvalósíthatók az Azure DNS saját zónáival kapcsolatos.

A gyakori kérdések és válaszok az Azure DNS privát zónái kapcsolatban, beleértve a viselkedést is várható bizonyos típusú műveletek, lásd: [– gyakori kérdések](./dns-faq.md#private-dns). 

További információ a DNS-zónák és rekordok funkcionáló [DNS-zónák és rekordok áttekintése](dns-zones-records.md).

Ebben a dokumentumban az Azure egyéb lényeges [hálózat képességeivel](../networking/networking-overview.md) ismerkedhet meg. 

