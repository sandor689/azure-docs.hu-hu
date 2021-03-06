---
title: Bot jelentéskészítő SDK használatának a C# az Azure-ban a robot a LUIS integrálása |} A Microsoft Docs
description: A robot a LUIS-alkalmazás a Bot keretrendszer használatával integrálva hozhat létre.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/06/2018
ms.author: diberry
ms.openlocfilehash: ca893dadfed04c90a4d89087299bf1cc2bf55eb9
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225958"
---
# <a name="web-app-bot-using-the-luis-template-for-c"></a>Webes alkalmazás robot a LUIS-sablon használatával a C#

Létrehozhat egy csevegőrobotot, integrált a language understanding használatának.

## <a name="prerequisite"></a>Előfeltétel

* [A LUIS alkalmazás HomeAutomation](luis-get-started-create-app.md). A leképezések a LUIS alkalmazás leképezés a robot párbeszédpanel kezelők számára. 

## <a name="luis-homeautomation-intents"></a>A LUIS HomeAutomation leképezések

| Leképezés | Példa utterance (kifejezés) | A robot funkció |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Kapcsolja be a lámpák. | Ha a LUIS-leképezés `HomeAutomation.TurnOn` észlel, a bot meghívja a `OnIntent` párbeszédpanel kezelő. Ezen a párbeszédpanelen, ahol meghívta egy IoT-szolgáltatás, kapcsolja be az eszközön, és ossza meg a felhasználót, hogy az eszköz be van kapcsolva. |
| HomeAutomation.TurnOff | Kapcsolja ki a szobája lámpa. | Ha a LUIS-leképezés `HomeAutomation.TurnOff` észlel, a bot meghívja a `OffIntent` párbeszédpanel kezelő. Ezen a párbeszédpanelen, ahol meghívta egy IoT-szolgáltatás, kapcsolja ki az eszközt, és ossza meg a felhasználót, hogy az eszköz ki van kapcsolva. |

## <a name="create-a-language-understanding-bot-with-bot-service"></a>A Language Understanding bot létrehozni a Bot Service-szel

