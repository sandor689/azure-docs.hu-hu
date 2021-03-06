---
title: Tekintse át a végpont kimondott szöveg aktív tanulás használata a Language Understanding (LUIS) – Azure |} A Microsoft Docs
description: "\"Ellenőrizze a végpont kimondott szöveg\" nevű aktív tanulás szolgáltatással teljesítmény előrejelzések gyorsabb javítása érdekében."
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: diberry
ms.openlocfilehash: 05b3404d318359c6966df44bfab9baff3ded980f
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222613"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Aktív tanulás engedélyezése endpoint utterances áttekintésével
Aktív tanulás az előrejelzési pontosság és a legegyszerűbb megvalósításához három stratégia egyik. 

## <a name="what-is-active-learning"></a>Mi az aktív tanulás
Aktív tanulás két lépésből áll. Először LUIS választ kap az alkalmazás végponti érvényesítési igénylő kimondott szöveg. A második lépést az alkalmazás tulajdonos vagy közreműködő, érvényesíteni a kiválasztott megcímkézzen [tekintse át](luis-how-to-review-endoint-utt.md), beleértve a megfelelő cél és minden olyan entitások a célt. A kimondott szöveg áttekintése, után betanítása, és tegye közzé újra az alkalmazást. 

## <a name="which-utterances-are-on-the-review-list"></a>A felülvizsgálati listán vannak melyik kimondott szöveg
A LUIS ad hozzá a kimondott szöveg felülvizsgálati szándékot kiváltó felső alacsony pontszámmal rendelkezik, vagy a felső két szándék pontszámok túl kicsi. 

## <a name="single-pool-for-utterances-per-app"></a>Egyetlen készletet az egyes alkalmazások kimondott szöveg
A **tekintse át a végpont utterances** lista alapján a verzió nem változik. Nincs egyetlen készletét kimondott szöveg tekintse át, függetlenül attól, hogy mely verzióját az utterance (kifejezés) aktívan szerkesztésekor vagy az alkalmazás melyik verziója lett közzétéve a végponton. 

## <a name="where-are-the-utterances-from"></a>Hol találhatók a megcímkézzen
Végpont utterances végfelhasználói lekérdezések, az alkalmazás HTTP-végponton kell venni. Ha az alkalmazás nincs közzétéve, vagy nem kapott találatok még, nem rendelkezik megszólalásokat áttekintéséhez. Nincs végpont találatok érkezik egy adott szándékot vagy entitáshoz, ha nem rendelkezik a kimondott szöveg tekintse át azokat. 

## <a name="schedule-review-periodically"></a>Rendszeres időközönként ütemezés áttekintése
Tekintse át a javasolt utterances nem kell végrehajtani minden nap, de a LUIS rendszeres karbantartása részének kell lennie. 

## <a name="delete-review-items-programmatically"></a>Programozott módon felülvizsgálati elemek törlése
Ha az alkalmazás túl nagy, előfordulhat, hogy tekintse át az egyes kimondott szöveg és választja programozott módon törli a többi a listából. Ehhez először [első](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) a listában, majd [törlése](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) által a kimondott szöveg

## <a name="next-steps"></a>További lépések

* Ismerje meg, hogyan [tekintse át](luis-how-to-review-endoint-utt.md) végpont kimondott szöveg
