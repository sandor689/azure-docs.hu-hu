---
title: Az Azure IoT Hub (Java) feladatok ütemezéséhez |} A Microsoft Docs
description: Hogyan közvetlen metódus meghívása és a egy kívánt tulajdonságot állítsa a több eszközre, az Azure IoT Hub feladat ütemezése. Az Azure IoT eszközoldali SDK for Java használatával a szimulált eszközalkalmazások, valamint az Azure IoT service megvalósítása a service-alkalmazás a feladat futtatása Java SDK valósítják meg.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 07/10/2017
ms.author: dobett
ms.openlocfilehash: 3161715ac2ff212e2de8a27ff8f8eb53fb858b92
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186805"
---
# <a name="schedule-and-broadcast-jobs-java"></a>Feladatok ütemezése és kiküldése (Java)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub használatával ütemezheti és nyomon követheti a feladatok, amely több millió eszköz frissítése. Feladatok használata:

* Eszköz kívánt tulajdonságainak frissítése
* A címkék frissítése
* Közvetlen metódusok meghívása

Egy feladat burkolja az alábbi műveletek egyikét, és nyomon követi az eszközök a futtatást. Egy eszköz ikereszköz-lekérdezés határozza meg, a feladat végrehajtása elleni eszközök. Például egy háttér-alkalmazás használatával egy feladat közvetlen metódus meghívása az 10 000 eszköz, amely az eszköz újraindul. Adjon meg az eszközök device twin lekérdezéssel, és a feladat futtatásához egy későbbi időpontban ütemezése. Mivel minden egyes, az eszköz előrehaladását nyomon követi kap, és hajtsa végre az újraindítás közvetlen metódus.

Az egyes képességek kapcsolatos további információkért lásd:

