---
title: Azure eszköz üzembe helyezése eszköz fogalmak |} Microsoft Docs
description: Eszköz-üzembehelyezési a eszköz kiépítése szolgáltatáshoz és az IoT-központ jellemző fogalmakat ismerteti
author: nberdy
ms.author: nberdy
ms.date: 09/05/2017
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: bd77a56acee948995bb2fcbb5beea60f69cda9ee
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34630153"
---
# <a name="iot-hub-device-provisioning-service-device-concepts"></a>IoT Hub eszköz kiépítése szolgáltatáshoz eszköz fogalmak

IoT Hub eszköz kiépítése szolgáltatáshoz olyan IoT hub, amely egy megadott IoT-központ nulla érintéssel eszközkiépítési konfigurálására használt segítő szolgáltatás. A Device Provisioning Service-szel több millió eszköz kiépítését végezheti el biztonságosan és skálázható módon.

Ez a cikk áttekintést a *eszköz* fogalmak részt vevő eszközök kiépítését. Ez a cikk az érintett személyeknek igényeinek jobban megfelelő a [gyártási lépés](about-iot-dps.md#manufacturing-step) az eszköz telepítési felkészülés.

## <a name="attestation-mechanism"></a>Állapotigazolási mechanizmus

Az igazolás módszer használata általában egy eszközidentitás használt módszert. Az igazolás mechanizmus a tanúsítványigénylési listája, amely arra kéri a létesítési szolgáltatás melyik adott eszközzel használni kívánt igazolási módszert is fontos.

> [!NOTE]
> Az IoT-központ "hitelesítési séma" hasonló fogalma az adott szolgáltatást használ.

Az eszköz kiépítése szolgáltatás igazolási két formáját támogatja:
* **X.509 tanúsítvány** a szabványos X.509 tanúsítvány hitelesítési folyamat alapján.
* **Platformmegbízhatósági modul (TPM) megbízható** nonce kihívást, a TPM szabvány a kulcsok használatával van a közös hozzáférésű Jogosultságkód (SAS) aláírt jogkivonat alapján. Ennek a fizikai TPM az eszközön nem szükséges, de a szolgáltatás várhatóan igazolják, hogy az ellenőrzőkulcsot / használatával az [TPM spec](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Hardveres biztonsági modul

A hardveres biztonsági modult, vagy a hardveres biztonsági MODULT, biztonságos, hardver-alapú eszköz titkos kulcsok tárolására szolgál, és a titkos tárolási legbiztonságosabb formája. X.509-tanúsítványokat és a SAS-tokenje tárolható a HSM-ben. Hardveres biztonsági modulok mindkét igazolás mechanizmusok használható a létesítési szolgáltatás támogat.

> [!TIP]
> Erősen ajánlott eszközök biztonságosan tárolni a titkos kulcsok az eszközök hardveres biztonsági modul használata.

Eszköz titkokat is tárolhatók a szoftver (memória), de tárolási kevésbé biztonságos formája, mint a hardveres biztonsági MODULT.

## <a name="registration-id"></a>Regisztrációs azonosító

A regisztrációs Azonosítót az eszköz kiépítése szolgáltatásban eszköz egyedi azonosítására szolgál. Az eszköz azonosítója a létesítési szolgáltatás egyedinek kell lennie [azonosító hatókör](#id-scope). Minden egyes eszközének rendelkeznie kell egy regisztrációs azonosítót. A regisztrációs Azonosítót alfanumerikus, nagybetűk, és kötőjeleket tartalmazhat.

* Esetén a TPM-regisztrációs Azonosítót biztosítja a TPM-be.
* Esetén a tanúsítvány X.509-alapú regisztrációs Azonosítót valósul meg a tanúsítvány tulajdonosának nevével.

## <a name="device-id"></a>Eszközazonosító

Az Eszközazonosítót az azonosítója megegyezik az IoT-központ jelenik meg. A regisztrációs bejegyzés adható meg a kívánt eszköz azonosítója, de nincs rá szükség kell beállítani. Ha nem kívánt eszköz azonosító van megadva a beléptetési listájában, a regisztrációs azonosítója lesz az Eszközazonosítót az eszköz regisztrálásakor. További információ [IoT-központ eszközazonosítók](../iot-hub/iot-hub-devguide-identity-registry.md).

## <a name="id-scope"></a>Hatókör azonosítója

Az azonosító hatókör van rendelve egy eszköz kiépítése szolgáltatáshoz, amikor a felhasználó által létrehozott, és egyedi módon azonosítja az adott létesítési szolgáltatás segítségével regisztrálja az eszközt. Az azonosító hatókörben az szolgáltatás által létrehozott és nem módosítható, amely biztosítja, hogy az egyediségi is.

> [!NOTE]
> Egyediségi hosszan futó üzembe helyezési műveletek és egyesülés és beszerzési forgatókönyvek esetében fontos.
