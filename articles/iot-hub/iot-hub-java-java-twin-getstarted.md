---
title: Ikereszközök Azure IoT Hub (Java) – első lépések |} A Microsoft Docs
description: Hogyan használható az Azure IoT Hub device twins címkéket adhat hozzá, majd az IoT Hub-lekérdezést. Az Azure IoT eszközoldali SDK for Java használatával az eszközalkalmazás és az Azure IoT Szolgáltatásoldali SDK-val Java megvalósítása a service-alkalmazás, amely hozzáadja a címkék és az IoT Hub-lekérdezést.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: dobett
ms.openlocfilehash: b8884cafbf250b9d7a88219b5647addafee9904a
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186900"
---
# <a name="get-started-with-device-twins-java"></a>Első lépések az ikereszközökhöz (Java)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Ebben az oktatóanyagban két Java-konzolalkalmazással hoz létre:

* **Adja hozzá a címkék-lekérdezés**, a Java háttér-alkalmazást, amely címkét ad hozzá, és lekérdezi az ikereszközök.
* **a szimulált eszköz**, egy Java eszköz-alkalmazást, amely csatlakozik az IoT hubhoz, és jelenti a kapcsolat állapotát a jelentett tulajdonságok használatával.

> [!NOTE]
> A cikk [Azure IoT SDK-k](iot-hub-devguide-sdks.md) használható eszköz és a háttér-alkalmazásokat hozhat létre az Azure IoT SDK-kkal kapcsolatos információkat biztosít.

Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:

* A legújabb [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) mindössze néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-service-app"></a>Az alkalmazás létrehozása

Ebben a szakaszban egy Java-alkalmazás, amely hozzáadja a hely metaadatok kapcsolódó címkét az eszközpárok a IoT Hub létrehozása **myDeviceId**. Az alkalmazás először lekéri az IoT hub, az Egyesült Államokban található eszközökre, majd a jelentés mobilhálózati kapcsolat eszközök esetében.

1. A fejlesztői gépen, hozzon létre egy nevű üres mappát `iot-java-twin-getstarted`.

1. A a `iot-java-twin-getstarted` mappában hozzon létre egy nevű Maven-projektet **hozzá címkéket-lekérdezés** használja az alábbi parancsot a parancssorba. Látható, hogy ez egyetlen hosszú parancs:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. A parancssorban navigáljon a `add-tags-query` mappát.

1. Egy szövegszerkesztővel nyissa meg a `pom.xml` fájlt a `add-tags-query` mappát, és adja hozzá a következő függőséget a **függőségek** csomópont. Ezzel a függőséggel használhatja az **iot-service-client** csomagot az alkalmazásban, az IoT hubbal való kommunikációhoz:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Ellenőrizze, hogy a legújabb **iot-service-client** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Adja hozzá a következő **összeállítása** csomópont után a **függőségek** csomópont. Ez a konfiguráció arra utasítja a használható a Java 1.8-as hozhat létre az alkalmazást a maven használatával:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Mentse és zárja be a `pom.xml` fájlt.

1. Egy szövegszerkesztővel nyissa meg a `add-tags-query\src\main\java\com\mycompany\app\App.java` fájlt.

1. Adja hozzá a következő **importálási** utasításokat a fájlhoz:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Adja hozzá a következő osztályszintű változókat az **App** osztályhoz. Cserélje le `{youriothubconnectionstring}` feljegyzett IoT hub kapcsolati karakterláncra az *hozzon létre egy IoT hubot* szakaszban:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. Frissítés a **fő** , többek között az alábbiak podpis metody `throws` záradékot:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Adja hozzá a következő kódot a **fő** metódussal hoz létre a **DeviceTwin** és **DeviceTwinDevice** objektumokat. A **DeviceTwin** objektum kezeli az IoT hub szolgáltatással való kommunikációhoz. A **DeviceTwinDevice** objektum képviseli az ikereszközök a tulajdonságok és címkék:

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. Adja hozzá a következő `try/catch` megakadályozza a **fő** módszer:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. Frissítése az **régió** és **üzem** eszköz az ikereszköz ikereszköz a címkék hozzáadása a következő kódot a `try` letiltása:

    ```java
    // Get the device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from the existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create the tags and attach them to the DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update the device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve the device twin with the tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

1. Az eszközpárok a IoT hub lekérdezési, adja hozzá a következő kódot a `try` letiltása az előző lépésben felvett kód után. A kód két lekérdezést futtat. Minden egyes lekérdezés visszaadja az egy legfeljebb 100 eszközök:

    ```java
    // Query the device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct the query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run the query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct the query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run the query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

1. Mentse és zárja be a `add-tags-query\src\main\java\com\mycompany\app\App.java` fájl

1. Hozhat létre a **hozzá címkéket-lekérdezés** alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban navigáljon a `add-tags-query` mappát, és futtassa a következő parancsot:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Eszközalkalmazás létrehozása

