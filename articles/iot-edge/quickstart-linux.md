---
title: Az Azure IoT Edge + Linux rövid útmutatója | Microsoft Docs
description: Ebben a rövid útmutatóban megismerheti, hogyan helyezhet üzembe távolról előre összeállított kódokat egy IoT Edge-eszközön.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: dfcb764d75b7328d1234d47d82afdae8d6a0deef
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413014"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Rövid útmutató: Az első IoT Edge-modul üzembe helyezése x64-es Linux-eszközön

Az Azure IoT Edge kiterjeszti a felhő képességeit IoT-eszközeire. Ebben a rövid útmutatóban megismerheti, hogyan helyezhet üzembe előre összeállított kódot távolról egy IoT Edge-eszközön a felhőalapú felület használatával.

Ennek a rövid útmutatónak a segítségével megtanulhatja az alábbiakat:

1. IoT Hub létrehozása
2. IoT Edge-eszköz regisztrálása az IoT Hubon
3. Az IoT Edge-futtatókörnyezet telepítése és elindítása az eszközén.
4. Modul távoli üzembe helyezése IoT Edge-eszközön.

![A rövid útmutató architektúrája][2]

Ebben a rövid útmutatóban Linux rendszerű számítógépét vagy virtuális gépét IoT Edge-eszközzé alakíthatja. Ezután egy modult helyezhet üzembe az eszközén az Azure Portalról. A jelen rövid útmutatóban üzembe helyezett modul egy szimulált érzékelő, amely hőmérséklet-, páratartalom- és nyomásadatokat állít elő. A további Azure IoT Edge-oktatóanyagok az itt elvégzett munkára építkeznek olyan modulok üzembe helyezésével, amelyek a szimulált adatok elemzésével üzleti megállapításokat hoznak létre. 

Ha nem rendelkezik aktív Azure-előfizetéssel, kezdetnek hozzon létre egy [ingyenes fiókot][lnk-account].


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

A rövid útmutató számos lépésében az Azure CLI használatára van szükség, és az Azure IoT egyik bővítményével további funkciókhoz férhet hozzá. 

Adja hozzá az Azure IoT bővítményt a Cloud Shell-példányhoz.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```
   
## <a name="prerequisites"></a>Előfeltételek

Felhőerőforrások: 

* Egy erőforráscsoport a rövid útmutató során létrehozott összes erőforrás kezelésére. 

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

* Egy Linux rendszerű virtuális gép, amely IoT Edge-eszközként szolgál majd. 

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username azureuser --generate-ssh-keys --size Standard_B1ms
   ```

## <a name="create-an-iot-hub"></a>IoT Hub létrehozása

A rövid útmutató első lépéseként hozza létre az IoT Hubot az Azure CLI használatával. 

![IoT Hub létrehozása][3]

Ehhez a rövid útmutatóhoz az IoT Hub ingyenes csomagja is elegendő. Ha korábban már használta az IoT Hubot, és már létrehozott egy ingyenes központot, használhatja azt is. Mindegyik előfizetés csak egy ingyenes IoT-központtal rendelkezhet. 

A következő kód egy ingyenes **F1** központot hoz létre az **IoTEdgeResources** erőforráscsoportban. A *{hub_name}* elemet cserélje le az IoT Hub központ egyedi nevére.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

   Ha hibaüzenetet kap, mert az előfizetése már tartalmaz egy ingyenes központot, akkor módosítsa az SKU-t **S1**-re. 

## <a name="register-an-iot-edge-device"></a>IoT Edge-eszköz regisztrálása

Regisztráljon egy IoT Edge-eszközt az újonnan létrehozott IoT Hubon.
![Eszköz regisztrálása][4]

Hozzon létre egy eszközidentitást a szimulált eszközhöz, hogy az kommunikálhasson az IoT Hubbal. Mivel az IoT Edge-eszközök másként viselkednek, mint a hagyományos IoT-eszközök, és kezelésük is másként történik, ezért IoT Edge-eszközként kell deklarálni a kezdetektől fogva. 

