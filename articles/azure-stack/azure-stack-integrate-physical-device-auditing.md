---
title: Az Azure Stack fizikai eszköz naplózása
description: Ismerje meg, hogyan integrálható a fizikai eszköz a fájlhozzáférés naplózása az Azure Stackben
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 08/01/2018
ms.author: patricka
ms.reviewer: fiseraci
keywords: ''
ms.openlocfilehash: 459cdf4e1a70ee02d818dd6abe101e4fc3475b68
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/10/2018
ms.locfileid: "40034765"
---
# <a name="azure-stack-datacenter-integration---physical-device-auditing"></a>Az Azure Stack adatközpont integrációja - naplózás fizikai eszköz

Minden fizikai eszköz az Azure Stackben, például az alaplapi felügyeleti vezérlővel (bmc) és a hálózati kapcsolók, hozzon létre vizsgálati naplókat, és azok integrálni kell a teljes naplózási megoldás. Mivel az eszközök az Azure Stack OEM különböző hardverszállítók eltérőek, forduljon a gyártója által biztosított naplózás integrációs dokumentáció. Az alábbi szakaszok a fizikai eszköz az Azure Stackben naplózás néhány általános adatok megadása.  

## <a name="physical-device-access-auditing"></a>Fizikai eszköz fájlhozzáférés naplózása

Minden fizikai eszköz az Azure Stackben TACACS vagy RADIUS-használatát támogatja. Ez magában foglalja a hozzáférést az alaplapi felügyeleti vezérlőnek (BMC) és a hálózati kapcsolók.

Az Azure Stack-megoldások tartalmaz RADIUS vagy a beépített TACACS. A megoldások azonban ellenőrzése a piacon elérhető meglévő RADIUS vagy TACACS megoldások támogatása érdekében.

A RADIUS csak az MSCHAPv2 lett érvényesítve. RADIUS használata a legbiztonságosabb megvalósítás Ez jelöli.
Tekintse meg az OEM hardvergyártójához TACAS vagy RADIUS engedélyezése az Azure Stack megoldás részét képező eszközök.

## <a name="syslog-forwarding-for-network-devices"></a>Syslog-továbbítást a hálózati eszközök

Az Azure Stackben minden fizikai hálózati eszköz támogatja a syslog-üzeneteket. Az Azure Stack-megoldások syslog-kiszolgálót tartalmaz. Azonban az eszközök ellenőrzése a piacon elérhető meglévő syslog megoldásokat üzenetek küldése támogatásához.

A syslog-célcím nem gyűjtött a központi telepítés kötelező paraméter, de azt is hozzáadhatók üzembe helyezés után. Forduljon az OEM hardver forgalmazójával, konfigurálja a syslog-továbbítást, a hálózati eszközökön.

## <a name="next-steps"></a>További lépések

[Karbantartási szabályzat](azure-stack-servicing-policy.md)
