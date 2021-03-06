---
title: Az SQL-kiszolgáló áttelepítése a Microsoft Azure PowerShell használata az Azure Database Migration Service modul a helyszíni Azure SQL DB-hez |} A Microsoft Docs
description: Megismerheti, hogyan kell áttelepíteni a helyszíni SQL Serverről az Azure SQL Azure PowerShell használatával.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 08/05/2018
ms.openlocfilehash: ebcd145689ea1d947ca8895b37b39543b1e097ed
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525909"
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-db-using-azure-powershell"></a>A helyszíni SQL Server migrálása az Azure SQL DB, Azure PowerShell-lel
Ebben a cikkben telepít át a **Adventureworks2012** adatbázis egy helyi példányára, az SQL Server 2016 vagy újabb visszaállítása egy Azure SQL Database a Microsoft Azure PowerShell használatával. Telepíthet át adatbázisokat egy helyszíni SQL Server-példány az Azure SQL Database használatával a `AzureRM.DataMigration` a Microsoft Azure PowerShell-modulja.

Ebben a cikkben az alábbiakkal ismerkedhet meg:
> [!div class="checklist"]
> * Hozzon létre egy erőforráscsoportot.
> * Hozza létre az Azure Database Migration Service egy példányát.
> * Migrálási projekt létrehozása az Azure Database Migration Service-példányában.
> * Végezze el az áttelepítést.

## <a name="prerequisites"></a>Előfeltételek
A lépések elvégzéséhez szüksége:

