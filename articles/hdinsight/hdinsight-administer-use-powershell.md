---
title: A PowerShell-lel – Azure HDInsight Hadoop-fürtök kezelése
description: Ismerje meg, hogyan hajthat végre felügyeleti feladatokat az Azure PowerShell használatával HDInsight a Hadoop-fürtök.
services: hdinsight
editor: jasonwhowell
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: 60868ceb58a9ed4935ea540ad15abd0e5d35f559
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595528"
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>A HDInsight Hadoop-fürtök kezelése az Azure PowerShell használatával
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Az Azure PowerShell segítségével szabályozhatja és automatizálhatja az üzembe helyezéséhez és felügyeletéhez a számítási feladatokat az Azure-ban. Ebből a cikkből elsajátíthatja az Azure HDInsight Hadoop-fürtök kezelése az Azure PowerShell használatával. A HDInsight PowerShell-parancsmagok listáját lásd: [HDInsight parancsmag-referencia][hdinsight-powershell-reference].

**Előfeltételek**

Ez a cikk elkezdéséhez a következőkkel kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Az Azure PowerShell telepítése
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Ha telepítette az Azure PowerShell-verzió 0,9 x, el kell távolítania azt egy újabb verzió telepítése előtt.

A telepített PowerShell a verzió ellenőrzéséhez:

```powershell
Get-Module *azure*
```

Távolítsa el a régebbi verziót, hogy futtassa a Vezérlőpult Programok és szolgáltatások.

