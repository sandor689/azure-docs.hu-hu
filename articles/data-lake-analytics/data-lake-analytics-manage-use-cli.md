---
title: Azure parancssori felület használatával Azure Data Lake Analytics kezelése
description: A cikkből megtudhatja, hogyan használható az Azure parancssori felület Data Lake Analytics-feladatok, a adatforrások és a felhasználók kezeléséhez.
services: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: 86fa41db2d21beac08015d067b79ce1375cd3ddf
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736089"
---
# <a name="manage-azure-data-lake-analytics-using-the-azure-command-line-interface-cli"></a>Azure Data Lake Analytics az Azure parancssori felület (CLI) használatával kezelése

[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Útmutató: Azure Data Lake Analytics-fiókok, adatforrások, felhasználók és az Azure parancssori felület használatával feladatok kezelése. Más eszközök használatával kapcsolatos témakörök megtekintéséhez kattintson a fenti lapválasztóra.


**Előfeltételek**

Ez az oktatóanyag megkezdése előtt rendelkeznie kell a következőket:

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

* Azure CLI. Lásd: [Install and configure Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) (Az Azure parancssori felület telepítése és konfigurálása).

   * Töltse le és telepítse az **előzetes kiadású** [Azure parancssori felületi eszközöket](https://github.com/MicrosoftBigData/AzureDataLake/releases) a bemutató elvégzéséhez.

* Hitelesítéshez használja a `az login` parancsot, és válassza ki a használni kívánt előfizetést. További információ a munkahelyi vagy iskolai fiók segítségével történő hitelesítésről: [Connect to an Azure subscription from the Azure CLI](/cli/azure/authenticate-azure-cli) (Csatlakozás Azure-előfizetéshez az Azure parancssori felületről).

   ```azurecli
   az login
   az account set --subscription <subscription id>
   ```

   A Data Lake Analytics és a Data Lake Store parancs érhetők el. A következő parancsot a Data Lake Store és a Data Lake Analytics parancsok listázásához:

   ```azurecli
   az dls -h
   az dla -h
   ```

## <a name="manage-accounts"></a>Fiókok kezelése

Minden Data Lake Analytics-feladatok futtatásához rendelkeznie kell egy Data Lake Analytics-fiók. Azure HDInsight eltérően nem kell fizetnie az Analytics-fiók, amikor nem fut egy feladat. Csak kell fizetnie, ha egy feladat fut. alkalommal.  További információkért lásd: [Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md).  

### <a name="create-accounts"></a>Fiókok létrehozása

A következő parancsot egy Data Lake-fiók létrehozása 

   ```azurecli
   az dla account create --account "<Data Lake Analytics account name>" --location "<Location Name>" --resource-group "<Resource Group Name>" --default-data-lake-store "<Data Lake Store account name>"
   ```

### <a name="update-accounts"></a>Fiókok frissítése

A következő parancsot egy meglévő Data Lake Analytics-fiók tulajdonságainak frissítése

   ```azurecli
   az dla account update --account "<Data Lake Analytics Account Name>" --firewall-state "Enabled" --query-store-retention 7
   ```

### <a name="list-accounts"></a>Fiókok listázása

Egy adott erőforráscsoportban lista Data Lake Analytics-fiókok

   ```azurecli
   az dla account list "<Resource group name>"
   ```

## <a name="get-details-of-an-account"></a>Részletek a fiók

   ```azurecli
   az dla account show --account "<Data Lake Analytics account name>" --resource-group "<Resource group name>"
   ```

### <a name="delete-an-account"></a>-Fiók törlése

   ```azurecli
   az dla account delete --account "<Data Lake Analytics account name>" --resource-group "<Resource group name>"
   ```

## <a name="manage-data-sources"></a>Adatforrások kezelése

A Data Lake Analytics jelenleg a következő két adatforrások támogatja:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

Az Analytics-fiók létrehozásakor ki kell jelölnie egy Azure Data Lake tárolási fiókot az alapértelmezett tárfiók. Az alapértelmezett Data Lake-tárfiókra feladatot metaadatok és a feladat a vizsgálati naplók tárolására szolgál. Az Analytics-fiók létrehozása után hozzáadhat további Data Lake-tárfiókokat és/vagy az Azure Storage-fiók. 

### <a name="find-the-default-data-lake-store-account"></a>Az alapértelmezett Data Lake Store-fiók található

Az alapértelmezett Data Lake Store-fiók futtató által használt megtekintheti a `az dla account show` parancsot. Alapértelmezett nevét megtalálható-e a defaultDataLakeStoreAccount tulajdonság.

   ```azurecli
   az dla account show --account "<Data Lake Analytics account name>"
   ```

### <a name="add-additional-blob-storage-accounts"></a>További Blob storage-fiókok hozzáadása

   ```azurecli
   az dla account blob-storage add --access-key "<Azure Storage Account Key>" --account "<Data Lake Analytics account name>" --storage-account-name "<Storage account name>"
   ```

> [!NOTE]
> Csak a Blob storage rövid nevek használata támogatott. Ne használjon teljes Tartománynevet, például "myblob.blob.core.windows.net".
> 

### <a name="add-additional-data-lake-store-accounts"></a>További Data Lake Store-fiók hozzáadása

A következő parancsot egy további Data Lake Store-fiókot a megadott Data Lake Analytics-fiók frissíti:

   ```azurecli
   az dla account data-lake-store add --account "<Data Lake Analytics account name>" --data-lake-store-account-name "<Data Lake Store account name>"
   ```

### <a name="update-existing-data-source"></a>Meglévő adatforrás frissítése.

A Blob storage meglévő fiókkulcs frissítése:

   ```azurecli
   az dla account blob-storage update --access-key "<New Blob Storage Account Key>" --account "<Data Lake Analytics account name>" --storage-account-name "<Data Lake Store account name>"
   ```

### <a name="list-data-sources"></a>Az adatforrások listája:

A Data Lake Store-fiók listázásához:

   ```azurecli
   az dla account data-lake-store list --account "<Data Lake Analytics account name>"
   ```

A Blob storage-fiók listázásához:

   ```azurecli
   az dla account blob-storage list --account "<Data Lake Analytics account name>"
   ```

![A Data Lake Analytics lista adatforrás](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Adatforrások törlése:
Data Lake Store-fiók törlése:

   ```azurecli
   az dla account data-lake-store delete --account "<Data Lake Analytics account name>" --data-lake-store-account-name "<Azure Data Lake Store account name>"
   ```

A Blob storage-fiók törlése:

   ```azurecli
   az dla account blob-storage delete --account "<Data Lake Analytics account name>" --storage-account-name "<Data Lake Store account name>"
   ```

## <a name="manage-jobs"></a>Feladatok kezelése
Data Lake Analytics-fiók egy feladat létrehozása előtt kell rendelkeznie.  További információkért lásd: [kezelése Data Lake Analytics-fiókok](#manage-accounts).

### <a name="list-jobs"></a>Lista feladatok

   ```azurecli
   az dla job list --account "<Data Lake Analytics account name>"
   ```

   ![A Data Lake Analytics lista adatforrás](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>Lekérhet feladatadatokat

   ```azurecli
   az dla job show --account "<Data Lake Analytics account name>" --job-identity "<Job Id>"
   ```

### <a name="submit-jobs"></a>Feladatok elküldéséhez

> [!NOTE]
> A feladat alapértelmezett érték 1000, és a párhuzamosság feladat alapértelmezett mértékét 1.
> 
   ```azurecli
   az dla job submit --account "<Data Lake Analytics account name>" --job-name "<Name of your job>" --script "<Script to submit>"
   ```

### <a name="cancel-jobs"></a>Feladatok megszakítása
A lista parancs segítségével keresse meg a feladatazonosítót, és majd a Mégse gombra a Mégse gombra a feladat.

   ```azurecli
   az dla job cancel --account "<Data Lake Analytics account name>" --job-identity "<Job Id>"
   ```

## <a name="pipelines-and-recurrences"></a>Folyamatok és ismétlődések

**Folyamatok és ismétlődések adatainak lekérése**

A korábban elküldött feladatok folyamatadatait az `az dla job pipeline` paranccsal tekintheti meg.

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

A korábban elküldött feladatok ismétlődési adatait az `az dla job recurrence` paranccsal tekintheti meg.

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="see-also"></a>Lásd még
* [A Microsoft Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
* [A Data Lake Analytics használatának első lépései az Azure Portallal](data-lake-analytics-get-started-portal.md)
* [Az Azure Data Lake Analytics kezelése az Azure Portal használatával](data-lake-analytics-manage-use-portal.md)
* [Az Azure Data Lake Analytics-feladatok figyelése és hibaelhárítása az Azure Portal használatával](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

