---
title: Az Azure IoT Hub eszköz kiépítése szolgáltatáshoz biztonsági fogalmak |} Microsoft Docs
description: Biztonsági kiépítése a eszköz kiépítése szolgáltatáshoz és az IoT-központ jellemző fogalmakat ismerteti
author: nberdy
ms.author: nberdy
ms.date: 03/30/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 82548ef8fd3f992eedd77c93be47cb5328a584c7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628640"
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>IoT Hub eszköz kiépítése szolgáltatáshoz biztonsági fogalmak 

IoT Hub eszköz kiépítése szolgáltatáshoz olyan IoT hub, amely egy megadott IoT-központ nulla érintéssel eszközkiépítési konfigurálására használt segítő szolgáltatás. Az eszköz kiépítése szolgáltatáshoz is [automatikus-kiépítési](concepts-auto-provisioning.md) biztonságos és skálázható módon eszközök millióira. Ez a cikk áttekintést a *biztonsági* fogalmak részt vevő eszközök kiépítését. Ez a cikk akkor szükséges, az eszköz telepítési Felkészülés részt vevő összes személyeknek.

## <a name="attestation-mechanism"></a>Állapotigazolási mechanizmus

Az igazolás módszer használata általában egy eszközidentitás használt módszert. Az igazolás mechanizmus a tanúsítványigénylési listája, amely arra kéri a létesítési szolgáltatás melyik adott eszközzel használni kívánt igazolási módszert is fontos.

> [!NOTE]
> Az IoT-központ "hitelesítési séma" hasonló fogalma az adott szolgáltatást használ.

