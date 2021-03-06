---
title: Az Azure AD alkalmazás-Proxy és Qlik logika |} Microsoft Docs
description: Alkalmazásproxy bekapcsolása az Azure portálon, és a fordított proxyhoz tartozó az összekötők telepítése.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 350a43dbb96e900c48a4207c808add1484237ef6
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589083"
---
# <a name="application-proxy-and-qlik-sense"></a>Proxy és Qlik logika 
Az Azure Active Directory Alkalmazásproxyjával és Qlik logika közösen együtt, hogy könnyen alkalmazásproxy használatával adja meg a távoli elérés az Qlik logika üzembe helyezéshez.  

## <a name="prerequisites"></a>Előfeltételek 
Ez a forgatókönyv további része azt feltételezi, hogy Ön a következőket:
 
- Konfigurált [Qlik logika](https://community.qlik.com/docs/DOC-19822). 
- [Az alkalmazásproxy-összekötő telepítése](manage-apps/application-proxy-enable.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Az alkalmazások közzététele az Azure-ban 
QlikSense közzététele, akkor két alkalmazások közzététele az Azure-ban.  

### <a name="application-1"></a>#1. alkalmazás: 
Kövesse az alábbi lépéseket az alkalmazás közzétételére. A részletes lépéseket bemutató 1-8, tekintse meg a még [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](manage-apps/application-proxy-publish-azure-portal.md). 


1. Jelentkezzen be globális rendszergazdaként az Azure-portálon. 
2. Válassza ki **Azure Active Directory** > **vállalati alkalmazások**. 
3. Válassza ki **Hozzáadás** a panel tetején. 
4. Válassza ki **helyszíni alkalmazás**. 
5.       Töltse ki a kötelező mezőket az új alkalmazás adatait tartalmazó. A következő útmutatást használhatja a beállításokat: 
    - **Belső URL-cím**: az alkalmazás egy belső URL-CÍMÉT, amely a QlikSense URL-cím kell rendelkeznie. Például **https&#58;//demo.qlikemm.com:4244** 
    - **Előhitelesítési módszer**: Azure Active Directoryban (de nem ajánlott) 
1.       Válassza ki **Hozzáadás** a panel alján. Az alkalmazás kerül, és a quick start menü megnyitása. 
2.       A quick start menüben válassza ki a **rendelje hozzá a felhasználót tesztelési**, és legalább egy felhasználói hozzáadni az alkalmazáshoz. Győződjön meg arról, hogy a teszt fiókjának van hozzáférési joga a helyszíni alkalmazások. 
3.       Válassza ki **hozzárendelése** menteni a teszt felhasználó hozzárendelése. 
4.       (Választható) Az alkalmazás-felügyelet panel válassza az egyszeri bejelentkezés. Válasszon **Kerberos által korlátozott delegálás** a legördülő menüből, és kitöltése során a kötelező mezőket a Qlik konfiguráció alapján. Kattintson a **Mentés** gombra. 

### <a name="application-2"></a>#2. alkalmazás: 
Kövesse a lépéseket, mint az alkalmazás 1, a következő kivételekkel: 

**#5. lépés**: belső URL-cím kell a QlikSense a hitelesítési portot, az alkalmazás által használt URL-cím. Az alapértelmezett érték **4244** HTTPS és a HTTP 4248. Például: **https&#58;//demo.qlik.com:4244**</br></br> 
 **#10. lépés:** nem beállítania egyszeri Bejelentkezést, és hagyja a **egyszeri bejelentkezés le van tiltva**
 
 
## <a name="testing"></a>Tesztelés 
Az alkalmazás tesztelése készen áll. Hozzáférési QlikSense közzétételéhez az alkalmazás 1 és bejelentkezési mindkét alkalmazáshoz rendelt felhasználó használt külső URL-CÍMÉT.  

## <a name="next-steps"></a>További lépések

- [Alkalmazások közzététele az alkalmazásproxy](manage-apps/application-proxy-publish-azure-portal.md)
- [Alkalmazásproxy összekötők használata](manage-apps/application-proxy-connector-groups.md).
