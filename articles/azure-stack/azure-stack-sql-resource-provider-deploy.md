---
title: SQL-adatbázisok használata az Azure Stackben |} A Microsoft Docs
description: Ismerje meg, hogyan telepítheti az SQL-adatbázisok szolgáltatásként az Azure Stack és az első lépéseket az SQL Server erőforrás-szolgáltató adapter üzembe helyezéséhez.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: f53b1e08da1cb2d0dc02381bf47c27e8f84cb1d0
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044832"
---
# <a name="deploy-the-sql-server-resource-provider-on-azure-stack"></a>Az SQL Server erőforrás-szolgáltató az Azure Stack üzembe helyezése
Az Azure Stack SQL Server erőforrás-szolgáltató használatával teszi közzé az SQL Database-adatbázisok Azure Stack szolgáltatásként. Az erőforrás-szolgáltató SQL szolgáltatásként fut, a Windows Server 2016 Server Core virtuális gépeken (VM).

## <a name="prerequisites"></a>Előfeltételek

Nincsenek számos előfeltételt kell lennie a helyen, az Azure Stack SQL erőforrás-szolgáltató telepítése előtt. Mindezen követelmények teljesítése érdekében végezze el az olyan számítógépre, amelyen a kiemelt jogosultságú végpont a virtuális gép érhető el az alábbi lépéseket:

