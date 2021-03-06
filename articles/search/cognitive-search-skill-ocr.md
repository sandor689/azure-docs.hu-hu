---
title: OCR kognitív keresési szakértelem (Azure Search) |} Microsoft Docs
description: Szöveg kinyerése képfájlok egy Azure Search dúsító folyamat.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 478afe81ed739b98487973eb092ee9cad0aa17fd
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059080"
---
# <a name="ocr-cognitive-skill"></a>OCR kognitív szakértelem

A **OCR** szakértelem szöveg kiolvassa a képfájlok. Támogatott formátumok a következők:

+ . JPEG
+ . JPG
+ . PNG
+ . BMP
+ . GIF


## <a name="skill-parameters"></a>Szakértelem paraméterek

A paraméterei a kis-és nagybetűket.

| Paraméter neve     | Leírás |
|--------------------|-------------|
| detectOrientation | Lehetővé teszi, hogy a kép tájolása autodetection. <br/> Érvényes értékek: true / false.|
|defaultLanguageCode | <p>  A bemeneti szöveg nyelvkódot. A támogatott nyelvek közé tartoznak a következők: <br/> zh-Hans (ChineseSimplified) <br/> zh-Hant (ChineseTraditional) <br/>cs (Cseh) <br/>da (dán) <br/>NL (holland) <br/>en (angol nyelven) <br/>Fi (finn)  <br/>FR (magyar) <br/>  Németország (német) <br/>el (görög) <br/> hu (magyar) <br/> az (olasz) <br/>  japán (japán) <br/> ko (koreai) <br/> NB (norvég) <br/>   pl (lengyel) <br/> PT (portugál) <br/>  RU (orosz) <br/>  es (spanyol) <br/>  SV (svéd) <br/>  tr (török) <br/> ar (arab) <br/> ro (román) <br/> az SR-Cyrl (SerbianCyrillic) <br/> az SR-Latn (SerbianLatin) <br/>  SK (szlovák). <br/>  (ismeretlen) UNK <br/><br/> Ha a nyelvi kód meghatározatlan vagy null, a nyelv a hálózati kártya automatikus észlelése. </p> |
| textExtractionAlgorithm | "nyomtatott" vagy "kézzel". A "kézzel" szöveget felismerés OCR algoritmus jelenleg előzetes verzióban érhetők, és csak akkor támogatott a angol. |

## <a name="skill-inputs"></a>Szakértelem bemenetek

| Bemeneti név      | Leírás                                          |
|---------------|------------------------------------------------------|
| image         | Összetett típus. Az Azure Blob indexelő által előállított jelenleg csak akkor működik a "/ dokumentum/normalized_images" mező, amikor ```imageAction``` értéke ```generateNormalizedImages```. Tekintse meg a [minta](#sample-output) további információt.|


## <a name="skill-outputs"></a>Szakértelem kimenetek
| Kimeneti neve     | Leírás                   |
|---------------|-------------------------------|
| szöveg          | A kép kinyert egyszerű szöveg.   |
| layoutText    | Összetett típus, amely leírja a kibontott szöveg, valamint a helyet, ahol a szöveg található.|


## <a name="sample-definition"></a>A minta meghatározása

```json
{
    "skills": [
      {
        "description": "Extracts text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": null,
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "myText"
          },
          {
            "name": "layoutText",
            "targetName": "myLayoutText"
          }
        ]
      }
    ]
 }
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>Szöveg- és layoutText kimenetpélda

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Példa: A dokumentum tartalma beágyazott képekhez kinyert szöveg egyesítése.

A szöveg egyesülés gyakori használati eset azt a képességet a képek (egy OCR szakértelem, vagy a kép felirat szöveg) képviselő szöveges alakot egyesítése a tartalom mezőbe egy dokumentum. 

A következő példa skillset létrehoz egy *merged_text* mező a szöveges tartalom a dokumentum, valamint a lemezképek mindegyikének OCRed szöveget tartalmazó beágyazott dokumentum. 

#### <a name="request-body-syntax"></a>Kéréstörzs szintaxisa
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```
A fenti skillset példa feltételezi, hogy van-e a normalizált képek mező. Ez a mező létrehozásához állítsa be a *imageAction* konfigurációját az indexelő-definíciót a *generateNormalizedImages* alább látható módon:

```json
{  
   //...rest of your indexer definition goes here ... 
  "parameters":{  
      "configuration":{  
         "dataToExtract":"contentAndMetadata",
         "imageAction":"generateNormalizedImages"
      }
   }
}
```

## <a name="see-also"></a>Lásd még
+ [Előre definiált képességek](cognitive-search-predefined-skills.md)
+ [TextMerger szakértelem](cognitive-search-skill-textmerger.md)
+ [Egy skillset definiálása](cognitive-search-defining-skillset.md)
+ [Az indexelő (REST) létrehozása](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
