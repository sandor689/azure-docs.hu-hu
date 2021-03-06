---
title: Az Azure Blob fájlrendszer illesztőprogramja Azure Data Lake Storage Gen2 előzetes verzió
description: A Hadoop-fájlrendszer ABFS illesztőprogram
services: storage
keywords: ''
author: jamesbak
ms.topic: article
ms.author: jamesbak
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 8be6df5f4098b8a97e41c73edc5664799fd3edbe
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520819"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>Az Azure Blob fájlrendszer illesztőprogram (ABFS): egy dedikált Azure Storage-illesztőprogram a Hadoophoz

Az egyik az elsődleges elérési módszerek az adatok Azure Data Lake Storage Gen2 előzetes verziója a [Hadoop-fájlrendszer](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html). Az Azure Data Lake Storage Gen2 funkciói egy társított illesztőprogram, az Azure Blob fájlrendszerének illesztőprogram vagy `ABFS`. ABFS részét képezi az Apache Hadoop, és számos, a Hadoop kereskedelmi disztribúciók tartalmazza. Használja az illesztőprogramot, számos alkalmazásokat és keretrendszereket nélkül érhetnek el a Data Lake Storage Gen2 adatok bármilyen kód explicit módon a Data Lake Storage Gen2 szolgáltatásra hivatkozik.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Előzetes funkció: A Windows Azure Storage-Blobba illesztőprogram

A Windows Azure Storage-Blobba illesztőprogram vagy [WASB illesztőprogram](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) megadott eredeti támogatást biztosít az Azure Storage-blobokat. Az illesztőprogram végre (mint a Hadoop-fájlrendszer felület által előírt) szemantikáját, hogy az objektum, amely tárolja a stílus felület az Azure Blob Storage által elérhetővé tett leképezés fájlrendszer összetett feladat. Az illesztőprogram továbbra is támogatja a nagy teljesítményű, blobokban tárolt adatokat villámgyors ebben a modellben, de jelentős részét a leképezést, így nehéz végrehajtása kódot tartalmaz. Emellett bizonyos műveleteket, mint például [FileSystem.rename()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) és [FileSystem.delete()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive) címtárakhoz alkalmazásakor hatalmas számos művelet (mert az objektum tárolók nem végrehajtásához az illesztőprogram megkövetelése könyvtárak támogatása), amely gyakran vezet a teljesítmény csökkenését. Az új Azure Data Lake Storage szolgáltatás úgy lett kialakítva, hogy a WASB rejlő hiányosságait.

## <a name="the-azure-blob-file-system-driver"></a>Az Azure Blob fájlrendszer illesztőprogramja

A [Azure Data Lake Storage REST-felület](https://docs.microsoft.com/en-us/rest/api/storageservices/data-lake-storage-gen2) keresztül az Azure Blob Storage támogatja a fájlrendszer szemantikáját. Tekintve, hogy a Hadoop-fájlrendszer is célja, hogy támogatja az azonos esetében nem követelmény az illesztőprogram egy összetett leképezés. Így az Azure Blob fájlrendszerének illesztőprogram (vagy a ABFS) egy puszta ügyfél segédkód a REST API-hoz.

Vannak azonban bizonyos funkciók, amely az illesztőprogram továbbra is el kell végeznie:

### <a name="uri-scheme-to-reference-data"></a>Referenciaadatok URI-séma

Más fájlrendszer-implementációkban Hadoop belül konzisztensek az ABFS illesztőprogram határozza meg a saját URI-séma, hogy az erőforrások (fájlok és könyvtárak) jelszórészek címezhetők. Az URI-séma leírása itt található [használata az Azure Data Lake Storage Gen2 URI](./introduction-abfs-uri.md). Az URI-t struktúráját a következő: `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

A fenti URI-formátum használata esetén standard Hadoop-eszközök és keretrendszerek használhatók ezek erőforrásokra kell hivatkoznia:

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data 
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/ 
```

Belsőleg az ABFS illesztőprogram fordítja le a fájlokat és könyvtárakat az URI Azonosítóban megadott erőforrás(ok), és az Azure Data Lake Storage REST API ezeket a hivatkozásokat-hívást hajt végre.

### <a name="authentication"></a>Hitelesítés

A ABFS illesztőprogram megosztott kulcsos hitelesítés jelenleg támogatja, így előfordulhat, hogy a Hadoop-alkalmazásokat biztonságosan elérheti a Data Lake Storage Gen2 bennük foglalt erőforrás. A kulcs titkosítva, és a Hadoop konfigurációs tárolja.

### <a name="configuration"></a>Konfiguráció

Minden konfiguráció a ABFS illesztőprogram tárolja a <code>core-site.xml</code> konfigurációs fájlt. Hadoop-disztribúciókon keresőmotorját [Ambari](http://ambari.apache.org/), is felügyelheti a konfigurációt a webes portálon vagy az Ambari REST API használatával.

Az összes támogatott konfigurációs részletek vannak megadva a [hivatalos Hadoop-dokumentáció](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

### <a name="hadoop-documentation"></a>Hadoop-dokumentáció

A ABFS illesztőprogram teljes leírása itt található a [hivatalos Hadoop-dokumentáció](http://hadoop.apache.org/docs/current/hadoop-azure/index.html)

## <a name="next-steps"></a>További lépések

- [HDInsight-fürtök beállítása](./quickstart-create-connect-hdi-cluster.md)
- [Az Azure Databricks-fürt létrehozása](./quickstart-create-databricks-account.md)
- [Használja az Azure Data Lake Storage Gen2 URI](./introduction-abfs-uri.md)
