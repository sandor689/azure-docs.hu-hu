---
title: A particionált Azure Service Bus-üzenetsorok és témakörök létrehozása |} Microsoft Docs
description: Ismerteti a Service Bus-üzenetsorok és témakörök használatával több üzenetet brókerek particionálásához.
services: service-bus-messaging
author: sethmanheim
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 06/06/2018
ms.author: sethm
ms.openlocfilehash: f9fb5f53496ea3f98a9d3341e77db283a3e3b570
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824370"
---
# <a name="partitioned-queues-and-topics"></a>Particionált üzenetsorok és témakörök

Az Azure Service Bus védelmi funkciókat alkalmaz, több üzenetet brókerek üzenetek feldolgozásához, és több üzenetküldési tároló üzenetek tárolásához. Hagyományos üzenetsor vagy témakör egyetlen üzenet ügynök által kezelt és egy üzenetküldési tárolóban tárolja. A Service Bus *partíciók* üzenetsorok és témakörök, engedélyezéséhez vagy *üzenetküldési entitások*, több üzenetet brókerek és üzenetküldési tárolók particionálandó. Particionálás, az azt jelenti, hogy a teljes átviteli sebesség egy particionált entitás már nem korlátozzák a egyetlen üzenet broker vagy az üzenetküldési tárolóban teljesítményét. Ezenkívül átmenetileg nem működik az üzenetküldési tárolóban nem képezhető le egy particionált üzenetsor vagy témakör nem érhető el. A particionált üzenetsorok és témakörök tartalmazhat összes speciális Service Bus-funkciók, például a tranzakciók és a munkamenetek támogatása.

> [!NOTE]
> Particionálás entitás létrehozása minden üzenetsorok és témakörök a Basic vagy Standard termékváltozat címen érhető el. A prémium szintű üzenetkezelési Termékváltozat nem érhető el, de a prémium névterekben bármely korábban meglévő particionált entitások továbbra is a várt módon működik.
 
Nem lehet módosítani a particionálási lehetőséget, a meglévő üzenetsor vagy témakör; az entitás létrehozásakor a beállítás csak állíthatja be.

## <a name="how-it-works"></a>Működés

Minden egyes particionált üzenetsor vagy témakör több töredék áll. Minden egyes töredék egy másik üzenetküldési tárolóban tárolja, és egy másik üzenet broker kezeli. Amikor egy üzenetet küld egy particionált üzenetsor vagy témakör, a Service Bus rendel az üzenet a töredék egyikéhez. A kijelölés véletlenszerűen történik, a Service Bus vagy a küldő megadó partíciós kulcs használatával.

Amikor egy ügyfél szeretne üzenetet kap egy particionált várólista, vagy egy particionált témakör, a Service Bus lekérdezések előfizetés összes részlete üzenetek, majd visszatér az első üzenet előállított, bármelyik az üzenetküldési tárolók a fogadó. A Service Bus-gyorsítótárak a másik állapotüzeneteit, és visszaadja azokat fogadásakor további kérések fogadásához. A fogadó ügyfél nincs tisztában a particionálás; az ügyfél irányultságú viselkedését particionált üzenetsor vagy témakör (például Olvasás, végezze el, késlelteti a kézbesítetlen levelek, prefetching) az olyan rendszeres entitás viselkedésének azonos.

Nincs üzenet küldésekor, vagy egy üzenet fogadását a particionált üzenetsor vagy témakör további költség nélkül.

## <a name="enable-partitioning"></a>Particionálás engedélyezése

Particionált üzenetsorok és témakörök az Azure Service Bus használatához az Azure SDK-val 2.2 vagy újabb verzióját, vagy adjon meg `api-version=2013-10` vagy későbbi részében a HTTP-kérelmekre.

### <a name="standard"></a>Standard

A normál üzenetkezelési rétegben Service Bus-üzenetsorok és témakörök 1, 2, 3, 4 vagy 5 GB-os méret (az alapértelmezett érték 1 GB-os) hozhat létre. A particionálás engedélyezve van, a Service Bus az entitás (16 partíciók) 16 másolatokat készít a megadott GB alapú egységárát. Így, ha létrehoz egy sort, amely 5 GB-nál, 16 partíciókat a várólista maximális hossza válik (5 \* 16) = 80 GB. Megjelenik a particionált üzenetsor vagy témakör maximális mérete alapján a bejegyzés a [Azure-portálon][Azure portal], a a **áttekintése** panel az adott entitáshoz.

### <a name="premium"></a>Prémium

Prémium szint névtérben particionálás entitások nem támogatott. Azonban továbbra is létrehozhat Service Bus-üzenetsorok és témakörök az 1, 2, 3, 4, 5, 10, 20, 40 vagy 80 GB-os méret (az alapértelmezett érték 1 GB-os). Megjelenik az üzenetsor vagy témakör mérete alapján a bejegyzés a [Azure-portálon][Azure portal], a a **áttekintése** panel az adott entitáshoz.