Ebben a szakaszban egy Java-konzolalkalmazást, amely beállítja az IoT hubra küldött jelentett tulajdonság értéke hoz létre.

1. Az a `iot-java-twin-getstarted` mappában hozzon létre egy nevű Maven-projektet **simulated-device** használja az alábbi parancsot a parancssorba. Látható, hogy ez egyetlen hosszú parancs:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. A parancssorban navigáljon a `simulated-device` mappát.

1. Egy szövegszerkesztővel nyissa meg a `pom.xml` fájlt a `simulated-device` mappát, és adja hozzá a következő függőségeket a **függőségek** csomópont. Ezzel a függőséggel használhatja az **iot-device-client** csomagot az alkalmazásban, az IoT hubbal való kommunikációhoz:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Ellenőrizze, hogy a legújabb **iot-device-client** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Adja hozzá a következő **összeállítása** csomópont után a **függőségek** csomópont. Ez a konfiguráció arra utasítja a használható a Java 1.8-as hozhat létre az alkalmazást a maven használatával:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Mentse és zárja be a `pom.xml` fájlt.

1. Egy szövegszerkesztővel nyissa meg a `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.

1. Adja hozzá a következő **importálási** utasításokat a fájlhoz:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Adja hozzá a következő osztályszintű változókat az **App** osztályhoz. Cseréje `{youriothubname}` helyőrzőt az IoT hub nevére, és `{yourdevicekey}` eszközkulcs értékét eszközkulcsértékre a *eszközidentitás létrehozása* szakaszban:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    Ez a mintaalkalmazás a **protocol** változót használja egy **DeviceClient** objektum példányának létrehozásakor. 

1. Adja hozzá a következő kódot a **fő** metódust:
    * Hozzon létre egy ügyfél az IoT hubbal való kommunikációhoz.
    * Hozzon létre egy **eszköz** objektum az eszköz-ikertulajdonságok tárolásához.

    ```java
    DeviceClient client = new DeviceClient(connString, protocol);

    // Create a Device object to store the device twin properties
    Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed to " + propertyValue);
      }
    };
    ```

1. Adja hozzá a következő kódot a **fő** metódussal hoz létre egy **connectivityType** jelentett tulajdonságot, és küldje el az IoT hubnak:

    ```java
    try {
      // Open the DeviceClient and start the device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it to your IoT hub.
      dataCollector.setReportedProp(new Property("connectivityType", "cellular"));
      client.sendReportedProperties(dataCollector.getReportedProp());
    }
    catch (Exception e) {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Adja hozzá a következő kódot végéhez a **fő** metódust. Várakozás a **Enter** kulcs lehetővé teszi az IoT Hub device twin műveletek állapotának jelentésére idő:

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. Mentse és zárja be a `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.

1. Hozhat létre a **simulated-device** alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban navigáljon a `simulated-device` mappát, és futtassa a következő parancsot:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Az alkalmazások futtatása

Most már készen áll a konzol alkalmazások futtatásához.

1. A parancsot a parancssorba a `add-tags-query` mappában futtassa a következő parancsot beírva futtassa a **hozzá címkéket-lekérdezés** service-alkalmazást:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![A Java IoT Hub-szolgáltatásalkalmazás címkeértékeket frissítése és eszköz-lekérdezések futtatása](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    Megtekintheti a **üzem** és **régió** az ikereszköz hozzá címkéket. Az első lekérdezés visszaadja az eszközt, de a második nem létezik.

1. A parancsot a parancssorba a `simulated-device` mappában a következő parancs futtatásával adja hozzá a **connectivityType** az ikereszköz jelentett tulajdonság:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Az eszközügyfél hozzáadja a ** connectivityType ** jelentett tulajdonság](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. A parancsot a parancssorba a `add-tags-query` mappában futtassa a következő parancsot beírva futtassa a **hozzá címkéket-lekérdezés** másodszor service-alkalmazást:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![A Java IoT Hub-szolgáltatásalkalmazás címkeértékeket frissítése és eszköz-lekérdezések futtatása](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Most már az eszköz által küldött a **connectivityType** tulajdonságot az IoT hubhoz, a második lekérdezés visszaadja az eszközt.

## <a name="next-steps"></a>További lépések

Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében. Címkeként, egy háttér-alkalmazásból hozzáadott eszközök metaadatait, és a egy eszközalkalmazás zapsáno do kapcsolat eszközadatokat a jelentés azokat az ikereszköz. Azt is megtanulta, hogyan kérdezhet le a az SQL-szerű IoT Hub lekérdezési nyelv használatával ikereszköz-információkat.

Az alábbi forrásanyagokból megtudhatja, hogyan lehet:

* Telemetriát az eszközökről a [IoT Hub használatának első lépései](quickstart-send-telemetry-java.md) oktatóanyag.
* Az eszközök, interaktív módon (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) szabályozhatja a [közvetlen metódusok használata](quickstart-control-device-java.md) oktatóanyag.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
