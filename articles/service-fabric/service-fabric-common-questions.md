---
title: A Microsoft Azure Service Fabric kapcsolatos gyakori kérdéseket |} A Microsoft Docs
description: A Service Fabric és a válaszok kapcsolatos gyakori kérdések
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: ''
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: chackdan
ms.openlocfilehash: d864a663604794a249b08a7c7be471c3abba32af
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971536"
---
# <a name="commonly-asked-service-fabric-questions"></a>Gyakori kérdések a Service Fabric

Nincsenek számos összetevővel kapcsolatos gyakori kérdésekre mi a Service Fabric képességeit és hogyan kell használni. Ez a dokumentum ismerteti ezeket a gyakori kérdéseket és a válaszok.

## <a name="cluster-setup-and-management"></a>Fürt beállítása és kezelése

### <a name="how-do-i-rollback-my-service-fabric-cluster-certificate"></a>Hogyan hajthatom végre visszaállítási saját Service Fabric-fürt tanúsítványt?

Az alkalmazásnak minden olyan frissítés szükséges visszagörgetése hibaészlelés állapotát a Service Fabric fürtök kvóruma számára a módosítás; véglegesítése előtt véglegesített módosítások csak előre lesz állítva. Eszkalációs mérnök a keresztül ügyfél-támogatási szolgálathoz, helyreállítani, szükség lehet, ha egy nem figyelt használhatatlanná tévő tanúsítvány változás fejlődéséből.  [Service Fabric-alkalmazás frissítése](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade?branch=master) vonatkozik [alkalmazásfrissítési paraméterek](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-parameters?branch=master), és nyújt nulla állásidő frissítési megtartva.  Az ajánlott alkalmazás a következő frissítési figyelt módot, állapot-ellenőrzések passzok, működés közbeni vissza automatikusan, ha egy alapértelmezett szolgáltatás frissítése nem sikerül típusán alapuló automatikus folyamat révén a frissítési tartományok.
 
Ha a fürt továbbra is használhatja a a Resource Manager-sablonban, ajánlott a klasszikus tanúsítvány-ujjlenyomat tulajdonság, [köznapi név származó tanúsítvány-ujjlenyomat-módosítás fürt](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-change-cert-thumbprint-to-cn), használhatja a modern titkos kulcsok felügyeleti funkciókat.

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Több Azure-régióban, vagy saját adatközpontok kiterjedő álló fürtöket tud létrehozni?

Igen. 

A core a Service Fabric-fürtözési technológia használható úgy, hogy bárhol a világon futtató gépek mindaddig, amíg egymáshoz hálózati kapcsolattal rendelkeznek. Ugyanakkor ilyen fürt készítése és bonyolult feladatnak bizonyulhat.

