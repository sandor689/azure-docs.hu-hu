---
title: Node.js - Azure-ben a távoli figyelési Raspberry Pi kiépítése |} A Microsoft Docs
description: Ismerteti, hogyan lehet egy Raspberry Pi-eszköz csatlakoztatása a távoli figyelési megoldásgyorsító Node.js nyelven írt alkalmazás használatával.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 01/24/2018
ms.author: dobett
ms.openlocfilehash: 696bd6ec80f39e8a9f3418426a754ffc038171e2
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325082"
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-solution-accelerator-nodejs"></a>A Raspberry Pi-eszköz csatlakoztatása a távoli figyelési megoldásgyorsító (Node.js)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Ez az oktatóanyag bemutatja, hogyan kell egy fizikai eszköz csatlakoztatása a távoli figyelési megoldásgyorsító. Ebben az oktatóanyagban, az Node.js, amely olyan környezetekben, ahol minimális erőforrás-korlátozások jó választás.

### <a name="required-hardware"></a>Szükséges hardver

Asztali számítógép ahhoz, hogy távolról csatlakozhat a parancssorban a Raspberry Pi-on.

[A Microsoft IoT-Kezdőcsomag a Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) vagy ezzel egyenértékű összetevőket. Ebben az oktatóanyagban a csomag a következő cikkeket:

- Raspberry pi 3 –
- (A NOOBS) MicroSD-kártyán
- Egy USB Mini-kábellel
- Egy Ethernet-kábelek

### <a name="required-desktop-software"></a>Szükséges asztali szoftverek

SSH-ügyfelet kell ahhoz, hogy a parancssor a Raspberry Pi-on érheti el távolról asztali gépén.

