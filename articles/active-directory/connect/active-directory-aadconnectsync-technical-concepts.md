---
title: 'Az Azure AD Connect szinkronizálása: technikai kulcsfogalmak |} A Microsoft Docs'
description: Ismerteti a technikai Azure AD Connect szinkronizálása.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 731cfeb3-beaf-4d02-aef4-b02a8f99fd11
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 4c7f1a0f1199daf9409f9ef3b4230e774321fe59
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918713"
---
# <a name="azure-ad-connect-sync-technical-concepts"></a>Az Azure AD Connect szinkronizálása: technikai kulcsfogalmak
Ebben a cikkben található egy összefoglaló az a témakör [ismertetése architektúra](active-directory-aadconnectsync-technical-concepts.md).

Az Azure AD Connect szinkronizálása egy szilárd metakönyvtár szinkronizálási platformra épít.
Az alábbi szakaszok a metakönyvtár szinkronizálási fogalmak vezetnek be.
Építve a MIIS ILM és FIM, az Azure Active Directory Sync szolgáltatás a következő adatforráshoz csatlakozik, adatforrások, valamint a kiépítés közötti szinkronizálása és -megszüntetés identitások platformot kínál a.

![Műszaki fogalmak](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

A következő szakaszok további információt a FIM szinkronizálási szolgáltatás a következő szempontokat:

* Összekötő
* Attribútumfolyam
* Összekötő-térben
* Metaverzum
* Kiépítés

## <a name="connector"></a>Összekötő
A csatlakoztatott címtárban folytatott kommunikációhoz használt kódmodulokat összekötők (korábbi nevén felügyeleti ügynökök (MAs)) nevezzük.

Az Azure AD Connect szinkronizálási futtató számítógépen van telepítve. Az összekötők lehetővé az ügynök nélküli kivételfigyelés Serverhez helyett speciális ügynökök telepítését a távoli rendszer protokollok használatával. Ez azt jelenti, hogy csökkent kockázat és üzembe helyezéshez szükséges idő, különösen akkor, ha a kritikus fontosságú alkalmazások és rendszerek.

A fenti képen az összekötő azonos a az összekötőtérben, de magában foglalja a külső rendszer folytatott minden kommunikáció.

Az összekötő felelős az összes importálás és exportálás funkció a rendszer, és fejlesztői számára való csatlakozás minden rendszer natív módon használata a deklaratív kiépítés adatátalakítások testreszabásához kellene szabadít.

Import és export csak fordulhat elő, ha ütemezett, ami lehetővé teszi a változások a rendszer, mivel a módosítások nem automatikusan továbbítódik a csatlakoztatott adatforráshoz további szigetelése. Emellett a fejlesztők is hozzon létre saját összekötőket az olyan gyakorlatilag bármilyen adatforráshoz való kapcsolódáshoz.

## <a name="attribute-flow"></a>Attribútumfolyam
A metaverzum az összes csatlakoztatott identitások a szomszédos összekötőterek összevont nézetet. A fenti ábrán a kimenő és bejövő folyamathoz nyílhegy vonalak Attribútumfolyam megfelelőként jelenik. Attribútumfolyam másolása és a egy másikra rendszer adatok átalakítása és az összes attribútum folyamatok (bejövő vagy kimenő).

Attribútumfolyam esetén az összekötőtérben, és a metaverzum között kétirányúan szinkronizálási (teljes vagy különbözeti) műveletek ütemezésére.

Attribútumfolyam csak akkor történik meg, ezek szinkronizálások futtatásakor. Szinkronizálási szabályok attribútumfolyamok vannak definiálva. Ezek lehetnek a bejövő (ISR a fenti képen) vagy a kimenő (OSR a fenti képen).

## <a name="connected-system"></a>Csatlakoztatott rendszer
Csatlakoztatott rendszer (más néven csatlakoztatott címtárhoz) címre a távoli rendszer az Azure AD Connect szinkronizálási csatlakozik-e, és olvasott és írt azonosító adatok, illetve onnan.

## <a name="connector-space"></a>Összekötő-térben
Csatlakoztatott adatforrások jelenik meg az objektumok és attribútumok az összekötőtérben egy szűrt részét.
Ez lehetővé teszi a szinkronizálási szolgáltatás kizárólag a helyi anélkül, hogy lépjen kapcsolatba a távoli rendszer, amikor az objektumok szinkronizálásának kellene, és korlátozza az import való interakció, és csak exportálja.

Ha az adatforrás és az összekötő módosításait (egy különbözeti importálást) biztosítani, majd a működési hatékonyságot jelentősen megnő, csak a változásokat, mivel a legutolsó lekérdezési ciklust cserélt. Az összekötő-térben insulates a csatlakoztatott adatforráshoz, a módosítások propagálása automatikusan úgy, hogy az összekötő ütemezés importálja és exportálja. A hozzáadott biztosítási biztosít Önnek adó megoldás tesztelése, megtekintés vagy a következő frissítés megerősítése közben.

## <a name="metaverse"></a>Metaverzum
A metaverzum az összes csatlakoztatott identitások a szomszédos összekötőterek összevont nézetet.

Identitások össze vannak kapcsolva, és a szolgáltató különböző attribútumok importálás megfeleltetéseket keresztül van hozzárendelve, a központi metaverzum-objektum elkezdi összesített adatokat több rendszer. Az ezen objektum Attribútumfolyam leképezések végrehajtott műveletek közül sokat kimenő információt.

Objektumok jönnek létre, amikor egy mérvadó rendszer-projektek, azokat a metaverzumba. Amint megtörtént az összes kapcsolat, a rendszer törli a metaverzum-objektum.

Közvetlenül a metaverzumban található objektumok nem szerkesztheti. Az objektum összes adatot kell lennie viszonyított Attribútumfolyam keresztül. A metaverzum kezeli az állandó összekötőik összes összekötőtérbe. Ezek az összekötők nem igényelnek újrakiértékelési minden egyes szinkronizálás futtatása. Ez azt jelenti, hogy az Azure AD Connect szinkronizálása nem kell minden alkalommal, amikor a megfelelő távoli objektum kereséséhez. Ezzel elkerülhető, hogy megakadályozza a módosítását az attribútumokat, amelyek általában felelős az objektumok naplókezelője költséges ügynökök van szükség.

Amelyek esetlegesen már létező objektumokat, amelyeket kezelni kell új adatforrások felfedezésére, amikor az Azure AD Connect szinkronizálása a join szabály nevű folyamat, amellyel kapcsolatot létesít a lehetséges jelöltek kiértékelheti, hogy használja.
A kapcsolat létrejött, az értékelés indítást, és a normál Attribútumfolyam kerül sor a távoli csatlakoztatott adatforráshoz, és a metaverzumba.

## <a name="provisioning"></a>Kiépítés
Ha egy mérvadó forrás projektek a metaverzumba egy új összekötőtér objektuma új objektumot egy másik összekötőt képviselő egy alsóbb rétegbeli csatlakoztatott adatforrás is létrehozható.

Ez természetüknél fogva hoz létre hivatkozást, és Attribútumfolyam kétirányúan folytathatja.

Minden alkalommal, amikor egy szabály határozza meg, hogy egy új összekötőtér objektuma kell létrehozni, kiépítés nevezzük. Azonban ez a művelet csak kerül sor az összekötőtérben belül, mert azt nem vállalunk a csatlakoztatott adatforrásba mindaddig, amíg az exportálás történik.

## <a name="additional-resources"></a>További források
* [Az Azure AD Connect-szinkronizálás: Szinkronizálási beállítások testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
