---
title: LDAPS-t használ az interneten keresztül felügyelt tartomány elérését DNS konfigurálása |} A Microsoft Docs
description: Az LDAPS-t használ az interneten keresztül az Azure AD Domain Services által felügyelt tartományokhoz eléréséhez DNS konfigurálása
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: a47f0f3e-2578-422a-a421-034f66de38f5
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: maheshu
ms.openlocfilehash: 669e0392cb77434c372c9af3c4d467d19cff8abd
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2018
ms.locfileid: "39501734"
---
# <a name="configure-dns-to-access-an-azure-ad-domain-services-managed-domain-using-secure-ldap-ldaps"></a>Biztonságos LDAP (LDAPS) használatával az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz eléréséhez DNS konfigurálása

## <a name="before-you-begin"></a>Előkészületek
Teljes [3. feladat: az Azure portal használatával a felügyelt tartomány secure LDAP engedélyezése](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)

## <a name="task-4-configure-dns-to-access-the-managed-domain-from-the-internet"></a>4. feladat: A felügyelt tartomány elérését az internetről érkező DNS konfigurálása
> [!TIP]
> **Nem kötelező feladat** – Ha nem tervezi elérni a felügyelt tartományra LDAPS-t az interneten keresztül, hagyja ki ezt a konfigurációs feladatot.
>
>

Mielőtt ezt a feladatot, teljes körű lépéseinek végrehajtása a [3. feladat](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md).

Miután engedélyezte a secure LDAP-hozzáférését az interneten keresztül, kell, hogy az ügyfélszámítógépek megtalálhatja a felügyelt tartomány DNS frissítése. A külső IP-cím megjelenik a **tulajdonságok** lapján **külső IP-cím az LDAPS-t ELÉRÉST**.

Állítsa a külső DNS-szolgáltatónál, hogy a külső IP-címre mutat a felügyelt tartomány (például ldaps.contoso100.com) DNS-nevét. Hozzon létre például a következő DNS-bejegyzés:

    ldaps.contoso100.com  -> 52.165.38.113

Ennyi az egész! Most már készen áll a felügyelt tartomány secure LDAP használatával az interneten keresztül csatlakozni.

> [!WARNING]
> Ne feledje, hogy ügyfélszámítógépek meg kell bíznia az LDAPS-t tanúsítvány kiállítója lehessen csatlakozni tudjon a felügyelt tartományra LDAPS-t. Ha egy nyilvánosan megbízható hitelesítésszolgáltató használ, nem kell semmit, mivel az ügyfélszámítógépek megbízható ezek tanúsítványkiállítók. Ha önaláírt tanúsítványt használ, telepítse az önaláírt tanúsítvány nyilvános részét az ügyfélszámítógépen a megbízható tanúsítványok tárolójába.
>
>

## <a name="next-step"></a>Következő lépés
[5. feladat: Kötés létrehozása a felügyelt tartománnyal és a biztonságos LDAP-hozzáférés zárolása](active-directory-ds-ldaps-bind-lockdown.md)