## <a name="create-clusters"></a>Fürtök létrehozása
Lásd: [Azure PowerShell használatával HDInsight-fürtök létrehozása Linux-alapú](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Fürtök listázása
A következő paranccsal listázhatja az összes fürt az aktuális előfizetésben:

```powershell
Get-AzureRmHDInsightCluster
```

## <a name="show-cluster"></a>Fürt megjelenítése
A következő paranccsal egy adott fürt részleteinek megjelenítése az aktuális előfizetésben:

```powershell
Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>Fürtök törlése
Használja a következő parancsot a fürt törléséhez:

```powershell
Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

A fürt törölheti az erőforráscsoportot, amely tartalmazza a fürt eltávolításával is. Egy erőforráscsoport törlése törli a csoportot, beleértve az alapértelmezett tárfiók található összes erőforrást.

```powershell
Remove-AzureRmResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>Fürtök méretezése
A fürtméretezés egy funkció lehetővé teszi, hogy a fürt újbóli létrehozása nélkül fut az Azure HDInsight-fürt által használt munkavégző csomópontok számának módosítását.

> [!NOTE]
> Csak 3.1.3 verziójú HDInsight-fürtök vagy újabb verziója támogatott. Ha biztos benne, hogy a fürt verziója, a Tulajdonságok lapon ellenőrizheti.  Lásd: [fürtök listázása és megjelenítése](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
>
>

A fürt a HDInsight által támogatott különböző típusú adatok csomópontok számának módosításával hatásai:

* Hadoop

    Zökkenőmentesen lehet növelni egy Hadoop-fürtöt, amely a folyamatban lévő vagy futó feladatok befolyásolása nélkül fut-e a munkavégző csomópontok számát. Új feladatok is lehet beküldeni, amíg a művelet folyamatban van. A skálázási művelet hibák szabályosan kezeli, úgy, hogy a fürt minden esetben működőképes állapotban marad.

    Ha egy Hadoop-fürtöt az adatcsomópontok száma csökkentésével vertikálisan leskálázni, a fürtben a szolgáltatások újra lesz indítva. Szolgáltatások újraindítása hatására az összes futó és a függőben lévő feladatok meghiúsulhatnak, a skálázási művelet befejezése után. A feladatok újból elküldheti, azonban a művelet befejeződése után.
* HBase

    Zökkenőmentesen adja hozzá vagy távolíthat el csomópontokat a HBase-fürt futás közben. Regionális kiszolgáló automatikusan kiegyensúlyozott vannak a skálázási művelet befejezése néhány percen belül. Azonban manuálisan is elosztása a regionális kiszolgálók jelentkezik be a fürt átjárócsomópontjával, és futtassa a következő parancsokat egy parancssorablakból:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

* Storm

    Zökkenőmentesen adja hozzá vagy távolít el a Storm-fürt adatcsomópontok futás közben. Azonban a topológia újraegyensúlyozására kell a skálázási művelet a sikeres telepítést követően.

    Az újraegyensúlyozás két módon is elvégezhető:

  * A Storm webes felhasználói felületen
  * Parancssori felület (CLI) eszköz

    Tekintse meg a [Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részletekért.

    A Storm webes felhasználói felületen a HDInsight-fürtön érhető el:

    ![HDInsight storm méretezési újraegyensúlyozási](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    Íme egy példa a Storm-topológia újraegyensúlyozására a CLI-parancs használatával:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Azure PowerShell-lel a Hadoop-fürt méretének módosításához futtassa a következő parancsot egy ügyfélszámítógépen:

```powershell
Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```


## <a name="grantrevoke-access"></a>GRANT/revoke-access
HDInsight-fürtök a következő HTTP webes szolgáltatások (ezen szolgáltatások mindegyikéhez kell RESTful végpontokon) rendelkezik:

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton eszközön keresztül végzett

Alapértelmezés szerint ezek a szolgáltatások hozzáférés kapnak. Akkor is visszavonása/biztosítása a hozzáférést. Visszavonásához:

```powershell
Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>
```

Megadását:

```powershell
$clusterName = "<HDInsight Cluster Name>"

# Credential option 1
$hadoopUserName = "admin"
$hadoopUserPassword = "<Enter the Password>"
$hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

# Credential option 2
#$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"

Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential
```

> [!NOTE]
> A hozzáférés biztosítása/visszavonása, amelyet alaphelyzetbe állítja a fürthöz tartozó felhasználónevet és jelszót.
>
>

Megadására, és visszavonja az elérését is elvégezhető a portálon keresztül. Lásd: [HDInsight felügyelheti az Azure portal használatával][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>A HTTP felhasználói hitelesítő adatok frissítése
Célszerű ugyanezt az eljárást, mint [Grant/revoke HTTP access](#grant/revoke-access). Ha a fürt a HTTP-hozzáférést kapott, meg kell először visszavonják.  És adja meg a hozzáférés az új HTTP-felhasználónál használt.

## <a name="find-the-default-storage-account"></a>Keresse meg az alapértelmezett tárfiókot
A következő PowerShell-parancsfájlt mutat be az alapértelmezett tárfiók neve és a kapcsolódó információk lekérése:

```powershell
#Connect-AzureRmAccount
$clusterName = "<HDInsight Cluster Name>"

$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$storageInfo = $clusterInfo.DefaultStorageAccount.split('.')
$defaultStoreageType = $storageInfo[1]
$defaultStorageName = $storageInfo[0]

echo "Default Storage account name: $defaultStorageName"
echo "Default Storage account type: $defaultStoreageType"

if ($defaultStoreageType -eq "blob")
{
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

    echo "Default Blob container name: $defaultBlobContainerName"
    echo "Default Storage account key: $defaultStorageAccountKey"
}
```


## <a name="find-the-resource-group"></a>Keresse meg az erőforráscsoport
A Resource Manager módban minden HDInsight-fürt Azure-erőforráscsoport tartozik.  Az erőforráscsoport megkeresése:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```


## <a name="submit-jobs"></a>Feladatok elküldése
**A MapReduce-feladatok elküldése**

Lásd: [futtassa a Hadoop MapReduce-minták a Windows-alapú HDInsight](hdinsight-run-samples.md).

**A Hive-feladatok**

Lásd: [futtatása Hive-lekérdezések a PowerShell-lel](hadoop/apache-hadoop-use-hive-powershell.md).

**A Pig-feladatok elküldése**

Lásd: [futtatása Pig-feladatok elvégzése PowerShell-lel](hadoop/apache-hadoop-use-pig-powershell.md).

**A Sqoop-feladatok elküldése**

Lásd: [Sqoop használata a HDInsight](hadoop/hdinsight-use-sqoop.md).

**Az Oozie-feladatok elküldése**

Lásd: [megadásához és a munkafolyamat futtatása a HDInsight Hadoop-keretrendszerrel használható Oozie](hdinsight-use-oozie.md).

## <a name="upload-data-to-azure-blob-storage"></a>Az Azure Blob storage-adatok feltöltése
Lásd: [Adatok feltöltése a HDInsightba][hdinsight-upload-data].

## <a name="see-also"></a>Lásd még:
* [HDInsight-parancsmagjának referenciadokumentációja][hdinsight-powershell-reference]
* [HDInsight felügyelete az Azure portal használatával][hdinsight-admin-portal]
* [Felügyelheti a HDInsight egy parancssori felülettel][hdinsight-admin-cli]
* [HDInsight-fürtök létrehozása][hdinsight-provision]
* [Adatok feltöltése a HDInsightba][hdinsight-upload-data]
* [Programozott módon a Hadoop-feladatok elküldése][hdinsight-submit-jobs]
* [Azure HDInsight – első lépések][hdinsight-get-started]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
