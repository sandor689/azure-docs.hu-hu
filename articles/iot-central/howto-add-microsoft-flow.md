---
title: Az IoT Central-összekötő a Microsoft Flow munkafolyamatok létrehozásához |} A Microsoft Docs
description: Az IoT Central-összekötő a Microsoft Flow használatával aktiválhatja a munkafolyamatokat és létrehozása, frissítése és törlése eszköz a munkafolyamatokban.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 06/12/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: peterpr
ms.openlocfilehash: 2414fb0576448339b268dce92dafe6c70108ba5d
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008950"
---
# <a name="build-workflows-with-the-iot-central-connector-in-microsoft-flow"></a>Hozzon létre munkafolyamatokat az IoT Central-összekötő a Microsoft Flow

Munkafolyamatok automatizálása a számos alkalmazásokat és szolgáltatásokat, üzleti felhasználók által használt, a Microsoft Flow használatával. Az IoT Central-összekötő használatával a Microsoft Flow, is indíthat a munkafolyamatok, amikor egy szabályt az IoT-központ lesz elindítva. Egy munkafolyamatban, IoT-központ vagy bármely más alkalmazás által aktivált a műveleteket használhatja az IoT Central-összekötő az eszköz létrehozásához, egy eszköz tulajdonságait és beállítások frissítése vagy eszköz törlése. Tekintse meg [a Microsoft Flow sablonok](https://aka.ms/iotcentralflowtemplates) IoT-központ más szolgáltatásokba, mint a mobil értesítések és a Microsoft Teams kapcsolódnak.

> [!NOTE] 
> Szüksége lesz egy Microsoft személyes és munkahelyi vagy iskolai fiókot a Microsoft Flow-ba való bejelentkezéshez. További tudnivalók a Microsoft Flow-tervek [Itt](https://aka.ms/microsoftflowplans).

## <a name="trigger-a-workflow-when-a-rule-is-fired"></a>Amikor egy szabály aktiválódik munkafolyamat elindítására

Ez a szakasz bemutatja, hogyan indítható el a Flow mobilalkalmazás mobil értesítés, ha egy szabályt az IoT-központ aktiválódik.

1. Első lépésként [olyan szabályt hoz létre az IoT-központ](howto-create-telemetry-rules.md). Miután menti a szabály feltételeit, válassza ki a **Microsoft Flow művelet** egy új művelet. Egy új böngészőlapon vagy ablakban kell nyissa meg a böngészőben, átirányítjuk a Microsoft Flow-bA.

1. Jelentkezzen be a Microsoft Flow szolgáltatásba. Ez nem kell ugyanazt a fiókot, mint amelyet az IoT-központ használja. Egy Áttekintés lap megjelenítése egy IoT-központ csatlakozó, amely egy egyéni műveleti nyílik.

1. Kattintson a **továbbra is**. Ekkor megnyílik a Microsoft Flow Tervező a munkafolyamat létrehozása alatt. A munkafolyamat, amely rendelkezik az alkalmazás és a szabály már kitöltött IoT-központ eseményindító tartozik.

1. Válasszon **+ új lépés** és **művelet hozzáadása**. Ezen a ponton a munkafolyamat bármely elvégzendő is hozzáadhat. Példaként nézzük mobil értesítés küldése. Keresse meg **értesítési**, és válassza a **értesítések - mobil értesítés küldése**.

1. A műveletet töltse ki a kívánt tegyük fel, hogy az értesítési szövegmezőben. Megadhat *dinamikus tartalom* az IoT-központ szabályból, mentén fontos információkat, például az eszköz és az időbélyegző-értesítések való átadásához.

    > [!NOTE]
    > Kattintson a "További információk" szöveget, mérés és tulajdonságértékeket, amely kiváltotta a szabály a dinamikus tartalom ablakban.

    ![Folyamatvezérlés dinamikus ablaktáblán nyissa meg a művelet szerkesztése](./media/howto-add-microsoft-flow/flowdynamicpane.PNG)

1. Ha elkészült a művelet szerkesztését, kattintson a **mentése**. Akkor jut el a munkafolyamatot – áttekintés oldalra. Itt láthatja a futtatási előzményeket, és megoszthatja más munkatársakkal.

    > [!NOTE]
    > Ha azt szeretné, más felhasználók az IoT-központ alkalmazásban ez a szabály szerkesztéséhez, meg kell osztania azt velük a Microsoft Flow. A Microsoft Flow-fiókok hozzáadása tulajdonosként a munkafolyamatban.

1. Ha visszatér az IoT Central alkalmazáshoz, megjelenik ez a szabály a Microsoft Flow művelet rendelkezik az műveletek területen.

Mindig megkezdheti az IoT Central-összekötő a Microsoft Flow munkafolyamat létrehozásához. Kiválaszthatja, melyik IoT Central-alkalmazást, és melyik szabály való csatlakozáshoz.

## <a name="create-a-device-in-a-workflow"></a>A munkafolyamat-eszköz létrehozásához

Ez a szakasz bemutatja, hogyan használatával hozhat létre egy új eszközt az IoT Central egy gombra a leküldéses egy mobileszközön, a Microsoft Flow mobilalkalmazás használatával. Ez a művelet a Flow segítségével például a Dynamics ERP-rendszerekkel integrálhatja IoT-központ új eszköz létrehozása, ha egy eszköz hozzáadása máshol.

1. Először hozzon létre egy üres munkafolyamatot a Microsoft Flow.

1. Keresse meg **folyamatgomb mobilhoz** eseményindítóként.

1. Az erre az eseményindítóra, adjon hozzá szövegbevitelt, nevű **eszköznév**. Módosítani kell a leíró szöveg **adja meg az eszköz neve, az új**.

1. Adjon meg új műveletet. Keresse meg a **az Azure IoT Central - eszköz létrehozása** művelet.

1. Válassza ki az alkalmazást, és válasszon egy sablont az eszköz az eszköz létrehozásához kijelölhető. Láthatja, hogy a művelet, bontsa ki a tulajdonságok és az eszköz beállítások megjelenítése.

1. Válassza ki az eszköz neve mezőt. A dinamikus tartalmú ablaktáblában válassza **eszköznév**. Ez az érték a bemeneti, a felhasználó megadja a mobilalkalmazáson keresztül lesznek átadva, és az új eszközt az IoT-központ neve lesz. Ebben a példában az egyetlen kötelezően kitöltendő mező az eszköz nevét, a piros csillaggal jelölve. Előfordulhat, hogy egy másik eszköz a sablon ki, hogy hozzon létre egy új eszközt szeretne több kötelező mező.

    ![Folyamat létrehozása eszköz dinamikus műveletpanel](./media/howto-add-microsoft-flow/flowcreatedevice.PNG)
1. (Nem kötelező) Adja meg a többi mező létrehozása új eszközökhöz tetszés szerint. Például választhatja, ha az eszköz szimulált-e, vagy sem.

1. Végül mentse a munkafolyamatot.

1. Próbálja ki a munkafolyamatot a Microsoft Flow mobilalkalmazásban. Nyissa meg a **gombok** lap az alkalmazásban. Az eszköz új munkafolyamat létrehozása -> gombra kell megjelennie. Adja meg az új eszköz nevét, és tekintse meg az IoT-központ megjelenítése.

    ![Folyamat létrehozása az eszköz mobil képernyőképe](./media/howto-add-microsoft-flow/flowmobilescreenshot.png)

## <a name="update-a-device-in-a-workflow"></a>A munkafolyamat-eszköz frissítése

Ez a szakasz bemutatja, hogyan eszközbeállítások, illetve egy mobileszközön, a Microsoft Flow mobilalkalmazás használatával egy gombra a leküldéses az IoT-központ tulajdonságainak frissítéséhez.

1. Először hozzon létre egy üres munkafolyamatot a Microsoft Flow.

1. Keresse meg **folyamatgomb mobilhoz** eseményindítóként.

1. Az erre az eseményindítóra, például a beviteli mód hozzáadása egy **hely** szövegbevitel, amely megfelel egy beállítást vagy tulajdonság módosítani szeretné. Módosítani leírásának szövege.

1. Adjon meg új műveletet. Keresse meg a **egy eszköz frissítése az Azure IoT Central -** művelet.

1. Válassza ki az alkalmazás a legördülő listából. Most szüksége lesz az eszköz azonosítója, a meglévő frissíteni szeretné. Az eszköz azonosítója kérhet le az IoT-központ a **Device Explorer**.

    ![IoT-központ device explorer eszköz azonosítója](./media/howto-add-microsoft-flow/iotcdeviceid.png)

1. Ezen a ponton frissítheti az eszköz nevét, és ha igen, egy szimulált eszközt, vagy sem. Az eszköz tulajdonságok és a beállítások bármelyikét frissítéséhez válassza ki a frissíteni kívánt az eszköz az eszköz sablon a **eszköz sablon** legördülő listából. A művelet csempe kibontása a tulajdonságok és frissítheti beállítások megjelenítése.

1. Válassza ki az egyes tulajdonságok és frissíteni a kívánt beállításokat. A dinamikus tartalmú ablaktábla válassza ki a megfelelő bemenet a trigger által. Ebben a példában a hely érték propagálja le tulajdonság frissítése az eszköz helyét.

    ![A folyamat frissítése eszköz művelet dinamikus panel](./media/howto-add-microsoft-flow/flowupdatedevice.PNG)

1. Végül mentse a munkafolyamatot.

1. Próbálja ki a munkafolyamatot a Microsoft Flow mobilalkalmazásban. Nyissa meg a **gombok** lap az alkalmazásban. Megjelenik a frissítési eszköz munkafolyamat -> gombra. Adja meg a bemeneti adatok, és tekintse meg az eszköz frissíti az IoT Central!

## <a name="delete-a-device-in-a-workflow"></a>A munkafolyamat eszköz törlése

Egy eszközt a device ID használatával törölheti az **eszköz törlése az Azure IoT Central -** művelet. Íme egy példa-munkafolyamat, amely törli egy eszköz a Microsoft Flow mobilalkalmazásban egy gombra a leküldéses.

   ![A folyamat törlése eszköz munkafolyamat](./media/howto-add-microsoft-flow/flowdeletedevice.PNG)
    
## <a name="troubleshooting"></a>Hibaelhárítás

Ha gondjai vannak szeretne létrehozni az Azure IoT Central-összekötő kapcsolatot, az alábbiakban néhány tipp.

1. Személyes Microsoft-fiókok (például @hotmail.com, @live.com, @outlook.com tartományok) jelenleg nem támogatottak. Az AAD munkahelyi vagy iskolai fiókjával kell.

2. Ha hiba történt az AAD-fiókjával kap, próbálja meg megnyitni a Windows PowerShell, és a következő parancsmagok esetén futtassa rendszergazdaként.
    ``` PowerShell
    Install-Module AzureAD
    Connect-AzureAD
    New-AzureADServicePrincipal -AppId 9edfcdd9-0bc5-4bd4-b287-c3afc716aac7 -DisplayName "Azure IoT Central"
    ```
    
## <a name="next-steps"></a>További lépések
Most, hogy megtanulhatta, hogyan használhatja a Microsoft Flow munkafolyamatok létrehozásához, a javasolt következő lépésre, hogy [Eszközkezelés](howto-manage-devices.md).
