---
title: Az Azure stack-beli szolgáltatásnév létrehozása |} A Microsoft Docs
description: Ismerteti, hogyan hozhat létre egy új egyszerű szolgáltatást, amely a szerepköralapú hozzáférés-vezérlés az Azure Resource Manager-erőforrásokhoz való hozzáférés kezelésére használható.
services: azure-resource-manager
documentationcenter: na
author: mattbriggs
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/21/2018
ms.author: mabrigg
ms.openlocfilehash: 0db3f19c99b786d7f32f126ad7bd70efc999a751
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444271"
---
# <a name="provide-applications-access-to-azure-stack"></a>Hozzáférést biztosít az alkalmazásoknak az Azure Stackhez

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Amikor egy alkalmazás központi telepítése, illetve az Azure Resource Manager erőforrások konfigurálása az Azure Stackben hozzáférésre van szüksége, hozzon létre egy egyszerű szolgáltatás, amely az alkalmazás hitelesítő adatot.  Ezután delegálhat egyszerű szolgáltatást csak a szükséges engedélyekkel.  

Tegyük fel szükség lehet egy Konfigurációkezelő eszközzel, amely a készlet az Azure-erőforrások Azure Resource Manager használatával.  Ebben a forgatókönyvben egyszerű szolgáltatás létrehozása, az Olvasó szerepkör biztosítani az egyszerű szolgáltatást, és korlátozhatja a csak olvasási hozzáféréssel az eszközbe. 

Szolgáltatásnevek használata előnyösebb az alkalmazást a saját hitelesítő adatokkal futtatja, mivel:

* Engedélyeket rendelhet a szolgáltatás egyszerű, amelyek eltérnek a saját fiók engedélyeit. Ezek az engedélyek jellemzően csak azt engedélyezik, amire az alkalmazásnak szüksége van.
* Nem kell módosítani az alkalmazás hitelesítő adatait, ha az Ön feladatkörei módosítása.
* A tanúsítvány segítségével automatizálhatja a hitelesítést egy felügyelet nélküli parancsfájl végrehajtása közben.  

## <a name="getting-started"></a>Első lépések

