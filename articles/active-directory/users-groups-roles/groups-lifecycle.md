---
title: Az Azure Active Directory csoportokat az Office 365-höz lejárati |} A Microsoft Docs
description: Az Azure Active Directory az Office 365-csoportok lejáratának beállítása
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 03/09/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 656577211a992432366033326362c9892f7ba6ef
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861769"
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Az Office 365-csoportok-elévülési szabályzatának konfigurálása

Kezelheti az Office 365-csoportok életciklusának elévülési szabályzatot beállításával a számukra. A kizárólag Office 365-csoportok az Azure Active Directoryban (Azure AD) lejárati házirendjében is megadhatja. 

Ha egyszer már megadta egy csoport lejár:
-   A csoport tulajdonosai értesítést kap a csoport megújítására, a lejárat megújítására
-   Minden olyan csoportot, amely nem újítja meg törlődik.
-   Minden Office 365-csoportot, hogy törölték a csoport tulajdonosai vagy a rendszergazda által állíthatók vissza 30 napon belül

> [!NOTE]
> Konfigurálása és használata az elévülési szabályzat Office 365-csoportok igényel, hogy az aktuális Azure AD Premium licenccel rendelkezik, amely az elévülési szabályzat vonatkozik a csoportok összes tagja számára.

További információkért töltse le és telepítse az Azure AD PowerShell-parancsmagok: [Graph 2.0.0.137 Azure Active Directory PowerShell](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="roles-and-permissions"></a>Szerepkörök és engedélyek
Az alábbiakban a szerepköröket, amelyek konfigurálhatja és használhatja a lejárati az Office 365-csoportok az Azure ad-ben.

Szerepkör | Engedélyek
-------- | --------
Globális rendszergazda vagy a felhasználói fiók | Létrehozása, olvasása, frissítése, vagy az Office 365 csoportok lejárati házirend-beállítások törlése<br>Minden Office 365-csoportot is megújítása
Felhasználó | Saját Office 365-csoportot is megújítása<br>Saját Office 365-csoportot is helyreállíthatja.<br>Olvashatja a lejárati házirend-beállítások

Engedélyek visszaállítani egy törölt csoport további információkért lásd: [visszaállítása egy törölt Office 365-csoportot az Azure Active Directoryban](../fundamentals/active-directory-groups-restore-azure-portal.md).

## <a name="set-group-expiration"></a>Csoport elévülés beállítása

1. Nyissa meg a [Azure AD felügyeleti központ](https://aad.portal.azure.com) egy olyan fiókkal, amely az Azure AD-bérlő globális rendszergazdája lesz.

2. Válassza ki **csoportok**, majd **lejárati** a lejárati beállításainak megnyitásához.
  
  ![Lejárati panel](./media/groups-lifecycle/expiration-settings.png)

4. Az a **lejárati** panelen is:

  * Állítsa be a csoport élettartama napban. Kiválaszthatja az előre megadott értékeket, vagy egy egyéni értéket (kell lennie, vagy több 31 napon belül). 
  * Adjon meg egy e-mail címet, ahol a megújítási és a lejárati értesítést küldni Ha egy csoportnak nincs tulajdonosa. 
  * Válassza ki, mely Office 365-csoportok lejár. Engedélyezheti a lejárat **összes** Office 365-csoportokat, ha szeretné, csak engedélyezi **kijelölt** Office 365-csoportokat, vagy válassza ki a **nincs** letiltása az összes csoport lejárati .
  * Ha elkészült, válassza a beállítások mentéséhez **mentése**.


E-mail-értesítések, például a érkeznek az Office 365-csoportok tulajdonosainak 30 nap, 15 napon keresztül, és a csoport lejárata előtt 1 nap.

![Lejárati értesítés e-mailben](./media/groups-lifecycle/expiration-notification.png)

Az a **megújítása csoport** értesítő e-mailt, közvetlenül a csoport tulajdonosai is elérni a Részletek lapján a hozzáférési panelen. A felhasználók, a csoport például annak leírása, amikor azt utolsó megújítani, a lejárat és is lehetővé teszi a csoport megújítása további információ kapható. A Részletek lapján is mostantól az Office 365-csoport forrásokra mutató hivatkozásokat, hogy a csoport tulajdonosa kényelmesen megtekintheti a tartalom és a tevékenység a csoportjaikhoz tartozó.

Amikor lejár egy csoportot, akkor a csoport egy nappal a lejárat után törlődik. E-mail-értesítést, például a érkezik az Office 365-csoportok tulajdonosainak, lejárati és további törlést az Office 365-csoportot, amely tájékoztatja őket.

![Csoport törlése e-mail-értesítés](./media/groups-lifecycle/deletion-notification.png)

A törléstől számított 30 napon belül tudja állítani a csoport kiválasztásával **csoport visszaállítása** vagy a PowerShell-parancsmagok használatával leírtak szerint [visszaállítása egy törölt Office 365-csoportot az Azure Active Directoryban](../fundamentals/active-directory-groups-restore-azure-portal.md).
    
Ha a csoportban van visszaállítása dokumentumokat, SharePoint-webhelyek vagy más állandó objektumok, teljes körűen visszaállítani a csoportot és annak tartalma akár 24 órát igénybe vehet.

> [!NOTE]
> * Amikor először állítsa be a lejárat, minden olyan csoportok, amelyek régebbiek lejárati időközét 30 lejáratig hátralévő napok vannak állítva. Az első megújítási értesítő e-mail küldése egy napon belül. 
>   Például csoport 400 napja hozták létre, és a lejárati időközét 180 napra van állítva. Lejárati beállítások alkalmazásakor csoport rendelkezik törlése előtt 30 nappal, kivéve, ha a tulajdonos megújítja azt.
> * Jelenleg csak egy elévülési szabályzatot is konfigurálható a bérlő az Office 365-csoportok.
> * Ha egy dinamikus csoport törlése és visszaállítása, látható egy új csoportot, és újra feltölti a szabálya szerint. Ez a folyamat akár 24 órát is igénybe vehet.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>Office 365-csoport lejárati működése egy postaláda jogi okokból történő megőrzést
Egy csoport lejár, és törlik, majd 30 napig törlés után a csoporthoz tartozó adatokat az alkalmazásokból, például a Planner, helyek, vagy a csoportok véglegesen törölve lesz, de a csoport-postaládát jogi okokból történő megőrzést marad, és nem véglegesen törlődik. A rendszergazda az Exchange-parancsmagok használatával állítsa helyre a postaládát az adatok beolvasása. 

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>Office 365-csoport lejárati adatmegőrzési szabályzat működése
A megőrzési házirend úgy van konfigurálva, a biztonsági és megfelelőségi központban. Beállítása az Office 365-csoportok adatmegőrzési amikor lejár egy csoportot, és törlik, ha az a csoport postaláda csoportbeszélgetések és fájlok csoport a helyen maradnak a visszatartási tároló az adott számú napig megőrzési meghatározott a házirend. A felhasználók nem jelenik meg a csoport vagy a lejárat után, de a hely és a postaláda adatait keresztül elektronikus felderítés állíthatja helyre.

## <a name="powershell-examples"></a>PowerShell-példák
Az alábbiakban példát, hogyan használhatja PowerShell-parancsmagok az Office 365-csoportok lejáratának beállításainak konfigurálása a bérlőben:

1. Telepítse a PowerShell 2.0 előzetes verzió modult (2.0.0.137), és jelentkezzen be a PowerShell-parancssorba:
  ````
  Install-Module -Name AzureADPreview
  connect-azuread 
  ````
2. A lejárati beállítások New-AzureADMSGroupLifecyclePolicy konfigurálása: Ez a parancsmag beállítja a élettartamának – 365 nap a bérlőben lévő összes Office 365-csoportok. A megújítására vonatkozó értesítéseket az Office 365 csoportok nélkül tulajdonosok küld "emailaddress@contoso.com"
  
  ````
  New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
  ````
3. A Get-AzureADMSGroupLifecyclePolicy meglévő házirend beolvasása: Ez a parancsmag lekéri az aktuális Office 365 csoport lejárati beállítások konfigurált. Ebben a példában látható:
  * A házirend-azonosító 
  * A élettartama a bérlőben lévő összes Office 365-csoportok – 365 nap van beállítva.
  * A megújítására vonatkozó értesítéseket az Office 365 csoportok nélkül tulajdonosok küld "emailaddress@contoso.com."
  
  ````
  Get-AzureADMSGroupLifecyclePolicy
  
  ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
  --                                    ------------------- ----------------- ---------------------------
  26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
  ```` 
   
4. A Set-AzureADMSGroupLifecyclePolicy meglévő szabályzat frissítése: ezt a parancsmagot használja egy meglévő szabályzatot frissítésére. Az alábbi példában a meglévő házirendben lévő csoport élettartama 180 nap módosul 365 nap. 
  
  ````
  Set-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”   -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
  ````
  
5. Adott csoportok felvétele a szabályzatba Add-AzureADMSLifecyclePolicyGroup: Ez a parancsmag hozzáadja egy csoporthoz az életciklus-szabályzat. Példa: 
  
  ````
  Add-AzureADMSLifecyclePolicyGroup -Id “26fcc232-d1c3-4375-b68d-15c296f1f077” -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
  ````
  
6. Távolítsa el a meglévő házirend Remove-AzureADMSGroupLifecyclePolicy: Ez a parancsmag törli az Office 365-csoport lejárati beállítások, de elengedhetetlen hozzá, a házirend-azonosítóhoz. Ezzel a lépéssel letiltja az Office 365-csoportok lejárati idejét. 
  
  ````
  Remove-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”
  ````
  
Az alábbi parancsmagok segítségével részletesebben a szabályzat konfigurálása. Tekintse meg a vonatkozó tudnivalókat, [PowerShell-dokumentáció](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups).

* Get-AzureADMSGroupLifecyclePolicy
* New-AzureADMSGroupLifecyclePolicy
* Get-AzureADMSGroupLifecyclePolicy
* Set-AzureADMSGroupLifecyclePolicy
* Remove-AzureADMSGroupLifecyclePolicy
* Add-AzureADMSLifecyclePolicyGroup
* Remove-AzureADMSLifecyclePolicyGroup
* Reset-AzureADMSLifeCycleGroup   
* Get-AzureADMSLifecyclePolicyGroup

## <a name="next-steps"></a>További lépések
E cikkekben további információk az Azure AD-csoportokat.

* [Tekintse meg a meglévő csoportok](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Csoport beállításainak kezelése](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Egy csoport tagjai kezelése](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Egy csoport tagságát kezelése](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [A csoport dinamikus szabályok kezelése](groups-dynamic-membership.md)
