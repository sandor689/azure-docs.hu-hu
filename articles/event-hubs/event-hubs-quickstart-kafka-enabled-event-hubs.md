---
title: Streamelés az Apache Kafkához készült Azure Event Hubsba | Microsoft Docs
description: Streamelés az Event Hubsba a Kafka-protokoll és a Kafka API-k használatával.
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2018
ms.author: bahariri
ms.openlocfilehash: ec6061cac7188f3f94fa1ec0bf138b9398387099
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413620"
---
# <a name="stream-into-event-hubs-for-the-apache-kafka"></a>Streamelés az Apache Kafkához készült Event Hubsba
Ez a rövid útmutató bemutatja, hogyan streamelhet a Kafka-kompatibilis Event Hubsba anélkül, hogy módosítaná a protokollügyfeleket vagy saját fürtöket futtatna. Megtudhatja, hogyan érheti el egy egyszerű konfigurációmódosítással az alkalmazásokban, hogy az előállítók és a fogyasztók kommunikáljanak a Kafka-kompatibilis Event Hubsszal. Az Azure Event Hubs az [Apache Kafka 1.0-s verzióját](https://kafka.apache.org/10/documentation.html) támogatja.

> [!NOTE]
> Ez a minta elérhető a [GitHubon](https://github.com/Azure/azure-event-hubs).

## <a name="prerequisites"></a>Előfeltételek

A rövid útmutató elvégzéséhez győződjön meg arról, hogy teljesülnek az alábbi előfeltételek:

* Azure-előfizetés. Ha még nincs előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), mielőtt hozzákezd.
* [Java fejlesztői készlet (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* Egy [letöltött](http://maven.apache.org/download.cgi) és [telepített](http://maven.apache.org/install.html) Maven bináris archívum.
* [Git](https://www.git-scm.com/)
* [Kafka-kompatibilis Event Hubs-névtér](event-hubs-create.md)

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Kafka-kompatibilis Event Hubs-névtér létrehozása

1. Jelentkezzen be az [Azure Portalra][Azure Portalra], és kattintson az **Erőforrás létrehozása** gombra a képernyő bal felső részén.

2. Keressen rá az Event Hubsra, és válassza az itt látható lehetőségeket:
    
    ![Event Hubs keresése a portálon](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Adjon meg egy egyedi nevet, és engedélyezze a Kafkát a névtéren. Kattintson a **Create** (Létrehozás) gombra.
    
    ![Névtér létrehozása](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. A névtér létrehozását követően a **Beállítások** lap **Megosztott elérési szabályzatok** elemére kattintva kérje le a kapcsolati sztringet.

    ![Kattintás a Megosztott elérési szabályzatok elemre](./media/event-hubs-create/create-event-hub7.png)

5. Választhatja az alapértelmezett **RootManageSharedAccessKey** szabályzatot, vagy hozzáadhat egy újat is. Kattintson a szabályzat nevére, és másolja a vágólapra a kapcsolati sztringet. 
    
    ![Szabályzat kiválasztása](./media/event-hubs-create/create-event-hub8.png)
 
6. Adja hozzá ezt a kapcsolati sztringet a Kafka-alkalmazás konfigurációjához.

Így már streamelheti az eseményeket az Event Hubsba a Kafka-protokollt használó alkalmazásaiból.

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Üzenetek küldése és fogadása a Kafkával az Event Hubsban

1. Klónozza az [Azure Event Hubs-adattárat](https://github.com/Azure/azure-event-hubs).

2. Nyissa meg a `azure-event-hubs/samples/kafka/quickstart/producer` címet.

3. Az alábbiakban látható módon módosítsa az előállító konfigurációs adatait az `src/main/resources/producer.config` fájlban:

    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
4. Futtassa az előállító kódját, és streameljen a Kafka-kompatibilis Event Hubsba:
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
5. Nyissa meg a `azure-event-hubs/samples/kafka/quickstart/consumer` címet.

6. Az alábbiakban látható módon módosítsa a fogyasztó konfigurációs adatait az `src/main/resources/consumer.config` fájlban:
   
    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

7. Futtassa a fogyasztó kódját, és végezze el a feldolgozást a Kafka-kompatibilis Event Hubsból a Kafka-ügyfelek használatával:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Ha az Event Hubs Kafka-fürtön vannak események, most el kell kezdeniük érkezni a fogyasztóról.

## <a name="next-steps"></a>További lépések
Ebben a cikkben bemutattuk, hogyan streamelhet Kafka-kompatibilis Event Hubsba anélkül, hogy módosítaná a protokollügyfeleket vagy saját fürtöket futtatna. További információért folytassa az alábbi oktatóanyaggal:

> [!div class="nextstepaction"]
> [Kafka MirrorMaker használata az Event Hubsszal](event-hubs-kafka-mirror-maker-tutorial.md)
