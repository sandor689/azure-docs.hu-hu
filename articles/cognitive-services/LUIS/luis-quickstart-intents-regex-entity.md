---
title: Oktatóanyag – LUIS-alkalmazás létrehozása reguláris kifejezésnek megfeleltetett adatok lekéréséhez – Azure | Microsoft Docs
description: Ebből az oktatóanyagból megtudhatja, hogyan hozhat létre egyszerű LUIS-alkalmazást szándékok és egy reguláris kifejezés használatával az adatok kinyerésére.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 994bd6f2a041e25d15c7e0b4a216952cec4101fa
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492823"
---
# <a name="tutorial-3-add-regular-expression-entity"></a>Oktatóanyag: 3. Reguláriskifejezés-entitás hozzáadása
Ebben az oktatóanyagban létrehoz egy alkalmazást, amely bemutatja, hogyan nyerhető ki konzisztensen formázott adat egy kimondott szövegből a **Reguláris kifejezés** entitás segítségével.


<!-- green checkmark -->
> [!div class="checklist"]
> * A reguláriskifejezés-entitások bemutatása 
> * LUIS-alkalmazás használata egy FindForm szándékot használó Emberi erőforrások (HR) tartományhoz
> * Reguláriskifejezés-entitás hozzáadása Űrlapszám kimondott szövegből való kinyerésére
> * Alkalmazás betanítása és közzététele
> * Alkalmazás végpontjának lekérdezése a LUIS által visszaadott JSON-válasz megtekintéséhez

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Előkészületek
Ha még nincs meg az Emberi erőforrások alkalmazása az [előre összeállított entitások](luis-tutorial-prebuilt-intents-entities.md) oktatóanyagából, [importálja](luis-how-to-start-new-app.md#import-new-app) a [LUIS-minták](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-prebuilts-HumanResources.json) GitHub-adattárból a JSON-t egy új alkalmazásba a [LUIS](luis-reference-regions.md#luis-website) webhelyén.

Ha meg szeretné tartani az eredeti Emberi erőforrások alkalmazást, klónozza a [Settings](luis-how-to-manage-versions.md#clone-a-version) (Beállítások) lapon a verziót, és adja neki a következő nevet: `regex`. A klónozás nagyszerű mód, hogy kísérletezhessen a különböző LUIS-funkciókkal anélkül, hogy az az eredeti verzióra hatással lenne. 


## <a name="purpose-of-the-regular-expression-entity"></a>A reguláriskifejezés-entitás célja
Az entitások célja a kimondott szövegekben található fontos adatok kinyerése. Az alkalmazás arra használja a reguláriskifejezés-entitást, hogy formázott Emberierőforrások- (HR-) űrlapszámokat nyerjen ki egy kimondott szövegből. Ez nem gépi tanulással történik. 

Egyszerű példák kimondott szövegekre:

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
```

A kimondott szövegek rövidített vagy szlenges verziói:

```
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
A reguláriskifejezés-entitás, amelyet meg kell feleltetni az űrlapszámnak: `hrf-[0-9]{6}`. Ez a reguláriskifejezés-entitás megfelelteti egymásnak a `hrf -` karaktereket, de figyelmen kívül hagyja, hogy kis- vagy nagybetűvel szerepelnek-e, illetve a kulturális változatokat. Megfelelteti egymásnak a számjegyeket 0-tól 9-ig, pontosan 6 számjegyet.

A HRF az emberierőforrások-űrlapot jelöli.

### <a name="tokenization-with-hyphens"></a>Lexikális elemzés kötőjelekkel
A LUIS elvégzi a kimondott szöveg lexikális elemzését, ha a kimondott szöveg hozzá van adva egy leképezéshez. Ezen kimondott szövegek lexikális elemzése szóközöket ad hozzá a kötőjel előtt és után, `Where is HRF - 123456?` A reguláris kifejezést a rendszer nyers formájában alkalmazza a kimondott szövegre, még a lexikális elemzés előtt. Mivel még a _nyers_ űrlapra van alkalmazva, a reguláris kifejezésnek nem kell foglalkoznia a szóhatárokkal. 


## <a name="add-findform-intent"></a>FindForm szándék hozzáadása

1. Győződjön meg arról, hogy az Emberi erőforrások alkalmazás a LUIS **Build** (Létrehozás) szakaszában van. Ha erre a szakaszra szeretne lépni, válassza a jobb felső menüsávon a **Build** (Létrehozás) elemet. 

2. Válassza a **Create new intent** (Új szándék létrehozása) lehetőséget. 

3. Az előugró párbeszédpanelen írja be a `FindForm` karakterláncot, majd válassza a **Kész** elemet. 

    ![Képernyőkép a Create new intent (Új szándék létrehozása) párbeszédpanelről, a keresőmezőben a Utilities (Segédprogramok) karaktersorral](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. Adjon hozzá kimondott példaszövegeket a szándékhoz.

    |Példák kimondott szövegekre|
    |--|
    |Mi a hrf-123456 URL-címe?|
    |Hol van a hrf-345678?|
    |Mikor frissült a hrf-456098?|
    |Frissítette Kovács János múlt héten a hrf-234639 űrlapot?|
    |Hány verziója van a hrf-345123 űrlapnak?|
    |Kinek kell jóváhagynia a hrf-123456 űrlapot?|
    |Hány személynek kell jóváhagynia a hrf-345678 űrlapot?|
    |A hrf-234123 dátuma?|
    |A hrf-546234 szerzője?|
    |A hrf-456234 címe?|

    [ ![Képernyőkép a Szándék lapról, az új kimondott szövegek kiemelésével](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    Az alkalmazásban fel lettek véve előre összeállított számentitások az előző oktatóanyagban, ezért minden űrlapszám meg van címkézve. Ez elég lehet az ügyfélalkalmazásához, de a szám nem lesz megcímkézve a szám típusával. Egy új entitás létrehozása a megfelelő névvel lehetővé teszi, hogy az ügyfélalkalmazás megfelelően dolgozza fel az entitást, amikor visszakapja a LUIS-ból.

## <a name="create-an-hrf-number-regular-expression-entity"></a>HRF-szám reguláriskifejezés-entitásának létrehozása 
A következő lépésekkel hozzon létre egy reguláriskifejezés-entitást, amely elmagyarázza a LUIS-nak, mi az a HRF-számformátum:

1. Válassza az **Entities** (Entitások) elemet a bal oldali ablaktáblán.

2. Az Entities (Entitások) lapon válassza a **Create new entity** (Új entitás létrehozása) gombot. 

3. A felugró párbeszédpanelen az új entitásnak adja a `HRF-number` nevet, válassza a **RegEx** entitástípust, a Regex értéke legyen `hrf-[0-9]{6}`, majd válassza a **Done** (Kész) lehetőséget.

    ![Képernyőkép az új entitás tulajdonságainak beállításakor felugró párbeszédpanelről](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Válassza az **Intents** (Szándékok) elemet, majd a **FindForm** szándékot kiválasztva megtekintheti a kimondott szövegekben a megcímkézett reguláris kifejezést. 

    [![Képernyőkép a Label (Címke) kimondott szövegről létező entitás és regex mintával](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Mivel az entitás nem gépi tanulással létrejött entitás, a címkét a rendszer a létrehozása után azonnal alkalmazza a kimondott szövegre és megjeleníti a LUIS webhelyén.

## <a name="train-the-luis-app"></a>A LUIS-alkalmazás betanítása

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Az alkalmazás közzététele a végpont URL-címének lekéréshez

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>A végpont lekérdezése egy másik kimondott szöveggel

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Lépjen az URL-cím végéhez, és írja be a következőt: `When were HRF-123456 and hrf-234567 published in the last year?`. Az utolsó lekérdezésisztring-paraméter `q`, a kimondott szöveg pedig a **query**. A kimondott szöveg nem egyezik meg egyik címkézett kimondott szöveggel sem, ezért tesztnek pont megfelelő, és `FindForm` szándékot kell visszaadnia `HRF-123456` és `hrf-234567` űrlapszámmal.

    ```
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9993477
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9993477
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0206110049
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00533067342
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.004215215
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00209096959
        },
        {
          "intent": "None",
          "score": 0.0017655947
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00109490135
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.0005704638
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.000525338168
        }
      ],
      "entities": [
        {
          "entity": "last year",
          "type": "builtin.datetimeV2.daterange",
          "startIndex": 53,
          "endIndex": 61,
          "resolution": {
            "values": [
              {
                "timex": "2017",
                "type": "daterange",
                "start": "2017-01-01",
                "end": "2018-01-01"
              }
            ]
          }
        },
        {
          "entity": "hrf-123456",
          "type": "HRF-number",
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "hrf-234567",
          "type": "HRF-number",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        },
        {
          "entity": "-234567",
          "type": "builtin.number",
          "startIndex": 28,
          "endIndex": 34,
          "resolution": {
            "value": "-234567"
          }
        }
      ]
    }
    ```

    A kimondott szövegben szereplő számokat a rendszer kétszer adja vissza, egyszer az új `hrf-number` entitásként, egyszer pedig az előre létrehozott `number` entitásként. Egy kimondott szöveghez tartozhat több entitás, és több azonos típusú entitás is, ahogy az ebből a példából is látszik. Ha reguláriskifejezés-entitást használ, a LUIS kinyeri a megnevezett adatokat, ami programozás szempontjából nagyobb segítséget nyújt a JSON-választ fogadó ügyfélalkalmazásnak.

## <a name="what-has-this-luis-app-accomplished"></a>Milyen műveleteket végzett el a LUIS-alkalmazás?
Ez az alkalmazás azonosította a szándékot és visszaadta a kinyert adatokat. 

A csevegőrobot már elég információval rendelkezik az elsődleges művelet (`FindForm`) és a keresésben szereplő űrlapszámok meghatározásához. 

## <a name="where-is-this-luis-data-used"></a>Hol vannak használatban ezek a LUIS-adatok? 
A LUIS végzett ezzel a kéréssel. A hívó alkalmazás, például egy csevegőrobot felhasználhatja a topScoringIntent eredményt és az űrlapszámokat, és kereshet egy harmadik fél API-t. Ezt nem végzi el a LUIS. A LUIS csak azt határozza meg, hogy mi a felhasználó szándéka, és kinyeri a szándék alapján az adatokat. 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [Tudnivalók a listaentitásról](luis-quickstart-intent-and-list-entity.md)