- Ha ezt még nem tette meg, [regisztrálása az Azure Stack](azure-stack-registration.md) az Azure-ral, letöltheti az Azure marketplace-elemek.
- Telepítenie kell az Azure és az Azure Stack PowerShell-modulokat a rendszer, amelyen futtatni fogja a telepítést. A rendszer a .NET-modul legújabb verzióját a Windows 10-es vagy Windows Server 2016 képnek kell lennie. Lásd: [PowerShell telepítése az Azure Stackhez](.\azure-stack-powershell-install.md).
- Az Azure Stack piactéren úgy, hogy letölti a szükséges Windows Server core virtuális gép hozzáadása a **Windows Server 2016 Datacenter - Server Core** kép. 
- Töltse le az SQL erőforrás-szolgáltató bináris, és futtassa a mappába, csomagolja ki a tartalmát egy ideiglenes könyvtárba. Az erőforrás-szolgáltató rendelkezik egy minimális megfelelő Azure Stack hozhat létre. Győződjön meg arról, hogy a megfelelő bináris Azure Stack, Ön által futtatott verziójának letöltése:

    |Az Azure Stack-verzió|SQL-RP-verzió|
    |-----|-----|
    |Verzió 1804 (1.0.180513.1)|[SQL-RP 1.1.24.0 verzió](https://aka.ms/azurestacksqlrp1804)
    |(1.0.180302.1) 1802-es verzió|[SQL-RP 1.1.18.0 verzió](https://aka.ms/azurestacksqlrp1802)|
    |     |     |

- Ellenőrizze, hogy adatközpont integrációja Előfeltételek teljesülését:

    |Előfeltétel|Leírások|
    |-----|-----|
    |Feltételes DNS-továbbítás megfelelően van beállítva.|[Az Azure Stack adatközpont integrációja - DNS](azure-stack-integrate-dns.md)|
    |Erőforrás-szolgáltatók bejövő portok nyitva.|[Az Azure Stack adatközpont integrációja – végpontok közzététele](azure-stack-integrate-endpoints.md#ports-and-protocols-inbound)|
    |PKI-tanúsítvány tulajdonosának és SAN helyesen van beállítva.|[Az Azure Stack üzembehelyezési kötelező nyilvános kulcsokra épülő infrastruktúra Előfeltételek](azure-stack-pki-certs.md#mandatory-certificates)<br>[Az Azure Stack üzembe helyezés PaaS tanúsítvány előfeltételei](azure-stack-pki-certs.md#optional-paas-certificates)|
    |     |     |

### <a name="certificates"></a>Tanúsítványok

_Csak az integrált rendszerek telepítés_. Meg kell adnia az SQL PaaS PKI-tanúsítványt a választható PaaS tanúsítványok szakaszában leírt [Azure Stack üzembe helyezési nyilvános kulcsokra épülő infrastruktúra követelményei](.\azure-stack-pki-certs.md#optional-paas-certificates). A megadott helyen helyezze el a .pfx-fájlt a **DependencyFilesLocalPath** paraméter. ASDK rendszerek nem biztosítanak egy tanúsítványt.

## <a name="deploy-the-sql-resource-provider"></a>Az SQL erőforrás-szolgáltató üzembe helyezése

Után minden előfeltétel telepítve van, futtassa a **DeploySqlProvider.ps1** üzembe helyezése erőforrás-szolgáltató az SQL-szkript. A DeploySqlProvider.ps1 parancsfájlt a SQL erőforrás-szolgáltató bináris letöltött az Azure Stack verziójának részeként ki kell olvasni.

Az SQL erőforrás-szolgáltató üzembe helyezése, nyissa meg a **új** emelt szintű PowerShell-ablakot (nem a PowerShell ISE), és módosítsa azt a könyvtárat, amelyben kibontotta az SQL resource provider bináris fájlokat. Azt javasoljuk, egy új PowerShell-ablakot a már betöltött PowerShell-modulok által okozott problémák elkerülése érdekében.

Futtassa a DeploySqlProvider.ps1 parancsfájlt, amely a következő feladatokat hajtja végre:

- A tanúsítványokat és más összetevőket tölt fel egy storage-fiókba az Azure Stacken.
- Katalóguscsomagok tesz közzé, így SQL-adatbázisok, a katalógus használatával telepítheti.
- Közzétesz egy gyűjteménycsomag üzemeltetési kiszolgáló üzembe helyezéséhez.
- Üzembe helyez egy virtuális Gépet a Windows Server 2016 core kép letöltött, és ezután telepíti az SQL erőforrás-szolgáltató használatával.
- Regisztrálja a helyi DNS-rekordot, amely a virtuális gép erőforrás-szolgáltató van leképezve.
- Az erőforrás-szolgáltató regisztrálása a helyi Azure Resource Managerrel az operátor fiók.

> [!NOTE]
> Az SQL erőforrás-szolgáltató telepítés indításakor, a **system.local.sqladapter** erőforráscsoportot kell létrehozni. Ez az erőforráscsoport, a szükséges központi telepítések befejezéséhez akár 75 perc is eltarthat.

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 paraméterek

A parancssorból a következő paramétereket is megadhat. Ha nem, vagy ha minden paraméter ellenőrzése sikertelen, kéri, hogy adja meg a szükséges paramétereket.

| Paraméter neve | Leírás | Megjegyzés vagy az alapértelmezett érték |
| --- | --- | --- |
| **CloudAdminCredential** | A felhő rendszergazdájához, a kiemelt végponthoz eléréséhez szükséges hitelesítő adatait. | _Szükséges_ |
| **AzCredential** | Az Azure Stack szolgáltatás-rendszergazdai fiók hitelesítő adatait. Használja az Azure Stack üzembe helyezéséhez használt hitelesítő adatokkal. | _Szükséges_ |
| **VMLocalCredential** | Az SQL-erőforrás-szolgáltató virtuális gép helyi rendszergazdai fiókjának hitelesítő adatait. | _Szükséges_ |
| **PrivilegedEndpoint** | Az IP-cím vagy a kiemelt végponthoz DNS-nevét. |  _Szükséges_ |
| **DependencyFilesLocalPath** | A csak integrált rendszerek a tanúsítvány .pfx fájlját ebben a könyvtárban kell elhelyezni. Szükség esetén egy Windows Update MSU csomag itt másolhatja. | _Nem kötelező_ (_kötelező_ integrált rendszerek) |
| **DefaultSSLCertificatePassword** | A .pfx tanúsítvány jelszava. | _Szükséges_ |
| **MaxRetryCount** | Többször ismételje meg minden művelet, ha sikertelen egy kívánt száma.| 2 |
| **RetryDuration** | Az időkorlát között eltelő időt másodpercben mérve. | 120 |
| **Eltávolítás** | Eltávolítja az erőforrás-szolgáltató és az összes társított erőforrást (lásd az alábbi megjegyzések). | Nem |
| **DebugMode** | Megakadályozza, hogy hiba esetén az automatikus tisztítás. | Nem |

## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>Az egyéni parancsfájl használata SQL erőforrás-szolgáltató üzembe helyezése

Minden manuális konfigurációs megszüntetésére, az erőforrás-szolgáltató üzembe helyezésekor, testre szabhatja az alábbi parancsfájlt. Az Azure Stack üzembe helyezéshez szükség szerint változtassa meg az alapértelmezett fiók adatait és a jelszavakat.

```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module  -Name AzureStack -RequiredVersion 1.3.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account can be Azure Active Directory or Active Directory Federation Services.
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change to the directory folder where you extracted the installation files. Do not provide a certificate on ASDK!
. $tempDir\DeploySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert

 ```

Amikor befejezi az erőforrás-szolgáltató telepítési parancsfájlt, frissítse a böngészőben győződjön meg arról, láthatja, hogy a legújabb frissítéseket.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Az Azure Stack portal használata a telepítés ellenőrzése

Használhatja a következő lépésekkel ellenőrizze, hogy az erőforrás-szolgáltató SQL sikeres üzembe helyezése.

1. Jelentkezzen be a felügyeleti portálon a szolgáltatás-rendszergazdaként.
2. Válassza ki **erőforráscsoportok**.
3. Válassza ki a **system.\< hely\>.sqladapter** erőforráscsoportot.
4. Az összefoglalás lapon erőforráscsoport áttekintése nincs sikertelen üzembe helyezés kell lennie.

      ![Az SQL erőforrás-szolgáltató telepítésének ellenőrzése](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)

## <a name="next-steps"></a>További lépések

[Üzemeltetési kiszolgáló hozzáadása](azure-stack-sql-resource-provider-hosting-servers.md)
