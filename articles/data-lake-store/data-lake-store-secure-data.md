---
title: Az Azure Data Lake Store-ban tárolt adatok védelme |} A Microsoft Docs
description: Ismerje meg, hogyan védi az adatait az Azure Data Lake Store a csoportok és hozzáférés-vezérlési listák
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 0ac6b90f2efc525cfb9767843c741f1e3cfc6de7
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450163"
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>Az Azure Data Lake Store-ban tárolt adatok védelme
Az adatok védelme az Azure Data Lake Store egy három lépéses megközelítést.  Mindkét szerepköralapú hozzáférés-vezérlés (RBAC), és a hozzáférés-vezérlési listák (ACL) kell állítani a felhasználók és biztonsági csoportok adatokhoz való hozzáférés teljes mértékű engedélyezéséhez.

1. Először hozzon létre biztonsági csoportokat az Azure Active Directory (AAD). A biztonsági csoportok használatban vannak a szerepköralapú hozzáférés-vezérlés (RBAC) megvalósítása az Azure Portalon. További információkért lásd: [szerepköralapú hozzáférés-vezérlés a Microsoft Azure-ban](../role-based-access-control/role-assignments-portal.md).
2. Rendelje hozzá az AAD biztonsági csoportokat az Azure Data Lake Store-fiókba. Ez vezérli a hozzáférést a portál vagy API-k a portál és a felügyeleti műveletek a Data Lake Store-fiókba.
3. Rendelje hozzá az AAD-biztonságicsoportok hozzáférés vezérlési listák (ACL) a Data Lake Store-fájlrendszer.
4. Emellett beállíthat egy IP-címtartományt férhet hozzá az adatokhoz, a Data Lake Store-ügyfelek esetében.

