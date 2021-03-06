---
title: Hozzáadhat és eltávolíthat egy Virtuálisgép-lemezkép az Azure Stackhez |} A Microsoft Docs
description: Adja hozzá, vagy távolítsa el a szervezet egyéni Windows vagy Linux rendszerű virtuális gép rendszerkép használata a bérlők számára.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: e5a4236b-1b32-4ee6-9aaa-fcde297a020f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 06/27/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: 5c2088ab39e32c049ce867698e84efba759c9a87
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447336"
---
# <a name="make-a-virtual-machine-image-available-in-azure-stack"></a>Egy virtuális gép rendszerképének elérhetővé az Azure Stackben

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Az Azure Stackben akkor is elérhetővé virtuálisgép-rendszerképek a felhasználók számára. Ezek a lemezképek hivatkozhat az Azure Resource Manager-sablonokkal, vagy felveheti őket az Azure Marketplace felhasználói felületén, a Piactéri elem. Vagy egy lemezkép-űrlap használata a globális Azure Marketplace-en, vagy adja hozzá a saját egyéni rendszerképét. Hozzáadhat egy virtuális Gépet a portálon vagy a Windows PowerShell használatával.

## <a name="add-a-vm-image-through-the-portal"></a>A portálon keresztül egy Virtuálisgép-rendszerkép hozzáadása

> [!NOTE]
> Ezzel a módszerrel külön-külön kell létrehozni a Piactéri elemet.

Lemezképek által blobtárolók URI azonosítójához használandó képesnek kell lennie. Készítse elő a VHD formátum (nem VHDX) a Windows vagy Linux operációs rendszer lemezképét, és ezután töltse fel a rendszerképet egy Azure-ban lévő tárfiókba. Ha a rendszerkép már fel van töltve a blob storage-ban az Azure-ban, kihagyhatja az 1. lépés.

1. [Windows Virtuálisgép-Rendszerkép feltöltése az Azure-bA a Resource Manager üzembe helyezések](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) vagy egy Linux-rendszerképek, kövesse a leírt utasításokat [üzembe helyezése Linux rendszerű virtuális gépek az Azure Stacken](azure-stack-linux.md). Mielőtt feltölti a lemezképet, fontos a következő tényezőket kell figyelembe venni:

   - Az Azure Stack a rögzített lemezes VHD formátumot támogatja. A rögzített formátum szerkezet a logikai lemezt lineárisan fájlon belül, így a lemez eltolás X van tárolva X. A blob végén egy kis lábléc írja le a VHD tulajdonságait. Győződjön meg arról, ha a lemezt rögzített, használja a [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) PowerShell-parancsot.  

    > [!IMPORTANT]
    >  Az Azure Stack nem támogatja a dinamikus lemez VHD-k. Egy virtuális Géphez van csatlakoztatva dinamikus lemez átméretezése sikertelen állapotban hagyja a virtuális Gépet. A probléma megoldásához törölje a virtuális Gépet a virtuális gép lemezét, a VHD-blob storage-fiókban lévő törlése nélkül. A, a konvertálás a virtuális Merevlemezt a dinamikus lemezről egy rögzített méretű lemezt, és hozza létre újból a virtuális gép.

   * Legyen hatékonyabb, ha egy rendszerkép feltöltése az Azure Stack blob storage-ba, mint az Azure blob storage-ban, mert a rendszerkép leküldése az Azure Stack lemezképtárban kevesebb időt vesz igénybe.

   * Feltöltése a [Windows Virtuálisgép-lemezkép](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/), le a **bejelentkezés az Azure** . lépés: az a [konfigurálása a PowerShell-környezet az Azure Stack-operátorokról](azure-stack-powershell-configure-admin.md) . lépés.  

   * Jegyezze fel a blob Storage URI, amikor feltölti a lemezképet. A blob storage URI azonosító formátuma a következő: *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;*.vhd.

   * Ahhoz, hogy a blob névtelenül elérhető-e, nyissa meg a storage-fiók blob tárolót, a Virtuálisgép-lemezkép VHD feltöltése. Válassza ki **Blob**, majd válassza ki **hozzáférési szabályzat**. Igény szerint is inkább a tároló közös hozzáférésű jogosultságkód létrehozása, és adja meg a blob URI részeként.

   ![Ugrás a storage-fiók BLOB](./media/azure-stack-add-vm-image/image1.png)

   ![A nyilvános blob-hozzáférés beállítása](./media/azure-stack-add-vm-image/image2.png)

2. Jelentkezzen be az Azure Stack operátori. Válassza a menüben **további szolgáltatások**. Ezután válassza ki **számítási** > **Virtuálisgép-rendszerképek** > **Hozzáadás**.

