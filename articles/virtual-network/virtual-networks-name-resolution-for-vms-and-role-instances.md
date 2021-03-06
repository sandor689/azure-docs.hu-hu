---
title: Erőforrások az Azure virtuális hálózataihoz névfeloldását |} Microsoft Docs
description: Név feloldása forgatókönyvek Azure IaaS, hibrid megoldások között különböző felhőalapú szolgáltatások, az Active Directory és a saját DNS-kiszolgálót használ.
services: virtual-network
documentationcenter: na
author: subsarma
manager: vitinnan
editor: ''
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: subsarma
ms.openlocfilehash: 32d4f72afb4cd18e6b66c52eb78b2fc7b6b75cbd
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/20/2018
ms.locfileid: "31603657"
---
# <a name="name-resolution-for-resources-in-azure-virtual-networks"></a>Egy Azure virtuális hálózatot az erőforrásokra névfeloldás

Attól függően, hogyan használja a Azure hibrid megoldások, IaaS és PaaS üzemeltetéséhez szükség lehet ahhoz, hogy a virtuális gépek (VM) és egyéb egymással kommunikálnak a virtuális hálózat üzembe helyezett erőforrás. IP-címek használatával engedélyezheti a kommunikációt, bár jóval egyszerűbb, használja a nevét, amely jól megjegyezhető, és ne módosítsa. 

Ha a virtuális hálózat üzembe helyezett erőforrás kell tartománynevek belső IP-címek, használhatnak módszerek egyikét:

* [Azure által biztosított névfeloldás](#azure-provided-name-resolution)
* [Névfeloldás a saját DNS-kiszolgálót használó](#name-resolution-that-uses-your-own-dns-server) (ami előfordulhat, hogy továbbítsa az Azure által biztosított DNS-kiszolgálók) 

A névfeloldás használata típusa attól függ, hogyan kell az erőforrások kommunikálnak egymással. A következő táblázat bemutatja a forgatókönyvek és a megfelelő név feloldása megoldások:

> [!NOTE]
> A forgatókönyvtől függően előfordulhat, hogy használni kívánt az Azure DNS saját zónák funkció, amely jelenleg a nyilvános előzetes verziójához. További információkért lásd: [titkos tartományok Azure DNS használatával](../dns/private-dns-overview.md).
>

| **A forgatókönyv** | **Megoldás** | **Suffix** |
| --- | --- | --- |
| Az azonos virtuális hálózatban vagy Azure Cloud Services szerepkörpéldányokat ugyanazt a felhőszolgáltatásban található található virtuális gépek közötti névfeloldás. | [Azure saját DNS-zónák](../dns/private-dns-overview.md) vagy [Azure által biztosított névfeloldás](#azure-provided-name-resolution) |állomásnév vagy teljes Tartományneve |
| Névfeloldás a különböző virtuális hálózatokon lévő virtuális gépekre vagy szerepkörpéldányokra elérhető különböző felhőszolgáltatások között. |[Azure saját DNS-zónák](../dns/private-dns-overview.md) vagy lekérdezések megoldás az Azure-ban (DNS-proxy) virtuális hálózatok közötti továbbítás ügyfél által felügyelt DNS-kiszolgálók. Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-that-uses-your-own-dns-server). |Kizárólag az FQDN esetében |
| Az Azure App Service (Web App, függvény vagy Botot) a névfeloldás az azonos virtuális hálózatban lévő szerepkörpéldányokat vagy a virtuális gépek virtuális hálózati integráció használatával. |Ügyfél által felügyelt DNS-kiszolgálók (DNS-proxy) Azure általi megoldása virtuális hálózatok közötti lekérdezések továbbítása. Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-that-uses-your-own-dns-server). |Kizárólag az FQDN esetében |
| Név feloldása az App Service Web Apps a virtuális gépek ugyanabban a virtuális hálózatban. |Ügyfél által felügyelt DNS-kiszolgálók (DNS-proxy) Azure általi megoldása virtuális hálózatok közötti lekérdezések továbbítása. Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-that-uses-your-own-dns-server). |Kizárólag az FQDN esetében |
| Nevű felbontása az App Service Web Apps egy virtuális hálózat a virtuális gépekhez, egy másik virtuális hálózatot. |Ügyfél által felügyelt DNS-kiszolgálók (DNS-proxy) Azure általi megoldása virtuális hálózatok közötti lekérdezések továbbítása. Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-that-uses-your-own-dns-server-for-web-apps). |Kizárólag az FQDN esetében |
| A helyi számítógép és a szolgáltatás neve a virtuális gépekre vagy szerepkörpéldányokra az Azure-ban feloldását. |Ügyfél által felügyelt DNS-kiszolgálók (a helyi tartományvezérlő, helyi írásvédett tartományvezérlő vagy másodlagos DNS zónaletöltés, például használata szinkronizálva). Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-that-uses-your-own-dns-server). |Kizárólag az FQDN esetében |
| A helyszíni számítógépek Azure állomásnevének feloldása. |Az ügyfél által felügyelt DNS-proxy kiszolgáló a megfelelő virtuális hálózat továbbítási kérelmek a proxykiszolgáló továbbítja lekérdezések Azure a feloldásához. Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-that-uses-your-own-dns-server). |Kizárólag az FQDN esetében |
| Névkeresési DNS belső IP-címekről. |[Névfeloldás a saját DNS-kiszolgáló segítségével](#name-resolution-that-uses-your-own-dns-server). |Nem alkalmazható |
| Névfeloldás közötti virtuális gépekre vagy szerepkörpéldányokra különböző felhőszolgáltatások, nem a virtuális hálózat található. |Nem alkalmazható. Virtuális gépek és a szerepkörpéldányok különböző felhőszolgáltatások közötti kapcsolat nem támogatja a virtuális hálózatokon kívül. |Nem alkalmazható|

## <a name="azure-provided-name-resolution"></a>Azure által biztosított névfeloldás

Nyilvános DNS-nevek feloldása, valamint Azure belső névfeloldást biztosít a virtuális gépek és a szerepkörpéldányok, amelyek az azonos virtuális hálózatban vagy a felhőalapú szolgáltatás. Virtuális gépek és a példány egy felhőalapú szolgáltatás ossza meg ugyanazt a DNS-utótagját, így egyedül gazdanevet is elegendő. Azonban a klasszikus telepítési modellel telepített virtuális hálózatokon, különböző felhőszolgáltatások más DNS-utótag. Ebben a helyzetben kell FQDN-neve eltérő felhőszolgáltatásaiban neveinek feloldásához. A virtuális hálózatok az Azure Resource Manager telepítési modell használatával telepített a DNS-utótag összhangban a virtuális hálózaton, így nincs szükség a teljes Tartománynevét. DNS-nevek a virtuális gépek és a hálózati adapterek is rendelhetők. Bár az Azure által biztosított névfeloldás kell konfigurálni, akkor nem a megfelelő választás az összes központi telepítési forgatókönyv, ahogy az előző táblázatban.

> [!NOTE]
> Ha a webes és feldolgozói szerepkörök használatával a felhőalapú szolgáltatások, a belső IP-címek szerepkörpéldányt beállítani az Azure Service Management REST API használatával is elérhető. További információkért lásd: a [Service Management REST API-referencia](https://msdn.microsoft.com/library/azure/ee460799.aspx). A cím a szerepkör nevét és példányszámát számán alapul. 
> 
> 

### <a name="features"></a>Szolgáltatások

Azure által biztosított névfeloldást a következő szolgáltatásokat tartalmazza:
* Könnyű használat. Nem igényel konfigurálást.
* Magas rendelkezésre állás. Nem kell létrehozni és fürtöket a saját DNS-kiszolgálók kezelése.
* Használhatja a szolgáltatást együtt a saját DNS-kiszolgálókkal, a helyszíni és az Azure-gazdagép nevét.
* Névfeloldás között a virtuális gépek és a szerepkörpéldányok belül az azonos felhőalapú szolgáltatás, a teljes tartománynév nélkül is használhatja.
* Virtuális gépek az Azure Resource Manager üzembe helyezési modelljével nélkül FQDN-t használó virtuális hálózatok közötti névfeloldás is használhatja. Virtuális hálózatok a klasszikus üzembe helyezési modellel egy teljes Tartománynevet igényelnek, különböző felhőszolgáltatások a nevek feloldásakor. 
* Használhatja a központi telepítések jellemző állomásnevek ahelyett, hogy működik-e automatikusan létrehozott névvel.

### <a name="considerations"></a>Megfontolandó szempontok

Azure által biztosított névfeloldás használata esetén megfontolandó szempontok:
* Nem lehet módosítani az Azure által létrehozott DNS-utótagot.
* Nem lehet manuálisan regisztrálni a saját rögzíti.
* WINS- és NetBIOS nem támogatottak. Nem található a virtuális gépek, a Windows Intézőben.
* Gazdagép nevének kell lennie a DNS-kompatibilis. Neveket kell használnia csak 0-9, a – z, és a "-", és nem kezdődhet vagy végződhet egy "-".
* DNS-lekérdezés forgalom az egyes virtuális gépek folyamatban van. Sávszélesség-szabályozás nem befolyásolja a legtöbb alkalmazást. Ha kérelem szabályozása, győződjön meg arról, hogy engedélyezve van-e az ügyféloldali gyorsítótárazás. További információkért lásd: [DNS-ügyfél konfigurációjának](#dns-client-configuration).
* Csak az első 180 felhőszolgáltatások a virtuális gépek minden virtuális hálózathoz, a klasszikus üzembe helyezési modellben van regisztrálva. Ez a korlátozás nem vonatkozik a virtuális hálózatok az Azure Resource Manager.

## <a name="dns-client-configuration"></a>DNS-ügyfél konfigurációja

Ez a fejezet ügyféloldali gyorsítótárazás és ügyféloldali újrapróbálkozások.

### <a name="client-side-caching"></a>Ügyféloldali gyorsítótárazás

Nem minden DNS-lekérdezést kell küldhető el a hálózaton keresztül. Ügyféloldali gyorsítótárazás segít a késés csökkentésére, és javítása a hálózati blips, a rugalmasság feloldása ismétlődő DNS-lekérdezések a helyi gyorsítótárból. DNS-rekordokat tartalmaz egy élő idő (TTL) mechanizmus, amely lehetővé teszi a gyorsítótár, amíg a rekord tárolásához rekordot frissesség befolyásolása nélkül. Így ügyféloldali gyorsítótárazás nem alkalmas a legtöbb esetben.

Az alapértelmezett Windows DNS-ügyfél a DNS-gyorsítótár beépített rendelkezik. Egyes Linux terjesztésekről gyorsítótárazás alapértelmezés szerint nem tartalmaznak. Ha talál meg, hogy a helyi gyorsítótárba még nem, a DNS-gyorsítótár hozzá minden egyes Linux virtuális gép.

Számos különböző DNS-, gyorsítótárazás csomagok érhető el (például dnsmasq). A leggyakoribb terjesztéseket dnsmasq telepítése a következő:

* **Ubuntu (használ resolvconf)**:
  * Telepítse a dnsmasq csomag `sudo apt-get install dnsmasq`.
* **SUSE (használ netconf)**:
  * Telepítse a dnsmasq csomag `sudo zypper install dnsmasq`.
  * Engedélyezze a dnsmasq szolgáltatást a `systemctl enable dnsmasq.service`. 
  * Indítsa el a dnsmasq szolgáltatást a `systemctl start dnsmasq.service`. 
  * Szerkesztés **/etc/sysconfig/network/config**, és módosítsa *NETCONFIG_DNS_FORWARDER = ""* való *dnsmasq*.
  * Frissítse a resolv.conf `netconfig update`, hogy a gyorsítótár állítja be a helyi DNS-feloldó.
* **OpenLogic (használja a NetworkManager)**:
  * Telepítse a dnsmasq csomag `sudo yum install dnsmasq`.
  * Engedélyezze a dnsmasq szolgáltatást a `systemctl enable dnsmasq.service`.
  * Indítsa el a dnsmasq szolgáltatást a `systemctl start dnsmasq.service`.
  * Adja hozzá *-neve-tartománykiszolgálók 127.0.0.1; illesztenie* való **/etc/dhclient-eth0.conf**.
  * Indítsa újra a hálózati szolgáltatást a `service network restart`, hogy a gyorsítótár állítja be a helyi DNS-feloldó.

> [!NOTE]
> A dnsmasq csomag egyike csak számos DNS gyorsítótárak Linux érhető el. Annak használata előtt az adott igényeknek megfelelően való alkalmasságát, és ellenőrizze, hogy telepítve van-e más gyorsítótár.
> 
> 
    
### <a name="client-side-retries"></a>Ügyféloldali újrapróbálkozások

DNS elsősorban UDP protokoll. Az UDP protokoll üzenetkézbesítést nem garantálja, mert a DNS protokoll újrapróbálkozási logika kezelése. Minden DNS-ügyfél (operációs rendszer) is virágot különböző újrapróbálkozási logika, attól függően, hogy a létrehozásához használt beállítások:

* Windows operációs rendszerek újrapróbálkozás után egy második, majd újra egy másik két másodperc, a 4 másodpercnél, és egy másik négy másodperc. 
* Az alapértelmezett Linux telepítő újrapróbálkozások öt másodperc múlva. Azt javasoljuk, hogy az újra gombra paramétereknek módosítása a ötször, egy másodperces időközönként.

Ellenőrizze az aktuális beállításait a Linux virtuális gép és `cat /etc/resolv.conf`. Tekintse meg a *beállítások* . sor, például:

```bash
options timeout:1 attempts:5
```

A resolv.conf fájl általában automatikusan létrehozott, és nem szerkeszthető. A megadott történő hozzáadásának lépései a *beállítások* terjesztési változhat sor:

* **Ubuntu** (használja a resolvconf):
  1. Adja hozzá a *beállítások* sort **/etc/resolveconf/resolv.conf.d/head**.
  2. Futtatás `resolvconf -u` frissítéséhez.
* **SUSE** (használja a netconf):
  1. Adja hozzá *timeout:1 kísérletek: 5* számára a **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** paraméterének **/etc/sysconfig/network/config**. 
  2. Futtatás `netconfig update` frissítéséhez.
* **OpenLogic** (használja a NetworkManager):
  1. Adja hozzá *echo "lehetőségek timeout:1 kísérletek: 5"* való **/etc/NetworkManager/dispatcher.d/11-dhclient**. 
  2. Frissítse `service network restart`.

## <a name="name-resolution-that-uses-your-own-dns-server"></a>A saját DNS-kiszolgálót használó névfeloldás

Ez a szakasz ismerteti a virtuális gépek, a szerepkörpéldányok és a web apps.

### <a name="vms-and-role-instances"></a>Virtuális gépek és a szerepkör-példányoknál

A névfeloldási szükségleteit túlmutató előfordulhat, hogy az az Azure által nyújtott szolgáltatásokat. Szükség lehet például a Microsoft Windows Server Active Directory-tartományok DNS-nevek virtuális hálózatok között. Ezek a forgatókönyvek fedik le, Azure lehetővé teszi a saját DNS-kiszolgálók használata.

A virtuális hálózaton belül DNS-kiszolgálók DNS-lekérdezések továbbíthatja a rekurzív feloldókat az Azure-ban. Ez lehetővé teszi, hogy adott virtuális hálózaton belül állomásneveket. Például az Azure-t futtató tartományvezérlő (DC) is válaszolni a tartomány DNS-lekérdezései, és további lekérdezések továbbítása az Azure-bA. Lekérdezések továbbítása lehetővé teszi, hogy a virtuális gépek, a helyszíni erőforrások (keresztül a tartományvezérlő) és az Azure által biztosított állomásnevek (keresztül a továbbító). A rekurzív feloldókat az Azure-ban a hozzáférést a virtuális IP-cím 168.63.129.16 keresztül valósul meg.

DNS-továbbítást is lehetővé teszi, hogy a DNS-feloldás virtuális hálózatok között, és lehetővé teszi, hogy az Azure által biztosított gazdagépnevekhez a helyszíni gépeket. Ahhoz, hogy a virtuális gép nevének feloldása, a DNS-kiszolgáló virtuális gép ugyanazon a virtuális hálózaton kell lennie, és úgy, hogy előre állomás névlekérdezését az Azure-bA. A DNS-utótagja eltér az egyes virtuális hálózati, mert feltételes továbbítás szabályok segítségével a névfeloldás a megfelelő virtuális hálózat DNS-lekérdezéseket küldjön. Az alábbi ábrán két virtuális hálózatot és egy helyszíni hálózat DNS-feloldás virtuális hálózatok között, ez a módszer használatával történt. Egy példa DNS-továbbító érhető el a [Azure gyors üzembe helyezés sablontárban](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) és [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> A szerepkör példánya végezheti a névfeloldás az azonos virtuális hálózaton belüli virtuális gépek. A teljes Tartománynevét, amely a virtuális gép állomásnevét ilyeneket és **internal.cloudapp.net** DNS-utótagot. Ebben az esetben névfeloldás azonban csak sikeres, ha a szerepkör példánya meghatározott Virtuálisgép neve a [szerepkör séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/jj156212.aspx). 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Szerepkör-példányok kell végrehajtani a névfeloldást a virtuális gépeket egy másik virtuális hálózaton (FQDN használatával a **internal.cloudapp.net** utótag) (egyéni DNS-kiszolgálók közötti továbbítás ebben a szakaszban leírt módon ehhez rendelkezik a két virtuális hálózatok).
>

![Virtuális hálózatok közötti DNS ábrája](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Biztosít, ha az Azure által biztosított névfeloldást használ, Azure Dynamic Host Configuration Protocol (DHCP) egy belső DNS-utótagot (**. internal.cloudapp.net**) minden egyes virtuális géphez. Ez utótag állomásnév lehetővé teszi, mert a gazdagép rekordjai szerepelnek a **internal.cloudapp.net** zóna. Ha saját nevet névfeloldási megoldást használ, ez utótag nincs megadva a virtuális gépekhez, mert azt más DNS-architektúrák (például a tartományhoz csatlakoztatott forgatókönyvek) gátolja. Ehelyett az Azure biztosít a nem működő helyőrző (*reddog.microsoft.com*).

Szükség esetén a belső DNS-utótag segítségével meghatározhatja az API-t vagy a PowerShell használatával:

* Virtuális hálózatok az Azure Resource Manager üzembe helyezési modellel, az utótaghoz érhető el keresztül a [hálózati illesztő REST API](/rest/api/virtualnetwork/networkinterfaces/get), a [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) PowerShell-parancsmag és a [az hálózati nic megjelenítése](/cli/azure/network/nic#az-network-nic-show) Azure CLI parancsot.
* A klasszikus üzembe helyezési modellek utótag megtalálható a [telepítési API beszerzése](https://msdn.microsoft.com/library/azure/ee460804.aspx) hívás vagy a [Get-AzureVM-Debug](/powershell/module/azure/get-azurevm) parancsmag.

Lekérdezések továbbítása az Azure-ba nem a igényeinek megfelelően, ha meg kell adnia a saját DNS-megoldást. A DNS-megoldást kell megfelelnie:

* Biztosítják a megfelelő gazdagép a névfeloldás, keresztül [DDNS](virtual-networks-name-resolution-ddns.md), például. Ha DDNS használ, szükség lehet tiltsa le a DNS-rekord kitakarítási. Az Azure DHCP-bérletek hosszú, és kitakarítási előfordulhat, hogy távolítsa el DNS-rekordok idő előtt. 
* Adja meg a megfelelő rekurziót külső tartomány nevek feloldását engedélyezéséhez.
* Elérhető (TCP és UDP 53-as porton) az ügyfelek szolgál, és fogja tudni érni az internetet.
* Védhető szemben az internetről, külső ügynökök által jelentett veszélyek mérséklése.

> [!NOTE]
> A legjobb teljesítmény érdekében DNS-kiszolgálóként, Azure virtuális gépek használata esetén IPv6 le kell tiltani. A [nyilvános IP-cím](virtual-network-public-ip-address.md) minden DNS-kiszolgáló virtuális gép legyen hozzárendelve. A témakör további Teljesítményelemzés és optimalizálási lehetőségeit, ha a DNS-kiszolgálóként használja a Windows Server [nevet névfeloldási teljesítményét egy rekurzív Windows DNS Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Webalkalmazások
Tegyük fel, végre kell hajtania a névfeloldást a webes alkalmazás App Service-ben a virtuális hálózat, az azonos virtuális hálózatban lévő virtuális gépekhez kapcsolódó használatával készültek. Egy egyéni DNS beállításán kiszolgálóra, amely tartalmaz egy DNS-továbbító, amely továbbítja a lekérdezéseket az Azure-ba (virtuális IP-cím 168.63.129.16), a következő lépésekkel:
1. A webalkalmazás virtuális hálózati integráció engedélyezése, ha ezt még nem tette, a [integrációt az alkalmazás a virtuális hálózati](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Az Azure portálon, az App Service-csomagot a webalkalmazás üzemeltetéséhez válassza **szinkronizálási hálózati** alatt **hálózati**, **virtuális hálózati integráció**.

    ![Képernyőkép a virtuális hálózati névfeloldás](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Ha szeretné végrehajtani a névfeloldást a webes alkalmazás beépített App Service-ben a virtuális hálózat egy másik virtuális hálózatot a virtuális gépekhez kapcsolódó használatával kell egyéni DNS-kiszolgálók mindkét virtuális hálózat, a következőképpen használhatja: 
* Állítsa be a virtuális gép, amely szintén továbbíthatja lekérdezések a rekurzív feloldó (virtuális IP-cím 168.63.129.16) Azure-ban a cél virtuális hálózat DNS-kiszolgáló. Egy példa DNS-továbbító érhető el a [Azure gyors üzembe helyezés sablontárban](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) és [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Állítsa be a DNS-továbbítók a forrás virtuális hálózatban a virtuális gép. Konfigurálja a DNS-továbbító továbbítják a lekérdezéseket a DNS-kiszolgálót a cél virtuális hálózaton.
* A forrás virtuális hálózati beállításainak a forrás DNS-kiszolgáló konfigurálása
* A webalkalmazás a forrás virtuális hálózat utasításait követve csatolása a virtuális hálózati integráció engedélyezése [integrációt az alkalmazás a virtuális hálózati](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Az Azure portálon, az App Service-csomagot a webalkalmazás üzemeltetéséhez válassza **szinkronizálási hálózati** alatt **hálózati**, **virtuális hálózati integráció**. 

## <a name="specify-dns-servers"></a>Adja meg a DNS-kiszolgálók
Saját DNS-kiszolgálókat használ, amikor az Azure lehetővé teszi a virtuális hálózatonként több DNS-kiszolgálók megadása. Több DNS-kiszolgáló egy hálózati csatoló (az Azure Resource Manager), vagy a felhőszolgáltatás (a klasszikus üzembe helyezési modellel) is megadható. Egy hálózati felület vagy a felhőalapú szolgáltatás számára megadott DNS-kiszolgálók sorrend beolvasása a virtuális hálózathoz megadott DNS-kiszolgálókon keresztül.

> [!NOTE]
> Hálózati kapcsolat tulajdonságai, például a DNS-kiszolgáló IP-címek, nem közvetlenül a Windows virtuális gépeken belül kell szerkeszteni. Ennek az az oka, akkor előfordulhat, hogy első újramegfeleltetés során szolgáltatás javítandó a virtuális hálózati adapter lekérdezi cseréje. 
> 
> 

Amikor az Azure Resource Manager telepítési modell használ, a virtuális hálózatokhoz és a hálózati illesztő DNS-kiszolgálók is megadhat. További információkért lásd: [egy virtuális hálózat kezeléséhez](manage-virtual-network.md) és [kezelheti egy adott hálózati csatoló](virtual-network-network-interface.md).

A klasszikus üzembe helyezési modellt használ, megadhatja a virtuális hálózat DNS-kiszolgálók az Azure portálon vagy a [hálózati konfigurációs fájl](https://msdn.microsoft.com/library/azure/jj157100). A felhőszolgáltatások, megadhatja a DNS-kiszolgálókon keresztül a [szolgáltatás konfigurációs fájl](https://msdn.microsoft.com/library/azure/ee758710) vagy a PowerShell, használatával [New-AzureVM](/powershell/module/azure/new-azurevm).

> [!NOTE]
> Ha a virtuális hálózati vagy a virtuális gépen, amelyen már telepítve van a DNS-beállítások módosításához kell egyes érintett virtuális gépek a módosítások életbe léptetéséhez indítsa újra.
> 
> 

## <a name="next-steps"></a>További lépések

Az Azure Resource Manager üzembe helyezési modellben:

* [Virtuális hálózat kezelése](manage-virtual-network.md)
* [Egy adott hálózati csatoló kezelése](virtual-network-network-interface.md)

Klasszikus telepítési modell:

* [Az Azure szolgáltatás konfigurációs séma](https://msdn.microsoft.com/library/azure/ee758710)
* [Virtuális hálózati konfiguráció séma](https://msdn.microsoft.com/library/azure/jj157100)
* [Virtuális hálózat konfigurálása a hálózati konfigurációs fájl segítségével](virtual-networks-using-network-configuration-file.md)