Attól függően, hogyan telepített Azure Stack első lépésként létrehozni egy szolgáltatásnevet.  Ez a dokumentum végigvezeti a szolgáltatásnév létrehozása [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad) és [Active Directory összevonási Services(AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).  Az egyszerű szolgáltatás létrehozása után használható-e az AD FS és az Azure Active Directory közös ismertetett lépések [engedélyeket delegálhatnak](azure-stack-create-service-principals.md#assign-role-to-service-principal) a szerepkörhöz.     

## <a name="create-service-principal-for-azure-ad"></a>Az Azure ad egyszerű szolgáltatás létrehozása

Ha már telepítette az Azure AD az ügyfélidentitás-tárolóval, mint az Azure Stack, létrehozhat egyszerű szolgáltatásokat, mint az Azure-ban végezhet el.  Ez a szakasz bemutatja, hogyan végezheti el a lépéseket a portálon keresztül.  Ellenőrizze, hogy rendelkezik a [szükséges Azure AD-engedélyekről](../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) megkezdése előtt.

### <a name="create-service-principal"></a>Egyszerű szolgáltatás létrehozása
Ebben a szakaszban az alkalmazást képviselő Azure AD-alkalmazásokhoz (egyszerű szolgáltatásnevének) létrehozása.

1. Jelentkezzen be az Azure-fiók révén a [az Azure portal](https://portal.azure.com).
2. Válassza ki **Azure Active Directory** > **alkalmazásregisztrációk** > **hozzáadása**   
3. Adja meg az alkalmazás nevét és URL-címét. Válassza ki vagy **webalkalmazás / API** vagy **natív** szeretne létrehozni az alkalmazás számára. Miután beállította az értékeket, válassza ki a **létrehozás**.

Létrehozott egy egyszerű szolgáltatást az alkalmazás.

### <a name="get-credentials"></a>Hitelesítő adatok beolvasása
Ha programozott módon jelentkezik be, ezt az Azonosítót használja az alkalmazáshoz, és a egy webalkalmazás / API-t, a hitelesítési kulcs. Az értékek beszerzéséhez kövesse az alábbi lépéseket:

1. A **alkalmazásregisztrációk** az Active Directoryban, válassza ki az alkalmazását.

2. Másolja ki az **Alkalmazásazonosítót**, és tárolja az alkalmazás kódjában. Az alkalmazások a [mintaalkalmazások](#sample-applications) szakaszban tekintse meg ezt az értéket az ügyfél-azonosítót.

     ![ügyfél-azonosító](./media/azure-stack-create-service-principal/image12.png)
3. Webes alkalmazás a hitelesítési kulcs létrehozásához / API-t, jelölje ki **beállítások** > **kulcsok**. 

4. Adjon meg egy leírást és egy időtartamot a kulcshoz. Ha elkészült, kattintson a **Mentés** elemre.

A kulcs mentése után megjelenik a kulcs értéke. Másolja ezt az értéket, mivel később nem lesz lehetősége lekérni a kulcsot. A kulcs értékét az alkalmazás aláírásához az alkalmazás azonosítójával adja meg. A kulcsértéket olyan helyen tárolja, ahonnan az alkalmazás le tudja kérni.

![mentett kulcs](./media/azure-stack-create-service-principal/image15.png)


Ha elkészült, lépjen tovább [az alkalmazás-szerepkör hozzárendelése](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>Az AD FS egyszerű szolgáltatás létrehozása
Ha telepítette az AD FS az Azure Stack, PowerShell segítségével hozzon létre egy egyszerű szolgáltatást, hozzáférés szerepkör hozzárendelése és jelentkezzen be a PowerShell használatával az identitásukat.

A parancsfájl-ERCS virtuális gépen fut az emelt szintű végpontról.


Követelmények:
- Egy tanúsítványra szükség.

**Paraméterek**

Az alábbi adatokra szükség az automation-paraméterek bemenetként:


|Paraméter|Leírás|Példa|
|---------|---------|---------|
|Name (Név)|Az SPN-fiók nevét|MyAPP|
|ClientCertificates|Tanúsítvány-objektumok tömbje|X509 tanúsítvány|
|ClientRedirectUris<br>(Választható lehetőség)|Alkalmazás átirányítási URI-ja|         |

**Példa**

1. Nyisson meg egy rendszergazda jogú Windows PowerShell-munkamenetet, és futtassa a következő parancsokat:

   > [!NOTE]
   > Ez a példa létrehoz egy önaláírt tanúsítványt. Ezen parancsok futtatása termelési környezetben, ha a Get-tanúsítvány használatával kérje le a használni kívánt tanúsítványt a tanúsítvány objektumot.

   ```PowerShell  
    # Credential for accessing the ERCS PrivilegedEndpoint typically domain\cloudadmin
    $creds = Get-Credential

    # Creating a PSSession to the ERCS PrivilegedEndpoint
    $session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $creds

    # This produces a self signed cert for testing purposes.  It is prefered to use a managed certificate for this.
    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<yourappname>" -KeySpec KeyExchange

    $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name '<yourappname>' -ClientCertificates $using:cert}
    $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }
    $session|remove-pssession

    # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

    # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

    # TenantID for the stamp. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $TenantID = $AzureStackInfo.AADTenantID

    # Register an AzureRM environment that targets your Azure Stack instance
    Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

    # Set the GraphEndpointResourceId value
    Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience `
    -EnableAdfsAuthentication:$true

    Add-AzureRmAccount -EnvironmentName "azurestackuser" `
    -ServicePrincipal `
    -CertificateThumbprint $ServicePrincipal.Thumbprint `
    -ApplicationId $ServicePrincipal.ClientId `
    -TenantId $TenantID
   ```

2. Az automation befejezését követően a szükséges adatokat, az egyszerű szolgáltatásnév használatával jeleníti meg. 

   Példa:

   ```
   ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
   ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
   Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
   ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
   PSComputerName        : azs-ercs01
   RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
   ```

### <a name="assign-a-role"></a>Szerepkör hozzárendelése
Az egyszerű szolgáltatás létrehozása után kell [rendelje hozzá egy szerepkörhöz](azure-stack-create-service-principals.md#assign-role-to-service-principal)

### <a name="sign-in-through-powershell"></a>Jelentkezzen be a PowerShell-lel
Miután hozzárendelte a szerepkört, bejelentkezhet az Azure Stackhez az egyszerű szolgáltatás használatával a következő paranccsal:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ClientId ` 
 -TenantId $directoryTenantId
```

## <a name="assign-role-to-service-principal"></a>Egyszerű szolgáltatás szerepkör hozzárendelése
Az előfizetésben lévő erőforrások eléréséhez, hozzá kell rendelnie az alkalmazás egy szerepkörhöz. Döntse el, melyik szerepkör jelöli az alkalmazást a megfelelő engedélyekkel. Az elérhető szerepkörök kapcsolatos további információkért lásd: [RBAC: beépített szerepkörök](../role-based-access-control/built-in-roles.md).

Beállíthatja a hatókör szintjén is az előfizetés, erőforráscsoport vagy erőforrás. Alacsonyabb szintű hatókör, az engedélyek öröklődnek. Például egy alkalmazás az Olvasó szerepkörhöz, egy erőforráscsoport hozzáadása azt jelenti, hogy azt az erőforráscsoportot és az összes benne található erőforrást olvashatja.

1. Az Azure Stack portálon lépjen a hatókör az alkalmazást hozzárendelni kívánt szintjét. Válassza ki például az előfizetések szintjén szerepkör hozzárendelése **előfizetések**. Ehelyett kiválaszthatja egy erőforráscsoportra vagy erőforrásra.

2. Válassza ki az adott előfizetés (erőforráscsoportra vagy erőforrásra) az alkalmazás hozzárendelése a.

     ![Válasszon hozzárendelés előfizetést](./media/azure-stack-create-service-principal/image16.png)

3. Válassza ki **hozzáférés-vezérlés (IAM)**.

     ![Jelölje be a hozzáférés](./media/azure-stack-create-service-principal/image17.png)

4. Válassza a **Hozzáadás** lehetőséget.

5. Válassza ki a az alkalmazáshoz hozzárendelni kívánt szerepkört.

6. Keresse meg az alkalmazást, és válassza ki azt.

7. Válassza ki **OK** befejeződik, a szerepkör hozzárendelése. Láthatja, hogy az alkalmazás a felhasználók az adott hatókörnél szerepköre a listában.

Most, hogy már létrehozott egy egyszerű szolgáltatást és szerepkört rendel hozzá, megkezdheti ezzel az Azure Stack-erőforrások eléréséhez az alkalmazáson belül.  

## <a name="next-steps"></a>További lépések

[Felhasználók hozzáadása az AD FS](azure-stack-add-users-adfs.md)
[felhasználói engedélyek kezelése](azure-stack-manage-permissions.md)
