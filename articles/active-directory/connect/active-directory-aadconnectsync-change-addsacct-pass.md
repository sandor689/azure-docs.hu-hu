---
title: 'Azure AD Connect szinkronizálása: az Active Directory tartományi szolgáltatások fiók jelszavának módosítása |} Microsoft Docs'
description: Ez a témakör a dokumentum ismerteti az Azure AD Connect frissítése után az Active Directory tartományi szolgáltatások fiók jelszava megváltozott.
services: active-directory
keywords: AD DS-fiókjához, az Active Directory-fiókot, a jelszót
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a4d0d062b28b03de7f1e606202dddae28bf6a2f3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34592459"
---
# <a name="changing-the-ad-ds-account-password"></a>Az Active Directory tartományi szolgáltatások fiók jelszavának módosítása
Az AD DS-fiókjához az Azure AD Connect a helyszíni Active Directory folytatott kommunikációhoz használt felhasználói fiókra hivatkozik. Ha módosítja a Tartományi fiók jelszavát, az új jelszóval frissítenie kell az Azure AD Connect szinkronizálási szolgáltatás. Ellenkező esetben a szinkronizálás a helyszíni Active Directory a már nem szinkronizálhat megfelelően, és találkozhat hibák a következők:

* A Synchronization Service Managert, bármely importálási vagy exportálási művelet a helyszíni AD meghiúsul, és **-start-hitelesítő adatok elhagyását** hiba.

* A Windows eseménynaplóban, az alkalmazások eseménynaplójában keresse meg a hibát tartalmaz **Event ID 6000** és üzenet **"a"contoso.com"kezelőügynök futtatása sikertelen, mert a hitelesítő adatok érvénytelenek voltak"**.


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a>Az AD DS-fiókjához új jelszót a Synchronization Service frissítése
A Synchronization Service frissítése az új jelszóval:

1. Indítsa el a Synchronization Service Managert (KEZDŐ → szinkronizálási szolgáltatás).
</br>![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  

2. Lépjen a **összekötők** fülre.

3. Válassza ki a **Címtárösszekötőben** , amely megfelel a az AD DS-fiókjához, amelynek a jelszava megváltozott.

4. A **műveletek**, jelölje be **tulajdonságok**.

5. Az előugró párbeszédpanelen válassza ki a **csatlakozás az Active Directory-erdő**:

6. Írja be az új Active Directory tartományi szolgáltatások fiókjának a **jelszó** szövegmező.

7. Kattintson a **OK** az új jelszó mentse és zárja be az előugró párbeszédpanelen.

8. Indítsa újra az Azure AD Connect szinkronizálási szolgáltatás a Windows szolgáltatásvezérlő. Ez azért szükséges, hogy az összes hivatkozás a régi jelszó törlődik a memória-gyorsítótárban.

## <a name="next-steps"></a>További lépések
**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)

* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
