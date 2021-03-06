---
title: A felhasználó hozzáférést egy alkalmazáshoz eltávolítása |} A Microsoft Docs
description: Megtudhatja, hogyan távolítsa el a felhasználó hozzáférést egy alkalmazáshoz
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 0deb5215c1379ac552a492f4b9e90df83201aebf
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364481"
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Alkalmazásokhoz való hozzáférés egy felhasználó eltávolítása

Ez a cikk segít megismerni, hogyan távolíthat el egy felhasználó hozzáférést egy alkalmazáshoz.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Egy alkalmazás egy adott felhasználó vagy csoport-hozzárendelés eltávolítása

Egy felhasználó vagy csoport-hozzárendelés alkalmazáshoz való eltávolításához kövesse a felsorolt lépéseket a [egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazásokat az Azure Active Directoryban](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) cikk.

. ## Letiltani a minden felhasználó számára az összes alkalmazás elérése

Összes felhasználói bejelentkezés egy alkalmazás letiltásához kövesse a felsorolt lépéseket a [letiltása a felhasználók bejelentkezési folyamatába egy vállalati alkalmazás az Azure Active Directoryban](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) cikk.

## <a name="i-want-to-delete-an-application-entirely"></a>Egy alkalmazás teljesen törölni

A **törölhető az alkalmazás**, kövesse az alábbi utasításokat:

1.  Nyissa meg a [ **az Azure portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdai** vagy **Társadminisztrátorként.**

2.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **minden szolgáltatás** a fő bal oldali navigációs menü tetején.

3.  Írja be a **"Azure Active Directory**" szöveget a szűrő keresőmezőbe, és válassza a **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **minden alkalmazás** az alkalmazások listájának megtekintéséhez.

   * Ha azt szeretné, hogy itt jelennek meg az alkalmazás nem látja, használja a **szűrő** vezérlőelem felső részén a **minden alkalmazás lista** és állítsa be a **megjelenítése** beállítást **összes Az alkalmazások.**

6.  Válassza ki a törölni kívánt alkalmazást.

7.  Ha az alkalmazás betöltött, kattintson a **törlése** ikonra a felső alkalmazás **áttekintése** ablaktáblán.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Szeretném tiltani minden jövőbeli felhasználói jóváhagyási művelet bármely alkalmazás

Felhasználói beleegyezés letiltása, a teljes címtárban megakadályozza, hogy a végfelhasználók hozzájárul ahhoz, hogy minden olyan alkalmazás esetében. A rendszergazdák továbbra is a felhasználó behalves hagyhatja jóvá. További információ az alkalmazás jóváhagyásának, és ezért lehetséges, hogy, vagy előfordulhat, hogy nem szeretne ehhez olvassa el a [ismertetése felhasználói és rendszergazdai jóváhagyás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

A **tiltsa le az összes jövőbeli felhasználói jóváhagyási művelet a teljes címtárban**, kövesse az alábbi utasításokat:

1.  Nyissa meg a [ **az Azure portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**

2.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **minden szolgáltatás** a fő bal oldali navigációs menü tetején.

3.  Írja be a **"Azure Active Directory**" szöveget a szűrő keresőmezőbe, és válassza a **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** a navigációs menü.

5.  Kattintson a **felhasználói beállítások**.

6.  Tiltsa le az összes jövőbeli felhasználó hozzájárulási műveleteket beállításával a **felhasználók engedélyezhetik alkalmazások hozzáférhetnek az adataikhoz való** kapcsolót **nem** , és kattintson a **mentése** gombra.


# <a name="next-steps"></a>További lépések
[Alkalmazásokhoz való hozzáférés kezelése](manage-apps/what-is-access-management.md)