Ha érdekli, ebben a forgatókönyvben, azt javasoljuk, hogy az ügyfél beolvasása vagy keresztül a [Service Fabric Github Hibalistájában](https://github.com/azure/service-fabric-issues) vagy a támogatási képviselő további útmutatást a beszerzéséhez. A Service Fabric csapata dolgozik azon, hogy az egyértelműség érdekében további, útmutatást és javaslatokat ehhez a forgatókönyvhöz. 

Néhány mérlegelendő szempont ezzel kapcsolatban: 

1. Az Azure-ban a Service Fabric fürterőforrás regionális még ma, mivel a virtuálisgép-méretezési csoport beállítja, hogy a fürt épül. Ez azt jelenti, hogy egy regionális hiba esetén előfordulhat, hogy elveszíti képes kezelni a fürtöt az Azure Resource Manager vagy az Azure Portalon keresztül. Ez akkor fordulhat elő, annak ellenére, hogy a fürt továbbra is futni fog, és közvetlenül használni tudná. Emellett az Azure jelenleg nem használt összegeket egyetlen virtuális hálózattal, amely használható régióban vannak. Ez azt jelenti, hogy az Azure-ban egy többrégiós fürt igényel vagy [minden egyes virtuális gépéhez a Virtuálisgép-méretezési csoportok nyilvános IP-címek](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) vagy [Azure VPN Gateway átjárók](../vpn-gateway/vpn-gateway-about-vpngateways.md). A fenti hálózatkezelési lehetőségek költségek, teljesítmény, különböző hatásokkal rendelkezik, és bizonyos mértékben az alkalmazások kialakítását, hogy így alapos elemzése és tervezése előtt meg kell adni az ilyen környezet felállítását illeti.
2. A karbantartás, kezeléséhez, és ezek a gépek figyelését is bonyolultakká válnak, különösen akkor, ha átnyúlhatnak _típusok_ környezetek, például közötti érvénnyel a különböző felhőszolgáltatók, vagy a helyszíni erőforrások és az Azure között. Ügyelni kell arra, hogy frissítéseket, figyelés, felügyeleti és diagnosztikai értelmezni tud a fürt és az alkalmazások az ilyen környezetekben éles számítási feladatok futtatása előtt. Ha már rendelkezik tapasztalattal az Azure-ban vagy a saját az adatközpontokon belül problémák megoldásához, majd valószínű, hogy ezek azonos megoldásokat létrehozni, vagy a Service Fabric-fürtön futó is alkalmazható. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Hajtsa végre a Service Fabric-csomópont automatikusan jogosultak operációs rendszer frissítéseit?

Nincs még ma de ez is egy közös kérelmet, amely Azure kívánja szállítani.

Addig rendelkezünk [alkalmazás megadott](service-fabric-patch-orchestration-application.md) , hogy az operációs rendszerek, a Service Fabric-csomópont alatt továbbra is a javított és naprakész.

Az operációs rendszer frissítése a kihívás abban áll, hogy azok használatához általában szükség van egy újraindítás a gépet, amely ideiglenes rendelkezésre állási adatvesztést eredményez. Önmagában ez nem probléma, mivel a Service Fabric automatikusan átirányítja ezeket a szolgáltatásokat a forgalom más csomópontokra. Azonban operációs rendszer frissítése nem koordinálja a fürtön, van-e az esélye, hogy egyszerre leáll sok csomópontot. Ilyen egyidejű újraindítások egy szolgáltatáshoz, vagy egy teljes rendelkezésre állási adatvesztést okozhat legalább (számára egy állapotalapú szolgáltatás) egy adott partícióra vonatkozóan.

A jövőben tervezzük támogatni egy operációsrendszer-frissítési szabályzat, amely teljes mértékben automatizált és frissítési tartományok között, hogy annak ellenére, hogy az újraindítások és más nem várt hibák rendelkezésre állás biztosítása.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Használható a saját SF cluster nagyméretű virtuálisgép-méretezési csoportok? 

**Válasz rövid** – nem 

**Mennyi ideig választ** – Bár a nagyméretű virtuálisgép-méretezési csoportok lehetővé teszik a virtuális gép méretezése méretezési csoport példányaihoz legfeljebb 1000 virtuális Gépet, akkor nem így az elhelyezési csoportra (PGs) segítségével történik. Tartalék tartományok és frissítési tartományok (frissítési) is csak tartalék és frissítési tartománnyal elhelyezési döntéseket a szolgáltatáspéldányok replikák/szolgáltatás az elhelyezési csoport Service fabric használja belül konzisztensek. Mivel a tartalék és frissítési tartománnyal összehasonlítható csak az elhelyezési csoporton belül, az SF használható. Például, ha a PG1 VM1 FD-topológiája = 0, és a PG2 VM9 FD-topológiája = 4, ez nem jelenti azt, hogy a VM1, VM2 és a két különböző hardver állványokon, ezért SF nem használható az FD-értékek ebben az esetben elhelyezési döntéseket.

Jelenleg más nagyméretű virtuálisgép-méretezési csoportokkal kapcsolatos problémák, például a 4. szint hiánya betölteni a terheléselosztási támogatását. További [részletei a nagy méretezési csoportok](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)



### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Mi a Service Fabric-fürt minimális mérete? Miért nem lenne, kisebb?

A minimális támogatott számítási feladatok futtatása a Service Fabric-fürt mérete öt csomópontot. Fejlesztési és tesztelési célra három csomópontot tartalmazó fürt nyújtunk támogatást.

E minimális díj léteznek, mivel a Service Fabric-fürt állapot-nyilvántartó rendszer szolgáltatások, többek között az elnevezési szolgáltatás és a Feladatátvevőfürt-kezelő. Ezek olyan szolgáltatásokat, amelyek nyomon követése, mely szolgáltatások a fürtben lett telepítve, és ha azok még jelenleg üzemel, erős konzisztencia függenek. A konzisztenciát, függ beszerezni lehetővé teszi egy *kvórum* bármely azok állapotát az adott frissítés szolgáltatási, ahol a kvórum jelenti a szigorú többsége a replikák (N/2 + 1) egy adott szolgáltatáshoz.

Végzett most vizsgálja meg néhány lehetséges fürtkonfigurációk:

**Egy csomópont**: Ez a beállítás nem biztosít magas rendelkezésre állású óta az egycsomópontos elvesztését bármilyen okból azt jelenti, hogy az egész fürt elvesztését.

**Két csomópont**: a két csomópontra telepített szolgáltatáshoz a kvórum (N = 2) a következő 2 (2/2 + 1 = 2). Egyetlen replika nem vesztek el, ha nem lehet létrehozni egy kvórum. Egy szolgáltatás frissítése szükséges ideiglenesen le egy replikát tart, ez a beállítás nem lehet hasznos.

**Három csomóponttal**: három csomóponttal (N = 3) a követelmény létrehozása egy kvórum, még két csomópont (3/2 + 1 = 2). Ez azt jelenti, hogy az egyes csomópontok elvesznek, és továbbra is a kvórum fenntartásához.

A három csomópontos fürtkonfiguráció azért támogatott fejlesztési és tesztelési biztonságosan végzi frissítéseket és stabilitást biztosít az egyes csomópontok meghibásodásai, mert mindaddig, amíg azok nem egyszerre következnek be. Az éles számítási feladatokhoz, rugalmasnak kell lennie az ilyen egyidejű hibája esetén, így öt csomópont szükség.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>Ki lehet kapcsolni a fürt, éjszaka és Hétvége költségeit?

Általában nem. A Service Fabric tárolja állapot helyi, rövid élettartamú lemezeken, ami azt jelenti, hogy a virtuális gép áthelyezését egy másik gazdagépre, ha az adatok nem helyezi át a vele. A normál működés ez nem probléma, az új csomópont más csomópontok naprakész állapotba. Azonban ha állítsa le az összes csomóponton, és később újraindíthatja, nincs jelentős lehetséges, hogy a csomópontok a legtöbb indítsa el az új gazdagépek számára, ellenőrizze a rendszer nem lehet helyreállítani.

Ha szeretné tesztelni az alkalmazását, mielőtt telepítené a fürtök létrehozása, javasoljuk, hogy dinamikusan hozzon létre ezen a fürtök részeként a [folyamatos integráció/folyamatos üzembe helyezési folyamat](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-to-windows-server-2016"></a>Hogyan frissíthetem az operációs rendszer (például a Windows Server 2012, Windows Server 2016-ra)?

Még dolgozunk egy továbbfejlesztett még ma, miközben Ön felelős a frissítést. Frissítenie kell az operációsrendszer-képet a virtuális gépek a fürt egy virtuális gép egyszerre. 

### <a name="can-i-encrypt-attached-data-disks-in-a-cluster-node-type-virtual-machine-scale-set"></a>Egy fürtcsomóponttípus (virtuálisgép-méretezési) a csatlakoztatott adatlemezekkel is titkosítani?
Igen.  További információkért lásd: [-fürt létrehozása csatlakoztatott adatlemezekkel rendelkező](../virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks.md#create-a-service-fabric-cluster-with-attached-data-disks), [(PowerShell) lemezek titkosítása](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md), és [(CLI) lemezek titkosítása](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-cli.md).

### <a name="what-are-the-directories-and-processes-that-i-need-to-exclude-when-running-an-anti-virus-program-in-my-cluster"></a>Mik azok a könyvtárak és folyamatokat kizárhat, amikor egy víruskereső program futtatása a fürtben kell?

| **A víruskereső kizárt könyvtárak** |
| --- |
| Program Files\Microsoft a Service Fabric |
| FabricDataRoot (a fürt konfiguráció) |
| FabricLogRoot (a fürt konfiguráció) |

| **A víruskereső kizárt folyamatok** |
| --- |
| Fabric.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |
 
## <a name="application-design"></a>Alkalmazás-tervezés

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>Mi az a legjobb módszer használatával adatokat lekérdezni, egy megbízható gyűjteményben partíciók között?

A Reliable collections jellemzően [particionált](service-fabric-concepts-partitioning.md) horizontális felskálázás nagyobb teljesítményt és az átviteli sebesség engedélyezéséhez. Ez azt jelenti, hogy az adott szolgáltatás állapota tíz vagy több száz gépek között előfordulhat, hogy lehetnek elosztva. A teljes adatkészletet keresztül műveletek végrehajtásához néhány lehetőségek állnak rendelkezésére:

- Létrehoz egy szolgáltatást, amely használatával a szükséges adatok kérhetők le egy másik szolgáltatás összes partíció.
- Hozzon létre egy szolgáltatás, amely egy másik szolgáltatás összes partíció fogadhat adatokat.
- Rendszeres időközönként adatok leküldése az egyes szolgáltatások egy külső tároló. Ez a módszer csak akkor megfelelő, ha a lekérdezéseket hajt végre nem részei az alapvető üzleti logikát.


### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>Mi az a legjobb módszer használatával adatokat lekérdezni az actors között?

Actors tervezték független egység állapota és a számítási erőforrásokat, ezért nem javasoljuk, hogy az aktorok állapotának futásidőben széles körű lekérdezéseket. Ha nincs szüksége a lekérdezés aktorállapot teljes szétosztva, érdemes vagy:

- Az aktor szolgáltatások cserélje le a stateful reliable services úgy, hogy a több hálózati kérések actors száma a szolgáltatás a partíciók száma az összes adatainak gyűjtésére.
- Az aktorok rendszeres időközönként le egy külső tároló egyszerűbb az állapotuk tervez. A fenti Ez a módszer csak akkor megvalósítható, ha a lekérdezéseket hajt végre nem szükségesek a működését.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Mennyi adatot tárolhatok a Reliable Collection?

A Reliable services általában particionáltak, így a tárolhatja csak korlátozza a fürtben található gépek száma, és ezeken a gépeken rendelkezésre álló memória mennyisége.

Például tegyük fel, hogy egy megbízható gyűjteményben 100 partíció és 3 replika egy szolgáltatásban átlagos 1 kb méretű objektumok tárolására. Most tegyük fel, hogy egy 16gb memóriája gépenként 10 gép fürtöt. Az egyszerűség és a óvatosan, feltételeztük, hogy az operációs rendszer és a helyrendszeri szolgáltatások, a Service Fabric-futtatókörnyezet és a szolgáltatások felhasználásához, 6gb, 10 GB-os gépenként érhető el, vagy 100 GB-os és a fürt.

Vegye figyelembe, hogy minden objektumnak kell lennie tartja tárolt három időpontokban (egy elsődleges és két replika), hogy körülbelül 35 millió objektumok számára elegendő memória a gyűjtemények teljes kapacitás üzemi. Azt javasoljuk azonban, egy hiba tartomány és a egy frissítési tartományt, amely körülbelül 1/3 kapacitás képvisel, és hány csökkentené nagyjából 23 millió egyidejű elvesztése folyamatban.

Vegye figyelembe, hogy a számítás is feltételezi, hogy:

- Hogy az adatok a partíciók közt a terjesztési nagyjából egységes vagy, hogy Ön már reporting terhelési mérőszámok a fürt Resource Manager. Alapértelmezés szerint a Service Fabric betölti egyenleg replika száma alapján. Az előző példában, amely célszerű 10 elsődleges replika és másodlagos replikák 20 helyezni a fürt minden csomópontján. Amely jól terhelés egyenletesen oszlik el a partíciók között működik. Ha terhelés nem még akkor is, jelentenie kell terhelés, hogy az erőforrás-kezelő is együtt csomag kisebb replikákat, és nagyobb replikák több memóriát az egyes csomóponton engedélyezi.

- Hogy a szóban forgó reliable Services a fürtben csak egy tárolását állapota. Mivel több szolgáltatást is üzembe helyezhet egy fürthöz, fordítson erőforrást kell, hogy minden egyes kell futtatni és felügyelni az állapotában.

- Hogy a fürt, másrészt nem növekvő vagy zsugorítását. Ha további gépeket ad hozzá, a Service Fabric fog újraegyensúlyozására kihasználhatja a megnövelt kapacitás, amíg azon gépek száma, mivel az egyes replika nem terjedhetnek ki gépeket teret hagy a szolgáltatáshoz, a partíciók száma a replikákat. Ezzel szemben az gépek törlésével csökkentse a fürt méretét, ha a replikák nagyobb mértékben vannak-e csomagolt és kevésbé kihasznált teljes kapacitás rendelkezik.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Mennyi adatot tárolhatok a egy szereplő?

Akárcsak a reliable services actor-szolgáltatások is tárolt adatok mennyisége csak korlátozza a teljes lemezterület és a rendelkezésre álló memória a fürtben található csomópontok között. Azonban az egyes actors tárolják az állapot és a kapcsolódó üzleti logika kisebb mennyiségű használatakor is leghatékonyabb. Általános szabály egy egyéni szereplő kilobájtban mért állapotban kell rendelkeznie.

## <a name="other-questions"></a>Egyéb kérdések

### <a name="how-does-service-fabric-relate-to-containers"></a>Hogyan kapcsolódnak a Service Fabric tárolók?

Tárolók csomag szolgáltatások és azok függőségeit egyszerű módszert kínálnak, úgy, hogy az összes környezetben konzisztens módon futtassa, és a egy egyetlen gépen elkülönített módon működhet. A Service Fabric egy telepíthetnek vagy kezelhetnek szolgáltatásokat, beleértve a megoldást kínál a [szolgáltatásokat, amelyek rendelkeznek egy tárolóban van csomagolva](service-fabric-containers-overview.md).

### <a name="are-you-planning-to-open-source-service-fabric"></a>Tervezi a nyílt forráskódú Service Fabric?

A Service Fabric nyílt forráskódú részeit van ([a reliable services-keretrendszer](https://github.com/Azure/service-fabric-services-and-actors-dotnet), [a reliable actors-keretrendszer](https://github.com/Azure/service-fabric-services-and-actors-dotnet), [ASP.NET Core-integráció kódtárak](https://github.com/Azure/service-fabric-aspnetcore), [ A Service Fabric Explorer](https://github.com/Azure/service-fabric-explorer), és [Service Fabric parancssori felület](https://github.com/Azure/service-fabric-cli)) a Githubon, és fogadja el azokat a projekteket, a közösségi közreműködést. 

Hogy [megtudhat](https://blogs.msdn.microsoft.com/azureservicefabric/2018/03/14/service-fabric-is-going-open-source/) , hogy tervezzük nyílt forráskódú Service Fabric-futtatókörnyezet. Rendelkezünk ezen a ponton a [Service Fabric-adattárat](https://github.com/Microsoft/service-fabric/) akár a Linux és a Githubon összeállításához és teszteléséhez eszközökkel, ami azt jelenti, hogy akkor is klónozza az adattárat, hozhat létre a Service Fabric Linux-, egyszerű tesztek futtatása, nyissa meg a problémákat és pull-kérelmek elküldéséhez. Dolgozunk a Windows-összeállító környezetet át, és teljes CI-környezet beolvasásához.

Kövesse a [Service Fabric blog](https://blogs.msdn.microsoft.com/azureservicefabric/) vagyunk bejelentésnek további részletekért.

## <a name="next-steps"></a>További lépések

- [A Service Fabric alapfogalmait és ajánlott eljárások ismertetése](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)