### <a name="create-a-partitioned-entity"></a>A particionált entitás létrehozása

Többféleképpen particionált üzenetsor vagy témakör létrehozásához. Amikor az alkalmazás az üzenetsor vagy témakör hoz létre, particionálás az üzenetsor vagy témakör rendre beállításával engedélyezheti a [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] vagy [ TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] tulajdonságot **igaz**. Ezeket a tulajdonságokat kell beállítani az üzenetsor vagy témakör jön létre, és csak a régebbi a rendelkezésre álló [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) könyvtárban. Ahogy korábban is hangsúlyoztuk, nincs lehetőség ezen egy meglévő üzenetsor vagy témakör tulajdonságainak módosítását. Példa:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Másik lehetőségként létrehozhat particionált üzenetsor vagy témakör, a [Azure-portálon][Azure portal]. A portálon, üzenetsor vagy témakör létrehozásakor a **particionálás engedélyezése** beállítást az üzenetsor vagy témakör **létrehozása** párbeszédpanelen alapértelmezés szerint be van jelölve. Csak letilthatja ezt a beállítást, a Standard csomag entitásban; a prémium tarifacsomagra particionálás nem támogatott, és a jelölőnégyzet nincs hatása. 

## <a name="use-of-partition-keys"></a>Partíciós kulcsok használata

Ha üzenetet a várólistában levő particionált üzenetsor vagy témakör azokat, a Service Bus ellenőrzi, hogy a partíciós kulcs. Ha megtalálja, a részlet kulcs alapján választja ki. Ha itt nem talál egy partíciókulcsot, a részlet egy belső algoritmus alapján választja ki.

### <a name="using-a-partition-key"></a>A partíciókulcsok használatával

Egyes esetekben, például a munkamenetet vagy tranzakciók, üzenetek egy adott töredékben szereplő tárolásához szükséges. Ezek a forgatókönyvek a partíciós kulcs használata szükséges. Ugyanazzal a partíciókulccsal használó összes üzenet ugyanazon töredék vannak hozzárendelve. Ha a töredéke átmenetileg nem érhető el, a Service Bus hibát ad vissza.

A forgatókönyvtől függően különböző üzenettulajdonságok egy partíció kulcsaként vannak használatban:

**Munkamenet-azonosító**: Ha egy üzenetet a [munkamenet-azonosító](/dotnet/api/microsoft.azure.servicebus.message.sessionid) tulajdonság, akkor a Service Bus használ **munkamenet-azonosító** partíciókulcsnak. Így az ugyanazon üzenet broker ugyanahhoz a munkamenethez tartozó összes üzenet kezeli. Munkamenetek engedélyezze a Service Bus message rendezés, valamint a munkamenet-állapotok konzisztencia biztosításához.

**PartitionKey**: Ha egy üzenetet a [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) tulajdonság, de nem a [munkamenet-azonosító](/dotnet/api/microsoft.azure.servicebus.message.sessionid) tulajdonság, akkor a Service Bus használ a [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) tulajdonság a partíciós kulcs érték. Ha az üzenet is szerepel a [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) és a [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) tulajdonságok beállítása tulajdonságot is meg kell egyeznie. Ha a [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) tulajdonsága egy másik értéket, mint a [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) tulajdonság, a Service Bus érvénytelen művelet kivételt adja vissza. A [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) tulajdonság használandó, ha a munkamenet-kompatibilis tranzakciós üzeneteket küldő. A partíciós kulcs biztosítja, hogy az azonos üzenetkezelési broker tranzakción belül küldött összes üzenet kezeli.

**MessageId**: Ha az üzenetsor vagy témakör a [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) tulajdonsága **igaz** és a [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) vagy [ PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) tulajdonságai nincsenek beállítva, akkor a [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) tulajdonság értéke a partíciós kulcs funkcionál. (A Microsoft .NET- és AMQP könyvtárak automatikusan rendeljen egy üzenet azonosítója, ha a küldő alkalmazás viszont nem.) Ebben az esetben ugyanaz az üzenet összes példányát a ugyanazon üzenet broker kezeli. Ez az azonosító lehetővé teszi, hogy a Service Bus észleli, és kiszűri a duplikált üzenetek. Ha a [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) tulajdonság értéke nem **igaz**, a Service Bus nem tekinti a [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) partíciókulcsként tulajdonság.

### <a name="not-using-a-partition-key"></a>Nem a partíciókulcsok használatával

