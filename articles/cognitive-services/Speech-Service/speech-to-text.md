---
title: Tudnivalók a beszédfelismerés
description: A Speech to Text API-funkcióinak áttekintése.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: ba6710c8b5b8de1c63fa6778ea3853ab52365254
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325336"
---
# <a name="about-the-speech-to-text-api"></a>Tudnivalók a Speech to Text API

A **Speech to Text** API *transcribes* audiostreamek lejátszásával, szöveg, amely az alkalmazás a felhasználót, vagy mint való reagáláshoz bemeneti parancsot. Az API-k is használható, az SDK ügyféloldali kódtár (a támogatott platformok és nyelvek) vagy a REST API-t.

A **Speech to Text** API a következő szolgáltatásokat nyújtja:

- A Microsoft beszédfelismerési technológiáját speciális – ugyanaz a Cortana, az Office és más Microsoft-termékek használják.

- Valós idejű folyamatos felismerése. **Beszédfelismerés** lehetővé teszi a felhasználóknak lefényképezze hangot szöveggé valós időben. Ezenkívül támogatja fogadása eddig felismerhető szavak köztes eredményeket. A szolgáltatás automatikusan észleli a speech végén. Felhasználók is beállíthatja a további formázási beállításokat, beleértve a kis-és nagybetűk és írásjelek, vulgáris maszkolás és más néven Inverz szöveg normalizálási.

- Optimalizált **Speech to Text** interaktív, beszélgetést, eredményeit és a Diktálás forgatókönyvek. 

- Számos használja a beszélt nyelv és Dialektusok támogatása. Minden egyes felismerés módban támogatott nyelvek teljes listáját lásd: [támogatott nyelvek](supported-languages.md#speech-to-text).

- Testreszabott nyelvi és akusztikai modell, így testre szabhatja a felhasználók speciális tartomány szöveg szóhasználati, hogy az alkalmazás, és beszéljen környezetben és különféle módja.

- Beszédfelismerés. Integráció révén [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), akkor származtathatók szándékok és entitások speech. A felhasználók nem szükséges ismerni az alkalmazás szöveg szóhasználati, de tud adni, amit szeretnének a saját szavaival.

## <a name="api-capabilities"></a>API-funkciók

Bizonyos képességeit a **Speech to Text** API REST-en keresztül nem érhetők el. Az alábbi táblázat foglalja össze az egyes módszerek az API eléréséhez képességeit.

| Használati eset | REST | SDK-k |
|-----|-----|-----|----|
| Lefényképezze az utterance (kifejezés egy rövid), például a parancs (hossz < 15 s). Nincs köztes eredményeket | Igen | Igen |
| A hosszabb utterance (> 15 mp) alapuló átírás | Nem | Igen |
| Streamelési audio-választható köztes eredményeket alapuló átírás | Nem | Igen |
| Hangszóró leképezések a LUIS-n keresztül ismertetése | nem\* | Igen |

\* *A LUIS szándékok és entitások egy külön LUIS-előfizetése lehet nyerni. Ennek az előfizetésnek az SDK a LUIS hívja meg, és adja meg az entitás- és leképezés eredményeket, valamint a speech beszédátírás. A LUIS hívása a REST API-val saját magának, hogy szándékokat és entitásokat a LUIS-előfizetéshez.*

## <a name="next-steps"></a>További lépések

* [Próbaverziós Speech-előfizetés beszerzése](https://azure.microsoft.com/try/cognitive-services/)
* [Gyors útmutató: beszédfelismerést a C#-ban](quickstart-csharp-dotnet-windows.md)
* [Tekintse meg, hogyan ismerhetik fel a szándék fog vonatkozni, a beszéd, a C#-ban](how-to-recognize-intents-from-speech-csharp.md)
