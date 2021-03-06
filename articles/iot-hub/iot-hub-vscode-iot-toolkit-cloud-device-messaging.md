---
title: Azure IoT Hub eszközök felhőalapú üzenetkezelése az Azure IoT-eszközkészlet bővítmény a Visual Studio Code |} A Microsoft Docs
description: Útmutató az Azure IoT-eszközkészlet bővítmény a Visual Studio Code használatával eszköz a felhőbe irányuló üzenetek figyeléséhez és a felhő küldeni az Azure IoT Hub eszköz üzeneteket.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 7/20/2018
ms.author: junhan
ms.openlocfilehash: 7bcb6eebdb6ceba6b07aeb19c1a74309fd4227d0
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/23/2018
ms.locfileid: "39206105"
---
# <a name="use-azure-iot-toolkit-extension-for-visual-studio-code-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Az eszközön, és az IoT Hub közötti üzenetek küldése és fogadása az Azure IoT-eszközkészlet bővítmény a Visual Studio Code használatával

![Végpontok közötti diagram](media/iot-hub-get-started-e2e-diagram/2.png)

[Az Azure IoT-eszközkészlet](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) egy hasznos, a Visual Studio Code-bővítmény, amely egyszerűbbé teszi az IoT Hub kezelése. Ez a cikk foglalkozik, az eszköz és az IoT hub közötti üzenetek küldése és fogadása az Azure IoT-eszközkészlet bővítmény a Visual Studio Code használatával.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Megismerheti, hogyan használható az Azure IoT-eszközkészlet bővítmény a Visual Studio Code-eszközt a felhőbe irányuló üzenetek figyeléséhez és a felhőből az eszközre irányuló üzenetek küldéséhez. Eszköz – felhő üzeneteket lehet, hogy az eszköz gyűjt, és ezután küld az IoT hub érzékelőktől kapott adatok. Felhőből az eszközre irányuló üzenetek lehet, amely az IoT hub küld az eszközre, amely az eszköz csatlakozik LED kurzorvillogás parancsokat.

## <a name="what-you-will-do"></a>Mit fog

- Azure IoT-eszközkészlet bővítmény a Visual Studio Code használatával eszköz a felhőbe irányuló üzenetek figyeléséhez.
- Azure IoT-eszközkészlet bővítmény a Visual Studio Code használatával üzenetküldés a felhőből az eszközre.

## <a name="what-you-need"></a>Mi szükséges

- Aktív Azure-előfizetés.
- Az Azure IoT hub az előfizetéséhez.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure IoT-eszközkészlet](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="sign-in-to-access-your-iot-hub"></a>Jelentkezzen be az IoT hub eléréséhez

1. A **Explorer** megtekintése a VS Code, bontsa ki a **Azure IoT Hub-eszközök** szakaszban a bal alsó sarokban.
1. Kattintson a **válassza ki az IoT Hub** helyi menüben.
1. Egy előugró ablakban megjelenik a jobb alsó sarokban lehetővé teszi, hogy az Azure-ban először jelentkezzen be.
1. Miután bejelentkezett, az Azure-előfizetési lista jelenik meg, majd válassza ki az Azure-előfizetés és az IoT Hub.
1. Az eszközlistában megjelenő **Azure IoT Hub-eszközök** lapon pár másodpercen belül.

   > [!Note]
   > Is elvégezhető a csoport mentése kiválasztásával **állítsa be IoT Hub kapcsolati karakterláncra**. Az IoT hub, amely az IoT-eszköz csatlakozik az előugró ablakban adja meg a kapcsolati karakterláncot.
   
## <a name="monitor-device-to-cloud-messages"></a>Eszköz a felhőbe irányuló üzenetek figyeléséhez

Az eszközről az IoT hubnak küldött üzenetek monitorozásához kövesse az alábbi lépéseket:

1. Kattintson a jobb gombbal az eszközt, és válassza ki **figyelési D2C üzenet Start**.
1. A figyelt üzenetek megjelenő **kimeneti** > **Azure IoT-eszközkészlet** megtekintése.
1. Figyelés leállításához kattintson a jobb gombbal a **kimeneti** megtekintéséhez és kijelöléséhez **Stop Monitoring D2C üzenet**.

## <a name="send-cloud-to-device-messages"></a>Üzenetküldés a felhőből az eszközökre

Küldjön üzenetet az IoT hubhoz az eszközt, kövesse az alábbi lépéseket:

1. Kattintson a jobb gombbal az eszközt, és válassza ki **C2D-üzenet küldése eszközre**. 
1. Adja meg a beviteli mezőbe.
1. Eredmények megjelenő **kimeneti** > **Azure IoT-eszközkészlet** megtekintése.

## <a name="next-steps"></a>További lépések

Bemutattuk, hogyan lehet az eszköz a felhőbe irányuló üzenetek figyeléséhez és a felhőből az eszközre irányuló üzenetküldés az IoT-eszköz és az Azure IoT Hub között.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
