---
title: Biztonsági megoldások integrálása az Azure Security Centerbe | Microsoft Docs
description: Megtudhatja, hogy az Azure Security Center hogyan integrálható a partnerekkel az Azure-erőforrások általános biztonságának növelése érdekében.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2018
ms.author: terrylan
ms.openlocfilehash: b0e674eb161af41a848f0456a033d615293a9947
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622789"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Biztonsági megoldások integrálása az Azure Security Centerbe
Ez a dokumentum az Azure Security Centerhez már csatlakoztatott biztonsági megoldások kezelésében és újak hozzáadásában segít.

## <a name="integrated-azure-security-solutions"></a>Integrált Azure biztonsági megoldások
A Security Center használatával egyszerűen engedélyezhet integrált biztonsági megoldásokat az Azure-ban. Az előnyök:

- **Egyszerűsített üzembe helyezés**: A Security Center segítségével az integrált partnermegoldások egy optimalizált folyamat mentén helyezhetőek üzembe. A kártevőirtó, sebezhetőségfelmérő és hasonló megoldások esetében a Security Center képes biztosítani a szükséges ügynököt a virtuális gépeken, a tűzfalberendezések esetében pedig elintézi a szükséges hálózati konfigurációs feladatok nagy részét.
- **Integrált észlelések**: A partnermegoldásoktól érkező biztonsági eseményeket a rendszer automatikusan összegyűjti, összesíti és megjeleníti a Security Center riasztásainak és incidenseinek részeként. Ezek az események más forrásoktól érkező észlelésekhez is kapcsolódnak, ami fejlett fenyegetésészlelési képességeket biztosít.
- **Egyesített állapotmonitorozás és -kezelés**: Az integrált állapotesemények lehetővé teszik az összes partnermegoldás gyors monitorozását. Az alapszintű felügyeletből könnyen elérhető a speciális beállítás a partnermegoldás használatával.

Jelenleg a következő integrált biztonsági megoldások érhetők el:

