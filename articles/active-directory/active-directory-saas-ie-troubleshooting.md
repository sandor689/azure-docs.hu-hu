---
title: Az Azure hozzáférési Panel bővítmény hibaelhárítási az Internet Explorer |} A Microsoft Docs
description: Hogyan lehet az Internet Explorer-bővítmény a saját alkalmazások portál telepítése a csoportházirenddel.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/30/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 682973e6781a1de2c8d9628e39347650a3852b81
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364777"
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Az Internet Explorer a hozzáférési Panel bővítmény hibaelhárítása
Ez a cikk a következő problémák elhárításához nyújt segítséget:

* Most már nem fér hozzá az alkalmazások a saját alkalmazások portál használatával az Internet Explorer használatával.
* Annak ellenére, hogy már telepítette a szoftvert a "Szoftver telepítése" üzenetet látja.

Ha Ön rendszergazda, lásd még: [csoportházirend használatával az Internet Explorer a hozzáférési Panel bővítmény telepítése](active-directory-saas-ie-group-policy.md)

## <a name="run-the-diagnostic-tool"></a>A diagnosztikai eszköz futtatása
A hozzáférési Panel bővítmény telepítési problémáinak diagnosztizálhatja letöltésével és futtatásával a hozzáférési Panel diagnosztikai eszköz:

1. [A diagnosztikai eszköz letöltéséhez kattintson ide.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Nyissa meg a fájlt, és nyomja le az **az összes kibontása** gombra.
   
    ![Nyomja le az összes kibontása](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. Nyomja le az **kinyerése** gombra a folytatáshoz.
   
    ![Nyomja le az Extract](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. Futtassa az eszközt, kattintson a jobb gombbal a nevű fájl **AccessPanelExtensionDiagnosticTool**, majd **nyissa meg a > a Microsoft Windows alapú parancsfájlfuttató**.
   
    ![Nyissa meg a > a Microsoft Windows Script Host alapján](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. Ekkor megjelenik az alábbi diagnosztikai ablak, amely leírja, mi az a telepítési hiba lehet.
   
    ![A diagnosztikai ablak minta](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. Kattintson a "**Igen**", hogy a program, hárítsa el a problémákat, amelyek találhatók.
7. Szeretné menteni ezeket a módosításokat, zárjon be minden az Internet Explorer ablakot, és újra az Internet Explorer megnyitásához.<br />Ha továbbra sem tud hozzáférni az alkalmazásokat, próbálkozzon az alábbi lépéseket.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Ellenőrizze, hogy engedélyezve van-e a hozzáférési Panel bővítmény
Annak ellenőrzése, hogy a hozzáférési Panel bővítmény engedélyezve van-e az Internet Explorerben:

1. Az Internet Explorerben kattintson a **fogaskerék ikont** a az ablak jobb felső sarkában. Válassza ki **Internetbeállítások**.<br />(Az Internet Explorer régebbi verzióiban ez alatt található **eszközök > Internetbeállítások**.
   
    ![Lépjen a Tools > Internetbeállítások](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. Kattintson a **programok** lapfülre, majd kattintson a **bővítmények kezelése** gombra.
   
    ![Kattintson a bővítmények kezelése](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. Ezen a párbeszédpanelen válassza ki a **hozzáférési Panel bővítmény** és kattintson a **engedélyezése** gombra.
   
    ![Kattintson az engedélyezés](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. Szeretné menteni ezeket a módosításokat, zárjon be minden az Internet Explorer ablakot, és nyissa meg újra az Internet Explorer.

## <a name="enable-extensions-for-inprivate-browsing"></a>Bővítmények engedélyezése az InPrivate-böngészés
Ha az InPrivate-böngészés módban használja:

1. Az Internet Explorerben kattintson a **fogaskerék ikont** a az ablak jobb felső sarkában. Válassza ki **Internetbeállítások**.<br />(Az Internet Explorer régebbi verzióiban ez alatt található **eszközök > Internetbeállítások**.
   
    ![A diagnosztikai ablak minta](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. Nyissa meg a **adatvédelmi** lapot, majd **törölje a jelet** feliratú jelölőnégyzet **eszköztárak és kiterjesztések letiltása, ha az InPrivate-böngészés indítása**</p>
   
    ![Törölje a jelet letiltása eszköztárak és kiterjesztések InPrivate-böngészés indításakor](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. Szeretné menteni ezeket a módosításokat, zárjon be minden az Internet Explorer ablakot, és nyissa meg újra az Internet Explorer.

## <a name="uninstall-the-access-panel-extension"></a>Távolítsa el a hozzáférési Panel bővítményt
A hozzáférési Panel bővítmény eltávolítása a számítógépről:

1. Nyomja le a billentyűzeten a **Windows kulcs** a Start menü megnyitásához. Ha meg nyitva a menüben, semmit sem kell keresés beírhatja. Írja be a "Vezérlőpult", majd nyissa meg a **Vezérlőpult** amikor megjelenik a keresési eredmények között.
   
    ![Vezérlőpult keresése](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. A jobb felső sarkában a Vezérlőpulton, módosítsa a **megtekintéséhez** beállítást **nagy méretű ikonok**. Majd keresse meg és kattintson a **programok és szolgáltatások** gombra.
   
    ![A nézet cseréli a nagy méretű ikonok megjelenítése](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. Válassza ki a listából **hozzáférési Panel bővítmény**, és kattintson a a **Eltávolítás** gombra.
   
    ![Kattintson az Eltávolítás gombra](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. Ezután próbálkozzon az ismételt használatával ellenőrizheti, ha a probléma megoldódott-bővítményének telepítése.

Ha a kiterjesztés eltávolítása problémák merülnek fel, akkor is használatával távolíthatja el a [Microsoft javítása,](https://go.microsoft.com/?linkid=9779673) eszközt.

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](manage-apps/what-is-single-sign-on.md)
* [A hozzáférési Panel bővítmény telepítése csoportházirend használatával az Internet Explorer](active-directory-saas-ie-group-policy.md)

