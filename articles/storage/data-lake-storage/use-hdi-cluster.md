---
title: Az Azure Data Lake Storage Gen2 előzetes verzió használata Azure HDInsight-fürtök
description: Ismerje meg, hogyan kérdezhet le adatokat az Azure Data Lake Storage Gen2 előzetes verzió, és tárolhatja az elemzések eredményeit.
keywords: hdfs, strukturált adatok, strukturálatlan adatok, a data lake store, Hadoop-bemenet, Hadoop-kimenet, hadoop-tárolás, hdfs-bemenet, hdfs-kimenet, hdfs-tárolás, wasb azure
services: hdinsight,storage
tags: azure-portal
author: jamesbak
ms.component: data-lake-storage-gen2
ms.service: storage
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: 4a9f79b292e58331dcd2f7cb656e24b244aa89ba
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39528508"
---
# <a name="use-azure-data-lake-storage-gen2-preview-with-azure-hdinsight-clusters"></a>Az Azure Data Lake Storage Gen2 előzetes verzió használata Azure HDInsight-fürtök

HDInsight-fürtben lévő adatok elemzéséhez, tárolhatja az adatokat az Azure Storage, az Azure Data Lake Storage Gen1 vagy az Azure Data Lake Storage Gen2 előzetes verzió tetszőleges kombinációját. Az összes tárolási lehetőség lehetővé, hogy biztonságosan törölje a felhasználói adatok elvesztése nélkül törölje a számításhoz használt HDInsight fürtöket.

A Hadoop támogatja az alapértelmezett fájlrendszert. Az alapértelmezett fájlrendszer egy alapértelmezett sémát és szolgáltatót is jelent. A relatív elérési utak feloldásához is használható. A HDInsight fürt létrehozása során blobtárolók megadhatja az Azure Storage vagy az Azure Data Lake Storage az alapértelmezett fájlrendszer. Másik lehetőségként a HDInsight 3.5, választhat Azure Storage vagy az Azure Data Lake Storage legyen az alapértelmezett fájlrendszer, néhány kivétellel.

Ebből a cikkből megtudhatja, hogyan Azure Data Lake Storage Gen2 használható a HDInsight-fürtökkel. Egy HDInsight-fürt létrehozásával kapcsolatos további információkért lásd: [állítsa be a HDInsight-fürtök az Azure Data Lake Storage használata a Hadoop, Spark, Kafka és további](quickstart-create-connect-hdi-cluster.md).

Az Azure Blob Storage egy robusztus, általános célú tárolómegoldás, amely zökkenőmentesen integrálható a HDInsight eszközzel. HDInsight fürthöz is használhatja az Azure Data Lake Storage az alapértelmezett fájlrendszerként a. A Hadoop elosztott fájlrendszer (HDFS) felületen keresztül a HDInsight összetevők teljes készlete működhet közvetlenül a fájlokat az Azure Data Lake Storage.

Nem javasoljuk, hogy üzleti adatokat tárolni használja az alapértelmezett fájlrendszer. Tárolási költségek csökkentése érdekében minden használat után érdemes az alapértelmezett fájlrendszer törlése folyamatban van. Vegye figyelembe, hogy az alapértelmezett tároló tartalmazza, az alkalmazás- és naplókat. A tároló törlése előtt gondoskodjon a naplók begyűjtéséről.

Egy fájlrendszer több fürt közötti megosztása nem támogatott.

## <a name="hdinsight-storage-architecture"></a>HDInsight tároló-architektúra

A következő ábra az Azure Storage-ot használó HDInsight tároló-architektúra absztrakt nézetét nyújtja:

![A Hadoop-fürtök a HDFS API-val érik el és tárolják a strukturált és strukturálatlan adatokat a Blob Storage-ban.](./media/use-hdi-cluster/HDI.ABFS.Arch.png "HDInsight-tárarchitektúra")

A HDInsight hozzáférést nyújt a helyileg a számítási csomópontokhoz csatlakozó elosztott fájlrendszerhez. Ez a fájlrendszer a teljes URI használatával érhető el, például:

    hdfs://<NAME_NODE_HOST>/<PATH>

Ezenfelül HDInsight lehetővé teszi, hogy az Azure Data Lake Storage-ban tárolt adatok elérésére. A szintaxis a következő:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<path>

Azure Storage-fiók és a HDInsight-fürtök együttes használatakor az alábbiakban néhány szempontot.

* **A fürthöz csatlakozó tárfiókokban lévő fájlok** van a fiók nevét és kulcsát a fürthöz társított létrehozása során. Ez a konfiguráció teljes hozzáférést biztosít fájlokat a fájlrendszerben.