- Végpontvédelem ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), [Symantec](https://www.symantec.com/products), [McAfee](https://www.mcafee.com/us/products.aspx), [Windows Defender](https://www.microsoft.com/windows/comprehensive-security) és [System Center Endpoint Protection](https://docs.microsoft.com/sccm/protect/deploy-use/endpoint-protection))
- Webalkalmazás-tűzfal ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/products.html) és [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))
- Új generációs tűzfalmegoldások ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2), [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html) és [Palo Alto Networks](https://www.paloaltonetworks.com/products))
- Biztonságirés-felmérés ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/) és [Rapid7](https://www.rapid7.com/products/insightvm/))

A végpontvédelem integrációja a megoldástól függően változhat. Az alábbi táblázat részletesen ismerteti az egyes megoldások által nyújtott felhasználói élményt:

| Endpoint Protection (Végpontok védelme)               | Platformok                             | Security Center telepítése | Security Center felderítése |
|-----------------------------------|---------------------------------------|------------------------------|---------------------------|
| Windows Defender (Microsoft-kártevőirtó)                  | Windows Server 2016                   | Nincs, az operációs rendszerbe van beépítve           | Igen                       |
| System Center Endpoint Protection (Microsoft-kártevőirtó) | Windows Server 2012 R2, 2012, 2008 R2 | Bővítmény útján                | Igen                       |
| Trend Micro – Összes verzió         | Windows Server termékcsalád                 | Nem                           | Igen                       |
| Symantec v12.1.1100+              | Windows Server termékcsalád                 | Nem                           | Igen                       |
| McAfee v10+                       | Windows Server termékcsalád                 | Nem                           | Igen                       |
| Kaspersky                         | Windows Server termékcsalád                 | Nem                           | Nem                        |
| Sophos                            | Windows Server termékcsalád                 | Nem                           | Nem                        |



## <a name="how-security-solutions-are-integrated"></a>A biztonsági megoldások integrálása
A Security Centerből üzembe helyezett Azure biztonsági megoldások automatikusan csatlakoztatva vannak. Csatlakoztathat egyéb biztonsági adatforrásokat is, köztük a következőket:

- Azure AD Identity Protection
- Helyszínen vagy más felhőkben futó számítógépek
- A Common Event Format (CEF) formátumot támogató biztonsági megoldások
- Microsoft Advanced Threat Analytics

![Partnermegoldások integrációja](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Integrált Azure biztonsági megoldások és egyéb adatforrások kezelése

1. Jelentkezzen be az [Azure Portalra](https://azure.microsoft.com/features/azure-portal/).

2. A **Microsoft Azure menüben** válassza a **Security Center** elemet. Megnyílik a **Security Center – Áttekintés** képernyő.

  ![Security Center – Áttekintés](./media/security-center-partner-integration/overview.png)

3. Az **Áttekintés** menüpontban válassza a **Biztonsági megoldások** elemet.

A **Biztonsági megoldások** területen megtekintheti az Azure integrált biztonsági megoldásainak állapotinformációit, valamint alapszintű felügyeleti feladatokat hajthat végre. Emellett egyéb típusú biztonsági adatforrásokat is csatlakoztathat, például Common Event Format (CEF) formátumú Azure Active Directory Identity Protection-riasztásokat és tűzfalnaplókat.

### <a name="connected-solutions"></a>Csatlakoztatott megoldások

A **Csatlakoztatott megoldások** szakasz a Security Centerhez jelenleg csatlakozó biztonsági megoldásokat, valamint az egyes megoldások állapotával kapcsolatos adatokat tartalmazza.  

![Csatlakoztatott megoldások](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

További tudnivalókért lásd a [csatlakoztatott partnermegoldások kezelését](security-center-partner-solutions.md).

### <a name="discovered-solutions"></a>Felderített megoldások

A Security Center automatikusan felderíti az Azure-ban futó, azonban a Security Centerhez nem csatlakoztatott biztonsági megoldásokat, és azokat a **Felderített megoldások** szakaszban jeleníti meg. Ez az Azure-beli megoldásokat, például az [Azure AD Identity Protectiont](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), valamint a partnermegoldásokat is tartalmazza.

> [!NOTE]
> A Felderített megoldások funkcióhoz a Standard szintű Security Centerre az előfizetés szintjén van szükség. A Security tarifacsomagjaival kapcsolatos további információért lásd a [díjszabást](security-center-pricing.md).
>
>

Az egyes megoldások alatt a **CSATLAKOZTATÁS** gombra kattintva integrálhatja azokat a Security Centerbe, és értesülhet a biztonsági riasztásokról.

![Felderített megoldások](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

A Security Center az előfizetésben üzembe helyezett azon megoldásokat is felderíti, amelyek képesek Common Event Format (CEF) formátumú naplókat továbbítani. Ismerje meg, hogyan [csatlakoztathat CEF-naplókat használó biztonsági megoldásokat](quick-security-solutions.md) a Security Centerhez.

### <a name="add-data-sources"></a>Adatforrások hozzáadása

Az **Adatforrások hozzáadása** szakasz sorolja fel az egyéb csatlakoztatható adatforrásokat. Az ezekből a forrásokból származó adatok hozzáadásával kapcsolatos utasításokért kattintson a **HOZZÁADÁS** gombra.

![Adatforrások](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="next-steps"></a>További lépések

Ebben a cikkben megismerkedett a partnermegoldások Security Centerrel való integrálásával. A Security Centerrel kapcsolatos további információkért olvassa el a következő cikkeket:

* [A Microsoft Advanced Threat Analytics csatlakoztatása az Azure Security Centerhez](security-center-ata-integration.md)
* [Az Azure Active Directory Identity Protection csatlakoztatása az Azure Security Centerhez](security-center-aadip-integration.md)
* [Biztonsági állapot monitorozása a Security Centerben](security-center-monitoring.md). Az Azure-erőforrások állapotának figyelését ismertető útmutató.
* [Partnermegoldások monitorozása a Security Centerrel](security-center-partner-solutions.md). A partnermegoldások állapotának figyelését ismertető útmutató.
* [Azure Security Center – gyakori kérdések](security-center-faq.md) Választ találhat a Security Center használatával kapcsolatos gyakori kérdésekre.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/). Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
