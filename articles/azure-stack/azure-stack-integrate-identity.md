---
title: Az Azure Stack adatközpont integrációja - identitás
description: 'Útmutató: Azure Stack az AD FS integrálhatók az adatközponttaql, az AD FS'
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 08/01/2018
ms.author: jeffgilb
ms.reviewer: wfayed
keywords: ''
ms.openlocfilehash: c22831ae781239f2474720ee9cd1b58f6e14fe41
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412865"
---
# <a name="azure-stack-datacenter-integration---identity"></a>Az Azure Stack adatközpont integrációja - identitás
Az identitás-szolgáltatóktól, Azure Active Directory (Azure AD) vagy az Active Directory összevonási szolgáltatások (AD FS) használatával az Azure-verem üzembe helyezhető. Ellenőrizze a kiválasztott Azure Stack üzembe helyezése előtt. Az AD FS telepítési is nevezik üzembe helyezése az Azure Stack kapcsolat nélküli módban.

Az alábbi táblázat a két identitás lehetőségek közötti különbségeket mutatja:

||Kapcsolódik az internethez|Csatlakozik az internethez|
|---------|---------|---------|
|Számlázás|Kapacitás kell lennie.<br> Nagyvállalati Szerződés (EA) csak|Kapacitás vagy a fizetési mint-akkor-használható<br>Nagyvállalati szerződés vagy a Cloud Solution Provider (CSP)|
|Identitás|Az AD FS kell lennie.|Az Azure AD vagy az AD FS|
|Piactér |Támogatott<br>BYOL-licencelés|Támogatott<br>BYOL-licencelés|
|Regisztráció|Javasoljuk, cserélhető adathordozóra van szükség<br> és a egy különálló csatlakoztatott eszközön.|Automatikus|
|A javítások és frissítések|Szükséges, cserélhető adathordozóra van szükség<br> és a egy különálló csatlakoztatott eszközön.|Frissítési csomag közvetlenül letölthető<br> az Azure Stack az internetről.|

> [!IMPORTANT]
> Az identitásszolgáltató nem válthat a teljes Azure Stack megoldás újbóli üzembe helyezése nélkül.

## <a name="active-directory-federation-services-and-graph"></a>Active Directory összevonási szolgáltatások és Graph

Az AD FS telepítése lehetővé teszi, hogy identitások a az Azure Stackben erőforrásokkal történő hitelesítéshez meglévő Active Directory-erdőben. A meglévő Active Directory-erdő megköveteli, hogy egy AD FS összevonási megbízhatósági kapcsolat létrehozása az AD FS üzembe helyezése.

Hitelesítés az identitás egy részét. Szerepkör alapú hozzáférés-vezérlés (RBAC) az Azure Stackben kezeléséhez, a Graph-összetevőt kell konfigurálni. Erőforráshoz való hozzáférés delegálása, a Graph-összetevő jelenik meg a felhasználói fiókot a meglévő Active Directory-erdőben, az LDAP protokoll használatával.

![Az Azure Stack az AD FS-architektúra](media/azure-stack-integrate-identity/Azure-Stack-ADFS-architecture.png)

A meglévő AD FS a fiók biztonsági jogkivonat-szolgáltatás (STS) által küldött jogcímek, az Azure Stack Active Directory összevonási szolgáltatások (az erőforrás STS). Az Azure Stackben automation hoz létre a jogcím-szolgáltatói megbízhatóság a metaadatok végpontján a meglévő AD FS számára.

A meglévő AD FS-ben található egy függőentitás-megbízhatóságot kell konfigurálni. Ebben a lépésben nem történik meg az automation, és meg kell adni az operátor. Az Azure Stack-metaadatok végpontja dokumentálja a AzureStackStampDeploymentInfo.JSON fájlban, vagy a kiemelt jogosultságú végponton keresztül a parancs futtatásával `Get-AzureStackInfo`.

A függő entitás megbízhatósági konfiguráció is megköveteli, hogy a Microsoft által biztosított jogcím-átalakítási szabályok konfigurálása.

A Graph-konfiguráció egy szolgáltatási fióknak kell lennie, feltéve, hogy rendelkezik-e olvasási engedéllyel a meglévő Active Directoryban. A fiókra akkor szükség ahhoz, hogy az RBAC-forgatókönyvek az automation bemenetként.

Az alapértelmezett szolgáltatója előfizetést az utolsó lépés egy új tulajdonos van konfigurálva. Ez a fiók összes olyan erőforrást, ha bejelentkezett az Azure Stack rendszergazdai portálon a teljes hozzáféréssel rendelkezik.

Követelmények:


|Összetevő|Követelmény|
|---------|---------|
|Graph|A Microsoft Active Directory 2012/2012 R2/2016|
|AD FS|A Windows Server 2012/2012 R2/2016|