- [Az SQL Server 2016 vagy újabb](https://www.microsoft.com/sql-server/sql-server-downloads) (bármely kiadás esetén)
- Ahhoz, hogy a TCP/IP protokoll, amely az SQL Server Express telepítése alapértelmezés szerint le van tiltva. Engedélyezze a TCP/IP protokollt a következő a [jelen cikkben lévő utasítások](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Konfigurálhatja a [hozzáféréshez a Windows tűzfal](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Az Azure SQL Database-példány. Létrehozhat egy Azure SQL Database-példány a következő a részletek a cikkben [egy Azure SQL database létrehozása az Azure Portalon](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 vagy újabb.
- Az Azure Database Migration Service van szükség a helyek közötti kapcsolatot a helyszíni adatforrás-kiszolgálók használatával biztosító Azure Resource Manager üzemi modell használatával létrehozott virtuális hálózat [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) vagy [ VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Befejeződött a Data Migration Assistant szolgáltatást használ, a cikkben leírtak szerint a helyi adatbázis és séma migrálási felmérést [ egy SQL Server migrálási felmérést végez](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem)
- Töltse le és telepítse a AzureRM.DataMigration modul PowerShell-galériából történő használatával [Install-Module PowerShell-parancsmag](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1)
- Az SQL Server-példányhoz való kapcsolódáshoz használt hitelesítő adatokkal kell rendelkeznie a [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) engedéllyel.
- A cél Azure SQL Database-példányhoz való kapcsolódáshoz használt hitelesítő adatok a cél Azure SQL Database-adatbázisok az adatbázis VEZÉRLÉSE engedéllyel kell rendelkeznie.
- Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Jelentkezzen be a Microsoft Azure-előfizetés
A cikk utasításokat használja [bejelentkezés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-4.4.1) PowerShell használatával jelentkezzen be az Azure-előfizetésében.

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot
Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. Hozzon létre egy erőforráscsoportot, egy virtuális gép létrehozása előtt.

Hozzon létre egy erőforráscsoportot a használatával a [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) parancsot. 

A következő példában létrehozunk egy erőforráscsoportot, nevű *myResourceGroup* a a *EastUS* régióban.

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```
## <a name="create-an-azure-database-migration-service-instance"></a>Hozzon létre egy Azure Database Migration Service-példányt 
Azure Database Migration Service új példányának használatával hozhat létre a `New-AzureRmDataMigrationService` parancsmagot. Ennek a parancsmagnak a következő szükséges paraméterek:
- *Azure erőforráscsoport-nevet*. Használhat [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) parancsot hozhat létre Azure-erőforráscsoporton előzőekben ismertetettek szerint, és a name paramétert adja meg.
- *Szolgáltatásnév*. Karakterlánc, amely megfelel a kívánt egyedi szolgáltatás nevének, Azure Database Migration Service 
- *Hely*. A szolgáltatás helyét adja meg. Adja meg az Azure data center egy helyen, például az USA nyugati RÉGIÓJA vagy Délkelet-Ázsia
- *Termékváltozat*. Ez a paraméter DMS Sku-név tartozik. Jelenleg támogatott Sku-nevek a következők *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*
- *Virtuális alhálózati azonosító*. Parancsmagot használhatja [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-4.4.1) alhálózat létrehozását. 

A következő példában létrehozunk egy elnevezett szolgáltatás *MyDMS* erőforráscsoportban *MyDMSResourceGroup*, található *USA keleti Régiójában* nevű virtuális hálózatot használó régió *MyVNET* és nevű alhálózat *MySubnet*.

```powershell
 $vNet = Get-AzureRmVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzureRmDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Migrálási projekt létrehozása
Miután létrehozott egy Azure Database Migration Service-példányt, hozzon létre egy migrálási projektet. Az Azure Database Migration Service-projekt kapcsolatadatok mind a forrás és cél-példányok, valamint az, hogy a projekt részeként migrálni kívánt adatbázisok listájának kell rendelkeznie.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Adatbázis-kapcsolati adatok objektum a forrás- és kapcsolatok létrehozása
Egy adatbázis-kapcsolati adatok objektum használatával is létrehozhat a `New-AzureRmDmsConnInfo` parancsmagot. Ennek a parancsmagnak a következő paraméterekkel:
- *ServerType*. Adatbázis-kapcsolatot kért, például az SQL, Oracle vagy MySQL típusú. Az SQL Server és SQL Azure SQL használata.
- *Adatforrás*. Név vagy IP-címét egy SQL-példányra vagy SQL Azure-kiszolgálón.
- *Érvénytelen AuthType*. A kapcsolat, amely lehet SqlAuthentication vagy WindowsAuthentication hitelesítési típusa.
- *TrustServerCertificate* paraméter, amely azt jelzi, hogy a csatorna során a rendszer megkerüli a tanúsítványlánc érvényesítése megbízhatósági walking titkosítva legyen-e értéket állítja be. Érték a true vagy FALSE (hamis) lehet.

Az alábbi példaparancs a forrás sql-hitelesítéssel MySQLSourceServer nevű SQL Server kapcsolati adatok objektum 

```powershell
$sourceConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource MySQLSourceServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Következő példa bemutatja a kapcsolati adatok sql-hitelesítéssel MySQLAzureTarget nevű SQL Azure adatbázis-kiszolgáló létrehozása

```powershell
$targetConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource "mysqlazuretarget.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Adja meg az adatbázisokat a migrálási projekt
Hozzon létre egy `AzureRmDataMigrationDatabaseInfo` , amely megadja az adatbázisok az Azure adatbázis-Migrálás részeként a projekt, amely objektumok adható meg paraméterként, a projekt létrehozásához. A parancsmag `New-AzureRmDataMigrationDatabaseInfo` AzureRmDataMigrationDatabaseInfo létrehozására használható. 

A következő példában létrehozunk `AzureRmDataMigrationDatabaseInfo` adatbázis AdventureWorks2016 projektre, és hozzáadja azt a listát adható meg paraméterként a projekt létrehozásához.

```powershell
$dbInfo1 = New-AzureRmDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks2016
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Projekt-objektum létrehozása
Végül hoz létre az Azure Database Migration-projekt nevű *MyDMSProject* található *USA keleti Régiójában* használatával `New-AzureRmDataMigrationProject` és a korábban létrehozott forrás- és kapcsolatok és listájának hozzáadása migrálni kívánt adatbázisok.

```powershell
$project = New-AzureRmDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLDB `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Hozzon létre, és indítsa el áttelepítési feladatot
Végül hozzon létre, és indítsa el az Azure adatbázis-Migrálási feladat. Az Azure adatbázis-Migrálási feladat kapcsolat hitelesítő adatokat igényel forrás és cél- és az előfeltételként létrehozott projekt már megadott információk mellett az áttelepítendő adatbázistáblák listája. 

### <a name="create-credential-parameters-for-source-and-target"></a>Forrás és a cél hitelesítő adat paramétereinek létrehozása
Kapcsolat biztonsági hitelesítő adatokat hozható létre egy [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) objektum. 

Az alábbi példa bemutatja *PSCredential* objektumokat biztosít a jelszavak, karakterlánc típusú változót a forrás- és cél kapcsolatok *$sourcePassword* és *$ targetPassword*. 

```powershell
secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-table-map-and-select-source-and-target-parameters-for-migration"></a>Hozzon létre egy tábla hozzárendelése és migráláshoz válassza forrás- és paraméterek
Az áttelepítéshez szükséges egy másik paraméter feltérképezése a forrásból táblák áttelepíteni kívánt cél. Hozzon létre táblák szótár, amely tartalmazza az áttelepítéshez a forrás- és táblák között. Az alábbi példában látható a forrás- és táblák közötti megfeleltetés az AdventureWorks 2016 adatbázis emberi erőforrások sémáját.

```powershell
$tableMap = New-Object 'system.collections.generic.dictionary[string,string]'
$tableMap.Add("HumanResources.Department", "HumanResources.Department")
$tableMap.Add("HumanResources.Employee","HumanResources.Employee")
$tableMap.Add("HumanResources.EmployeeDepartmentHistory","HumanResources.EmployeeDepartmentHistory")
$tableMap.Add("HumanResources.EmployeePayHistory","HumanResources.EmployeePayHistory")
$tableMap.Add("HumanResources.JobCandidate","HumanResources.JobCandidate")
$tableMap.Add("HumanResources.Shift","HumanResources.Shift")
```

A következő lépés az, hogy válassza ki a forrás- és adatbázisokat, és adja meg a tábla hozzárendelése használatával paraméterként áttelepíteni a `New-AzureRmDmsSqlServerSqlDbSelectedDB` parancsmag, az alábbi példában látható módon:

```powershell
$selectedDbs = New-AzureRmDmsSqlServerSqlDbSelectedDB -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -TableMap $tableMap
```

### <a name="create-and-start-a-migration-task"></a>Hozzon létre, és indítsa el áttelepítési feladatot

Használja a `New-AzureRmDataMigrationTask` parancsmag segítségével hozzon létre egy áttelepítési feladat elindításához. Ennek a parancsmagnak a következő paraméterekkel:
- *TaskType*. Áttelepítési feladat létrehozása az SQL Server az SQL Azure áttelepítési típus típusú *MigrateSqlServerSqlDb* várt. 
- *Erőforráscsoport-nevet*. Azure-erőforráscsoportot, amelyben létrehozza a feladat nevét.
- *Szolgáltatásnév*. Azure Database Migration Service példány, amelyben a feladat létrehozásához.
- *Projektnév*. Azure Database Migration-projekt, amelyben létrehozza a feladat neve. 
- *Feladatnév*. A létrehozandó feladat neve. 
- *Adatforrás-kapcsolat*. Adatforrás-kapcsolatot képviselő AzureRmDmsConnInfo objektum.
- *Kapcsolati cél*. AzureRmDmsConnInfo a cél kapcsolatot képviselő objektum.
- *SourceCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) objektum forrás-kiszolgálójához való kapcsolódás.
- *TargetCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) objektum a célkiszolgálóhoz való kapcsolódáshoz.
- *SelectedDatabase*. A forrás- és adatbázisok közötti képviselő AzureRmDmsSqlServerSqlDbSelectedDB objektum.

Az alábbi példa létrehoz és nevű myDMSTask áttelepítési feladat elindítása:

```powershell
$migTask = New-AzureRmDataMigrationTask -TaskType MigrateSqlServerSqlDb `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs`
```

## <a name="monitor-the-migration"></a>A migrálás figyelésére
Az áttelepítési feladat fut a feladat a következő példában látható módon állapot tulajdonságának lekérdezésével figyelheti:

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="next-steps"></a>További lépések
- Tekintse át a migrálási útmutató segítséget nyújt a Microsoft [adatbázis-Migrálási útmutató](https://datamigration.microsoft.com/).