* **A storage-fiókok fürthöz nem csatlakozó nyilvános fájlokat** tegye elérhetővé a csak olvasási engedéllyel a fájlokat a fájlrendszerben.
  
  > [!NOTE]
  > Nyilvános rendszerek lehetővé teszik, hogy minden fájl a fájlrendszerben elérhető listáját, és hozzáfér. Nyilvános rendszerek lehetővé teszik, hogy a fájlok elérése csak akkor, ha ismeri a pontos URL-címe. További információkért lásd: [tárolókhoz és blobokhoz való hozzáférés korlátozása](http://msdn.microsoft.com/library/windowsazure/dd179354.aspx) (tárolók és blobok szabályok működnek az azonos előterének fájlok és a fájlrendszerben).
 
* **A storage-fiókok fürthöz nem csatlakozó személyes fájlrendszerek** nem teszik lehetővé az access-fájlok a fájlrendszer, kivéve, ha a WebHCat-feladatok elküldésekor határozza meg a tárfiókot. Ez a korlátozás oka magyarázatát a cikk későbbi részében.

A storage-fiókok a létrehozási folyamat és a kulcsaik meghatározott tárolt *%HADOOP_HOME%/conf/core-site.xml* a fürtcsomópontokon. A HDInsight alapértelmezett viselkedését, hogy a meghatározott tárfiókok használata a *core-site.xml* fájlt. A beállítást az [Ambari](../../hdinsight/hdinsight-hadoop-manage-ambari.md) használatával módosíthatja

Több WebHCat-feladat (beleértve a Hive, MapReduce, Hadoop-stream és Pig-feladatokat) is tartalmazhatja a tárfiókok és a metaadatok leírását. (Ez a megközelítés jelenleg működik a Pig és a storage-fiókok, de a metaadatok nem.) További információkért lásd: [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx) (HDInsight-fürtök használata alternatív tárfiókokkal és metaadattárakkal).

## <a id="benefits"></a>Az Azure Storage előnyei

A számítási fürtök és tárolási erőforrások nem egy helyre helyezésével járó teljesítményköltségeket csökkenti a számítási fürtöknek a tárfiók erőforrásainak közelében való létrehozása az Azure-régión belül, ahol a nagysebességű hálózat hatékonnyá teszi a számítási csomópontok számára az Azure Storage-ban lévő adatok elérését.

Több előnye is van annak, ha az adatokat a HDFS helyett az Azure Blob Storage tárolóban tárolja:

* **Adatok újbóli használata és megosztása:** A HDFS-ben az adatok a számítási fürtön belül találhatóak. Csak a számítási fürtöt elérő alkalmazások használhatják az adatokat HDFS API-k használatával. Az Azure Blob Storage-ban lévő adatok a HDFS API-kon vagy a [Blob Storage REST API-kon][blob-storage-restAPI] keresztül érhetők el. Így az alkalmazások (beleértve más HDInsight fürtöket) és eszközök nagyobb készlete használható az adatok előállításához és fogyasztásához.

* **Adatarchiválás:** Az adatok Azure Blob Storage tárolóban végzett tárolása lehetővé teszi, hogy biztonságosan törölje a számításhoz használt HDInsight fürtöket a felhasználói adatok elvesztése nélkül.

* **Adattárolási költség:** adatok tárolására a natív HDFS-ben, a hosszú távú költségesebb, mint az adatok tárolása az Azure storage-ban, mert a számítási fürt költsége nagyobb, mint az Azure storage költsége. Ezenkívül mivel az adatokat nem kell újból betölteni minden számítási fürt létrehozásakor, az adatbetöltési költségeket is megtakaríthatja.

* **Rugalmas kibővítés:** Bár a HDFS kibővített fájlrendszert biztosít, a léptéket a fürthöz létrehozott csomópontok száma határozza meg. A lépték módosítása bonyolultabb folyamattá válhat, mintha az Azure Blob Storage tárolóban automatikusan elérhető rugalmas léptékezési képességekre támaszkodna.

* **Georeplikáció:** az Azure storage-adatok a georeplikált lehet. Bár ez a képesség nyújt földrajzi helyreállítást és adatredundanciát, a georeplikált helyre feladatátvétel súlyosan támogatása hatással van a teljesítményre, és további költségeket vonhat. Ezért, válassza ki a georeplikáció, gondosan és csak az adatok érték esetén a további költségeket.

* **Életciklus-felügyelet:** bármely fájlrendszerben lévő összes adatot halad végig a saját életciklusát. Adatokat gyakran ki nagyon fontosak, és a gyakran használt elindul, kevésbé értékes folyamatban, és kevesebb hozzáférést igénylő értékre vált, és végső soron igényel a archiválása vagy törlése. Az Azure Storage biztosít adatokat rétegezést és életciklus, hogy az adatok megfelelő módon rétegzi a életciklusfázis kezelési szabályzatokat.

Bizonyos MapReduce-feladatok és csomagok olyan köztes eredményeket hozhatnak létre, amelyeket nem érdemes az Azure Blob Storage tárolóban tárolni. Ebben az esetben a helyi HDFS-ben is tárolhatja az adatokat. Valójában a HDInsight a natív HDFS implementációra (amely nevezzük az elosztott Fájlrendszerbeli) használja több ilyen köztes eredményhez a Hive-feladatokban és egyéb folyamatokban.

> [!NOTE]
> A legtöbb HDFS parancs (például `ls`, `copyFromLocal` és `mkdir`) továbbra is a várt módon működik. Csak azok a parancsok, mint például az elosztott Fájlrendszerbeli jellemző `fschk` és `dfsadmin`, eltérő viselkedéssel megjelenítése az Azure storage-ban.

## <a name="create-an-data-lake-storage-file-system"></a>Hozzon létre egy Data Lake Storage fájlrendszer

A fájlrendszer használatához először hozzon létre egy [Azure Storage-fiók][azure-storage-create]. Ez a folyamat részeként adja meg az Azure régióban, amelyben létrehozza a tárfiókot. A fürtnek és a tárfióknak ugyanabban a régióban kell lennie. A Hive-metaadattár SQL Server adatbázisának és az Oozie-metaadattár SQL Server adatbázisának is ugyanabban a régióban kell lennie.

Akárhol él, mindegyik létrehozott blob tartozik egy fájlrendszer az Azure Data Lake Storage-fiókban. 

Az alapértelmezett Data Lake Storage fájlrendszer fürtre jellemző információkat, például a feladatelőzményeket és a naplókat tárolja. Ne ossza meg egy alapértelmezett Data Lake Storage fájlrendszer több HDInsight-fürtökkel. Ez károsíthatja a feladatelőzményeket. Javasolt egy másik fájlrendszer használni mindegyik fürthöz és a megosztott adatok elhelyezése az alapértelmezett tárfiók helyett az összes kapcsolódó fürt üzemelő példányában meghatározott kapcsolt tárfiókra. A kapcsolt tárfiókok konfigurálásáról további információért lásd: [HDInsight-fürtök létrehozása][hdinsight-creation]. Alapértelmezett tároló fájlrendszer azonban használhat az eredeti HDInsight fürt törlése után. A HBase fürtök esetén is megőrzi a HBase táblasémát és adatokat hoz létre egy új HBase fürtöt a törölt HBase-fürt törölve lett által használt alapértelmezett blobtárolóval.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a>Az Azure Portal használata

Egy HDInsight-fürt létrehozása a portálról, ha a közül választhat (ahogyan az az alábbi képernyőképen látható), adja meg a storage-fiókjának adatait. Megadhatja azt is kívánja-e egy tárfiókot a fürthöz társított, és ha igen, választhatja ki a rendelkezésre álló tár beállításokat a tárhely.

![HDInsight hadoop létrehozási adatforrás](./media/use-hdi-cluster/create-storage-account.png)

> [!WARNING]
> A rendszer nem támogatja további tárfiókok használatát a HDInsight-fürtön kívül eső helyeken.

### <a name="use-azure-powershell"></a>Azure PowerShell használatával

Ha Ön [telepítette és konfigurálta az Azure PowerShell-lel][powershell-install], használhatja a következő kódot az Azure PowerShell parancssorról egy tárfiók és tároló létrehozásához:

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "WEST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Connect-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
      -Name StorageAccountName `
      -Location $Location `
      -SkuName Standard_LRS `
      -Kind StorageV2 
      -HierarchialNamespace $True

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

> [!NOTE]
> Tároló létrehozása az azonos a fájlrendszer létrehozása az Azure Data Lake Storage.

### <a name="use-azure-cli"></a>Az Azure parancssori felület használatával

[!INCLUDE [use-latest-version](../../../includes/hdinsight-use-latest-cli.md)]

Ha [telepítette és konfigurálta az Azure CLI parancssori felületet](../../cli-install-nodejs.md), a következő parancs használható a tárfiókokhoz és tárolóhoz.

```bash
az storage account create \
    --name <STORAGE_ACCOUNT_NAME> \
    --resource-group <RESOURCE_GROUP_NAME> \
    --location westus2 \
    --sku Standard_LRS \
    --kind StorageV2 \
    --Enable-hierarchical-namespace true
```

> [!NOTE]
> A nyilvános előzetes verzióban a Data Lake Storage Gen2 csak `--sku Standard_LRS` használata támogatott.

A rendszer kéri, hogy adja meg a földrajzi régiót, amelyben a tárfiók létre lett hozva. Hozzon létre a tárfiókot ugyanabban a régióban, amely a HDInsight-fürt létrehozását tervezi.

A tárfiók létrehozása után a következő paranccsal kérheti le a tárfiók kulcsait:

    azure storage account keys list <STORAGE_ACCOUNT_NAME>

Egy tároló létrehozásához használja az alábbi parancsot:

    azure storage container create <CONTAINER_NAME> --account-name <STORAGE_ACCOUNT_NAME> --account-key <STORAGE_ACCOUNT_KEY>

> [!NOTE]
> Tároló létrehozása az azonos a fájlrendszer létrehozása az Azure Data Lake Storage.

## <a name="address-files-in-azure-storage"></a>Az Azure Storage tárolóban található címfájlok

Az Azure Storage tárolóban a HDInsight eszközről végzett fájlelérés URI sémája a következő:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.widows.net/<PATH>

Az URI séma titkosítatlan hozzáférést biztosít (az a *abfs:* előtag) és SSL titkosított hozzáférést (a *abfss*). Azt javasoljuk, *abfss* amikor csak lehetséges, akkor is, ha ugyanabban a régióban, az Azure-ban lévő adatokat éri el.

* &lt;FILE_SYSTEM_NAME&gt; elérési útját a fájlrendszerben Azure Data Lake Storage azonosítja.
* &lt;ACCOUNT_NAME&gt; azonosítja az Azure Storage-fiók nevét. Szükség van a teljes tartománynévre (FQDN-re).

    Ha az értékek &lt;FILE_SYSTEM_NAME&gt; sem &lt;ACCOUNT_NAME&gt; van megadva, az alapértelmezett fájlrendszer szolgál. Az alapértelmezett fájlrendszeren tárolt fájlok esetén relatív elérési utat vagy abszolút elérési utat használhat. Ha például a *hadoop-mapreduce-examples.jar* fájlt, amely együttműködik a HDInsight-fürtök a következő útvonalak egyikét használva lehet hivatkozni:
    
        abfs://myfilesystempath@myaccount.dfs.core.widows.net/example/jars/hadoop-mapreduce-examples.jar
        abfs:///example/jars/hadoop-mapreduce-examples.jar
        /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> A fájlnév a *hadoop-examples.jar* a HDInsight 2.1-es és 1.6-s verziójú fürtökben.

* A &lt;elérési út&gt; a fájl vagy könyvtár HDFS elérési út.

> [!NOTE]
> Ha HDInsight-on kívül fájljain végzett munka, legtöbb segédprogram nem ismeri fel a ABFS formázhatja, és ehelyett például az egy alapvető elérési út formátumot várhatóan `example/jars/hadoop-mapreduce-examples.jar`.
 
## <a name="use-additional-storage-accounts"></a>További tárfiókok használata

HDInsight-fürt létrehozásakor meg kell adnia azt az Azure Storage-fiókot, amelyet a fürthöz társítani kívánja. Ezen a tárfiókon kívül további tárfiókokat vehet fel ugyanabból az Azure-előfizetésből vagy más Azure-előfizetésekből a létrehozási folyamat során vagy a fürt létrehozása után. Útmutatás további tárfiókok hozzáadásához: [HDInsight-fürtök létrehozása](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> A rendszer nem támogatja további tárfiókok használatát a HDInsight-fürtön kívül eső helyeken.

## <a name="next-steps"></a>További lépések

Ebből a cikkből megtanulta, hogyan használhat HDFS-kompatibilis Azure-tárolót a HDInsighttal. Ez a megközelítés lehetővé teszi, skálázható, hosszú távú adatarchiváló beszerzési megoldások és a HDInsight segítségével az információ található a tárolt strukturált és strukturálatlan adatok zárolásának feloldásához.

További információkért lásd:

* [Az Azure Data Lake Storage Gen2 ABFS Hadoop-fájlrendszer illesztőprogramja](abfs-driver.md)
* [Az Azure Data Lake Storage bemutatása](introduction.md)
* [Azure Data Lake Storage használata a Hadoop, Spark, Kafka és több HDInsight-fürtök beállítása](quickstart-create-connect-hdi-cluster.md)
* [Betölteni az adatokat az Azure Data Lake Storage a distcp használata](use-distcp.md)

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: ../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/use-hdi-cluster/HDI.PowerShell.BlobCommands.png