## <a name="setting-up-graph-integration"></a>Graph-integráció beállítása

Gráf csak egyetlen Active Directory-erdővel rendelkező integrációs támogatja. Ha több erdő, csak a konfigurációban megadott erdő használandó felhasználók és csoportok beolvasása.

A következő információkat szükség, mint a bemenetek az automatizálás paraméterek:


|Paraméter|Leírás|Példa|
|---------|---------|---------|
|CustomADGlobalCatalog|A cél az Active Directory-erdő teljes Tartományneve<br>hogy szeretné-e integrálása|Contoso.com|
|CustomADAdminCredentials|Egy LDAP-olvasási engedéllyel rendelkező felhasználó|YOURDOMAIN\graphservice|

### <a name="create-user-account-in-the-existing-active-directory-optional"></a>Felhasználói fiók létrehozása a meglévő Active Directoryban (nem kötelező)

Igény szerint hozhat létre egy fiókot a Graph szolgáltatás a meglévő Active Directory. Hajtsa végre ezt a lépést, ha még nem rendelkezik egy használni kívánt fiókot.

1. A meglévő Active Directory létre a következő felhasználói fiók (javaslat):
   - **Felhasználónév**: graphservice
   - **Jelszó**: erős jelszó használata<br>Konfigurálja a jelszó soha ne járjon le.

   Nincsenek különleges engedélyekhez és a tagság nem szükséges.

#### <a name="trigger-automation-to-configure-graph"></a>Az eseményindító automation graph konfigurálása

Ebben az eljárásban az Adatközpont-hálózatát, amely képes kommunikálni az Azure Stack a rendszerjogosultságú végpont számítógépet használni.

2. Nyisson meg egy rendszergazda jogú Windows PowerShell-munkamenetet (Futtatás rendszergazdaként), és csatlakozzon a rendszerjogosultságú végpont IP-címét. A hitelesítő adatok használata **CloudAdmin** hitelesítéséhez.

   ```PowerShell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

3. Most, hogy a kiemelt végponthoz csatlakozik, futtassa a következő parancsot: 

   ```PowerShell  
   Register-DirectoryService -CustomADGlobalCatalog contoso.com
   ```

   Amikor a rendszer kéri, adja meg, hogy a Graph szolgáltatás (például graphservice) használni kívánt felhasználói fiók hitelesítő adatait. A Register-DirectoryService parancsmag bemenete kell lennie az erdő nevét / az erdő tartományi helyett a más tartományokba az erdő gyökér.

   > [!IMPORTANT]
   > Várja meg a hitelesítő adatait az előugró (a Get-Credential nem támogatott a rendszerjogosultságú végpontját), és adja meg a Graph szolgáltatás fiók hitelesítő adatait.

#### <a name="graph-protocols-and-ports"></a>Graph-protokollok és portok

Graph szolgáltatás az Azure Stack egy írható globális katalógus Server (GC) és a kulcsszolgáltató (KDC), és a cél az Active Directory-erdőben található bejelentkezési kérések feldolgozására képes kommunikálni a következő protokollokat és portokat használja.

Az Azure Stackben Graph szolgáltatás a következő protokollok és portok segítségével kommunikál a cél az Active Directory:

|Típus|Port|Protokoll|
|---------|---------|---------|
|LDAP|389|A TCP ÉS UDP|
|LDAP SSL|636|TCP|
|LDAP GLOBÁLIS KATALÓGUS|3268|TCP|
|LDAP GC SSL|3269|TCP|

## <a name="setting-up-ad-fs-integration-by-downloading-federation-metadata"></a>Az AD FS-integráció beállítása az összevonási metaadatok letöltésével

Az alábbi adatokra szükség az automation-paraméterek bemenetként:

|Paraméter|Leírás|Példa|
|---------|---------|---------|
|CustomAdfsName|A jogcímeket szolgáltató neve. <cr>Úgy tűnik, így az AD FS kezdőlapján.|Contoso|
|CustomAD<br>FSFederationMetadataEndpointUri|Összevonási metaadatok hivatkozás|https://ad01.contoso.com/federationmetadata/2007-06/federationmetadata.xml|


### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Az eseményindító automation beállítása az Azure Stack a jogcím-szolgáltatói megbízhatóság

Ebben az eljárásban használja olyan számítógépre, amely képes kommunikálni az Azure Stack a kiemelt végponthoz. Valószínű, hogy a tanúsítványt használja a fiók **az AD STS FS** Azure Stack megbízhatónak.

1. Nyisson meg egy rendszergazda jogú Windows PowerShell-munkamenetet, és a kiemelt végponthoz csatlakozik.

   ```PowerShell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Most, hogy a kiemelt végponthoz csatlakozik, futtassa a következő parancsot, a környezetnek megfelelő paraméterek használatával:

   ```PowerShell  
   Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataEndpointUri https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml
   ```

