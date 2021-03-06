---
title: Egy általános Node.js ügyfél-alkalmazás csatlakoztatása az Azure IoT Central |} A Microsoft Docs
description: Eszköz fejlesztőként egy általános Node.js-eszköz csatlakoztatása az Azure IoT Central alkalmazáshoz.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 55ce85702804d99d806220d7f0a4ea0820975f4f
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/23/2018
ms.locfileid: "39206037"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Az Azure IoT Central-alkalmazást (Node.js) egy általános ügyfél-alkalmazás csatlakoztatása

Ez a cikk azt ismerteti, hogyan eszköz a fejlesztők egy általános Node.js-alkalmazást képviselő egy fizikai eszköz a Microsoft Azure IoT Central alkalmazáshoz csatlakozhat.

## <a name="before-you-begin"></a>Előkészületek

A cikkben leírt lépések elvégzéséhez a következőkre lesz szüksége:

1. Azure IoT Central alkalmazáshoz. További információkért lásd: [az Azure IoT központi alkalmazás létrehozása](howto-create-application.md).
1. A fejlesztői gépen való [Node.js](https://nodejs.org/) 4.0.0-s verzió vagy újabb verziója szükséges. Futtathat `node --version` a parancssorban a verzió ellenőrzéséhez. A Node.js az operációs rendszerek széles körében elérhető.

## <a name="create-a-device-template"></a>Egy eszköz-sablon létrehozása

Az Azure IoT Central-alkalmazás lesz szüksége a következő mérések és eszköztulajdonságok definiált eszköz sablon:

### <a name="telemetry-measurements"></a>Telemetria mérések

Adja hozzá a következő telemetriát a **mérések** oldalon:

| Megjelenített név | Mező neve  | Mértékegységek | Min | Maximum | Tizedeshelyek |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Hőmérséklet  | hőmérséklet | P     | 60  | 110 | 0              |
| Nedvességtartalom     | páratartalom    | %     | 0   | 100 | 0              |
| Pressure     | pressure    | kPa   | 80  | 110 | 0              |

> [!NOTE]
  A telemetriai adatok mérték adattípusának dupla.

Adja meg a mezők neve pontosan, ahogy az a tábla az eszköz sablonba be. Ha a mező nevei nem egyeznek, a telemetria az alkalmazás nem lehet megjeleníteni.

### <a name="state-measurements"></a>Állapot mérések

Adja hozzá a következő állapotot a a **mérések** oldalon:

| Megjelenített név | Mező neve  | 1. érték | Megjelenített név | 2. érték | Megjelenített név |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Ventilátor mód     | fanmode     | 1.       | Futtatás      | 0       | Leállítva      |

> [!NOTE]
  Az állapot mérték adattípusának egy karakterlánc.

Adja meg a mezők neve pontosan, ahogy az a tábla az eszköz sablonba be. Ha a mező nevei nem egyeznek, az állapot nem lehet megjeleníteni az alkalmazásban.

### <a name="event-measurements"></a>Esemény mérések

Adja hozzá a következő esemény a **mérések** oldalon:

| Megjelenített név | Mező neve  | Súlyosság |
| ------------ | ----------- | -------- |
| Életbe  | melegedjen túl    | Hiba    |

> [!NOTE]
  Az esemény mérték adattípusának egy karakterlánc.

### <a name="device-properties"></a>Eszköztulajdonságok

Adja hozzá a következő eszköztulajdonságokat a a **tulajdonságai lap**:

| Megjelenített név        | Mező neve        | Adattípus |
| ------------------- | ----------------- | --------- |
| Sorozatszám       | serialNumber      | szöveg      |
| Eszköz gyártója | gyártó      | szöveg      |

A mezők nevét adja meg a pontosan az eszköz sablonba be a táblázatban látható módon. Ha a mező nevei nem egyeznek, az alkalmazás nem jeleníthető meg a tulajdonság értéke.

### <a name="settings"></a>Beállítások

Adja hozzá a következő **szám** beállításait a **beállítások lapon**:

| Megjelenített név    | Mező neve     | Mértékegységek | Tizedesjegyek | Minimum | Maximum  | Kezdeti |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Sebesség ventilátor       | fanSpeed       | rpm   | 0        | 0   | 3000 | 0       |
| Megadott hőmérséklet | setTemperature | P     | 0        | 20  | 200  | 80      |

Adja meg a mező neve pontosan a eszköz sablonba a táblázatban látható módon. Ha a mező nevei nem egyeznek, akkor az eszköz a beállítás értéke nem tud fogadni.

## <a name="add-a-real-device"></a>Valós eszköz hozzáadása

Az Azure IoT Central-alkalmazás hozzáadása egy igazi eszközön az eszköz sablonból, létrehozása, és jegyezze fel az eszköz kapcsolati karakterláncát. További információkért lásd: [valós eszköz hozzáadása az Azure IoT Central alkalmazáshoz](tutorial-add-device.md)

### <a name="create-a-nodejs-application"></a>Node.js alkalmazás létrehozása

A következő lépések bemutatják, hogyan hozhat létre, amely megvalósítja az az alkalmazáshoz hozzáadott valós eszköz ügyfélalkalmazás.

1. Hozzon létre egy `connected-air-conditioner-adv` nevű mappát a gépén. Keresse meg a mappát a parancssori környezetben.

1. A Node.js-projektet inicializálása, futtassa a következő parancsokat:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Hozzon létre egy fájlt nevű **connectedAirConditionerAdv.js** a a `connected-air-conditioner-adv` mappát.

1. Adja hozzá a következő `require` elején található utasításokat a **connectedAirConditionerAdv.js** fájlt:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    ```

1. Adja a következő változódeklarációkat a fájlhoz:

    ```javascript
    var connectionString = '{your device connection string}';
    var targetTemperature = 0;
    var client = clientFromConnectionString(connectionString);
    ```

    Frissítse a helyőrző `{your device connection string}` az eszköz kapcsolati karakterlánccal. Ez az érték a kapcsolat részleteit megjelenítő oldalon másolja, a valós eszköz hozzáadásakor. Ebben a példában azt inicializálása `targetTemperature` nulla, igény szerint elvégezhető az eszköz aktuális olvasásakor vagy értéket az ikereszközről. 

1. A telemetriai adatokat, állapota és esemény mérések küldése az Azure IoT Central alkalmazáshoz, adja hozzá a fájlhoz a következő függvényt:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var humidity = 70 + (Math.random() * 10);
      var pressure = 90 + (Math.random() * 5);
      var fanmode = 0;
      var data = JSON.stringify({ 
        temperature: temperature, 
        humidity: humidity, 
        pressure: pressure,
        fanmode: (temperature > 25) ? "1" : "0",
        overheat: (temperature > 35) ? "ER123" : undefined });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    1. Eszköztulajdonságokat küldik az Azure IoT Central alkalmazáshoz, adja hozzá a fájlhoz a következő függvényt:

    ```javascript
    // Send device properties.
    function sendDeviceProperties(twin) {
      var properties = {
        serialNumber: '123-ABC',
        manufacturer: 'Contoso'
      };
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Az eszköz válaszol-beállításainak megadásához adja hozzá a következő-definíciót:

    ```javascript
    // Add any settings your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'fanSpeed': (newValue, callback) => {
          // Simulate it taking 1 second to set the fan speed.
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Az Azure IoT Central alkalmazásnak a frissített beállítások kezelése érdekében adja hozzá a következő fájl:

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

1. Vegye fel a kapcsolatot az Azure IoT Central, és a funkciókat az Ügyfélkód kapcsolni a következőket:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            sendDeviceProperties(twin);
            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    // Start the device (connect it to Azure IoT Central).
    client.open(connectCallback);
    ```

## <a name="run-your-nodejs-application"></a>A Node.js-alkalmazás futtatása

Futtassa a következő parancsot a parancssori környezetben:

```cmd/sh
node connectedAirConditionerAdv.js
```

Az Azure IoT központi alkalmazás kezelőként a valódi eszköz a következőket teheti:

* A telemetria megtekintése a **mérések** oldalon:

    ![Telemetria megtekintése](media/howto-connect-nodejs/viewtelemetry.png)

* Tekintse meg az eszköz tulajdonság értékeket, a rendszer küldi az eszközről a **tulajdonságok** lapot.

    ![Az eszköz tulajdonságai](media/howto-connect-nodejs/viewproperties.png)

* Állítsa be a ventilátor sebesség és a cél hőmérséklet a **beállítások** lapot.

    ![Set Ventilátor sebessége](media/howto-connect-nodejs/setfanspeed.png)

## <a name="next-steps"></a>További lépések

Most, hogy rendelkezik egy általános Node.js-ügyfél csatlakoztatása az Azure IoT Central alkalmazáshoz, Íme a javasolt következő lépések:
* [Készítse elő és a Raspberry Pi csatlakoztatása](howto-connect-raspberry-pi-python.md)
<!-- Next how-tos in the sequence -->