Ez a cikk útmutatást nyújt a fenti feladatok elvégzéséhez az Azure portal használatával. Hogyan valósítja meg a Data Lake Store a fiók és adatok szintjén biztonsági részletes információkért lásd: [biztonság az Azure Data Lake Store](data-lake-store-security-overview.md). Milyen az Azure Data Lake Store ACL-ek találhatók meg a részletes információkért lásd: [áttekintése a hozzáférés-vezérlés a Data Lake Store](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Data Lake Store-fiókot**. Létrehozásával kapcsolatos utasításokért lásd: [Azure Data Lake Store használatának első lépései](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Az Azure Active Directory biztonsági csoportok létrehozása
AAD biztonsági csoportok létrehozásával és felhasználók hozzáadása a csoporthoz, lásd: [kezelése az Azure Active Directory biztonsági csoportok](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

> [!NOTE] 
> Adhat hozzá felhasználókat és a többi csoport egy csoportot az Azure ad-ben az Azure portal használatával. Azonban ahhoz, hogy a szolgáltatásnév hozzáadása a csoporthoz, használható [az Azure AD PowerShell-modul](../active-directory/users-groups-roles/groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get the desired group and service principal and identify the correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add the service principal to the group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Felhasználók vagy biztonsági csoportok hozzárendelése az Azure Data Lake Store-fiókok
Amikor a felhasználók vagy biztonsági csoportok hozzárendelése az Azure Data Lake Store-fiókok, a műveletek az Azure Portallal és az Azure Resource Manager API-k a fiók elérését Ön szabályozza. 

1. Nyisson meg egy Azure Data Lake Store-fiókot. A bal oldali ablaktáblában kattintson **összes erőforrás**, és az összes erőforrás panelen kattintson a fiók nevét, amelyhez szeretné egy felhasználó vagy biztonsági csoport hozzárendelése.

2. A Data Lake Store-fiók panelén kattintson **hozzáférés-vezérlés (IAM)**. A panel alapértelmezés szerint az előfizetés-tulajdonost tulajdonosként sorolja fel.
   
    ![Biztonsági csoport hozzárendelése az Azure Data Lake Store-fiók](./media/data-lake-store-secure-data/adl.select.user.icon.png "biztonsági csoport hozzárendelése az Azure Data Lake Store-fiókba")

3. Az a **hozzáférés-vezérlés (IAM)** panelen kattintson a **Hozzáadás** megnyitásához a **engedélyek hozzáadása** panelen. Az a **engedélyek hozzáadása** panelen válassza ki a **szerepkör** a felhasználó vagy csoport. Keresse meg a korábban létrehozott Azure Active Directory biztonsági csoport, és válassza ki azt. Ha a felhasználók és csoportok kell keresni a sok, használja a **kiválasztása** a csoport nevére szűrésére szolgáló szövegmező. 
   
    ![A felhasználó szerepkör hozzáadása](./media/data-lake-store-secure-data/adl.add.user.1.png "a felhasználó szerepkör hozzáadása")
   
    A **tulajdonosa** és **közreműködői** szerepkör számos felügyeleti feladatot a hozzáférést biztosítsanak a data lake-fiók. A felhasználók számára, akik működjön együtt a data lake, ugyanakkor a fiók felügyeleti információk megtekintéséhez szükséges adatok, hozzáadhatja őket a **olvasó** szerepkör. Ezek a szerepkörök hatókörének az Azure Data Lake Store-fiókhoz kapcsolódó felügyeleti műveletek korlátozódik.
   
    Adatműveletek egyes fájlrendszerre vonatkozó engedélyekkel határozza meg a felhasználók mit tehetnek. Ezért egy olvasói szerepkörrel rendelkező felhasználói is csak a fiókhoz tartozó felügyeleti beállítások megtekintése, de képes potenciálisan olvasási és írási hozzájuk rendelt fájlrendszeri engedélyek alapján. A Data Lake Store fájlrendszeri engedélyek leírását [biztonsági csoport hozzárendelése ACL-ként az Azure Data Lake Store-fájlrendszerhez](#filepermissions).

    > [!IMPORTANT]
    > Csak a **tulajdonosa** szerepkör automatikusan lehetővé teszi, hogy a fájlrendszer elérése. A **közreműködői**, **olvasó**, és a többi szerepkör szükséges ahhoz, hogy bármilyen szinten fájlok és mappák hozzáférési ACL-ek.  A **tulajdonosa** szerepkör felügyelő fájl- és mappaengedélyeket, amely nem bírálható felül az ACL-alapú biztosít. Hogyan leképezése az RBAC-házirendeket adatelérési további információkért lásd: [fiókkezeléshez RBAC](data-lake-store-security-overview.md#rbac-for-account-management).

4. Ha szeretné-e, amely nem szerepel a csoport vagy felhasználó hozzáadása a **engedélyek hozzáadása** panelen meghívhatja munkatársait, azokat a saját e-mail cím beírásával a **kiválasztása** szövegmezőbe, és válassza a listából.
   
    ![Adja meg a biztonsági csoport](./media/data-lake-store-secure-data/adl.add.user.2.png "biztonsági csoport hozzáadása")
   
5. Kattintson a **Save** (Mentés) gombra. A biztonsági csoport hozzáadva a lent látható módon kell megjelennie.
   
    ![Biztonsági csoport hozzáadva](./media/data-lake-store-secure-data/adl.add.user.3.png "biztonsági csoport hozzáadása")

6. A felhasználó vagy biztonsági csoport most már hozzáférhet az Azure Data Lake Store-fiók. Ha szeretne hozzáférést biztosítanak az egyes felhasználókhoz, felveheti őket a biztonsági csoporthoz. Hasonlóképpen ha vissza szeretné vonni a hozzáférést egy felhasználó, eltávolíthatja őket a biztonsági csoportból. Több biztonsági csoportot is rendelhet egy fiókot. 

## <a name="filepermissions"></a>Felhasználók vagy biztonsági csoport hozzárendelése ACL-ként az Azure Data Lake Store-fájlrendszerhez
A felhasználó vagy biztonsági csoportok hozzárendelése az Azure Data Lake fájlrendszert, beállíthat egy hozzáférés-vezérlés az Azure Data Lake Store-ban tárolt adatokkal kapcsolatos.

1. A Data Lake Store-fiók panelén kattintson a **Data Explorer** (Adatkezelő) elemre.
   
    ![Megtekintheti az adatkezelő segítségével adatokat](./media/data-lake-store-secure-data/adl.start.data.explorer.png "adatkezelő keresztül adatok megtekintése")
2. Az a **adatkezelő** panelen kattintson arra a mappára, amelyhez konfigurálni az ACL-t, és kattintson a kívánt **hozzáférés**. ACL-ek hozzárendelése egy fájlt, először kattintson a tekintse meg a lapot, majd kattintson a fájl **hozzáférés** származó a **Fájlelőnézet** panelen.
   
    ![Állítsa be a hozzáférés-vezérlési listák Data Lake fájlrendszerben](./media/data-lake-store-secure-data/adl.acl.1.png "beállítva hozzáférés-vezérlési listák Data Lake fájlrendszer")
3. A **hozzáférés** panel felsorolja a tulajdonosok között, és már hozzá van rendelve a legfelső szintű engedélyekkel. Kattintson a **Hozzáadás** ikonra kattintva adja hozzá a további hozzáférési ACL-ek.
    > [!IMPORTANT]
    > Egyetlen fájl a hozzáférési engedélyek beállítása nem feltétlenül biztosít hozzáférést egy felhasználó/csoport, amely a fájl. A fájl elérési útját a hozzárendelt felhasználói csoportok számára elérhetőnek kell lennie. További információért és példákért lásd: [az engedélyekhez kapcsolódó gyakori helyzetek](data-lake-store-access-control.md#common-scenarios-related-to-permissions).
   
    ![Standard és egyéni hozzáférési listán](./media/data-lake-store-secure-data/adl.acl.2.png "szabványos és egyéni hozzáférési listázása")
   
   * A **tulajdonosok** és **mindenki más** UNIX típusú hozzáférést biztosíthat, adhatja meg Olvasás, írás, hajtsa végre (rwx) a három különböző felhasználói osztályok: tulajdonos, csoport és mások.
   * **Hozzárendelt engedélyek** a POSIX hozzáférés-vezérlési listák, amelyek lehetővé teszik, hogy az engedélyek beállítása az adott nevesített felhasználók vagy csoportok a fájl tulajdonosa vagy a csoport nem felel meg. 
     
     További információkért lásd: [HDFS hozzáférés-vezérlési listák](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Hogyan Data Lake Store ACL-ek találhatók meg a további információkért lásd: [hozzáférés-vezérlés a Data Lake Store](data-lake-store-access-control.md).
4. Kattintson a **Hozzáadás** ikonra kattintva nyissa meg a **engedélyek hozzárendelése** panelen. Ezen a panelen kattintson a **felhasználó vagy csoport kiválasztása**, majd **felhasználó vagy csoport kiválasztása** panelen keresse meg a korábban létrehozott Azure Active Directory biztonsági csoport. Ha csoportokat kell keresni a sok, a szövegmező felső szűréséhez használja a csoport nevére. Kattintson a csoport hozzáadása, és kattintson a kívánt **kiválasztása**.
   
    ![Csoport hozzáadása](./media/data-lake-store-secure-data/adl.acl.3.png "csoport hozzáadása")
5. Kattintson a **engedélyek kiválasztása**, jelölje be az engedélyeket, hogy engedélyeket kell alkalmazni rekurzív módon és, hogy kívánja-e az engedélyek hozzárendelése a hozzáférési ACL-t, mint alapértelmezett ACL-t, vagy mindkettőt. Kattintson az **OK** gombra.
   
    ![Engedélyek csoporthoz való hozzárendelése](./media/data-lake-store-secure-data/adl.acl.4.png "csoporthoz engedélyek hozzárendelése")
   
    A Data Lake Store és az alapértelmezett/hozzáférési ACL-ek engedélyekkel kapcsolatos további információkért lásd: [hozzáférés-vezérlés a Data Lake Store](data-lake-store-access-control.md).
6. Kattintás után **Ok** a a **engedélyek kiválasztása** panelen, az újonnan létrehozott csoport és az engedélyek most már megjelenik a **hozzáférés** panelen.
   
    ![Engedélyek csoporthoz való hozzárendelése](./media/data-lake-store-secure-data/adl.acl.5.png "csoporthoz engedélyek hozzárendelése")
   
   > [!IMPORTANT]
   > A jelenlegi kiadásban, használhat akár 28 bejegyzések **engedélyeket az**. Ha azt szeretné, több mint 28 felhasználók hozzáadása, hozzunk létre biztonsági csoportokat, felhasználók hozzáadása a biztonsági csoportok, adja hozzá ezeket a csoportokat a Data Lake Store-fiók elérést biztosíthat.
   > 
   > 
7. Szükség esetén módosíthatja is a hozzáférési engedélyeket a csoport hozzáadása után. Törölje a jelet, vagy jelölje be az egyes engedélyt (olvasási, írási, végrehajtási) alapján, hogy távolítsa el, vagy hogy engedélyeket rendeljenek hozzá a biztonsági csoport. Kattintson a **mentése** menti a módosításokat, vagy **elveti** visszavonja a módosításokat.

## <a name="set-ip-address-range-for-data-access"></a>Állítsa be az IP-címtartományt az adatok eléréséhez
Az Azure Data Lake Store lehetővé teszi további zárolási hálózati szinten az adattárhoz való hozzáférés. Tűzfal engedélyezése, adja meg az IP-cím vagy IP-címtartomány megadása a megbízható ügyfelek számára. Ha engedélyezve van, csak a definiált tartományon belüli IP-címek rendelkező ügyfelek a tároló csatlakozhat.

![Tűzfalbeállítások és IP-hozzáférési](./media/data-lake-store-secure-data/firewall-ip-access.png "tűzfalbeállítások és IP-cím")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Egy Azure Data Lake Store-fiók biztonsági csoportok eltávolítása
Biztonsági csoportok az Azure Data Lake Store-fiókból való eltávolításakor csak változtatja a műveletek az Azure Portallal és az Azure Resource Manager API-k a fiók hozzáférési.  

Adatokhoz való hozzáférés nem változik, és továbbra is felügyeli a hozzáférési ACL-ek.  A kivétel ez alól a tulajdonosok szerepkörű felhasználók/csoportok.  Felhasználók/csoportok eltávolítva a tulajdonos szerepkör már nem felügyelők és hozzáférésüket visszavált hozzáférési ACL-beállítások. 

1. A Data Lake Store-fiók panelén kattintson **hozzáférés-vezérlés (IAM)**. 
   
    ![Biztonsági csoport hozzárendelése az Azure Data Lake-fiók](./media/data-lake-store-secure-data/adl.select.user.icon.png "biztonsági csoport hozzárendelése az Azure Data Lake-fiókba")
2. Az a **hozzáférés-vezérlés (IAM)** panelen kattintson az eltávolítani kívánt biztonsági csoport(ok). Kattintson a **eltávolítása**.
   
    ![Biztonsági csoport eltávolítva](./media/data-lake-store-secure-data/adl.remove.group.png "biztonsági csoport eltávolítva")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Távolítsa el a biztonsági csoport hozzáférés-vezérlési listák Azure Data Lake Store fájlrendszer
Ha eltávolítja az ACL-ek biztonsági csoportot az Azure Data Lake Store-fájlrendszer, a Data Lake Store az adatok hozzáférés módosítja.

1. A Data Lake Store-fiók panelén kattintson a **Data Explorer** (Adatkezelő) elemre.
   
    ![Könyvtárak létrehozása a Data Lake-fiók](./media/data-lake-store-secure-data/adl.start.data.explorer.png "könyvtárak létrehozása a Data Lake-fiók")
2. Az a **adatkezelő** panelen kattintson arra a mappára, amelyhez az ACL eltávolítása, és kattintson a kívánt **hozzáférés**. Egy fájl hozzáférés-vezérlési listák eltávolításához először kattintson a tekintse meg a lapot, majd kattintson a fájl **hozzáférés** a a **Fájlelőnézet** panelen. 
   
    ![Állítsa be a hozzáférés-vezérlési listák Data Lake fájlrendszerben](./media/data-lake-store-secure-data/adl.acl.1.png "beállítva hozzáférés-vezérlési listák Data Lake fájlrendszer")
3. Az a **hozzáférés** panelen kattintson az eltávolítani kívánt biztonsági csoportot. Az a **részleteit** panelen kattintson a **eltávolítása**.
   
    ![Engedélyek csoporthoz való hozzárendelése](./media/data-lake-store-secure-data/adl.remove.acl.png "csoporthoz engedélyek hozzárendelése")

## <a name="see-also"></a>Lásd még
* [Az Azure Data Lake Store áttekintése](data-lake-store-overview.md)
* [Adatok másolása az Azure Storage-Blobokból a Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)
* [PowerShell-lel a Data Lake Store használatának első lépései](data-lake-store-get-started-powershell.md)
* [Ismerkedés a Data Lake Store .NET SDK használatával](data-lake-store-get-started-net-sdk.md)
* [A Data Lake Store diagnosztikai naplóinak elérése](data-lake-store-diagnostic-logs.md)

