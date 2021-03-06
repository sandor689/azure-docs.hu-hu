---
title: Transzparens adattitkosítás az Azure SQL Database és a Data Warehouse |} A Microsoft Docs
description: Transzparens adattitkosítás az SQL Database és a Data Warehouse áttekintése. A dokumentum áttekint annak előnyeit és a konfigurációt, amely tartalmazza a szolgáltatás által kezelt transzparens adattitkosítás és Bring Your Own Key lehetőségeket.
services: sql-database
author: becczhang
manager: craigg
ms.prod: ''
ms.reviewer: carlrab
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: aliceku
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 0ed05fd2d55f1c4c80bec9f64925be2eddddc067
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/11/2018
ms.locfileid: "40043859"
---
# <a name="transparent-data-encryption-for-sql-database-and-data-warehouse"></a>Transzparens adattitkosítás az SQL Database és a Data warehouse-bA

Transzparens adattitkosítás (TDE) segítségével az Azure SQL Database és az Azure Data Warehouse a rosszindulatú tevékenység elleni. Valós idejű titkosítási és visszafejtési az adatbázis, azokhoz kapcsolódó biztonsági mentési és tranzakciós naplófájlokra inaktív azt végez anélkül, hogy a módosításokat. TDE alapértelmezés szerint engedélyezve van a újonnan üzembe helyezett Azure SQL Database. TDE nem használható titkosításához a logikai **fő** SQL Database-adatbázis.  A **fő** adatbázis tartalmazza a felhasználói adatbázisok a TDE műveletek végrehajtásához szükséges objektumok.

TDE manuálisan a régebbi adatbázisokhoz vagy Azure SQL Data Warehouse engedélyezni kell.  

Transzparens adattitkosítás a tárolót a teljes adatbázisra az adatbázis-titkosítási kulcs nevű szimmetrikus kulcs használatával titkosítja. A transzparens titkosítási védelme védi az adatbázis-titkosítási kulcs. A védő, vagy a szolgáltatás által kezelt tanúsítvány (szolgáltatás által kezelt transzparens adattitkosítás) vagy aszimmetrikus kulccsal (Bring Your Own Key) az Azure Key vaultban tárolt. Beállíthatja a transzparens titkosítási védelmet a kiszolgáló szintjén. 

Az adatbázis indításakor a titkosított adatbázis-titkosítási kulcs visszafejteni és visszafejtési és újbóli titkosítása az adatbázisfájlokat az SQL Server adatbázismotor folyamat használja. Transzparens adattitkosítás valós idejű i/o-titkosítás és a visszafejtés, a lapszintű hajt végre. Minden egyes lap visszafejtése a memóriába, és ezután írás előtt titkosítja a lemezre. Transzparens adattitkosítás általános ismertetését lásd: [transzparens adattitkosítás](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).

Is-Azure virtuális gépen futó SQL Server használhatja a Key Vaultból aszimmetrikus kulccsal. A konfigurációs lépések különböznek az aszimmetrikus kulcs használatával az SQL Database-ben. További információkért lásd: [bővíthető kulcskezelés az Azure Key Vault (SQL Server)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).

## <a name="service-managed-transparent-data-encryption"></a>Szolgáltatás által kezelt transzparens adattitkosítás

Az Azure-ban az alapértelmezett beállítás a transzparens adattitkosítás, hogy az adatbázis-titkosítási kulcs egy beépített kiszolgálói tanúsítvány védi. A beépített kiszolgálói tanúsítvány egy egyedülálló megoldás minden olyan kiszolgáló esetén. Ha egy adatbázis georeplikációs kapcsolatban, az elsődleges adatbázis szülő kiszolgálókulcs védi az elsődleges és a geo-secondary adatbázis. Ha a két adatbázis ugyanazon a kiszolgálón csatlakozik, a beépített tanúsítvány osztoznak. A Microsoft legalább 90 naponként automatikusan elforgatja ezeket a tanúsítványokat.

A Microsoft is zökkenőmentesen helyezi és kezeli a georeplikációhoz szükséges kulcsokat, és helyreállítja. 

> [!IMPORTANT]
> Minden újonnan létrehozott SQL-adatbázis szolgáltatás által kezelt transzparens adattitkosítás használatával alapértelmezés szerint vannak titkosítva. Meglévő adatbázisok előtt 2017 május és a visszaállítás, georeplikáció és adatbázis-másolat létrehozott alapértelmezés szerint nem titkosított.
>

## <a name="bring-your-own-key"></a>A saját kulcs használata

A transzparens titkosítási kulcsok és férhet hozzá őket elvégezhető Bring Your Own Key-támogatással, és mikor. A Key Vault, amely az Azure-alapú külső kulcs-kezelési rendszer, akkor az első kulcskezelő szolgáltatás transzparens adattitkosítás Bring Your Own Key-támogatás van integrálva. A Bring Your Own Key-támogatásával az adatbázis-titkosítási kulcs a Key Vault aszimmetrikus kulccsal védett. Az aszimmetrikus kulcs sosem hagyja el a Key Vaultban. Után a kiszolgáló, egy kulcstárolóba engedélyekkel rendelkezik, a kiszolgáló alapszintű kulcsműveletet kéréseket küld, Key Vaulton keresztül. Az aszimmetrikus kulcs állítsa be a kiszolgáló szintjén, és az adott kiszolgáló összes adatbázisára örökli azt.