- Windows nem tartalmazza az SSH-ügyfelet. Azt javasoljuk, [PuTTY](http://www.putty.org/).
- A legtöbb Linux-disztribúciók és Mac OS tartalmazza az SSH parancssori segédprogramot. További információkért lásd: [SSH használatával Linux vagy Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Raspberry Pi szükséges szoftverek

Ha még nem tette meg, telepítse a Node.js 4.0.0-s verzió vagy annál újabb verzió a Raspberry Pi. A következő lépések bemutatják, hogyan Node.js 6-os verziójának telepítése a Raspberry Pi-on:

1. Csatlakozzon a Raspberry Pi `ssh`. További információkért lásd: [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) a a [Raspberry Pi webhely](https://www.raspberrypi.org/).

1. A következő paranccsal a Raspberry Pi frissítése:

    ```sh
    sudo apt-get update
    ```

1. A következő parancsokat használja minden olyan meglévő Node.js telepített és eltávolítása a Raspberry Pi:

    ```sh
    sudo apt-get remove nodered -y
    sudo apt-get remove nodejs nodejs-legacy -y
    sudo apt-get remove npm  -y
    ```

1. A következő paranccsal töltse le és telepítse a Node.js 6-os verziója a Raspberry Pi-on:

    ```sh
    curl -sL https://deb.nodesource.com/setup_6.x | sudo bash -
    sudo apt-get install nodejs npm
    ```

1. A következő paranccsal ellenőrizze, hogy telepítette a Node.js v6.11.4 sikeresen:

    ```sh
    node --version
    ```

## <a name="create-a-nodejs-solution"></a>Hozzon létre egy Node.js-megoldás

Hajtsa végre a következő lépések segítségével a `ssh` a Raspberry Pi kapcsolatot:

1. Hozzon létre egy nevű `remotemonitoring` a kezdőmappát a Raspberry Pi-on. Keresse meg a mappát a parancssorban:

    ```sh
    cd ~
    mkdir remotemonitoring
    cd remotemonitoring
    ```

1. Töltse le és telepítse át kell adnia a mintaalkalmazás a csomagokat, futtassa a következő parancsokat:

    ```sh
    npm install async azure-iot-device azure-iot-device-mqtt
    ```

1. Az a `remotemonitoring` mappában hozzon létre egy fájlt nevű **remote_monitoring.js**. Nyissa meg ezt a fájlt egy szövegszerkesztőben. A Raspberry Pi-on is használhat a `nano` vagy `vi` szövegszerkesztő.

1. Az a **remote_monitoring.js** fájlt, adja hozzá a következő `require` utasításokat:

    ```nodejs
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    var async = require('async');
    ```

1. Adja hozzá a következő változódeklarációkat az `require` utasítások után. A helyőrző értékét cserélje le `{device connection string}` a távoli figyelési megoldás kiépítése az eszköz feljegyzett értékkel:

    ```nodejs
    var connectionString = '{device connection string}';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Néhány alapvető telemetriai adatok megadásához adja hozzá a következő változókat:

    ```nodejs
    var temperature = 50;
    var temperatureUnit = 'F';
    var humidity = 50;
    var humidityUnit = '%';
    var pressure = 55;
    var pressureUnit = 'psig';
    ```

1. Néhány tulajdonság értékek megadásához adja hozzá az alábbi változókat:

    ```nodejs
    var temperatureSchema = 'chiller-temperature;v1';
    var humiditySchema = 'chiller-humidity;v1';
    var pressureSchema = 'chiller-pressure;v1';
    var interval = "00:00:05";
    var deviceType = "Chiller";
    var deviceFirmware = "1.0.0";
    var deviceFirmwareUpdateStatus = "";
    var deviceLocation = "Building 44";
    var deviceLatitude = 47.638928;
    var deviceLongitude = -122.13476;
    var deviceOnline = true;
    ```

1. Adja hozzá a következő változót a jelentett tulajdonságokat küldhet a megoldás meghatározásához. E tulajdonságok közé tartozik a módszerek leíró metaadatok és az eszköz telemetriai használja:

    ```nodejs
    var reportedProperties = {
      "Protocol": "MQTT",
      "SupportedMethods": "Reboot,FirmwareUpdate,EmergencyValveRelease,IncreasePressure",
      "Telemetry": {
        "TemperatureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
          "MessageSchema": {
            "Name": temperatureSchema,
            "Format": "JSON",
            "Fields": {
              "temperature": "Double",
              "temperature_unit": "Text"
            }
          }
        },
        "HumiditySchema": {
          "Interval": interval,
          "MessageTemplate": "{\"humidity\":${humidity},\"humidity_unit\":\"${humidity_unit}\"}",
          "MessageSchema": {
            "Name": humiditySchema,
            "Format": "JSON",
            "Fields": {
              "humidity": "Double",
              "humidity_unit": "Text"
            }
          }
        },
        "PressureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"pressure\":${pressure},\"pressure_unit\":\"${pressure_unit}\"}",
          "MessageSchema": {
            "Name": pressureSchema,
            "Format": "JSON",
            "Fields": {
              "pressure": "Double",
              "pressure_unit": "Text"
            }
          }
        }
      },
      "Type": deviceType,
      "Firmware": deviceFirmware,
      "FirmwareUpdateStatus": deviceFirmwareUpdateStatus,
      "Location": deviceLocation,
      "Latitude": deviceLatitude,
      "Longitude": deviceLongitude,
      "Online": deviceOnline
    }
    ```

1. Műveleti eredmények nyomtatáshoz, adja hozzá a következő segédfüggvény:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Adja hozzá a következő segédfüggvény véletlenszerűvé tétele a telemetriai adatok értékek használatával:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Adja hozzá a következő általános függvényt kezelni a megoldás a közvetlen metódust hívja. A funkciót a közvetlen metódus meghívása, de ebben a példában nem módosítja az eszköz bármilyen módon információit jeleníti meg. A megoldás használja a közvetlen metódusok való működésre eszközökön:

    ```nodejs
    function onDirectMethod(request, response) {
      // Implement logic asynchronously here.
      console.log('Simulated ' + request.methodName);

      // Complete the response
      response.send(200, request.methodName + ' was called on the device', function (err) {
        if (err) console.error('Error sending method response :\n' + err.toString());
        else console.log('200 Response to method \'' + request.methodName + '\' sent successfully.');
      });
    }
    ```

1. Adja hozzá a következő függvényt kezelni a **FirmwareUpdate** közvetlen metódust hívja a megoldásról. A függvény ellenőrzi a közvetlen metódus hasznos adatainak átadott paraméterek, és aszinkron módon fut egy belső vezérlőprogram frissítési szimuláció:

    ```node.js
    function onFirmwareUpdate(request, response) {
      // Get the requested firmware version from the JSON request body
      var firmwareVersion = request.payload.Firmware;
      var firmwareUri = request.payload.FirmwareUri;
      
      // Ensure we got a firmware values
      if (!firmwareVersion || !firmwareUri) {
        response.send(400, 'Missing firmware value', function(err) {
          if (err) console.error('Error sending method response :\n' + err.toString());
          else console.log('400 Response to method \'' + request.methodName + '\' sent successfully.');
        });
      } else {
        // Respond the cloud app for the device method
        response.send(200, 'Firmware update started.', function(err) {
          if (err) console.error('Error sending method response :\n' + err.toString());
          else {
            console.log('200 Response to method \'' + request.methodName + '\' sent successfully.');

            // Run the simulated firmware update flow
            runFirmwareUpdateFlow(firmwareVersion, firmwareUri);
          }
        });
      }
    }
    ```

1. Adja hozzá a szimulálása a hosszan futó belső vezérlőprogram frissítési folyamat, amely a folyamat jelentést küld vissza a megoldás a következő függvényt:

    ```node.js
    // Simulated firmwareUpdate flow
    function runFirmwareUpdateFlow(firmwareVersion, firmwareUri) {
      console.log('Simulating firmware update flow...');
      console.log('> Firmware version passed: ' + firmwareVersion);
      console.log('> Firmware URI passed: ' + firmwareUri);
      async.waterfall([
        function (callback) {
          console.log("Image downloading from " + firmwareUri);
          var patch = {
            FirmwareUpdateStatus: 'Downloading image..'
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(10000, callback);
        },
        function (callback) {
          console.log("Downloaded, applying firmware " + firmwareVersion);
          deviceOnline = false;
          var patch = {
            FirmwareUpdateStatus: 'Applying firmware..',
            Online: false
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(8000, callback);
        },
        function (callback) {
          console.log("Rebooting");
          var patch = {
            FirmwareUpdateStatus: 'Rebooting..'
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(10000, callback);
        },
        function (callback) {
          console.log("Firmware updated to " + firmwareVersion);
          deviceOnline = true;
          var patch = {
            FirmwareUpdateStatus: 'Firmware updated',
            Online: true,
            Firmware: firmwareVersion
          };
          reportUpdateThroughTwin(patch, callback);
          callback(null);
        }
      ], function(err) {
        if (err) {
          console.error('Error in simulated firmware update flow: ' + err.message);
        } else {
          console.log("Completed simulated firmware update flow");
        }
      });

      // Helper function to update the twin reported properties.
      function reportUpdateThroughTwin(patch, callback) {
        console.log("Sending...");
        console.log(JSON.stringify(patch, null, 2));
        client.getTwin(function(err, twin) {
          if (!err) {
            twin.properties.reported.update(patch, function(err) {
              if (err) callback(err);
            });      
          } else {
            if (err) callback(err);
          }
        });
      }

      function sleep(milliseconds, callback) {
        console.log("Simulate a delay (milleseconds): " + milliseconds);
        setTimeout(function () {
          callback(null);
        }, milliseconds);
      }
    }
    ```

1. Adja hozzá a következő kódot a telemetriai adatokat küldeni a megoldást. Az ügyfélalkalmazás az üzenet azonosítására az üzenet-sémát ad hozzá a tulajdonságai:

    ```node.js
    function sendTelemetry(data, schema) {
      if (deviceOnline) {
        var d = new Date();
        var payload = JSON.stringify(data);
        var message = new Message(payload);
        message.properties.add('$$CreationTimeUtc', d.toISOString());
        message.properties.add('$$MessageSchema', schema);
        message.properties.add('$$ContentType', 'JSON');

        console.log('Sending device message data:\n' + payload);
        client.sendEvent(message, printErrorFor('send event'));
      } else {
        console.log('Offline, not sending telemetry');
      }
    }
    ```

1. Adja hozzá az ügyfél-példány létrehozása a következő kódot:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Adja hozzá a következő kódot:

    * Nyissa meg a kapcsolat.
    * Állítsa be a kívánt tulajdonságok kezelő.
    * Jelentett tulajdonságokat küldhet.
    * Regisztrálja a kezelők számára a közvetlen metódusok. A példa egy külön kezelő belső vezérlőprogram frissítési közvetlen metódus.
    * Indítsa el a telemetriai adatokat küldenek.

    ```nodejs
    client.open(function (err) {
      if (err) {
        printErrorFor('open')(err);
      } else {
        // Create device Twin
        client.getTwin(function (err, twin) {
          if (err) {
            console.error('Could not get device twin');
          } else {
            console.log('Device twin created');

            twin.on('properties.desired', function (delta) {
              // Handle desired properties set by solution
              console.log('Received new desired properties:');
              console.log(JSON.stringify(delta));
            });

            // Send reported properties
            twin.properties.reported.update(reportedProperties, function (err) {
              if (err) throw err;
              console.log('Twin state reported');
            });

            // Register handlers for all the method names we are interested in.
            // Consider separate handlers for each method.
            client.onDeviceMethod('Reboot', onDirectMethod);
            client.onDeviceMethod('FirmwareUpdate', onFirmwareUpdate);
            client.onDeviceMethod('EmergencyValveRelease', onDirectMethod);
            client.onDeviceMethod('IncreasePressure', onDirectMethod);
          }
        });

        // Start sending telemetry
        var sendTemperatureInterval = setInterval(function () {
          temperature += generateRandomIncrement();
          var data = {
            'temperature': temperature,
            'temperature_unit': temperatureUnit
          };
          sendTelemetry(data, temperatureSchema)
        }, 5000);

        var sendHumidityInterval = setInterval(function () {
          humidity += generateRandomIncrement();
          var data = {
            'humidity': humidity,
            'humidity_unit': humidityUnit
          };
          sendTelemetry(data, humiditySchema)
        }, 5000);

        var sendPressureInterval = setInterval(function () {
          pressure += generateRandomIncrement();
          var data = {
            'pressure': pressure,
            'pressure_unit': pressureUnit
          };
          sendTelemetry(data, pressureSchema)
        }, 5000);

        client.on('error', function (err) {
          printErrorFor('client')(err);
          if (sendTemperatureInterval) clearInterval(sendTemperatureInterval);
          if (sendHumidityInterval) clearInterval(sendHumidityInterval);
          if (sendPressureInterval) clearInterval(sendPressureInterval);
          client.close(printErrorFor('client.close'));
        });
      }
    });
    ```

1. A módosítások mentése a **remote_monitoring.js** fájlt.

1. A mintaalkalmazás indításához futtassa a következő parancsot a parancssorban a Raspberry Pi-on:

    ```sh
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
