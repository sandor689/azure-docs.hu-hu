---
title: Az Azure Data Lake Storage Gen2 előzetes bemutatása
description: Az Azure Data Lake Storage Gen2 előzetes verzió nyújt áttekintést
services: storage
author: jamesbak
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 51f38cf7ade01b58ad5ce7925af5546d1a4f1a0c
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525382"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Bevezetés az Azure Data Lake Storage Gen2 előzetes verzió

Az Azure Data Lake Storage Gen2 előzetes verzió egy olyan dedikált big data-elemzés, a beépített funkciók [Azure Blob storage](../blobs/storage-blobs-introduction.md). Lehetővé teszi az adatok használata mindkét fájl rendszer és a objektum tárolási paradigmákat csatoló. Ez lehetővé teszi Data Lake Storage Gen2 a csak felhőalapú több modális tárolási szolgáltatás, amellyel analytics értéket nyerhet az összes adatát.

Data Lake Storage Gen2 elemzési adatok teljes életciklusát szükséges összes minőségű szolgáltatást. Ez a két meglévő tárolási szolgáltatásaink képességeit beépül eredménye. A szolgáltatások [Azure Data Lake Storage Gen1](../../data-lake-store/index.md), például a fájlrendszer szemantikáját, fájlszintű biztonsági és méretezési csoport alacsony költségű, többrétegű tárolást, a magas rendelkezésre állás és vész-helyreállítási funkciókat és a egy nagy SDK és eszközök együtt az ökoszisztéma [Azure Blob storage](../blobs/storage-blobs-introduction.md). A Data Lake Storage Gen2 objektumtárolás az összes minőség hozzáadása egy fájlrendszer felületen előnyei optimalizált elemzési számítási feladatok közben továbbra is.

## <a name="designed-for-enterprise-big-data-analytics"></a>Vállalati big data-elemzés tervezve

Data Lake Storage Gen2 egy data Lake tárolók (EDL) létrehozásához a vállalati eligazodást tárolási szolgáltatás az Azure-ban. Úgy alakítottuk ki elindítása közben az átviteli sebesség Gigabit több száz fenntartási információkat több petabájtnyi kiszolgálásához, Data Lake Storage Gen2 lehetővé teszi nagy mennyiségű adatot egyszerűen.

A Data Lake Storage Gen2 alapvető jellemzője, igény szerinti hozzáadásával egy [hierarchikus névtér](./namespace.md) objektumok vagy fájlokat rendszerezi egy nagy teljesítményű adatelérés könyvtár-hierarchia, amely Blob storage szolgáltatásra. A hierarchikus névtér azt is lehetővé teszi, hogy a Data Lake Storage Gen2 mindkét objektumtároló támogatják, és a egy időben a rendszer paradigmákat fájl. Például egy közös tároló elnevezési konvenciót objektum használja perjeleket a név egy hierarchikus mappastruktúra referenciaszámítógépnek. Ez a struktúra válik a Data Lake Storage Gen2 valós. Műveletek, például a átnevezése vagy törlése egy könyvtár egyetlen elemi metaadat-művelet a címtár helyett enumerálása és annak a könyvtárnak a nevét előtagot használó összes objektum feldolgozása a válnak.

Múltbeli időpont felhőalapú elemzési kellett veszélyeztetheti a teljesítmény, a felügyelet és biztonság területéhez. Data Lake Storage Gen2-címek ezeket a szempontokat mindegyike a következő módon:

- **Teljesítmény** van optimalizálva, mert nem kell másolnia, vagy az adatok átalakítása elemzés előfeltételeként. A hierarchikus névtér nagy mértékben javítja a könyvtár-felügyeleti műveleteket, ami javítja az általános feladat teljesítményt.

- **Felügyeleti** azért egyszerűbben rendszerezheti és a fájlkezelő könyvtárak és alkönyvtárak keresztül.

- **Költséghatékonysága** lehetséges legyen, a Data Lake Storage Gen2 épül az alacsony költségű [Azure Blob storage](../blobs/storage-blobs-introduction.md). A további szolgáltatások tovább csökkentheti a teljes tulajdonosi költség, a big data-elemzés futtatása az Azure-ban.

## <a name="key-features-of-data-lake-storage-gen2"></a>Data Lake Storage Gen2 főbb funkciói

