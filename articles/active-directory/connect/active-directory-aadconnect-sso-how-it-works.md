---
title: 'Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezés – hogyan működik |} A Microsoft Docs'
description: Ez a cikk ismerteti az Azure Active Directory zökkenőmentes egyszeri bejelentkezés funkció működése.
services: active-directory
keywords: Mi az Azure AD Connect, Active Directory telepítése szükséges összetevők SSO, Azure AD egyszeri bejelentkezés
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 6507158a63de508164fc74bcafe39785046a2c79
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213350"
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Az Azure Active Directory zökkenőmentes egyszeri bejelentkezés: részletes technikai bemutatása

Ez a cikk biztosít technikai részletek be az Azure Active Directory zökkenőmentes egyszeri bejelentkezés (zökkenőmentes SSO) funkció működése.

## <a name="how-does-seamless-sso-work"></a>Közvetlen egyszeri bejelentkezés működése

Ez a szakasz a következő három részből hozzá:
1. A közvetlen egyszeri bejelentkezés funkció beállítását.
2. Egy tranzakció egyetlen felhasználó jelentkezzen be a webböngésző működése a közvetlen egyszeri bejelentkezés.
3. Egy tranzakció egyetlen felhasználó jelentkezzen be egy natív ügyfél működése a közvetlen egyszeri bejelentkezés.

### <a name="how-does-set-up-work"></a>Hogyan állítsa be a munkahelyi?

Közvetlen egyszeri bejelentkezés engedélyezve van az Azure AD Connect használatával, ahogy [Itt](active-directory-aadconnect-sso-quick-start.md). A funkció engedélyezése közben a következő lépések történnek:
- Egy számítógép-fiók nevű `AZUREADSSOACC` (amely jelöli az Azure AD) jön létre a helyszíni Active Directory (AD) minden olyan AD-erdőben.
- A számítógépfiók Kerberos visszafejtési kulcs biztonságosan megosztott Azure AD-val. Ha több AD-erdőkkel, egyenként kell a saját Kerberos visszafejtési kulcsot.
- Emellett két Kerberos egyszerű szolgáltatásnevek (SPN) jön létre, amelyek a két URL-címet, amelyekre az Azure AD-be.

>[!NOTE]
> Minden AD-erdőben szinkronizál az Azure AD-(Azure AD Connect használatával), és segít a felhasználóknak a közvetlen egyszeri bejelentkezés azt szeretné, a számítógép fiókjának és a Kerberos egyszerű szolgáltatásnevek jönnek létre. Helyezze át a `AZUREADSSOACC` számítógépfiók, egy szervezeti egység (OU) más számítógépfiókokat tároló biztosíthatja, hogy ugyanúgy kezeli, és nem törlődik.

>[!IMPORTANT]
>Kifejezetten ajánljuk, hogy Ön [vihetők át a Kerberos-visszafejtési kulcs](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account) , a `AZUREADSSOACC` számítógépfiók legalább 30 nap.

A beállítás befejezése után a közvetlen egyszeri bejelentkezés működik ugyanúgy, mint bármely más jelentkezzen be, amely integrált Windows-hitelesítés (IWA) használja.

### <a name="how-does-sign-in-on-a-web-browser-with-seamless-sso-work"></a>Hogyan jelentkezzen be a közvetlen egyszeri bejelentkezés munkahelyi böngészőben?

A böngészőben a bejelentkezési folyamat a következőképpen történik:

