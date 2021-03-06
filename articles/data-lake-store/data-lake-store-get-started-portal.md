---
title: Ismerkedés a Data Lake tárolási Gen1 az Azure portál használatával |} Microsoft Docs
description: Az Azure-portál használatával a Data Lake Store-fiók létrehozása és alapszintű műveletek végrehajtása a data lake store-ban
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: e23b2496ccb69bb530bd825a1feb99abcc4ab35b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036196"
---
# <a name="get-started-with-azure-data-lake-storage-gen1-using-the-azure-portal"></a>Ismerkedés az Azure Data Lake tárolási Gen1 az Azure portál használatával

> [!div class="op_single_selector"]
> * [Portál](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
>
> 

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Ismerje meg, hogyan hozhat létre Azure Data Lake Store-fiókot az Azure Portal használatával, illetve hogyan végezhet el olyan alapvető műveleteket, mint például a mappák létrehozása, adatfájlok le- és feltöltése, a fiók törlése stb. További információkért lásd: [áttekintés az Azure Data Lake tárolási Gen1](data-lake-store-overview.md).

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-an-azure-data-lake-store-account"></a>Azure Data Lake Store-fiók létrehozása

1. Jelentkezzen be az új [Azure Portalra](https://portal.azure.com).
2. Kattintson az **Erőforrás létrehozása > Storage > Data Lake Store** lehetőségre.
3. Az **Új Data Lake Store** panelen adja meg az értékeket az alábbi képernyőfelvételen látható módon:
   
    ![Új Azure Data Lake Store-fiók létrehozása](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Új Azure Data Lake Store-fiók létrehozása")
   
   * **Név**. Adja meg a Data Lake Store-fiók egyedi nevét.
   * **Előfizetés**. Válassza ki az előfizetést, amelyhez létre kíván hozni egy új Data Lake Store-fiókot.
   * **Erőforráscsoport**. Válasszon ki egy meglévő erőforráscsoportot, vagy válassza a **Create new** (Új létrehozása) elemre, ha szeretne létrehozni egyet. Az erőforráscsoport egy tároló, amely alkalmazásokhoz kapcsolódó erőforrásokat tárol. További információk: [Erőforráscsoportok az Azure-ban](../azure-resource-manager/resource-group-overview.md#resource-groups).
   * **Hely**: Válassza ki, hol szeretné létrehozni a Data Lake Store-fiókot.
   * **Titkosítási beállítások**. Három beállítás érhető el:
     
     * **A titkosítás letiltása**.
     * **A Data Lake Store által kezelt kulcsok használata**.  Ha azt szeretné, hogy az Azure Data Lake Store kezelje a titkosítási kulcsokat.
     * **Saját tulajdonú Key Vault-kulcsok használata**. Kiválaszthat egy meglévő Azure Key Vault-tárolót, vagy létrehozhat egy újat. Az Azure Key Vaultban lévő kulcsok használatához engedélyeket kell rendelnie az Azure Data Lake Store-fiókhoz az Azure Key Vault elérése érdekében. Az utasításokért lásd [az engedélyek az Azure Key Vaulthoz történő hozzárendelését](#assign-permissions-to-azure-key-vault) ismertető cikket.
       
        ![Data Lake Store-titkosítás](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Data Lake Store-titkosítás")
       
        Kattintson az **OK** gombra a **Titkosítási beállítások** panelen.

        További információkat [az adatok az Azure Data Lake Store-ban történő titkosítását](./data-lake-store-encryption.md) ismertető cikkben talál.

4. Kattintson a **Create** (Létrehozás) gombra. Ha úgy döntött, hogy rögzíti a fiókot az irányítópulton, visszakerül az irányítópultra, ahol láthatja a Data Lake Store-fiók üzembe helyezésének folyamatát. A Data Lake Store-fiók üzembe helyezése után megjelenik a fiókpanel.

## <a name="assign-permissions-to-azure-key-vault"></a>Engedélyek hozzárendelése az Azure Key Vaulthoz
Ha az Azure Key Vaultban lévő kulcsokkal konfigurálta a titkosítást a Data Lake Store-fiókban, konfigurálnia kell a Data Lake Store-fiók és az Azure Key Vault-fiók közötti hozzáférést. Ehhez kövesse az alábbi lépéseket.

1. Ha az Azure Key Vault kulcsait használta, a Data Lake Store panele felül egy figyelmeztetést jelenít meg. Kattintson a figyelmeztetésre a **Titkosítás** panel megnyitásához.
   
    ![Data Lake Store-titkosítás](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Data Lake Store-titkosítás")
2. A panelen két lehetőség szerepel a hozzáférés konfigurálásához.

    ![Data Lake Store-titkosítás](./media/data-lake-store-get-started-portal/adls-encryption-4.png "Data Lake Store-titkosítás")
   
   * Az első lehetőségnél kattintson az **Engedélyek megadása** gombra a hozzáférés konfigurálásához. Az első lehetőség csak akkor engedélyezett, ha a Data Lake Store-t létrehozó felhasználó egyben az Azure Key Vault rendszergazdája is.
   * A másik lehetőség a panelen megjelenő PowerShell parancsmag futtatása. Az Azure Key Vault tulajdonosának kell lennie, vagy képesnek kell lennie engedélyeket hozzárendelni az Azure Key Vaulton. A parancsmag futtatása után térjen vissza a panelre, és kattintson az **Engedélyezés** gombra a hozzáférés konfigurálásához.

> [!NOTE]
> Az Azure Resource Manager-sablonok használatával is létrehozhat Data Lake Store-fiókokat. Ezek a sablonok az [Azure gyors üzembehelyezési sablonokból](https://azure.microsoft.com/resources/templates/?term=data+lake+store) érhetők el:
    - Adattitkosítás nélkül: [Azure Data Lake Store-fiók üzembe helyezése adattitkosítás nélkül](https://azure.microsoft.com/resources/templates/101-data-lake-store-no-encryption/).
    - A Data Lake Store adattitkosításának használatával: [Azure Data Lake Store-fiók üzembe helyezése titkosítással (Data Lake)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/).
    - Az Azure Key Vault adattitkosításának használatával: [Azure Data Lake Store-fiók üzembe helyezése titkosítással (Key Vault)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/).
> 
> 



## <a name="createfolder"></a>Mappák létrehozása az Azure Data Lake Store-fiókban
A Data Lake Store-fiókjában mappákat hozhat létre az adatok kezelésére és tárolására.

1. Nyissa meg a létrehozott Data Lake Store-fiókot. A bal oldali ablaktáblában kattintson az **Összes erőforrás** elemre, majd az Összes erőforrás panelen kattintson a fiók nevére, amelyben mappákat kíván létrehozni. Ha rögzítette a fiókot a kezdőpulton, kattintson a fiók csempéjére.
2. A Data Lake Store-fiók panelén kattintson a **Data Explorer** (Adatkezelő) elemre.
   
    ![Mappák létrehozása a Data Lake Store-fiókban](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Mappák létrehozása a Data Lake Store-fiókban")
3. A Data Explorer (Adatkezelő) panelen kattintson a **New Folder** (Új mappa) elemre, adjon nevet az új mappának, majd kattintson az **OK** gombra.
   
    ![Mappák létrehozása a Data Lake Store-fiókban](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Mappák létrehozása a Data Lake Store-fiókban")
   
    Az újonnan létrehozott mappa megjelenik az **Adatkezelő** panelen lévő listában. Bármilyen szinten létrehozhat beágyazott mappákat.
   
    ![Mappák létrehozása a Data Lake-fiókban](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Mappák létrehozása a Data Lake-fiókban")

## <a name="uploaddata"></a>Adatok feltöltése az Azure Data Lake Store-fiókba
Adatait közvetlenül, gyökérszinten is feltöltheti az Azure Data Lake Store-fiókba, vagy a fiókon belül létrehozott mappába. 

1. A **Data Explorer** (Adatkezelő) panelen kattintson az **Upload** (Feltöltés) elemre. 
2. Az **Upload files** (Fájlok feltöltése) panelen keresse meg a feltölteni kívánt fájlokat, majd kattintson az **Add selected files** (Kiválasztott fájlok hozzáadása) gombra. Egyszerre több fájlt is kijelölhet a feltöltéshez.

    ![Adatok feltöltése](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Adatok feltöltése")

Ha feltölthető mintaadatokra van szüksége, használhatja az [Azure Data Lake Git-tárában](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) található **Ambulance Data** mappát.

## <a name="properties"></a>A tárolt adatokhoz elérhető műveletek
Kattintson a fájl melletti, három pontot ábrázoló ikonra, majd az előugró menüben kattintson arra a műveletre, amelyet el kíván végezni az adatokon.

![Az adatok tulajdonságai](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Az adatok tulajdonságai") 

## <a name="secure-your-data"></a>Az adatok védelme
Az Azure Active Directory és a hozzáférés-vezérlés (ACLs) segítségével gondoskodhat az Azure Data Lake Store-fiókban tárolt adatok védelméről. Az erre vonatkozó utasításokért lásd: [Az adatok védelme az Azure Data Lake Store-ban](data-lake-store-secure-data.md).

## <a name="delete-azure-data-lake-store-account"></a>Az Azure Data Lake Store-fiók törlése
Az Azure Data Lake Store-fiók törléséhez kattintson a **Delete** (Törlés) elemre a Data Lake Store panelen. A művelet megerősítéséhez a rendszer a törölni kívánt fiók nevének megadását kéri. Adja meg a fiók nevét, majd kattintson a **Delete** (Törlés) gombra.

![Data Lake-fiók törlése](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Data Lake-fiók törlése")

## <a name="next-steps"></a>További lépések
* [Az Azure Data Lake Store használata big data-követelményekhez](data-lake-store-data-scenarios.md) 
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)