Bring Your Own Key-támogatással mostantól vezérelheti fontos kezelési feladatok, például a kulcsok cseréjét, és a key vault-engedélyek. Is törli a kulcsokat és az összes titkosítási kulcs a naplózás és jelentéskészítés engedélyezéséhez. A Key Vault központi kulcskezelési biztosít, és szorosan figyelt hardveres biztonsági modulokat használ. A Key Vault elősegíti a kulcsok és adatok, amelyek az előírásoknak való megfelelés felügyeleti elkülönítése. A Key Vaulttal kapcsolatos további információkért tekintse meg a [Key Vault-dokumentációs oldalát](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

Transzparens adattitkosítás az SQL Database és az adatraktár Bring Your Own Key-támogatásával kapcsolatos további információkért lásd: [transzparens adattitkosítás Bring Your Own Key-támogatással rendelkező](transparent-data-encryption-byok-azure-sql.md).

Transzparens adattitkosítás használatával Bring Your Own Key-támogatással rendelkező indításához lásd: útmutatója [transzparens adattitkosítás kapcsolja be a Key Vaultból saját kulcs használatával a PowerShell-lel](transparent-data-encryption-byok-azure-sql-configure.md).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Transzparens-titkosítás által védett adatbázis áthelyezése

Nem kell adatbázisok Azure-ban műveletek visszafejtéséhez. A transzparens adattitkosítási beállítások a forrás-adatbázison vagy az elsődleges adatbázis transzparens módon öröklődnek a célon. Részét képező Operations jár:
- A GEO-visszaállítás.
- Önkiszolgáló időponthoz visszaállítását.
- Törölt adatbázis visszaállítása.
- Aktív georeplikáció.
- Adatbázis-másolat létrehozása.

Transzparens titkosítás által védett adatbázis exportálásakor az exportált tartalmat, az adatbázis nem titkosított. Az exportált tartalmat titkosítatlan BACPAC-fájlok tárolják. Győződjön meg arról, megfelelően védeni a BACPAC-fájlok és a transzparens adattitkosítás engedélyezése után az új adatbázis importálása befejeződött.

Például az a BACPAC-fájlba exportál egy helyszíni SQL Server-példány, ha az importált tartalom az új adatbázis nem automatikusan titkosítja. Hasonlóképpen ha a BACPAC-fájlba exportálja a helyszíni SQL Server-példány, az új adatbázis is nem automatikusan titkosítja.

Az egyetlen kivétel a és a egy SQL database-ből való exportálás. Transzparens adattitkosítás engedélyezve van az új adatbázisban található, de maga a BACPAC-fájlba továbbra is nem titkosított.

## <a name="manage-transparent-data-encryption-in-the-azure-portal"></a>Transzparens adattitkosítás az Azure Portalon kezelheti.

Az Azure Portalon keresztül transzparens adattitkosításának konfigurálásához, csatlakoznia kell az Azure tulajdonos, közreműködő vagy SQL-biztonságkezelő. 

Transzparens adattitkosítás meg az adatbázis-szint. Ahhoz, hogy az adatbázisok transzparens adattitkosítás, nyissa meg a [az Azure portal](https://portal.azure.com) , és jelentkezzen be az Azure-rendszergazdájának vagy Közreműködőjének fiókjával. Keresse meg a transzparens adattitkosítási beállítások szerint a felhasználói adatbázishoz. Alapértelmezés szerint a szolgáltatás által kezelt transzparens adattitkosítás szolgál. A transzparens adattitkosítási tanúsítványának automatikusan jön létre a kiszolgáló, amely tartalmazza az adatbázist. 

![Szolgáltatás által kezelt transzparens adattitkosítás](./media/transparent-data-encryption-azure-sql/service-managed-tde.png)  

Beállíthatja a transzparens titkosítási főkulcs, más néven a transzparens titkosítási védelmet a kiszolgáló szintjén. Transzparens adattitkosítás használata a Bring Your Own Key-támogatás és az adatbázisok védelmét a Key Vaultból egy kulccsal: a transzparens adattitkosítási beállítások a kiszolgáló alatt. 

![Transzparens adattitkosítás Bring Your Own Key-támogatással](./media/transparent-data-encryption-azure-sql/tde-byok-support.png) 

## <a name="manage-transparent-data-encryption-by-using-powershell"></a>Transzparens adattitkosítás kezelése a PowerShell használatával

Transzparens adattitkosítás Powershellen keresztüli konfigurálásához, csatlakoznia kell az Azure tulajdonos, közreműködő vagy SQL-biztonságkezelő. 

| Parancsmag | Leírás |
| --- | --- |
| [Set-AzureRmSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) |Engedélyezheti vagy letilthatja a-adatbázis transzparens adattitkosítás|
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) |Lekéri egy adatbázis transzparens titkosítási állapotát |
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption-Activity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) |Egy adatbázis a titkosítási folyamat állapotát ellenőrzi |
| [Adjon hozzá AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) |A Key Vault-kulcs ad hozzá egy SQL Server-példány |
| [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) |A Key Vault-kulcsok egy SQL server-példány beolvasása  |
| [Set-azurermsqlservertransparentdataencryptionprotector parancsmag](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) |A transzparens titkosítási védelmet egy SQL Server-példány beállítása |
| [Get-azurermsqlservertransparentdataencryptionprotector parancsmag](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) |Lekérdezi a transzparens titkosítási védelme |
| [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) |A Key Vault-kulcs távolít el egy SQL Server-példány |
|  | |