3. Futtassa a következő parancsot a környezetnek megfelelő paraméterekkel az alapértelmezett szolgáltatója előfizetés tulajdonosának frissítése:

   ```PowerShell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

## <a name="setting-up-ad-fs-integration-by-providing-federation-metadata-file"></a>Összevonási metaadatait tartalmazó fájl azáltal, hogy az AD FS-integráció beállítása

Ezt a módszert használja, a következő feltételek bármelyikének teljesülése esetén:

- A tanúsítványlánc nem egyezik az AD FS az Azure Stackben minden más végpontok képest.
- Nincs hálózati kapcsolat a meglévő AD FS-kiszolgálóra az AD FS-példányt az Azure Stack.

Az alábbi adatokra szükség az automation-paraméterek bemenetként:


|Paraméter|Leírás|Példa|
|---------|---------|---------|
|CustomAdfsName|A jogcímeket szolgáltató neve. Ezzel a módszerrel a az AD FS kezdőlapja jelenik meg.|Contoso|
|CustomADFSFederationMetadataFileContent|Metaadat-tartalom|$using: federationMetadataFileContent|

### <a name="create-federation-metadata-file"></a>Összevonási metaadatait tartalmazó fájl létrehozása

A következő eljárás olyan számítógépen, amelyen a hálózati kapcsolat és a meglévő AD FS üzembe helyezése, amely a fiók STS válik kell használnia. Emellett a szükséges tanúsítványokkal kell telepíteni.

1. Nyisson meg egy rendszergazda jogú Windows PowerShell-munkamenetet, és futtassa a következő parancsot, és válasszon a környezetének megfelelő paraméterekkel:

   ```PowerShell  
   [XML]$Metadata = Invoke-WebRequest -URI https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml -UseBasicParsing

   $Metadata.outerxml|out-file c:\metadata.xml
   ```

2. Másolja a metaadatait tartalmazó fájl egy számítógépre, amely képes kommunikálni a kiemelt végponthoz.

### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Az eseményindító automation beállítása az Azure Stack a jogcím-szolgáltatói megbízhatóság

Ez az eljárás, amely képes kommunikálni az Azure Stack a rendszerjogosultságú végpont, és hozzáférjen a metaadat-fájlt az előző lépésben létrehozott számítógépet használni.

1. Nyisson meg egy rendszergazda jogú Windows PowerShell-munkamenetet.

   ```PowerShell  
   $federationMetadataFileContent = get-content c:\metadata.cml
   $creds=Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataFileContent $using:federationMetadataFileContent
   ```

2. Futtassa a következő parancsot a környezetnek megfelelő paraméterekkel az alapértelmezett szolgáltatója előfizetés tulajdonosának frissítése:

   ```PowerShell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

## <a name="configure-relying-party-on-existing-ad-fs-deployment-account-sts"></a>Függő entitás konfigurálása a meglévő AD FS üzembe helyezése (STS-fiók)

A Microsoft biztosít egy parancsfájlt, amely a függőentitás-megbízhatóságot, többek között a jogcím-átalakítási szabályok konfigurálása. A parancsfájlt nem kötelező használni, mivel a parancsok manuálisan is futtatható.