1. A felhasználó megpróbál hozzáférni egy webes alkalmazás (például az Outlook Web App - https://outlook.office365.com/owa/) tartományhoz csatlakoztatott vállalati eszköz a vállalati hálózaton belül.
2. Ha a felhasználó még nem jelentkezett be, a rendszer átirányítja a felhasználót az Azure AD bejelentkezési oldal.
3. A felhasználó begépeli a felhasználó neve, az Azure AD bejelentkezési oldal.

  >[!NOTE]
  >A [bizonyos alkalmazások](./active-directory-aadconnect-sso-faq.md#what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso), 2 & 3. lépéseket a rendszer kihagyja.

4. A JavaScript használatával a háttérben, az Azure AD kihívást a böngészőben a 401-es jogosulatlan választ, adja meg a Kerberos-jegy-n keresztül.
5. A böngészőben a jegyet, az Active Directory kér a `AZUREADSSOACC` számítógépfiók (amely az Azure AD).
6. Az Active Directory megkeresi a számítógépfiók, és adja vissza egy Kerberos-jegyet a böngészőnek a számítógépfiók titkos kulcs titkosított.
7. A böngészőben a Kerberos-jegyet, akkor megszerezte az Active Directoryból az Azure AD továbbítja.
8. Az Azure AD visszafejti a Kerberos-jegy, amely tartalmazza a felhasználó a vállalati eszköz be van jelentkezve a korábban megosztott kulcs használatával.
9. Értékelés után az Azure AD az alkalmazásnak egy tokent ad vissza, vagy arra kéri a felhasználót a kiegészítő igazolásokat, például a többtényezős hitelesítés végrehajtásához.
10. Ha a felhasználói bejelentkezés sikeres, a felhasználó nem tudja elérni az alkalmazást.

A következő ábra szemlélteti az összes összetevő és a folyamat lépéseit.

![Zökkenőmentes egyszeri bejelentkezést a – webes alkalmazás folyamat](./media/active-directory-aadconnect-sso/sso2.png)

Közvetlen egyszeri bejelentkezés az alkalmi, ami azt jelenti, ha sikertelen, a bejelentkezési felület áll vissza a normál viselkedési – azaz, a felhasználó adja meg a jelszavát, hogy jelentkezzen be kell.

### <a name="how-does-sign-in-on-a-native-client-with-seamless-sso-work"></a>Hogyan jelentkezzen be egy natív ügyfél közvetlen egyszeri bejelentkezés munkahelyi?

Egy natív ügyfél bejelentkezési folyamata a következőképpen történik:

1. A felhasználó megpróbál hozzáférni egy natív alkalmazást (például az Outlook ügyfélprogram) tartományhoz csatlakoztatott vállalati eszköz a vállalati hálózaton belül.
2. Ha a felhasználó még nem jelentkezett be, a natív alkalmazás az eszköz Windows-munkamenetből kérdezi le a felhasználó felhasználóneve.
3. Az alkalmazás a felhasználónév küld az Azure ad-hez, és kéri le a bérlője WS-Trust MEX végpont.
4. Az alkalmazás ezután lekérdezi a WS-Trust MEX végpontot annak ellenőrzéséhez, hogy integrált hitelesítés végpont érhető el.
5. Ha a 4. lépés sikeres, a Kerberos kihívást jelenik meg.
6. Ha az alkalmazás nem sikerült beolvasni a Kerberos-jegy, továbbítja, akár az Azure AD integrált hitelesítési végpontja.
7. Azure ad-ben a Kerberos-jegy visszafejti, majd érvényesíti azt.
8. Azure ad-ben a felhasználó bejelentkezik, és egy SAML-jogkivonat problémák az alkalmazáshoz.
9. Az alkalmazás ezután elküldi az OAuth2 jogkivonat végpontra az Azure AD SAML-jogkivonat.
10. Azure ad-ben az SAML-jogkivonat ellenőrzi, és problémák az alkalmazáshoz egy hozzáférési jogkivonatot és a egy frissítési jogkivonatot a megadott erőforrás és a egy azonosító jogkivonat.
11. A felhasználó élvezheti a hozzáférést az alkalmazás-erőforrást.

A következő ábra szemlélteti az összes összetevő és a folyamat lépéseit.

![Zökkenőmentes egyszeri bejelentkezés – natív alkalmazások](./media/active-directory-aadconnect-sso/sso14.png)

## <a name="next-steps"></a>További lépések

- [**Gyors üzembe helyezési** ](active-directory-aadconnect-sso-quick-start.md) – és az Azure AD közvetlen egyszeri bejelentkezés futtatása.
- [**Gyakran ismételt kérdések** ](active-directory-aadconnect-sso-faq.md) – a gyakran feltett kérdésekre adott válaszok.
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-sso.md) – ismerje meg, a szolgáltatással kapcsolatos gyakori problémák megoldása.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkcióra vonatkozó javaslata tárolásához.