3. A **Virtuálisgép-rendszerkép hozzáadása**, írja be a közzétevő, ajánlat, Termékváltozat és a virtuálisgép-lemezkép verzióját. Ezek neve a szegmensek tekintse meg a Resource Manager-sablonok Virtuálisgép-rendszerkép. Ügyeljen arra, hogy válassza ki a **osType** megfelelő értékét. A **operációs rendszer Lemezblobjának URI azonosítója**, ahol a rendszerképet feltöltöttük Blob URI-t adja meg. Ezután válassza ki **létrehozás** a Virtuálisgép-lemezkép létrehozásának megkezdéséhez.

   ![A lemezkép létrehozásának megkezdése](./media/azure-stack-add-vm-image/image4.png)

   A rendszerkép sikeresen létrejött, amikor a virtuális gép rendszerkép állapota **sikeres**.

4. Ahhoz, hogy a virtuálisgép-lemezkép jobban elérhető a felhasználók által megjeleníthető a felhasználói felületen, érdemes [Piactéri elem létrehozása](azure-stack-create-and-publish-marketplace-item.md).

## <a name="remove-a-vm-image-through-the-portal"></a>Távolítsa el a Virtuálisgép-rendszerkép a portálon keresztül

1. Nyissa meg a felügyeleti portálon, a [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).

2. Válassza ki **Marketplace felügyeleti**, és válassza ki a törölni kívánt virtuális Gépet.

3. Kattintson a **Törlés** gombra.

## <a name="add-a-vm-image-to-the-marketplace-by-using-powershell"></a>Egy Virtuálisgép-lemezkép hozzáadása a Marketplace-en a PowerShell-lel

> [!Note]  
> Amikor hozzáadja a lemezkép csak akkor lesz elérhető az Azure Resource Manager-alapú sablonok és PowerShell központi telepítések. A lemezkép számára elérhetővé tenni a Piactéri elem, mint a felhasználók a cikkben ismertetett lépések Piactéri elem közzététele [létrehozása és a Piactéri elem közzététele](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-and-publish-marketplace-item)

1. [Az Azure Stack PowerShell telepítése](azure-stack-powershell-install.md).  

2. Jelentkezzen be az Azure Stack kezelőként. Útmutatásért lásd: [jelentkezzen be az Azure Stack-kezelőként](azure-stack-powershell-configure-admin.md).