Eszköz kiépítése szolgáltatás igazolási két formáját támogatja:
* **X.509 tanúsítvány** a szabványos X.509 tanúsítvány hitelesítési folyamat alapján.
* **Platformmegbízhatósági modul (TPM) megbízható** nonce kihívást, a TPM szabvány a kulcsok használatával van a közös hozzáférésű Jogosultságkód (SAS) aláírt jogkivonat alapján. Ennek a fizikai TPM az eszközön nem szükséges, de a szolgáltatás várhatóan igazolják, hogy az ellenőrzőkulcsot / használatával az [TPM spec](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Hardveres biztonsági modul

A hardveres biztonsági modult, vagy a hardveres biztonsági MODULT, biztonságos, hardver-alapú eszköz titkos kulcsok tárolására szolgál, és a titkos tárolási legbiztonságosabb formája. X.509-tanúsítványokat és a SAS-tokenje tárolható a HSM-ben. A HSM lehet használni mindkét igazolás mechanizmusok az üzembe helyezési támogatja.

> [!TIP]
> Erősen ajánlott eszközök biztonságosan tárolni a titkos kulcsok az eszközök hardveres biztonsági modul használata.

Eszköz titkokat is tárolhatók a szoftver (memória), de tárolási kevésbé biztonságos formája, mint a hardveres biztonsági MODULT.

## <a name="trusted-platform-module"></a>Platformmegbízhatósági modul

TPM jelentheti a platform hitelesítéséhez használt kulcsokat biztonságosan tárolásához standard, vagy a modulok, a standard végrehajtási együttműködhet használt i/o-felület hivatkozhat. TPM diszkrét hardverként integrált hardver, a belső vezérlőprogram-alapú vagy szoftveres létezhet. További információ [TPM és TPM igazolás](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation). Eszköz kiépítése szolgáltatáshoz csak a TPM 2.0 támogatja.

TPM-igazolási nonce feladat, amely használja az ellenőrzőkulcs és a tárolási legfelső szintű kulcs a közös hozzáférésű Jogosultságkód (SAS) aláírt jogkivonat alapul.

### <a name="endorsement-key"></a>Ellenőrzőkulcs

Az ellenőrzőkulcsot a TPM-be, amelyek belsőleg generált, vagy olyanok, idő gyártási VHD aszimmetrikus kulcsok, és minden TPM egyedi. Az ellenőrző kulcs nem lehet módosítani vagy eltávolítani. Az ellenőrzőkulccsal titkos része soha nem szabadul kívül a TPM-, míg az ellenőrző kulcs nyilvános részének az eredeti TPM ismeri fel. További információ a [ellenőrzőkulccsal](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

### <a name="storage-root-key"></a>Tároló-gyökérkulcs

A legfelső szintű kulcsot a TPM tárolja, és alkalmazások által létrehozott TPM-kulcsok védelmét, hogy ezek a kulcsok nem használható a TPM nélkül. A tároló-gyökérkulcs jön létre, amikor Ön saját tulajdonba a TPM; Új felhasználó tulajdonba, törölje a jelet a TPM-be, amikor egy új tároló-gyökérkulcs jön létre. További információ a [tároló-gyökérkulcs](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="x509-certificates"></a>X.509 tanúsítvány

X.509-tanúsítványokat használ egy igazoló mechanizmusként módja egy kiváló éles méretezhető és egyszerűsítse az eszközök kiépítését. X.509-tanúsítványokat általában bizalmi kapcsolat, amelyben a tanúsítványlánc minden tanúsítványa a következő magasabb tanúsítványt, és így tovább, leállítja az önaláírt legfelső szintű tanúsítvány titkos kulcsával aláírt tanúsítványlánc vannak rendezve. Ezzel megadja a delegált a legfelső szintű tanúsítvány megbízható legfelső szintű hitelesítésszolgáltató (CA) le minden közbenső hitelesítésszolgáltató végfelhasználói "levél" tanúsítvány telepítve az eszközön keresztül jön létre megbízhatósági lánc. További tudnivalókért lásd: [X.509 CA-tanúsítványok használatával Device Authentication](/azure/iot-hub/iot-hub-x509ca-overview). 

A tanúsítványlánc gyakran bizonyos eszközök társított logikai és fizikai hierarchia jelöli. Például egy gyártó előfordulhat, hogy:
- egy önaláírt legfelső szintű hitelesítésszolgáltató-tanúsítvány kiállításához
- a legfelső szintű tanúsítványt használja minden gyári egyedi köztes Hitelesítésszolgáltatói tanúsítvány létrehozásához
- minden éles sor egyedi köztes Hitelesítésszolgáltatói tanúsítványt létrehozni a gyár minden gyári tanúsítvány használatára
- és végül a termelési sor-tanúsítvány használatával az egyes eszközök a sor gyártott (végfelhasználói) az eszköz egyedi tanúsítvány jön létre. 

További tudnivalókért lásd: [X.509 Hitelesítésszolgáltatói tanúsítványok az IoT-iparág fogalmi ismertetése](/azure/iot-hub/iot-hub-x509ca-concept). 

### <a name="root-certificate"></a>Legfelső szintű tanúsítvány

Egy legfelső szintű tanúsítványt a hitelesítésszolgáltatótól (CA) jelölő önaláírt X.509 tanúsítvány. A pontjáig, vagy megbízhatósági kapcsolati alap, a tanúsítványlánc. Legfelső szintű tanúsítványok saját szervezet által kibocsátott, vagy egy legfelső szintű hitelesítésszolgáltatótól származó. További tudnivalókért lásd: [beolvasása X.509 CA-tanúsítványok](/azure/iot-hub/iot-hub-security-x509-get-started#get-x509-ca-certificates). A legfelső szintű tanúsítvány is lehet hivatkozni, a legfelső szintű Hitelesítésszolgáltatói tanúsítványt.

### <a name="intermediate-certificate"></a>Köztes tanúsítvány

Egy köztes tanúsítványhoz egy X.509 tanúsítvány, amely a főtanúsítványt (vagy egy másik köztes tanúsítványt a főtanúsítvány láncában) aláírta. Az a lánc utolsó köztes tanúsítványa a levél tanúsítvány aláírására használatos. Egy köztes tanúsítvány is is hivatkozhatnak egy köztes Hitelesítésszolgáltatói tanúsítványhoz.

### <a name="end-entity-leaf-certificate"></a>Végfelhasználói "levél" tanúsítvány

A levél tanúsítvány, vagy végfelhasználói tanúsítvány, azonosítja a tanúsítvány tulajdonosa. Rendelkezik a legfelső szintű tanúsítvány a tanúsítványlánc, valamint nulla vagy több köztes tanúsítványa. A levél nem tanúsítvánnyal bármely egyéb tanúsítványok aláírásához. Egyedi módon azonosítja az eszközt, hogy a létesítési szolgáltatás, és az eszköz tanúsítványa néha nevezzük. A hitelesítés során az eszköz használatával a a tanúsítványhoz tartozó titkos kulcs birtokában challenge igazolása válaszolni a szolgáltatás. További tudnivalókért lásd: [eszközök hitelesítése aláírva, X.509 CA-tanúsítványok](/azure/iot-hub/iot-hub-x509ca-overview#authenticating-devices-signed-with-x509-ca-certificates).

## <a name="controlling-device-access-to-the-provisioning-service-with-x509-certificates"></a>A létesítési szolgáltatás X.509 tanúsítvánnyal rendelkező eszköz-hozzáférés szabályozásáról

A létesítési szolgáltatás regisztrációs bejegyzés használatával, amely segítségével az X.509 tanúsítvány mechanizmus használó eszközök a hozzáférés-vezérlésének két típusú mutatja:  

- [Az egyes beléptetési](./concepts-service.md#individual-enrollment) bejegyzések az eszköz tanúsítványát egy adott eszközhöz hozzárendelt vannak konfigurálva. Ezeket a bejegyzéseket szabályozhatja az egyes eszközökre történő regisztrációt.
- [Beléptetési csoport](./concepts-service.md#enrollment-group) társítva van egy adott köztes vagy a legfelső szintű Hitelesítésszolgáltatói tanúsítványt a rendszer. Ezek a bejegyzések regisztrációkat az összes eszköz, amely rendelkezik, amely köztes vagy a tanúsítványt a tanúsítványlánc legfelső szintű vezérlő. 

Amikor egy eszköz csatlakozik a létesítési szolgáltatás, a szolgáltatás kevesebb adott regisztrációs bejegyzéseket keresztül helyezi előtérbe pontosabb regisztrációs bejegyzéseket. Ez azt jelenti, hogy ha egy egyéni regisztrációt az adott eszköz már létezik, a létesítési szolgáltatás vonatkozik, a bejegyzést. Nem egyedi az eszköz regisztrációját, valamint egy beléptetési csoport számára az eszköz tanúsítványláncában szereplő első köztes tanúsítvány létezik-e, ha a szolgáltatás vonatkozik, hogy bejegyzést, és így tovább, a legfelső szintű lánc fel. A szolgáltatás vonatkozik az első vonatkozó bejegyzés talált, úgy, hogy:

- Ha az első regisztrációs bejegyzés található engedélyezve van, a szolgáltatás látja el az eszközt.
- Ha az első regisztrációs bejegyzés található le van tiltva, a szolgáltatás nem biztosítsa az eszköz.  
- Ha az eszköz tanúsítványláncában szereplő tanúsítványok egyikének sem található regisztrációs bejegyzés, a szolgáltatás nem biztosítsa az eszköz. 

A mechanizmus és a hierarchikus tanúsítványláncok hogyan szabályozhatja a hozzáférést az eszközök esetében az egyes eszközök is hatékony rugalmasságot biztosít. Tegyük fel a következő tanúsítványláncok öt eszközöket: 

- *1 eszköz*: legfelső szintű tanúsítványt A tanúsítvány eszköz 1 -> tanúsítvány ->
- *Az eszköz 2*: legfelső szintű tanúsítványt A -> eszköz 2 tanúsítvány tanúsítvány ->
- *Eszköz 3*: legfelső szintű tanúsítványt A -> eszközt 3 tanúsítvány tanúsítvány ->
- *Eszköz 4*: legfelső szintű tanúsítvány B -> 4 eszköz tanúsítvány tanúsítvány ->
- *5 eszköz*: legfelső szintű tanúsítvány B -> 5 eszköz tanúsítvány tanúsítvány ->

Kezdetben a legfelső szintű tanúsítvány engedélyezze a hozzáférést minden öt eszköz egy egyetlen engedélyezett csoport regisztrációs bejegyzést is létrehozhat. B tanúsítvány később biztonságának sérülése esetén, ha egy letiltott beléptetési csoport bejegyzést tanúsítvány B megelőzése érdekében létrehozhat *eszköz 4* és *eszköz 5* regisztrációja. Ha még mindig újabb *eszköz 3* akkor kerül sérült, létrehozhat egy letiltott egyedi regisztrációs bejegyzést tanúsítványát. Ez visszavonja a hozzáférési *eszköz 3*, de továbbra is lehetővé teszi, hogy *eszköz 1* és *eszköz 2* regisztrálásához.