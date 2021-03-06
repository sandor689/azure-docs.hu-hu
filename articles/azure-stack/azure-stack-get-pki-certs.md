---
title: Azure verem nyilvános kulcsokra épülő infrastruktúrát tanúsítványok integrált Azure verem rendszerek központi telepítés létrehozása |} Microsoft Docs
description: Integrált Azure verem rendszerek Azure verem PKI tanúsítványt telepítési folyamatát mutatjuk be.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: b5adc1bb5a5aae96f37cc312588aa71e57d8342e
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083226"
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Az Azure verem tanúsítványok aláírási kérelem létrehozása

A jelen cikkben ismertetett Azure verem készültségi ellenőrző eszköz [a PowerShell-galériából](https://aka.ms/AzsReadinessChecker). Az eszköz létrehoz egy Azure Alkalmazásveremben üzembe alkalmas aláíró tanúsítványkérelmek (ügyfélszolgálati). Tanúsítványok legyen kért, jön létre, és elegendő időt a telepítés előtt tesztelje érvényesíteni.

A Azure verem készültségi-ellenőrző eszközt (AzsReadinessChecker) a következő tanúsítványkérelmek hajtja végre:

 - **Standard tanúsítványkérelmek**  
    A következők szerint kérelem [PKI-tanúsítványok létrehozása az Azure verem üzembe helyezéséhez](azure-stack-get-pki-certs.md).
 - **Platform,--szolgáltatás**  
    Opcionálisan a platform,--szolgáltatás (PaaS) nevek a megadott tanúsítványok kérése [Azure verem nyilvános kulcsokra épülő infrastruktúrát tanúsítványkövetelmények - választható PaaS tanúsítványokat](azure-stack-pki-certs.md#optional-paas-certificates).



## <a name="prerequisites"></a>Előfeltételek

A rendszer a PKI-tanúsítványok telepítését bemutató Azure verem CSR(s) létrehozása előtt kell felelnie a következő előfeltételek teljesülését:

 - A Microsoft Azure verem készültségének ellenőrzése
 - Tanúsítvány attribútumok:
    - Régió neve
    - Külső, teljesen minősített tartománynevét (FQDN)
    - Tárgy
 - Windows 10 vagy Windows Server 2016
 
  > [!NOTE]
  > Amikor megjelenik a tanúsítványok biztonsági a hitelesítésszolgáltatótól származó lépéseit [előkészítése Azure verem PKI-tanúsítványok](azure-stack-prepare-pki-certs.md) hajtható végre, ugyanarra a rendszerre kell!

## <a name="generate-certificate-signing-requests"></a>A tanúsítvány-aláírási kérelem (kérelmek) létrehozása

Használja ezeket a lépéseket az Azure verem PKI-tanúsítványok ellenőrzése: 

1.  AzsReadinessChecker telepíthessenek egy PowerShell-parancssorba (5.1-es vagy újabb), a következő parancsmagot:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ````

2.  Deklarálja a **tulajdonos** , egy rendezett szótárban. Példa: 

    ````PowerShell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"} 
    ````
    > [!note]  
    > Ha egy köznapi nevének (CN) meg van adva ez felülírja az első DNS-nevét a tanúsítványkérelemben.

3.  Egy már létező kimeneti könyvtár deklarálható. Példa:

    ````PowerShell  
    $outputDirectory = "$ENV:USERPROFILE\Documents\AzureStackCSR"
    ````
4.  Deklarálja rendszer azonosítása

    Azure Active Directory

    ```PowerShell
    $IdentitySystem = "AAD"
    ````

    Active Directory összevonási szolgáltatások

    ```PowerShell
    $IdentitySystem = "ADFS"
    ````

5. Deklarálja **régió neve** és egy **külső FQDN** az Azure-verem használandók.

    ```PowerShell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ````

    > [!note]  
    > `<regionName>.<externalFQDN>` alapját adja meg, amely minden külső DNS-nevek Azure verem létrejönnek, ebben a példában, a portál lenne `portal.east.azurestack.contoso.com`.  

6. Tanúsítvány-aláírási kérelem minden DNS-név létrehozásához:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Tartalmazza a PaaS szolgáltatások adja meg a kapcsoló ```-IncludePaaS```

7. Másik lehetőségként a fejlesztési és tesztelési célú környezetekben. Létrehozásához több alternatív tulajdonosnevek egyetlen tanúsítványkérelemben hozzáadása **- RequestType SingleCSR** paraméter és érték (**nem** éles környezetekhez ajánlott):

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Tartalmazza a PaaS szolgáltatások adja meg a kapcsoló ```-IncludePaaS```
    
8. Tekintse át a kimenetet:

    ````PowerShell  
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Request Generation

    CSR generating for following SAN(s): dns=*.east.azurestack.contoso.com&dns=*.blob.east.azurestack.contoso.com&dns=*.queue.east.azurestack.contoso.com&dns=*.table.east.azurestack.cont
    oso.com&dns=*.vault.east.azurestack.contoso.com&dns=*.adminvault.east.azurestack.contoso.com&dns=portal.east.azurestack.contoso.com&dns=adminportal.east.azurestack.contoso.com&dns=ma
    nagement.east.azurestack.contoso.com&dns=adminmanagement.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\username\Documents\AzureStackCSR\wildcard_east_azurestack_contoso_com_CertRequest_20180405233530.req
    Certreq.exe output: CertReq: Request Created

    Finished Certificate Request Generation

    AzsReadinessChecker Log location: C:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\1.1803.405.3\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

9.  Küldje el a **. KÉRÉS** fájl jön létre a hitelesítésszolgáltatóhoz (belső vagy nyilvános).  A kimeneti könyvtár **Start-AzsReadinessChecker** elküldése a hitelesítésszolgáltatónak kell CSR(s) tartalmazza.  A kérés generálásakor, referenciaként használt INF-fájlokat tartalmazó gyermek könyvtár is tartalmaz. Ne feledje, hogy a hitelesítésszolgáltató létrehozza-e tanúsítványok segítségével létrehozott kérését, amelyek megfelelnek a [Azure verem nyilvános kulcsokra épülő infrastruktúra követelményei](azure-stack-pki-certs.md).

## <a name="next-steps"></a>További lépések

[Azure verem PKI-tanúsítványok előkészítése](azure-stack-prepare-pki-certs.md)