1. Az a [az Azure portal](https://portal.azure.com), jelölje be **hozzon létre új erőforrást** bal felső menüjében.

    ![Új erőforrás létrehozása](./media/luis-tutorial-cscharp-web-bot/bot-service-creation.png)

2. A keresőmezőbe keresése **Web App Bot**. 

    ![Új erőforrás létrehozása](./media/luis-tutorial-cscharp-web-bot/bot-service-selection.png)

3. Kattintson a Web App Bot ablakban **létrehozás**.

4. A **Bot Service**, adja meg a szükséges információkat, majd kattintson **létrehozás**. Ez létrehozza és telepíti a bot service és a LUIS-alkalmazás az Azure-bA. Ha a használni kívánt [speech betanítási művelet](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), tekintse át [régió követelmények](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming) előtt a robot létrehozásának. 
    * Állítsa be **alkalmazásnév** a robot neve. A robot a felhőben (például mynotesbot.azurewebsites.net) történő telepítése során az altartomány neve lesz. <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Válassza ki az előfizetést [erőforráscsoport](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), App service-csomagot, és [hely](https://azure.microsoft.com/regions/).
    * Válassza ki a **Language understanding (C#)** sablonját a **Bot sablon** mező.
    * Válassza ki a **LUIS alkalmazás hely**. Ez az a szerzői műveletekhez részben [régió](luis-reference-regions.md) jön létre az alkalmazást.
    * Jelölje be a megerősítő a jogi nyilatkozat. A jogi nyilatkozat feltételeit az alábbiakban olvashatják be a jelölőnégyzetet.

    ![Robotszolgáltatás](./media/luis-tutorial-cscharp-web-bot/bot-service-setting-callout-template.png)


5. Győződjön meg arról, hogy a bot service van telepítve.
    * Kattintson az értesítések (Harang ikon, amely mentén felső szegélyéhez, hogy az Azure Portalon található). Az értesítés megváltozik, a **központi telepítés elindítva** való **üzembe helyezés sikeres**.
    * Miután az értesítés megváltozik, és **üzembe helyezés sikeres**, kattintson a **erőforrás megnyitása** adott értesítésre.

> [!Note]
> A web app bot létrehozását is létrehoz egy új LUIS-alkalmazás. Van már betanított és az Ön számára közzétett. 

## <a name="try-the-default-bot"></a>Próbálja ki az alapértelmezett robotot

Győződjön meg arról, hogy a robot kiválasztásával lett-e telepítve. a **értesítések** jelölőnégyzetet. Az értesítéseket a változik **üzembe helyezés folyamatban...**  való **üzembe helyezés sikeres**. Kattintson a **erőforrás megnyitása** gombra kattintva nyissa meg a robot erőforrásokat.

A robot üzembe helyezése után kattintson a **vizsgálat a webes csevegési** a webes csevegési ablak megnyitásához. Írja be a "hello", a webes csevegési.

  ![A robot webes csevegési tesztelése](./media/luis-tutorial-cscharp-web-bot/bot-service-web-chat.png)

A robot válaszol kimondásával "elérte üdvözlés. Azt mondta: hello ".  Ez a válasz megerősíti, hogy a robot a üzenetet kapott, és alapértelmezés szerint a LUIS-alkalmazást, amely a létrehozást, az átadott. Ez az alapértelmezés a LUIS-alkalmazás egy üdvözlés leképezés észlelhető. A következő lépésben a robot a LUIS alkalmazás helyett a LUIS-alkalmazás alapértelmezett korábban létrehozott fogja kötni.

## <a name="connect-your-luis-app-to-the-bot"></a>A robot a LUIS-alkalmazás csatlakoztatása

Nyissa meg **Alkalmazásbeállítások** és szerkesztheti a **LuisAppId** mező tartalmazza a LUIS-alkalmazás Alkalmazásazonosítója. A HomeAutomation LUIS-alkalmazás a nyugati régióban hozta létre, ha módosítani szeretné a **LuisAPIHostName** is. A **LuisAPIKey** jelenleg az Authoring Tool kulccsal van beállítva. Módosítja ezt a végpont kulcs, amikor a forgalom meghaladja az ingyenes szint kvótát. 

  ![Frissítés az Azure-ban a LUIS alkalmazás azonosítója](./media/luis-tutorial-cscharp-web-bot/bot-service-app-settings.png)

> [!Note]
> Ha nem rendelkezik a LUIS alkalmazás Azonosítóját a [kezdőlap Automation alkalmazás](luis-get-started-create-app.md), jelentkezzen be a [LUIS](luis-reference-regions.md) webhelyen, amellyel belép Azure-bA ugyanazzal a fiókkal. 
> 1. Kattintson a **saját alkalmazások**. 
> 2. Keresse meg a korábban létrehozott, LUIS alkalmazást, amely tartalmazza a szándékok és entitások HomeAutomation tartományból.
> 3. Az a **beállítások** a LUIS alkalmazás lapon keresse meg és másolja az alkalmazás azonosítóját. Győződjön meg arról, hogy [betanított](luis-interactive-test.md) és [közzétett](luis-how-to-publish-app.md). 

    > [!WARNING]
    > If you delete your app ID or LUIS key, the bot will stop working.

## <a name="modify-the-bot-code"></a>A robot kód módosítása

1. Kattintson a **hozhat létre** majd **online kódszerkesztő megnyitása**.

   ![Online kódszerkesztő megnyitása](./media/luis-tutorial-cscharp-web-bot/bot-service-build.png)

2. Kattintson a jobb gombbal `build.cmd` válassza **futtatása konzolról** hozhat létre az alkalmazást. Nincsenek több létrehozási lépések a szolgáltatás automatikusan az Ön számára befejeződött. A build elkészült, a befejezésekor "Sikeresen befejeződött."

3. A kódszerkesztőben nyissa meg a `/Dialogs/BasicLuisDialog.cs`. A következő kódot tartalmazza:

   [!code-csharp[Default BasicLuisDialog.cs](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/Default_BasicLuisDialog.cs "Default BasicLuisDialog.cs")]

## <a name="change-code-to-homeautomation-intents"></a>Kód módosítása HomeAutomation leképezések


1. Távolítsa el a három leképezési attribútumok és módszerek a **üdvözlőszöveget**, **Mégse**, és **súgó**. Ezek a leképezések nem használ előre összeállított HomeAutomation a tartományban. Győződjön meg arról, hogy a **nincs** szándék attribútum és módja. 

2. Adja a függőségeket, a más függőségekkel a fájl elejéhez:

   [!code-csharp[Dependencies](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=4-5&dedent=8 "dependencies")]

3. Adjon állandókat tetején karakterláncok kezelése a `BasicLuisDialog ` osztály:

   [!code-csharp[Add Intent and Entity Constants](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=23-32&dedent=8 "Add Intent and Entity Constants")]

4. Adja hozzá a kódot, az új leképezések a `HomeAutomation.TurnOn` és `HomeAutomation.TurnOff` belül a `BasicLuisDialog ` osztály:

   [!code-csharp[Add Intents](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=61-71&dedent=8 "Add Intents")]

5. Adja hozzá a kódot a LUIS belül található entitások beolvasása a `BasicLuisDialog ` osztály:

   [!code-csharp[Collect entities](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=34-53&dedent=8 "Collect entities")]

6. Változás **ShowLuisResult** metódus az a `BasicLuisDialog ` kerekítendő a pontszám, az entitások összegyűjtése és megjelenítése a válaszüzenet a csevegőrobot osztály:

   [!code-csharp[Display message in chatbot](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=73-83&dedent=8 "Display message in chatbot")]

## <a name="build-the-bot"></a>A robot létrehozása
A kódszerkesztőben, kattintson a jobb gombbal a `build.cmd` válassza **futtatása konzolról**.

![Webes bot létrehozása ](./media/luis-tutorial-cscharp-web-bot/bot-service-build-run-from-console.png)

A kód nézet megjelenítése a állapotának és eredményeinek megtekintéséhez a build terminálablak cseréli le.

![Webes bot létrehozása sikeres](./media/luis-tutorial-cscharp-web-bot/bot-service-build-success.png)

> [!TIP]
> Egy alternatív módszer a robotot hozhat létre, hogy a robot neve a felső kék sávban válassza ki és **Kudu konzol megnyitása**. Ekkor megnyílik a konzol **D:\home**. 
> 
> Módosítsa a könyvtárat a **D:\home\site\wwwroot** beírásával: `cd site\wwwroot`
>
> A build a parancsprogram futtatásához írja be: `build.cmd`

## <a name="test-the-bot"></a>A robot tesztelése

Az Azure Portalon kattintson a **tesztelni, webes csevegési** a robot teszteléséhez. Írja be az üzenetek like "kapcsolja fel a lámpákat" és "kapcsolja ki a saját melegítőkráter" meghívni a korábban hozzáadott, szándék fog vonatkozni.

   ![A webes csevegési HomeAutomation bot tesztelése](./media/luis-tutorial-cscharp-web-bot/bot-service-chat-results.png)

> [!TIP]
> A LUIS-alkalmazás a robot kód módosítás nélkül ténytábláknál. Lásd: [példa beszédmódok hozzáadása](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) és [taníthat vagy tesztelhet a LUIS-alkalmazás](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-interactive-test). 

## <a name="download-the-bot-to-debug"></a>Töltse le a robot hibakeresése
A robot nem működik, ha a helyi gépre töltse le a projektet, és továbbra is [hibakeresés](https://docs.microsoft.com/bot-framework/bot-service-debug-bot#debug-an-azure-app-service-web-app-c-bot). 

## <a name="learn-more-about-bot-framework"></a>További tudnivalók a Bot Framework
Tudjon meg többet [Bot Framework](https://dev.botframework.com/) és a [3.x](https://github.com/Microsoft/BotBuilder) és [4.x](https://github.com/Microsoft/botbuilder-dotnet) SDK-k.

## <a name="next-steps"></a>További lépések

Adja hozzá a LUIS-leképezések és a Bot service párbeszédpanelek kezelésére vonatkozó **súgó**, **Mégse**, és **üdvözlőszöveget** szándék fog vonatkozni. Ne felejtse el, betanítását és tehet közzé, és [hozhat létre](#build-the-bot) a web app bot. A LUIS, mind a robot a azonos leképezések kell rendelkeznie.

> [!div class="nextstepaction"]
> [Leképezések hozzáadása](./luis-how-to-add-intents.md)
> [Speech betanítási művelet](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


<!-- Links -->
[Github-BotFramework-Emulator-Download]: https://aka.ms/bot-framework-emulator
[Github-LUIS-Samples]: https://github.com/Microsoft/LUIS-Samples
[Github-LUIS-Samples-cs-hotel-bot]: https://github.com/Microsoft/LUIS-Samples/tree/master/bot-integration-samples/hotel-finder/csharp
[Github-LUIS-Samples-cs-hotel-bot-readme]: https://github.com/Microsoft/LUIS-Samples/blob/master/bot-integration-samples/hotel-finder/csharp/README.md
[BFPortal]: https://dev.botframework.com/
[RegisterInstructions]: https://docs.microsoft.com/bot-framework/portal-register-bot
[BotFramework]: https://docs.microsoft.com/bot-framework/
[VisualStudio]: https://www.visualstudio.com/

<!-- tested on Win10 -->