A segítő szkriptet letöltheti [Azure Stack eszközök](https://github.com/Azure/AzureStack-Tools/tree/vnext/DatacenterIntegration/Identity) a Githubon.

Ha úgy dönt, hogy manuálisan futtassa a parancsokat, kövesse az alábbi lépéseket:

1. A helyi adatközpontban az AD FS-példány vagy farm tag egy .txt fájl (például c:\ClaimRules.txt elmentve) másolja az alábbi tartalommal:

   ```text
   @RuleTemplate = "LdapClaims"
   @RuleName = "Name claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"), query = ";userPrincipalName;{0}", param = c.Value);

   @RuleTemplate = "LdapClaims"
   @RuleName = "UPN claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"), query = ";userPrincipalName;{0}", param = c.Value);

   @RuleTemplate = "LdapClaims"
   @RuleName = "ObjectID claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"]
   => issue(Type = "http://schemas.microsoft.com/identity/claims/objectidentifier", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, Value = c.Value, ValueType = c.ValueType);

   @RuleName = "Family Name and Given claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname", "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"), query = ";sn,givenName;{0}", param = c.Value);

   @RuleTemplate = "PassThroughClaims"
   @RuleName = "Pass through all Group SID claims"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
   => issue(claim = c);

   @RuleTemplate = "PassThroughClaims"
   @RuleName = "Pass through all windows account name claims"
   c:[Type == http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname]
   => issue(claim = c);
   ```

2. Windows Forms-alapú hitelesítés engedélyezéséhez nyisson meg egy Windows PowerShell-munkamenetet emelt szintű felhasználóként, és futtassa a következő parancsot:

   ```PowerShell  
   Set-AdfsProperties -WIASupportedUserAgents @("MSAuthHost/1.0/In-Domain","MSIPC","Windows Rights Management Client","Kloud")
   ```

3. A függő entitás megbízhatóságának hozzáadása, futtassa a következő Windows PowerShell-parancsot az AD FS-példány vagy farm tagja. Ne feledje frissíteni az AD FS-végpont, és az 1. lépésben létrehozott fájlra mutat.

   **Az AD FS 2016**

   ```PowerShell  
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true -AccessControlPolicyName "Permit everyone"
   ```

   **Az AD FS 2012/2012 R2-ben**

   ```PowerShell  
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true
   ```

   > [!IMPORTANT]
   > Kell használnia az AD FS beépülő MMC-modulban a Kiállításengedélyezési szabályok konfigurálása a Windows Server 2012 vagy 2012 R2 AD FS használata esetén.

4. Azure Stack eléréséhez használt Internet Explorer vagy a Microsoft Edge böngészőt, amikor token kötések, kell figyelmen kívül. Ellenkező esetben a bejelentkezési kísérlet sikertelen. Az AD FS-példány vagy farm tagja futtassa a következő parancsot:

   > [!note]  
   > Ez a lépés nem alkalmazható, ha a Windows Server 2012 vagy 2012 R2 AD FS használatával. Hagyja ki ezt a parancsot, és folytassa az integráció biztonságos legyen.

   ```PowerShell  
   Set-AdfsProperties -IgnoreTokenBinding $true
   ```

5. Az Azure Stack-portálok és azokat az eszközöket (a Visual Studio) szükséges frissítési biztonsági jogkivonat. Ezeket a függő entitás kell konfigurálni. Nyisson meg egy rendszergazda jogú Windows PowerShell-munkamenetet, és futtassa a következő parancsot:

   ```PowerShell  
   Set-ADFSRelyingPartyTrust -TargetName AzureStack -TokenLifeTime 1440
   ```

## <a name="spn-creation"></a>Egyszerű szolgáltatásnév létrehozása

Nincsenek számos forgatókönyv, amely egy egyszerű szolgáltatásnevét (SPN) hitelesítés használatának megkövetelése. Néhány példa a következők:

- Az AD FS üzembe helyezése az Azure Stack parancssori felület használat
- A System Center felügyeleti csomag az Azure Stackhez az AD FS telepítésekor
- Erőforrás-szolgáltatók az Azure Stackben és az AD FS telepítésekor
- Számos olyan alkalmazás
- A nem interaktív bejelentkezést igényelnek

> [!Important]  
> Az AD FS csak az interaktív bejelentkezési munkamenetek támogatja. Ha egy nem interaktív bejelentkezést egy automatizált keresztül van szükség, egy egyszerű Szolgáltatásnevet kell használnia.

Egy egyszerű Szolgáltatásnevet létrehozásával kapcsolatos további információkért lásd: [az AD FS egyszerű szolgáltatás létrehozása](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-ad-fs).


## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="configuration-rollback"></a>Visszaállítási konfiguráció

Ha hiba lép fel, amelyek a környezetet, akkor nem tudják hitelesíteni állapotban hagyja, a visszaállítási lehetőség érhető el.

1. Nyisson meg egy rendszergazda jogú Windows PowerShell-munkamenetet, és futtassa a következő parancsokat:

   ```PowerShell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ezután futtassa a következő parancsmagot:

   ```PowerShell  
   Reset-DatacenterIntegationConfiguration
   ```

   Miután a visszaállítási művelethez, az összes konfigurációs módosítás visszavonásra. Csak a beépített hitelesítés **CloudAdmin** felhasználói lehetőség.

   > [!IMPORTANT]
   > Konfigurálnia kell az eredeti tulajdonost az alapértelmezett szolgáltatója előfizetés

   ```PowerShell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "azurestackadmin@[Internal Domain]"
   ```

### <a name="collecting-additional-logs"></a>További naplók gyűjtését.

A parancsmagok bármelyike sikertelen, ha további naplók segítségével begyűjtheti a `Get-Azurestacklogs` parancsmagot.

1. Nyisson meg egy rendszergazda jogú Windows PowerShell-munkamenetet, és futtassa a következő parancsokat:

   ```PowerShell  
   $creds = Get-Credential
   Enter-pssession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ezután futtassa a következő parancsmagot:

   ```PowerShell  
   Get-AzureStackLog -OutputPath \\myworstation\AzureStackLogs -FilterByRole ECE
   ```


## <a name="next-steps"></a>További lépések

[Külső figyelési megoldások integrálása](azure-stack-integrate-monitor.md)