1. Az Azure Cloud Shellben a következő paranccsal hozza létre a **myEdgeDevice** nevű eszközt a központjában.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
   ```

1. Kérje le az eszköze kapcsolati sztringjét, amely összeköti a fizikai eszközt az IoT Hubban tárolt identitással. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Másolja és mentse a kapcsolati sztringet. Erre az értékre a következő szakaszban, az IoT Edge-futtatókörnyezet konfigurálásához lesz szükség. 


## <a name="install-and-start-the-iot-edge-runtime"></a>Az IoT Edge-futtatókörnyezet telepítése és elindítása

Telepítse és indítsa el eszközén az Azure IoT Edge-futtatókörnyezetet. 
![Eszköz regisztrálása][5]

Az IoT Edge-futtatókörnyezet minden IoT Edge-eszközön üzembe van helyezve. Három összetevőből áll. Az **IoT Edge biztonsági démon** az Edge-eszközök indulásakor lép működésbe, és az IoT Edge-ügynök elindításával elvégzi az eszköz rendszerindítását. Az **IoT Edge-ügynök** a modulok üzembe helyezését és monitorozását segíti az IoT Edge-eszközön, beleértve az IoT Edge-központot is. Az **IoT Edge-központ** az IoT Edge-eszközön lévő modulok, valamint az eszköz és az IoT Hub közötti kommunikációt kezeli. 

Hajtsa végre a következő lépéseket a rövid útmutatóhoz előkészített linuxos gépen vagy virtuális gépen. 

### <a name="register-your-device-to-use-the-software-repository"></a>Eszköz regisztrálása a szoftveradattár használatához

Az IoT Edge-futtatókörnyezet futtatásához szükséges csomagok kezelése egy szoftveradattárban történik. Az IoT Edge-eszközt az adattárhoz való hozzáférésre konfigurálhatja. 

A jelen szakasz lépései az **Ubuntu 16.04** rendszerű eszközökre vonatkoznak. Ha másik Linux-verzióról szeretné elérni a szoftveradattárat, tekintse meg az [Azure IoT Edge-futtatókörnyezet telepítése Linuxon (x64)](how-to-install-iot-edge-linux.md) vagy az [Azure IoT Edge-futtatókörnyezet telepítése Linuxon (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) című részt.

1. Telepítse az adattár konfigurációját az IoT Edge-eszközként használt számítógépen.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Telepítsen egy nyilvános kulcsot az adattárhoz való hozzáféréshez.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Tároló-futtatókörnyezet telepítése

Az IoT Edge-futtatókörnyezet egy tárolókészlet, és az IoT Edge-eszközön üzembe helyezett logika tárolókként van csomagolva. Készítse elő eszközét ezekhez az összetevőkhöz egy tároló-futtatókörnyezet telepítésével.

Frissítse az **apt-get** parancsot.

   ```bash
   sudo apt-get update
   ```

Telepítse a **Moby** tároló-futtatókörnyezetet.

   ```bash
   sudo apt-get install moby-engine
   ```

Telepítse a Moby CLI-parancsait. 

   ```bash
   sudo apt-get install moby-cli
   ```

### <a name="install-and-configure-the-iot-edge-security-daemon"></a>Az IoT Edge biztonsági démon telepítése és konfigurálása

A biztonsági démon rendszerszolgáltatásként lesz telepítve, így az IoT Edge-futtatókörnyezet minden alkalommal elindul az eszköz indításakor. A telepítés egy **hsmlib** verziót is magában foglal, amely lehetővé teszi, hogy a biztonsági démon kommunikáljon az eszköz hardveres biztonsági rendszerével. 

1. Töltse le és telepítse az IoT Edge biztonsági démont. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. Nyissa meg az IoT Edge konfigurációs fájlját. Ez egy védett fájl, ezért lehet, hogy megemelt jogosultsági szintre van szükség az eléréséhez.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. Adja hozzá az IoT Edge-eszköz kapcsolati sztringjét. Keresse meg a **device_connection_string** értéket, és frissítse arra a sztringre, amelyet az eszköz regisztrálása után másolt ki.

4. Mentse és zárja be a fájlt. 

   `CTRL + X`, `Y`, `Enter`

4. Indítsa újra az IoT Edge biztonsági démont.

   ```bash
   sudo systemctl restart iotedge
   ```

5. Ellenőrizze, hogy az Edge biztonsági démon rendszerszolgáltatásként fut-e.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Az Edge démon rendszerszolgáltatásként való futásának megtekintése](./media/quickstart-linux/iotedged-running.png)

   Az Edge biztonsági démon naplóit is megtekintheti a következő parancs futtatásával:

   ```bash
   journalctl -u iotedge
   ```

6. Tekintse meg az eszközön futó modulokat. 

   >[!TIP]
   >Ha `iotedge`-parancsokat futtat, a *sudo* kifejezést kell eléjük írnia. Az engedélyek frissítéséhez jelentkezzen ki a gépről, majd jelentkezzen újra be. Ezután már futtathat emelt szintű jogosultságok nélkül is `iotedge`-parancsokat. 

   ```bash
   sudo iotedge list
   ```

   ![Egy modul megtekintése az eszközön](./media/quickstart-linux/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Modul üzembe helyezése

Azure IoT Edge-eszközeit kezelheti a felhőből, és üzembe helyezhet egy olyan modult, amely telemetriaadatokat küld az IoT Hubra.
![Eszköz regisztrálása][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>A létrejött adatok megtekintése

Ebben a rövid útmutatóban létrehozott egy új IoT Edge-eszközt, és telepítette rajta az IoT Edge-futtatókörnyezetet. Ezután az Azure Portal segítségével úgy futtatta az IoT Edge-modult az eszközön, hogy magát az eszközt nem kellett módosítania. Ebben az esetben az Ön által továbbított modul az oktatóanyagokhoz használható környezeti adatokat hoz létre. 

Nyissa meg újra a parancssort a szimulált eszközt futtató számítógépen. Győződjön meg arról, hogy a felhőből üzembe helyezett modul fut az IoT Edge-eszközön:

   ```bash
   sudo iotedge list
   ```
   Kijelentkezést és bejelentkezést követően a *sudo* nem szükséges a fenti parancshoz.

   ![Három modul megtekintése az eszközön](./media/quickstart-linux/iotedge-list-2.png)

Tekintse meg a tempSensor modul által küldött üzeneteket:

   ```bash
   sudo iotedge logs tempSensor -f 
   ```
Kijelentkezést és bejelentkezést követően a *sudo* nem szükséges a fenti parancshoz.

![A modulból származó adatok megtekintése](./media/quickstart-linux/iotedge-logs.png)

Előfordulhat, hogy a hőmérsékletérzékelő modul az Edge Hubhoz való csatlakozásra vár, ha a napló utolsó sora `Using transport Mqtt_Tcp_Only`. Próbálja meg leállítani a modult, és hagyja, hogy az Edge-ügynök újraindítsa. A modult a következő paranccsal állíthatja le: `sudo docker stop tempSensor`.

Az eszköz által küldött telemetriát is megtekintheti az [IoT Hub Explorer eszközzel][lnk-iothub-explorer] vagy a [Visual Studio Code Azure IoT Toolkit bővítményével](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 


## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha tovább szeretne dolgozni az IoT Edge-oktatóanyagokkal, használhatja az ebben a rövid útmutatóban regisztrált és létrehozott eszközt. Ha nem, törölheti a létrehozott Azure-erőforrásokat, és eltávolíthatja az IoT Edge-futtatókörnyezetet az eszközről. 

### <a name="delete-azure-resources"></a>Azure-erőforrások törlése

Ha a virtuális gépet és az IoT Hubot egy új erőforráscsoportban hozta létre, törölheti azt a csoportot és az összes társított erőforrást. Ha van valami abban az erőforráscsoportban, amit meg szeretne tartani, csak azokat a különálló erőforrásokat törölje, amelyektől meg szeretne szabadulni. 

Távolítsa el az **IoTEdgeResources** csoportot. 

   ```azurecli-interactive
   az group delete --name IoTEdgeResources 
   ```

### <a name="remove-the-iot-edge-runtime"></a>Az IoT Edge-futtatókörnyezet eltávolítása

Ha el szeretné távolítani a telepítéseket az eszközéről, azt a következő parancsokkal teheti meg.  

Távolítsa el az IoT Edge-futtatókörnyezetet.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Ha eltávolította az IoT Edge-futtatókörnyezetet, az általa létrehozott tárolók leállnak, de továbbra is ott lesznek az eszközön. Tekintse meg az összes tárolót.

   ```bash
   sudo docker ps -a
   ```

Törölje azokat a tárolókat, amelyeket az IoT Edge-futtatókörnyezet hozott létre az eszközén. Ha más nevet adott neki, módosítsa a tempSensor tároló nevét. 

   ```bash
   sudo docker rm -f tempSensor
   sudo docker rm -f edgeHub
   sudo docker rm -f edgeAgent
   ```

Távolítsa el a tároló-futtatókörnyezetet.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>További lépések

Ez a rövid útmutató minden IoT Edge-oktatóanyag előfeltétele. Továbbléphet bármely másik oktatóanyagra, és megtudhatja, hogyan alakíthatja üzleti megállapításokká ezeket az adatokat a peremhálózaton az Azure IoT Edge segítségével.

> [!div class="nextstepaction"]
> [Érzékelők adatainak szűrése Azure Functions-függvényekkel](tutorial-deploy-function.md)



<!-- Images -->
[0]: ./media/quickstart-linux/cloud-shell.png
[1]: ./media/quickstart-linux/view-module.png
[2]: ./media/quickstart-linux/install-edge-full.png
[3]: ./media/quickstart-linux/create-iot-hub.png
[4]: ./media/quickstart-linux/register-device.png
[5]: ./media/quickstart-linux/start-runtime.png
[6]: ./media/quickstart-linux/deploy-module.png
[7]: ./media/quickstart-linux/iotedged-running.png
[8]: ./media/tutorial-simulate-device-linux/running-modules.png
[9]: ./media/tutorial-simulate-device-linux/sensor-data.png


<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