## <a name="manage-transparent-data-encryption-by-using-transact-sql"></a>Transzparens adattitkosítás kezelése Transact-SQL használatával

Csatlakozás az adatbázis, amely rendszergazdája vagy tagja bejelentkezés használatával a **dbmanager** szerepkör a master adatbázisban.

| Parancs | Leírás |
| --- | --- |
| [Az ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) | BEÁLLÍTVA TITKOSÍTÁSI be-/ kikapcsolási titkosítja, és mindig visszafejti az adatbázis |
| [sys.dm_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Titkosítási kulcsok egy adatbázis és a kapcsolódó adatbázis titkosítási állapotával kapcsolatos információkat ad vissza |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Információt ad vissza a titkosítási állapotát az egyes adatok adatraktár-csomópont és a társított adatbázis titkosítási kulcsai | 
|  | |

Nem lehet átváltani a transzparens titkosítási védelmet egy kulcsot a Key Vaultból Transact-SQL használatával. Használja a Powershellt vagy az Azure Portalon.

## <a name="manage-transparent-data-encryption-by-using-the-rest-api"></a>Transzparens adattitkosítás kezelése a REST API-val
 
A REST API-n keresztül transzparens adattitkosításának konfigurálásához, csatlakoznia kell az Azure tulajdonos, közreműködő vagy SQL-biztonságkezelő. 

| Parancs | Leírás |
| --- | --- |
|[Hozzon létre vagy frissítési kiszolgáló](/rest/api/sql/servers/createorupdate)|Az Azure Active Directory-identitás hozzáadása egy SQL Server-példány (hozzáférés biztosítása a Key Vault használatos)|
|[Hozzon létre vagy kiszolgálói kulcs frissítése](/rest/api/sql/serverkeys/createorupdate)|A Key Vault-kulcs ad hozzá egy SQL Server-példány|
|[Kiszolgálói kulcs törlése](/rest/api/sql/serverkeys/delete)|A Key Vault-kulcs távolít el egy SQL Server-példány|
|[Kiszolgáló-kulcsok beolvasása](/rest/api/sql/serverkeys/get)|Egy adott Key Vault-kulcs olvas be egy SQL Server-példány|
|[Kiszolgáló kiszolgálói kulcsok listázása](/rest/api/sql/serverkeys/listbyserver)|Az SQL Server-példányt a Key Vault-kulcsok beolvasása |
|[Létrehozás vagy frissítés titkosítási védelme](/rest/api/sql/encryptionprotectors/createorupdate)|A transzparens titkosítási védelmet egy SQL Server-példány beállítása|
|[Titkosítási beolvasása](/rest/api/sql/encryptionprotectors/get)|A transzparens titkosítási védelme SQL Server-példány beolvasása|
|[Lista titkosítási Protectors-kiszolgáló](/rest/api/sql/encryptionprotectors/listbyserver)|A transzparens titkosítási kulcsvédők egy SQL Server-példány beolvasása |
|[Hozzon létre vagy a transzparens titkosítási konfiguráció frissítése](/rest/api/sql/transparentdataencryptions/createorupdate)|Engedélyezheti vagy letilthatja a-adatbázis transzparens adattitkosítás|
|[Transzparens titkosítási konfiguráció beolvasása](/rest/api/sql/transparentdataencryptions/get)|Az adatbázis transzparens titkosítási konfiguráció beolvasása|
|[Lista transzparens titkosítási konfiguráció eredmények](/rest/api/sql/transparentdataencryptionactivities/ListByConfiguration)|Lekéri a titkosítási eredmény adatbázis|

## <a name="next-steps"></a>További lépések

- Transzparens adattitkosítás általános ismertetését lásd: [transzparens adattitkosítás] ((https://docs.microsoft.com/sql/relational-databases/security/transparent-data-encryption).
- Transzparens adattitkosítás az SQL Database és az adatraktár Bring Your Own Key-támogatásával kapcsolatos további információkért lásd: [transzparens adattitkosítás Bring Your Own Key-támogatással rendelkező](transparent-data-encryption-byok-azure-sql.md).
- Transzparens adattitkosítás használatával Bring Your Own Key-támogatással rendelkező indításához lásd: útmutatója [transzparens adattitkosítás kapcsolja be a Key Vaultból saját kulcs használatával a PowerShell-lel](transparent-data-encryption-byok-azure-sql-configure.md).
- A Key Vaulttal kapcsolatos további információkért tekintse meg a [Key Vault-dokumentációs oldalát](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