A partíciós kulcs hiányában Service Bus osztja el a ciklikus multiplexeléssel a particionált várólista vagy a témakör a szilánkok üzenetek. A kiválasztott töredéke nem érhető el, ha a Service Bus rendel az üzenet különböző töredéket. Ezzel a módszerrel a küldési művelet sikeres, annak ellenére, hogy a elérhetetlenség egy üzenetküldési tárolóban. Azonban Ön nem érhető el a garantált rendezés, amely a partíciós kulcs biztosít.

A rendelkezésre állási (nincs partíciós kulcs) és a konzisztencia (a partíciókulcsok használatával) közötti kompromisszumot részletesebb tárgyalását lásd: [Ez a cikk](../event-hubs/event-hubs-availability-and-consistency.md). Ezt az információt a Service Bus particionált entitások egyaránt vonatkozik.

Ahhoz, hogy megkapja a Service Bus elegendő időt várakozási sorba állítani az üzenet be egy másik kódrészletet a [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) érték van megadva, az ügyfél által küldött, az üzenet 15 másodperc nagyobbnak kell lennie. Javasoljuk, hogy állítsa a [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) tulajdonság alapértelmezett értéke 60 másodperc.

A partíciós kulcs "PIN" egy üzenetet, amely egy adott töredéket. Az üzenetküldési tárolóban, amely tárolja az e töredékben nem érhető el, ha a Service Bus hibát ad vissza. A partíciós kulcs hiányában a Service Bus használhatja a különböző töredékkel, és a művelet sikeres. Ezért ajánlott, hogy nem ad meg partíciókulcsot csak szükség esetén.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Speciális témakörök: tranzakciók használata a particionált entitások