3. Nyissa meg a PowerShell egy rendszergazda jogú parancssort, és futtassa:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<offer>" `
      -sku "<sku>" `
      -version "<#.#.#>” `
      -OSType "<ostype>" `
      -OSUri "<osuri>"
  ````

  A **Add-AzsPlatformimage** parancsmag a Virtuálisgép-lemezkép hivatkozik az Azure Resource Manager-sablonok segítségével értékeket adja meg. Az értékek a következők:
  - **publisher**  
    Például:`Canonical`  
    A közzétevő neve, a felhasználók használni, amikor a lemezképet Virtuálisgép-lemezkép szegmens. Például **Microsoft**. Vegyen fel egy szóközt vagy speciális karaktereket ezt a mezőt.  
  - **offer**  
    Például:`UbuntuServer`  
    Az ajánlat neve, a felhasználók használni, amikor azok a Virtuálisgép-rendszerkép üzembe helyezése Virtuálisgép-lemezkép szegmens. Például **WindowsServer**. Vegyen fel egy szóközt vagy speciális karaktereket ezt a mezőt.  
  - **sku**  
    Például:`14.04.3-LTS`  
    A felhasználók használni, amikor azok a Virtuálisgép-rendszerkép üzembe helyezése Virtuálisgép-lemezkép Termékváltozat neve szegmens. Például **Datacenter2016**. Vegyen fel egy szóközt vagy speciális karaktereket ezt a mezőt.  
  - **Verzió**  
    Például:`1.0.0`  
    A felhasználók használni, amikor azok a Virtuálisgép-rendszerkép üzembe helyezése Virtuálisgép-lemezkép verziója. Ez a verzió a következő formátumban kell  *\#.\#. \#*. Például **1.0.0-s**. Vegyen fel egy szóközt vagy speciális karaktereket ezt a mezőt.  
  - **osType**  
    Például:`Linux`  
    A kép a osType kell lennie, vagy **Windows** vagy **Linux**.  
  - **OSUri**  
    Például:`https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
    A blob-tároló URI-t is megadhat a egy `osDisk`.  

    További információkért tekintse meg a PowerShell-referencia, az a [Add-AzsPlatformimage](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage) parancsmag és a [New-DataDiskObject](https://docs.microsoft.com/powershell/module/Azs.Compute.Admin/New-DataDiskObject) parancsmagot.

## <a name="add-a-custom-vm-image-to-the-marketplace-by-using-powershell"></a>Egyéni Virtuálisgép-rendszerkép hozzáadása a Marketplace-en a PowerShell-lel

1. [Az Azure Stack PowerShell telepítése](azure-stack-powershell-install.md).

  ```PowerShell  
    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint $ArmEndpoint

    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience $GraphAudience

    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
      -EnvironmentName AzureStackAdmin

    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ```

2. Ha használ **Active Directory összevonási szolgáltatások**, használja a következő parancsmagot:

  ```PowerShell
  # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAuidence endpoint for your environment>"

  # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint
    ```

3. Jelentkezzen be az Azure Stack kezelőként. Útmutatásért lásd: [jelentkezzen be az Azure Stack-kezelőként](azure-stack-powershell-configure-admin.md).

4. Hozzon létre egy tárfiókot a globális Azure-ban tárolja az egyéni Virtuálisgép-rendszerképet. Útmutatásért lásd: [a rövid útmutató: blobok feltöltése, letöltése, és listát az Azure portal használatával](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

5. Készítse elő a VHD formátum (nem VHDX) a Windows vagy Linux operációs rendszer lemezképét, a Rendszerkép feltöltése a storage-fiók és az URI-t, amelyben a Virtuálisgép-lemezkép lekérhetők a PowerShell első.  

  ````PowerShell  
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ````

6. (Opcionális) Az adatlemezeket a Virtuálisgép-lemezkép részeként tömbje tölthet fel. Az adatlemezeket, a New-DataDiskObject parancsmaggal hozzon létre. Nyissa meg a Powershellt egy rendszergazda jogú parancssorba, majd futtassa:

  ````PowerShell  
    New-DataDiskObject -Lun 2 `
    -Uri "https://storageaccount.blob.core.windows.net/vhds/Datadisk.vhd"
  ````

7. Nyissa meg a PowerShell egy rendszergazda jogú parancssort, és futtassa:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" -offer "<offer>" -sku "<sku>" -version "<#.#.#>” -OSType "<ostype>" -OSUri "<osuri>"
  ````

    Az Add-AzsPlatformimage parancsmag és a New-DataDiskObject parancsmaggal kapcsolatos további információkért lásd: a Microsoft PowerShell [Azure Stack-operátorokról modul dokumentációjában](https://docs.microsoft.com/powershell/module/).

## <a name="remove-a-vm-image-by-using-powershell"></a>Egy Virtuálisgép-lemezkép törlése PowerShell használatával

Ha már nincs szüksége a virtuálisgép-lemezkép feltöltött, törölheti a Marketplace-ről a következő parancsmag használatával:

1. [Az Azure Stack PowerShell telepítése](azure-stack-powershell-install.md).

2. Jelentkezzen be az Azure Stack kezelőként.

3. Nyissa meg a PowerShell egy rendszergazda jogú parancssort, és futtassa:

  ````PowerShell  
  Remove-AzsPlatformImage `
    -publisher "<publisher>" `
    -offer "<offer>" `
    -sku "<sku>" `
    -version "<version>" `
  ````
  A **Remove-AzsPlatformImage** parancsmag a Virtuálisgép-lemezkép hivatkozik az Azure Resource Manager-sablonok segítségével értékeket adja meg. Az értékek a következők:
  - **publisher**  
    Például:`Canonical`  
    A közzétevő neve, a felhasználók használni, amikor a lemezképet Virtuálisgép-lemezkép szegmens. Például **Microsoft**. Vegyen fel egy szóközt vagy speciális karaktereket ezt a mezőt.  
  - **offer**  
    Például:`UbuntuServer`  
    Az ajánlat neve, a felhasználók használni, amikor azok a Virtuálisgép-rendszerkép üzembe helyezése Virtuálisgép-lemezkép szegmens. Például **WindowsServer**. Vegyen fel egy szóközt vagy speciális karaktereket ezt a mezőt.  
  - **sku**  
    Például:`14.04.3-LTS`  
    A felhasználók használni, amikor azok a Virtuálisgép-rendszerkép üzembe helyezése Virtuálisgép-lemezkép Termékváltozat neve szegmens. Például **Datacenter2016**. Vegyen fel egy szóközt vagy speciális karaktereket ezt a mezőt.  
  - **Verzió**  
    Például:`1.0.0`  
    A felhasználók használni, amikor azok a Virtuálisgép-rendszerkép üzembe helyezése Virtuálisgép-lemezkép verziója. Ez a verzió a következő formátumban kell  *\#.\#. \#*. Például **1.0.0-s**. Vegyen fel egy szóközt vagy speciális karaktereket ezt a mezőt.  
    
    A Remove-AzsPlatformImage parancsmaggal kapcsolatos további információkért lásd: a Microsoft PowerShell [Azure Stack-operátorokról modul dokumentációjában](https://docs.microsoft.com/powershell/module/).

## <a name="next-steps"></a>További lépések

[Virtuális gép kiépítése](azure-stack-provision-vm.md)
