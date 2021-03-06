---
title: Az Azure Active Directory B2C létrehozni bérlők hibáinak elhárítása |} A Microsoft Docs
description: Hibák és megoldásaik létrehozásához az Azure Active Directory vagy az Azure Active Directory B2C-bérlő.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 009f7ac2f7e614b7e07623e41888973f1a2b254d
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440980"
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Az Azure Active Directory vagy az Azure Active Directory B2C-bérlő létrehozásával kapcsolatos hibaelhárítás 

## <a name="create-an-azure-ad-tenant"></a>Az Azure AD-bérlő létrehozása
Ha nem hozható létre egy Azure Active Directory (Azure AD) bérlő az első kísérlet alkalmával, próbálkozzon újra. Ha a probléma tartósan fennáll, forduljon az Azure ügyfélszolgálatához.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C bérlő létrehozása
Ha problémák merülnek fel amikor, [hozzon létre egy Azure Active Directory B2C (Azure AD B2C) bérlő](active-directory-b2c-get-started.md), próbálja meg a következő beállításokat:

* Ha az Azure AD B2C-bérlő nem jelenik meg a bérlők a listában, próbálkozzon újra a bérlő létrehozásához.
* Ha az Azure AD B2C-bérlő a bérlők listájában jelenik meg, és megjelenik a következő hibaüzenetet kapja, a bérlő törlése, és újra létre kell hoznia:

    "Nem sikerült végrehajtani a B2C-bérlő"contosob2c"létrehozását. Látogasson el a [hivatkozás](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) további útmutatást. "
* Vannak ismert problémákat, ha törli egy meglévő Azure AD B2C-bérlő, majd hozza létre újból az ugyanazon a néven. Amikor létrehoz egy új Azure AD B2C-bérlő, a másik tartománynevet kell használnia.
* Ha ezek a megoldások nem működnek, forduljon az Azure ügyfélszolgálatához. További információkért lásd: [fájl támogatási kéréseket az Azure AD B2C](active-directory-b2c-support.md).