* Ikereszköz és tulajdonságok: [ikereszközök – első lépések](iot-hub-java-java-twin-getstarted.md)
* Közvetlen metódusok: [az IoT Hub fejlesztői útmutató – közvetlen metódusok](iot-hub-devguide-direct-methods.md) és [oktatóanyag: közvetlen metódusok használata](quickstart-control-device-java.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Ez az oktatóanyag a következőket mutatja be:

* Egy eszközalkalmazás létrehozása, amely megvalósítja a meghívott közvetlen metódusra **lockDoor**. Az eszköz alkalmazás kívánt tulajdonságmódosítás is fogad a háttéralkalmazás.
* Hozzon létre egy háttér-alkalmazást, amely létrehoz egy feladatot, amely hívja az **lockDoor** közvetlen metódus több eszközön. Egy másik feladat több eszközre elküldi a kívánt tulajdonság frissítéseket.

Ez az oktatóanyag végén rendelkezésére áll majd egy java konzolalkalmazást eszköz és a egy java konzolalkalmazást háttér:

**a szimulált eszköz** , amely csatlakozik az IoT hubhoz, valósítja meg a **lockDoor** közvetlen metódust, és kezelni kívánt tulajdonságmódosítás.

**feladatok ütemezése** feladatok segítségével, amely hívja az **lockDoor** közvetlen metódust, és több eszközön az ikereszköz kívánt tulajdonságait frissíti.

> [!NOTE]
> A cikk [Azure IoT SDK-k](iot-hub-devguide-sdks.md) használható eszköz és a háttér-alkalmazásokat hozhat létre az Azure IoT SDK-kkal kapcsolatos információkat biztosít.

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:

* A legújabb [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) mindössze néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Is használhatja a [IoT-bővítmény az Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) eszközt az IoT hub-eszköz hozzáadásához.

## <a name="create-the-service-app"></a>Az alkalmazás létrehozása

Ebben a szakaszban egy Java-konzolalkalmazást, amely a feladatok hoz létre:

* Hívja a **lockDoor** közvetlen metódus több eszközön.
* Kívánt tulajdonságok küldése több eszközre.

Az alkalmazás létrehozása:

1. A fejlesztői gépen, hozzon létre egy nevű üres mappát `iot-java-schedule-jobs`.

1. Az a `iot-java-schedule-jobs` mappában hozzon létre egy nevű Maven-projektet **feladatok ütemezése** használja az alábbi parancsot a parancssorba. Látható, hogy ez egyetlen hosszú parancs:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. A parancssorban navigáljon a `schedule-jobs` mappát.

1. Egy szövegszerkesztővel nyissa meg a `pom.xml` fájlt a `schedule-jobs` mappát, és adja hozzá a következő függőséget a **függőségek** csomópont. Ezzel a függőséggel használhatja az **iot-service-client** csomagot az alkalmazásban, az IoT hubbal való kommunikációhoz:

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

1. Egy szövegszerkesztővel nyissa meg a `schedule-jobs\src\main\java\com\mycompany\app\App.java` fájlt.

1. Adja hozzá a következő **importálási** utasításokat a fájlhoz:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Pair;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Query;
    import com.microsoft.azure.sdk.iot.service.devicetwin.SqlQuery;
    import com.microsoft.azure.sdk.iot.service.jobs.JobClient;
    import com.microsoft.azure.sdk.iot.service.jobs.JobResult;
    import com.microsoft.azure.sdk.iot.service.jobs.JobStatus;

    import java.util.Date;
    import java.time.Instant;
    import java.util.HashSet;
    import java.util.Set;
    import java.util.UUID;
    ```

1. Adja hozzá a következő osztályszintű változókat az **App** osztályhoz. Cserélje le `{youriothubconnectionstring}` feljegyzett IoT hub kapcsolati karakterláncra az *hozzon létre egy IoT hubot* szakaszban:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long the job is permitted to run without
    // completing its work on the set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. Adja hozzá a következő metódust a **alkalmazás** osztály ütemezni a feladatot, amely frissíti a **épület** és **emelet** kívánt tulajdonságot az ikereszköz:

    ```java
    private static JobResult scheduleJobSetDesiredProperties(JobClient jobClient, String jobId) {
      DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
      Set<Pair> desiredProperties = new HashSet<Pair>();
      desiredProperties.add(new Pair("Building", 43));
      desiredProperties.add(new Pair("Floor", 3));
      twin.setDesiredProperties(desiredProperties);
      // Optimistic concurrency control
      twin.setETag("*");

      // Schedule the update twin job to run now
      // against a single device
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleUpdateTwin(jobId, 
          "deviceId='" + deviceId + "'",
          twin,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling desired properties job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    }
    ```

1. Hívja a feladat ütemezése a **lockDoor** metódust, adja hozzá a következő metódust a **alkalmazás** osztály:

    ```java
    private static JobResult scheduleJobCallDirectMethod(JobClient jobClient, String jobId) {
      // Schedule a job now to call the lockDoor direct method
      // against a single device. Response and connection
      // timeouts are set to 5 seconds.
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleDeviceMethod(jobId,
          "deviceId='" + deviceId + "'",
          "lockDoor",
          5L, 5L, null,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling direct method job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    };
    ```

1. A feladat figyeléséhez adja hozzá a következő metódust a **alkalmazás** osztály:

    ```java
    private static void monitorJob(JobClient jobClient, String jobId) {
      try {
        JobResult jobResult = jobClient.getJob(jobId);
        if(jobResult == null)
        {
          System.out.println("No JobResult for: " + jobId);
          return;
        }
        // Check the job result until it's completed
        while(jobResult.getJobStatus() != JobStatus.completed)
        {
          Thread.sleep(100);
          jobResult = jobClient.getJob(jobId);
          System.out.println("Status " + jobResult.getJobStatus() + " for job " + jobId);
        }
        System.out.println("Final status " + jobResult.getJobStatus() + " for job " + jobId);
      } catch (Exception e) {
        System.out.println("Exception monitoring job: " + jobId);
        System.out.println(e.getMessage());
        return;
      }
    }
    ```

1. Lekérdezés a futtatott feladatok részleteit, adja hozzá a következő metódust:

    ```java
    private static void queryDeviceJobs(JobClient jobClient, String start) throws Exception {
      System.out.println("\nQuery device jobs since " + start);

      // Create a jobs query using the time the jobs started
      Query deviceJobQuery = jobClient
          .queryDeviceJob(SqlQuery.createSqlQuery("*", SqlQuery.FromType.JOBS, "devices.jobs.startTimeUtc > '" + start + "'", null).getQuery());

      // Iterate over the list of jobs and print the details
      while (jobClient.hasNextJob(deviceJobQuery)) {
        System.out.println(jobClient.getNextJob(deviceJobQuery));
      }
    }
    ```

1. Frissítés a **fő** , többek között az alábbiak podpis metody `throws` záradékot:

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. Futtatás és egymás után két feladatok monitorozásához adja hozzá a következő kódot a **fő** módszer:

    ```java
    // Record the start time
    String start = Instant.now().toString();

    // Create JobClient
    JobClient jobClient = JobClient.createFromConnectionString(iotHubConnectionString);
    System.out.println("JobClient created with success");

    // Schedule twin job desired properties
    // Maximum concurrent jobs is 1 for Free and S1 tiers
    String desiredPropertiesJobId = "DPCMD" + UUID.randomUUID();
    scheduleJobSetDesiredProperties(jobClient, desiredPropertiesJobId);
    monitorJob(jobClient, desiredPropertiesJobId);

    // Schedule twin job direct method
    String directMethodJobId = "DMCMD" + UUID.randomUUID();
    scheduleJobCallDirectMethod(jobClient, directMethodJobId);
    monitorJob(jobClient, directMethodJobId);

    // Run a query to show the job detail
    queryDeviceJobs(jobClient, start);

    System.out.println("Shutting down schedule-jobs app");
    ```

1. Mentse és zárja be a `schedule-jobs\src\main\java\com\mycompany\app\App.java` fájl

1. Hozhat létre a **feladatok ütemezése** alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban navigáljon a `schedule-jobs` mappát, és futtassa a következő parancsot:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Eszközalkalmazás létrehozása

Ebben a szakaszban egy Java-konzolalkalmazást, amely az IoT hubról küldött kívánt tulajdonságok kezeli, és implementálja a közvetlen metódus meghívása hoz létre.

1. Az a `iot-java-schedule-jobs` mappában hozzon létre egy nevű Maven-projektet **simulated-device** használja az alábbi parancsot a parancssorba. Látható, hogy ez egyetlen hosszú parancs:

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
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Ez a mintaalkalmazás a **protocol** változót használja egy **DeviceClient** objektum példányának létrehozásakor.

1. Nyomtassa ki a device twin értesítések a konzol, adja hozzá a következő beágyazott osztály a **alkalmazás** osztály:

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    ```

1. A konzol közvetlen metódus értesítések nyomtatáshoz, adja hozzá a következő beágyazott osztály a **alkalmazás** osztály:

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to direct method operation with status " + status.name());
      }
    }
    ```

1. Közvetlen metódust hívja az IoT Hub kezelése érdekében adja hozzá a következő beágyazott osztály a **alkalmazás** osztály:

    ```java
    // Handler for direct method calls from IoT Hub
    protected static class DirectMethodCallback
        implements DeviceMethodCallback {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context) {
        DeviceMethodData deviceMethodData;
        switch (methodName) {
          case "lockDoor": {
            System.out.println("Executing direct method: " + methodName);
            deviceMethodData = new DeviceMethodData(METHOD_SUCCESS, "Executed direct method " + methodName);
            break;
          }
          default: {
            deviceMethodData = new DeviceMethodData(METHOD_NOT_DEFINED, "Not defined direct method " + methodName);
          }
        }
        // Notify IoT Hub of result
        return deviceMethodData;
      }
    }
    ```

1. Frissítés a **fő** , többek között az alábbiak podpis metody `throws` záradékot:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. Adja hozzá a következő kódot a **fő** metódust:
    * Hozzon létre egy ügyfél az IoT hubbal való kommunikációhoz.
    * Hozzon létre egy **eszköz** objektum az eszköz-ikertulajdonságok tárolásához.

    ```java
    // Create a device client
    DeviceClient client = new DeviceClient(connString, protocol);

    // An object to manage device twin desired and reported properties
    Device dataCollector = new Device() {
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context)
      {
        System.out.println("Received desired property change: " + propertyKey + " " + propertyValue);
      }
    };
    ```

1. Az eszköz ügyfélszolgáltatások, adja hozzá a következő kódot a **fő** módszer:

    ```java
    try {
      // Open the DeviceClient
      // Start the device twin services
      // Subscribe to direct method calls
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
    } catch (Exception e) {
      System.out.println("Exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

1. Várjon, amíg a felhasználó lenyomja az a **Enter** leállítása előtt kulcsban, adja hozzá a következő kódot végéhez a **fő** módszer:

    ```java
    // Close the app
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. Mentse és zárja be a `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.

1. Hozhat létre a **simulated-device** alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban navigáljon a `simulated-device` mappát, és futtassa a következő parancsot:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Az alkalmazások futtatása

Most már készen áll a konzol alkalmazások futtatásához.

1. A parancsot a parancssorba a `simulated-device` mappában futtassa a következő parancsot a kívánt tulajdonságmódosítás és közvetlen metódust hívja figyeli az eszköz-alkalmazás elindításához:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Az eszközügyfél elindul](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. A parancsot a parancssorba a `schedule-jobs` mappában futtassa a következő parancsot beírva futtassa a **feladatok ütemezése** service-alkalmazás két feladatok futtatásához. Az első adja meg a kívánt tulajdonságot az értékeket, a második meghívja a közvetlen metódust:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT Hub-szolgáltatásalkalmazás két feladatot hoz létre.](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. Az eszköz alkalmazás kezeli a kívánt tulajdonság módosítása és a közvetlen metódus meghívása:

    ![Az eszközügyfél válaszol a módosításokat](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a>További lépések

Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében. Létrehozott két feladatok futtatásához egy háttér-alkalmazást. Az első feladat beállítani kívánt tulajdonságértékeket, és a második feladat közvetlen metódus neve.

Az alábbi forrásanyagokból megtudhatja, hogyan lehet:

* Telemetriát az eszközökről a [IoT Hub használatának első lépései](quickstart-send-telemetry-java.md) oktatóanyag.
* Az eszközök, interaktív módon (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) szabályozhatja a [közvetlen metódusok használata](quickstart-control-device-java.md) oktatóanyag.
