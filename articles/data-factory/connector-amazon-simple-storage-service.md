---
title: Adatok másolása az Amazon egyszerű Társzolgáltatás Azure Data Factory használatával |} Microsoft Docs
description: További tudnivalók az adatok másolása az Amazon egyszerű tároló szolgáltatás (S3) támogatott fogadó adattárolókhoz Azure Data Factory használatával.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: jingwang
ms.openlocfilehash: 3635e8bf1d9ba4061da5b8f416a3b755f7064000
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045636"
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Adatok másolása az Amazon egyszerű Társzolgáltatás Azure Data Factory használatával
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [1-es verziójával](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Aktuális verzió](connector-amazon-simple-storage-service.md)

Ez a cikk ismerteti, hogyan használható a másolási tevékenység során az Azure Data Factory adatok másolása az Amazon S3. Buildekről nyújtanak a [másolása tevékenység áttekintése](copy-activity-overview.md) cikket, amely megadja a másolási tevékenység általános áttekintést.

## <a name="supported-capabilities"></a>Támogatott képességei

Adatok Amazon S3 bármely támogatott fogadó adattár másolhatja. A másolási tevékenység által támogatott adatforrások vagy mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](copy-activity-overview.md#supported-data-stores-and-formats) tábla.

Pontosabban, az Amazon S3 összekötő támogatja-e a fájlok másolása,-, vagy a fájlok elemzése a [támogatott formátumok és a tömörítési kodek](supported-file-formats-and-compression-codecs.md).

## <a name="required-permissions"></a>Szükséges engedélyek

Adatok másolása az Amazon S3, győződjön meg arról, hogy rendelkezik a következő engedélyekkel:

- `s3:GetObject` és `s3:GetObjectVersion` Amazon S3 objektum műveletekhez.
- `s3:ListBucket` vagy `s3:GetBucketLocation` Amazon S3 gyűjtő műveletekhez. A Data Factory másolása varázsló használata `s3:ListAllMyBuckets` is szükség.

A teljes listát az Amazon S3 engedélyekkel kapcsolatos részletekért lásd: [megadása engedélyeket egy házirendben](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Első lépések

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

A következő szakaszok részletesen bemutatják a Data Factory tartozó entitások az Amazon S3 megadása használt tulajdonságokat.

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok

Amazon S3 kapcsolódó szolgáltatás támogatott a következő tulajdonságokkal:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A type tulajdonságot meg kell **AmazonS3**. | Igen |
| accessKeyId | A titkos hívóbetű azonosítója. |Igen |
| secretAccessKey | A titkos hívóbetű magát. Ez a mező megjelölése a SecureString tárolja biztonságos helyen, a Data factoryban vagy [hivatkozik az Azure Key Vault tárolt titkos kulcs](store-credentials-in-key-vault.md). |Igen |
| connectVia | A [integrációs futásidejű](concepts-integration-runtime.md) csatlakozni az adattárolóhoz használandó. Használhat Azure integrációs futásidejű vagy Self-hosted integrációs futásidejű (amennyiben az adattároló magánhálózaton található). Ha nincs megadva, akkor használja az alapértelmezett Azure integrációs futásidejű. |Nem |

>[!NOTE]
>Ennél az összekötőnél követelmény az adatok másolása az Amazon S3 IAM-fiók hozzáférési kulcsainak listázása. [Ideiglenes biztonsági hitelesítő adatok](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) nem támogatott.
>

Például:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AmazonS3",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                    "type": "SecureString",
                    "value": "<secret access key>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai

Szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listájáért tekintse meg az adatkészletek cikket. Ez a témakör az Amazon S3 dataset által támogatott tulajdonságokról.

Adatok másolása az Amazon S3, állítsa be a type tulajdonságot az adathalmaz **AmazonS3Object**. A következő tulajdonságok támogatottak:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A type tulajdonságot az adathalmaz értékre kell állítani: **AmazonS3Object** |Igen |
| bucketName | S3 gyűjtő neve. Helyettesítő karakter szűrő nem támogatott. |Igen |
| kulcs | A **nevét vagy helyettesítő karakter szűrő** S3 objektum kulcs alatt a megadott gyűjtőjét. Érvényes, csak ha "előtag" tulajdonság nincs megadva. <br/><br/>A helyettesítő karakter szűrő csak a fájl neve része, de nem mappa rész támogatott. Helyettesítő karakterek engedélyezett: `*` (nulla vagy több karakter megegyezik) és `?` (nulla megegyezik vagy önálló karakter).<br/>-1. példa: `"key": "rootfolder/subfolder/*.csv"`<br/>– 2. példa: `"key": "rootfolder/subfolder/???20180427.txt"`<br/>Használjon `^` -e a tényleges fájlnév helyettesítő vagy a escape karaktere belül karaktert. |Nem |
| előtag | S3 objektum kulcshoz előtag. Kiválasztott objektumok, amelynek kulcsait a előtaggal kezdődik. Csak akkor, ha a "key" tulajdonság nincs megadva érvényes. |Nem |
| verzió: | A S3 objektum, ha engedélyezve van a S3 versioning verziója. |Nem |
| Formátumban | Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.<br/><br/>Szeretne elemezni, vagy egy adott formátumú fájlok létrehozása, ha a következő fájl formátuma típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét. További információkért lásd: [szövegformátum](supported-file-formats-and-compression-codecs.md#text-format), [Json formátumban](supported-file-formats-and-compression-codecs.md#json-format), [az Avro formátum](supported-file-formats-and-compression-codecs.md#avro-format), [Orc formátum](supported-file-formats-and-compression-codecs.md#orc-format), és [Parquet formátum](supported-file-formats-and-compression-codecs.md#parquet-format) szakaszok. |Nem (csak a bináris másolásának esetéhez) |
| tömörítés | Adja meg a típus és az adatok tömörítése szintjét. További információkért lásd: [támogatott formátumok és a tömörítési kodek](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.<br/>Támogatott szintek a következők: **Optimal** és **leggyorsabb**. |Nem |

>[!TIP]
>Másolja az összes fájlt egy mappában, adja meg a **bucketName** gyűjtő és **előtag** mappa részére.<br>Adja meg a megadott nevű egyetlen fájl másolásához **bucketName** gyűjtő és **kulcs** rész plusz fájlba mappanév.<br>Másol egy mappát a fájlok egy részét, adja meg a **bucketName** gyűjtő és **kulcs** mappa része, és helyettesítő karakteres szűrőhöz.

**Példa: előtag használatával**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3Object",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "prefix": "testFolder/test",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

**Példa: használatával kulcs és a verziója (nem kötelező)**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "key": "testFolder/testfile.csv.gz",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai

Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [folyamatok](concepts-pipelines-activities.md) cikk. Ez a témakör az Amazon S3 forrás által támogatott tulajdonságokról.

### <a name="amazon-s3-as-source"></a>Amazon S3 forrásaként

Adatok másolása az Amazon S3, állítsa be a forrás típusa a másolási tevékenység **FileSystemSource** (amely tartalmazza az Amazon S3). A következő tulajdonságok támogatottak a másolási tevékenység **forrás** szakasz:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A type tulajdonságot a másolási tevékenység forrás értékre kell állítani: **FileSystemSource** |Igen |
| rekurzív | Azt jelzi, hogy az adatok olvasható rekurzív módon az almappák vagy csak a megadott mappát. Megjegyzés: Ha a rekurzív értéke true, és a fogadó fájlalapú tároló, üres mappa/alterület-folder nem lesz másolva vagy hozható létre a fogadó.<br/>Két érték engedélyezett: **igaz** (alapértelmezett), **hamis** | Nem |

**Példa**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Amazon S3 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```
## <a name="next-steps"></a>További lépések
Támogatott források és mosdók által a másolási tevékenység során az Azure Data Factory adattárolókhoz listájáért lásd: [adattárolókhoz támogatott](copy-activity-overview.md##supported-data-stores-and-formats).