> [!NOTE]
> A Data Lake Storage Gen2 a nyilvános előzetes verzióban az alább felsorolt funkciók némelyike változhat, a rendelkezésre állásuk. Új szolgáltatások és régiók az előzetes programhoz során kiadott, ezeket az adatokat közölni kell.
> [Regisztráció](https://aka.ms/adlsgen2signup) a Data Lake Storage Gen2 a nyilvános előzetesét.  

- **Hadoop-kompatibilis hozzáférés**: kezelését és az adatok eléréséhez, ugyanúgy, mint a Data Lake Storage Gen2 lehetővé teszi egy [Hadoop elosztott fájlrendszer (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Az új [ABFS illesztőprogram](./abfs-driver.md) elérhető összes Apache Hadoop-környezetekben, beleértve a [Azure HDInsight](../../hdinsight/index.yml) és [Azure Databricks](../../azure-databricks/index.yml) Data Lake Storage-ban tárolt adatok elérését Gen2.

- **Több protokoll és a többszörös modális adatelérési**: Data Lake Storage Gen2 számít egy **több modális** , mert a storage szolgáltatás biztosít objektumtároló és a fájl ugyanazokat az adatokat a rendszer felületek **egyszerre idő**. Ez azáltal, hogy több protokollvégpontokat olyan ugyanazokkal az adatokkal érhető el. 

    Ellentétben más elemzési megoldásokkal Data Lake Storage Gen2-ban tárolt adatok nem kell áthelyezni vagy lesz átalakítva, elemzési eszközök különböző futtatása előtt. Hagyományos keresztül az adatok eléréséhez [a Blob storage API-k](../blobs/storage-blobs-introduction.md) (például: keresztül adatokat [Event Hubs Capture](../../event-hubs/event-hubs-capture-enable-through-portal.md)) és a egy időben HDInsight vagy Azure Databricks használatával adatokat feldolgozni. 

- **Költséghatékony**: Data Lake Storage Gen2 funkciói költséghatékony tárolási kapacitás és a tranzakciók. A teljes körű életciklus keresztül adatok transitions lehetőségnél szerint díjszabása nem módosul módosítja a megtartja költségeket a beépített funkciók használatával minimális például [Azure Blob storage életciklus](../common/storage-lifecycle-managment-concepts.md).

- **A Blob storage-eszközökkel, keretrendszerek és alkalmazások működik**: Data Lake Storage Gen2 továbbra is az eszközöket, keretrendszerek és alkalmazások a Blob Storage jelenleg létező széles választékának működik.

- **Optimalizált illesztőprogram**: A `abfs` illesztőprogram [kifejezetten optimalizált](./abfs-driver.md) big data-elemzőeszközöket. A megfelelő REST API-k végzetesnek a `dfs` végpont, `dfs.core.windows.net`.

## <a name="scalability"></a>Méretezhetőség

Az Azure Storage szolgáltatás elvárt méretezhető, Data Lake Storage Gen2- vagy Blob storage felületéről férhet hozzá. Képes tárolni és *több exabájt adat*. Szükséges tárhelyet a bemeneti/kimeneti műveletek száma másodpercenként (IOPS) magas szintű (GB/s) mért átviteli sebesség érhető el. Csak adatmegőrzés túli feldolgozási kérésenkénti közel állandó késések, amelyek mérése történik, a szolgáltatás, a fiók és a fáj szinteken lévő kerülnek végrehajtásra.

## <a name="cost-effectiveness"></a>Költséghatékonyság

Létrehozásához, Data Lake Storage Gen2 épülő Azure Blob storage számos előnyének egyike a [alacsony költségű](https://azure.microsoft.com/pricing/details/storage) tárolási kapacitás és a tranzakciók. Ellentétben más felhőalapú tárolási szolgáltatásokról Data Lake Storage Gen2 kínál a költségek csökkentésére mert adatok nem helyezhető át vagy elemzése előtt át kell.

Továbbá szolgáltatások, például a a [hierarchikus névtér](./namespace.md) jelentősen növelhető az analytics-feladatok általános teljesítményét. Ez a teljesítmény fokozása azt jelenti, hogy megkövetelése kevesebb számítási teljesítmény az azonos mennyiségű adatot fel, ami a teljes körű analytics-feladat egy alacsonyabb teljes birtoklási költség.

## <a name="next-steps"></a>További lépések

Az alábbi cikkek ismertetik azokat a Data Lake Storage Gen2 és részletes főbb fogalmakat tárolása, elérése, kezelése és a data-betekintések:

* [Hierarchikus névtér](./namespace.md)
* [Tárfiók létrehozása](./quickstart-create-account.md)
* [Hozzon létre egy HDInsight-fürtön az Azure Data Lake Storage Gen2](./quickstart-create-connect-hdi-cluster.md)
* [Egy Azure Data Lake Storage Gen2-fiók használata az Azure Databricksben](./quickstart-create-databricks-account.md) 