A tranzakciók keretében küldött üzenetekben meg kell adni egy partíciókulcsot. A kulcs a következő tulajdonságok egyike lehet: [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), vagy [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid). Ugyanabban a tranzakcióban részeként küldött összes üzenet ugyanazzal a partíciókulccsal kell adnia. Ha úgy próbálja elküldeni egy üzenetet, nem tartozik partíciós kulcs tranzakción belül, a Service Bus érvénytelen művelet kivételt adja vissza. Ha megkísérli különböző partíciókulcsúak rendelkező ugyanazon a tranzakción belül több üzenetet küld, a Service Bus érvénytelen művelet kivételt adja vissza. Példa:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    Message msg = new Message("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.SendAsync(msg); 
    ts.CompleteAsync();
}
committableTransaction.Commit();
```

Ha partíciókulcsként átadott tulajdonságainak vannak beállítva, a Service Bus PIN az üzenetre adott töredéket. Ez akkor fordul elő, attól függetlenül, hogy egy tranzakció használható. Javasoljuk, hogy nincs megadva a partíciós kulcs esetén nem szükséges.

## <a name="using-sessions-with-partitioned-entities"></a>Munkamenetek használata a particionált entitások

A munkamenet-kompatibilis témakör vagy a várólista tranzakciós üzenet küldése, az üzenet kell rendelkeznie a [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) tooltipservice.ToolTip tulajdonság. Ha a [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) tulajdonság is meg van adva, akkor meg kell egyeznie a [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) tulajdonság. Ha ezek eltérnek, a Service Bus érvénytelen művelet kivételt adja vissza.

Normál (nem particionált) üzenetsorok és témakörök nincs lehetőség több üzenetet küldeni a különböző munkamenetekben egyetlen tranzakció használatával. Kísérlet történt, ha a Service Bus érvénytelen művelet kivételt adja vissza. Példa:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    Message msg = new Message("This is a message");
    msg.SessionId = "mySession";
    messageSender.SendAsync(msg); 
    ts.CompleteAsync();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>A particionált entitások automatikus üzenetek továbbítása

A Service Bus továbbítási az, hogy vagy particionált entitások közötti automatikus üzenet támogatja. Automatikus üzenettovábbítással engedélyezéséhez állítsa a [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] forrásvárólista vagy előfizetési tulajdonság. Ha az üzenet ad meg partíciókulcsot ([SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), vagy [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid)), a cél entitás használt partíciós kulcs.

## <a name="considerations-and-guidelines"></a>Szempontok és irányelveket
* **Magas konzisztencia szolgáltatások**: Ha entitás például munkamenetek, kettős észlelés vagy partíciós kulcs explicit irányítását funkciókat használ, akkor az üzenetkezelési műveletek mindig adott töredék legyenek átirányítva. A szilánkok bármelyikét észlel, nagy forgalmat, vagy a mögöttes tároló állapota nem kifogástalan, ha ezek a műveletek sikertelenek, és a rendelkezésre állási csökken. A teljes a konzisztencia, továbbra is sokkal nagyobb mint nem particionált entitások; a forgalom csak egy részhalmazát, szemben az összes forgalom hibákat észlelt. További információkért tekintse meg a [rendelkezésre állási és konzisztencia](../event-hubs/event-hubs-availability-and-consistency.md).
* **Felügyeleti**: műveletek, például a létrehozási, frissítési és törlési az entitás összes részlete kell végrehajtani. Bármely töredék állapota nem kifogástalan, ha az ezekhez a műveletekhez hibákat okozhat. A Get művelethez adatokat például üzenetben számlálás kell összesíteni az összes részlete. Ha bármely töredék állapota nem megfelelő, entitás rendelkezésre állását korlátozott akkor számít.
* **Üzenet forgatókönyvek kevés**: ilyen helyzetekben, különösen akkor, ha a HTTP protokollt használ, előfordulhat, hogy több végrehajtásához műveletek annak érdekében, hogy az üzenetek fogadására. A fogadási kéréseket az előtér receive végez a töredék, és gyorsítótárba helyezi azt a kapott válaszokat. Az azonos kapcsolaton keresztül egy későbbi fogadási kérést ehhez kihasználhassa a gyorsítótárazást és fogadására késések alacsonyabb lesz. Azonban ha több kapcsolatot vagy a HTTP Protokollt használja, amely kapcsolatot hoz létre új egyes kérésekre vonatkozóan. Nincs ilyen nem biztos, hogy az ugyanazon a csomóponton volna megnyílik. Ha az összes meglévő üzenetek zárolva van, és tárolja a rendszer egy másik előtér, a fogadási művelet visszaadja **null**. Üzenetek végül hagyja elévülni, majd újra fogadhatja. HTTP életben tartási ajánlott.
* **A tallózási/betekintés üzenetek**: csak a régebbi a [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) könyvtárban. [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) nem mindig ad vissza a megadott üzenetek száma a [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.messagecount) tulajdonság. Ez a viselkedés két oka van. Egyik ok: az, hogy a gyűjtemény az üzenetek összesített mérete meghaladja a maximális 256 KB-os. A másik OK, hogy ha az üzenetsor vagy témakör a [EnablePartitioning tulajdonság](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning) beállítása **igaz**, a partíció nem lehet végrehajtani a kért számú üzenetek elég üzenetek. Általában egy bizonyos számú üzeneteket az alkalmazás kéri, ha azt meg kell hívnia az [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) ismételten, amíg azt lekérdezi, hogy az üzenetek száma, vagy nincsenek való további üzenetek. További információt, Kódminták, beleértve a [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) vagy [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient.peekbatch) API dokumentációját.

## <a name="latest-added-features"></a>Legújabb szolgáltatással

* Hozzáadni vagy eltávolítani a szabály mostantól támogatják a particionált entitások. Nem particionált entitások eltérő, ezek a műveletek nem támogatottak a tranzakciók. 
* AMQP támogatása az üzenetek küldése és fogadása a és a particionált entitás.
* AMQP mostantól támogatott a következő műveletek: [kötegelt küldése](/dotnet/api/microsoft.servicebus.messaging.queueclient.sendbatch), [kötegelt kap](/dotnet/api/microsoft.servicebus.messaging.queueclient.receivebatch), [fogadási sorszáma szerint](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive), [Belepillantás](/dotnet/api/microsoft.servicebus.messaging.queueclient.peek), [ Újítsa meg a zárolást](/dotnet/api/microsoft.servicebus.messaging.queueclient.renewmessagelock), [üzenet ütemezése](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync), [ütemezett üzenet törlése](/dotnet/api/microsoft.azure.servicebus.queueclient.cancelscheduledmessageasync), [szabály hozzáadása](/dotnet/api/microsoft.azure.servicebus.ruledescription), [szabály eltávolítása](/dotnet/api/microsoft.azure.servicebus.ruledescription), [Munkamenet Renew zárolási](/dotnet/api/microsoft.servicebus.messaging.messagesession.renewlock), [munkamenet-állapot beállítása](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate), [Get munkamenet-állapot](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate), és [munkamenetek enumerálása](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions).

## <a name="partitioned-entities-limitations"></a>Particionált entitások korlátozásai

Jelenleg a Service Bus ró a particionált üzenetsorok és témakörök a következő korlátozások vonatkoznak:

* A particionált üzenetsorok és témakörök nem támogatottak a prémium szintű üzenetkezelési rétegben.
* Particionált üzenetsorok és témakörök nem támogatják a különböző munkamenetekben egy tranzakción belül tartozó üzenetek küldését.
* A Service Bus névterenként jelenleg 100 particionált üzenetsort vagy témakört képes kezelni. Minden egyes particionált üzenetsor vagy témakör megjeleníti a beállított kvótát (érvényes prémium csomagban) névtér száma 10 000 entitások felé.

## <a name="next-steps"></a>További lépések

Olvassa el az AMQP 1.0 üzenetküldési meghatározása az alapfogalmakat a [AMQP 1.0 protokoll az útmutató](service-bus-amqp-protocol-guide.md).

[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablepartitioning
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
