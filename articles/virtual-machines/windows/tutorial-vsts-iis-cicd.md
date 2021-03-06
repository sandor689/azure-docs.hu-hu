---
title: Oktatóanyag – az Azure-ban és a Team Services CI/CD-folyamat létrehozása |} A Microsoft Docs
description: Ebben az oktatóanyagban elsajátíthatja, hogyan lehet folyamatos integrációt és teljesítést, amely telepíti az IIS Windows virtuális gépen az Azure webalkalmazás a Visual Studio Team Services-folyamatok létrehozására.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b23cec90573c4be73a73daf0bc0e793da012585c
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932092"
---
# <a name="tutorial-create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a>Oktatóanyag: Egy folyamatos integrációs folyamat létrehozása a Visual Studio Team Services és az IIS
Automatizálja a buildelési, tesztelési és üzembe helyezési szakaszok az alkalmazásfejlesztés, használhatja a folyamatos integráció és készregyártás (CI/CD) folyamatot. Ebben az oktatóanyagban létrehozhat egy CI/CD folyamatot a Visual Studio Team Services és a egy Windows virtuális gép (VM) használatával az IIS-t futtató Azure-ban. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Team Services-projektbe egy ASP.NET-alkalmazás közzététele
> * Kód véglegesítése által aktivált builddefiníció létrehozása
> * IIS telepítése és konfigurálása az Azure-beli virtuális gépen
> * A Team Services üzembe helyezési csoport az IIS-példány hozzáadása
> * Hozzon létre egy kiadási definíció új webalkalmazás közzététele az IIS-csomagok üzembe helyezése
> * A CI/CD-folyamat tesztelése

Az oktatóanyaghoz az Azure PowerShell-modul 5.7.0-s vagy újabb verziójára lesz szükség. A verzió azonosításához futtassa a következőt: `Get-Module -ListAvailable AzureRM`. Ha frissíteni szeretne, olvassa el [az Azure PowerShell-modul telepítését](/powershell/azure/install-azurerm-ps) ismertető cikket.


## <a name="create-project-in-team-services"></a>Projekt létrehozása a Team Servicesben
A Visual Studio Team Services lehetővé teszi egyszerű együttműködést és fejlesztői egy a helyszíni-felügyeleti megoldás fenntartása nélkül. Team Services biztosítja a felhőalapú kódot tesztelés, elkészítéséhez és az application insights. Választhat egy verzió tárházat és a kód fejlesztési legjobban illő IDE. Ebben az oktatóanyagban egy alapszintű ASP.NET-webalkalmazás és a CI/CD-folyamat létrehozásához használhatja egy ingyenes fiókot. Ha egy Team Services-fiók már nem kell [hozzon létre egyet](http://go.microsoft.com/fwlink/?LinkId=307137).

A kód véglegesítésére folyamat kezeléséhez, builddefinícióiról és kiadási definícióiról, hozzon létre egy projektet a Team Services a következő:

1. Egy webböngészőben nyissa meg a Team Services-irányítópultot, és válassza a **új projekt**.
2. Adja meg *myWebApp* számára a **projektnév**. Hagyja meg az összes többi alapértelmezett értéket használandó *Git* verziókövetés és *Agile* munkaelemet bekérő folyamat.
3. Választania, **megoszthatja** *csapat tagjai*, majd **létrehozás**.
5. A projekt létrehozása után válassza az lehetőséget **információs vagy gitignore inicializálása**, majd **inicializálása**.
6. Az új projekt belül válassza **irányítópultok** tetején, majd válassza ki **Megnyitás a Visual Studióban**.


## <a name="create-aspnet-web-application"></a>ASP.NET-webalkalmazás létrehozása
Az előző lépésben hozott létre egy projektet a Team Services használatával. Az utolsó lépés az új projekt megnyílik a Visual Studióban. A kód véglegesítéseket kezelheti a **Team Explorer** ablak. Hozzon létre egy helyi példányát az új projektet, majd egy ASP.NET-alkalmazás létrehozása sablon alapján a következő:

1. Válassza ki **Klónozás** egy helyi git-tárház a Team Services-projekt létrehozása.
    
    ![A tárház klónozása a Team Services-projektbe](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. A **megoldások**válassza **új**.

    ![Webalkalmazási megoldás létrehozása](media/tutorial-vsts-iis-cicd/new_solution.png)

3. Válassza ki **webes** sablonokat, majd válassza a **ASP.NET-alkalmazás** sablont.
    1. Adja meg egy nevet az alkalmazásnak, például *myWebApp*, és törölje a jelet a jelölőnégyzetből **megoldás könyvtár létrehozása**.
    2. Ha a beállítás érhető el, törölje a **Application Insights hozzáadása a projekthez**. Az Application Insights megköveteli, hogy engedélyezze a webalkalmazás az Azure Application insights segítségével. A legyen ez az oktatóanyag az egyszerű, hagyja ki ezt a folyamatot.
    3. Kattintson az **OK** gombra.
4. Válasszon **MVC** a sablon listából.
    1. Válassza ki **hitelesítés módosítása**, válassza a **nincs hitelesítés**, majd **OK**.
    2. Válassza ki **OK** a megoldás létrehozásához.
5. Az a **Team Explorer** ablakban válassza a **módosítások**.

    ![A Team Services git-adattár helyi módosítások véglegesítése](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. A véglegesítés szövegbeviteli mezőben adjon meg egy üzenetet például *eredeti véglegesítési*. Válasszon **összes véglegesítése és szinkronizálási** a legördülő menüből.


## <a name="create-build-definition"></a>Builddefiníció létrehozása
A Team Services szolgáltatásban a builddefiníció kidolgozására, hogyan kell építeni az alkalmazás használhatja. Ebben az oktatóanyagban létrehozunk egy alapszintű-definíciót, amely el a forráskódban követően felépíti a megoldást, majd hozza létre a webes vesz igénybe a webalkalmazás futtatása az IIS-kiszolgálón történő használatával csomag központi telepítéséhez.

1. Válassza ki a Team Services-projektben **Build & Release** tetején, majd válassza ki **buildek**.
3. Válassza ki **+ új definíció**.
4. Válassza ki a **ASP.NET (előzetes verzió)** sablont, és válassza ki **alkalmaz**.
5. A feladat értéket hagyja az összes alapértelmezett. A **adatforrások beolvasása**, ügyeljen arra, hogy a *myWebApp* tárház és *fő* ág ki van jelölve.

    ![Builddefiníció létrehozása a Team Services-projektbe](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. Az a **eseményindítók** lapon, a csúszkát a **engedélyezése erre az eseményindítóra** való *engedélyezve*.
7. Mentse a builddefiníció és üzenetsor új build kiválasztásával **várólistára & mentése** , majd **várólistára & mentése** újra. Hagyja bejelölve az alapértelmezett beállításokat, és válassza ki **várólista**.

Tekintse meg a build van ütemezve egy üzemeltetett ügynökön, majd megkezdi a. A kimenet a következő példához hasonló:

![Project Team Services build sikeres létrehozása](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a>Virtuális gép létrehozása
Az ASP.NET-webalkalmazás futtatása platformot biztosítani, IIS-t futtató Windows virtuális gép van szükség. Team Services használatával kommunikálhat az IIS-példány, amikor kódot véglegesít és buildek esetén ügynököt használ.

Hozzon létre egy Windows Server 2016 virtuális gép a [új-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). A következő példában létrehozunk egy nevű virtuális Gépet *myVM* a a *USA keleti Régiójában* helyét. Az erőforráscsoport *myResourceGroupVSTS* és a támogató hálózati erőforrások is jönnek létre. Webes forgalom, a TCP-port engedélyezéséhez *80-as* a virtuális gép meg van nyitva. Amikor a rendszer kéri, adja meg egy felhasználónevet és jelszót, hogy a virtuális gép a bejelentkezési hitelesítő adatok használhatók:

```powershell
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a virtual machine
New-AzureRmVM `
  -ResourceGroupName "myResourceGroupVSTS" `
  -Name "myVM" `
  -Location "East US" `
  -ImageName "Win2016Datacenter" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -SecurityGroupName "myNetworkSecurityGroup" `
  -PublicIpAddressName "myPublicIp" `
  -Credential $cred `
  -OpenPorts 80
```

A virtuális Géphez való csatlakozáshoz, szerezze be a nyilvános IP-címet [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) módon:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

Hozzon létre egy távoli asztali munkamenetet a virtuális géphez:

```cmd
mstsc /v:<publicIpAddress>
```

Nyissa meg a virtuális gép egy **rendszergazda PowerShell** parancssort. Az IIS és a szükséges .NET-szolgáltatásokat a következőképpen telepítheti:

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a>Üzembe helyezési csoport létrehozása
Leküldenie a web deploy csomag az IIS-kiszolgálóra, egy üzembe helyezési csoport határozza meg a Team Services használatával. Ez a csoport lehetővé teszi, hogy adja meg, hogy mely kiszolgálók új buildek célját, amikor kódot véglegesít a Team Services és a buildek végezhető el.

1. Válassza ki a Team Services **Build & Release** majd **üzembe helyezési csoportok**.
2. Válasszon **központi telepítés hozzáadása csoporthoz**.
3. Adja meg egy nevet a csoportnak, például *myIIS*, majd **létrehozás**.
4. Az a **gépek regisztrálása** csoportjában biztosítása *Windows* van kiválasztva, majd a jelölőnégyzet bejelölésével **a személyes hozzáférési tokent a szkriptben a hitelesítéshez használandó**.
5. Válassza ki **parancsfájl másolása a vágólapra**.


### <a name="add-iis-vm-to-the-deployment-group"></a>Az üzembe helyezési csoport az IIS virtuális gép hozzáadása
Az üzembe helyezési csoport létrehozása, az egyes IIS-példány hozzáadása a csoporthoz. Egy parancsfájl, amely letölti és konfigurálja az ügynököt, a virtuális gépen, amely megkapja az új webes állít elő, Team Services-csomagok üzembe helyezése, majd a alkalmazni fogja azt az IIS.

1. Térjen vissza a **rendszergazda PowerShell** munkamenet a virtuális Gépen, illessze be, és futtassa a parancsprogramot másolta a Team Services.
2. Amikor a rendszer kéri, a címkék konfigurálása az ügynök számára, válassza ki a *Y* , és adja meg *webes*.
3. Ha a rendszer kéri a felhasználói fiók, nyomja le az ENTER *vissza* , fogadja el az alapértelmezett értéket.
4. Várjon, amíg befejeződik, egy üzenet a parancsfájl *sikeresen elindult a szolgáltatás vstsagent.account.computername*.
5. Az a **üzembe helyezési csoportok** lapján a **Build & Release** menüben nyissa meg a *myIIS* üzembe helyezési csoport. Az a **gépek** lapra, győződjön meg arról, hogy a virtuális gép szerepel-e.

    ![Virtuális gép a Team Services üzembe helyezési csoport sikeresen hozzáadva](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a>Kiadási definíció létrehozása
A buildek közzétételéhez a Team Services hoz létre a kiadási definíció. Ez a definíció által a build sikeres létrehozása az alkalmazás automatikusan aktiválódik. Úgy dönt, hogy az üzembe helyezési csoport paranccsal küldje le a webalkalmazás-csomag telepítése, és a megfelelő az IIS-beállítások megadása.

1. Válasszon **Build & Release**, majd **buildek**. Válassza ki az előző lépésben létrehozta a builddefiníció.
2. A **nemrégiben befejezett**, válassza ki a legújabb buildre, majd válassza ki **kiadási**.
3. Válasszon **Igen** kiadási definíció létrehozása.
4. Válassza ki a **üres** sablont, majd válassza ki **tovább**.
5. Ellenőrizze, hogy a projekt és a forrás builddefiníció fel van töltve a projektet.
6. Válassza ki a **folyamatos üzembe helyezés** jelölőnégyzetet, majd válassza ki **létrehozás**.
7. Válassza ki a legördülő lista melletti **+ tevékenységek hozzáadása a** válassza *adjon hozzá egy csoporthoz az üzembe helyezési fázisnak*.
    
    ![Kiadás definíciója a Team Servicesben a feladat hozzáadása](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. Válassza a **Hozzáadás** melletti **IIS webes alkalmazás Deploy(Preview)**, majd **Bezárás**.
9. Válassza ki a **futtassa az üzembe helyezési csoport** szülőfeladat.
    1. A **üzembe helyezési csoport**, válassza ki a korábban létrehozott üzembe helyezési csoport például *myIIS*.
    2. Az a **címkék Machine** jelölje ki **Hozzáadás** , és válassza a *webes* címke.
    
    ![Kiadási definíció üzembe helyezési csoport feladat IIS-hez](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. Válassza ki a **üzembe helyezés: az IIS webes alkalmazás üzembe helyezése** feladat konfigurálása az IIS példány beállításokat az alábbiak szerint:
    1. A **webhelynevet**, adja meg *Default Web Site*.
    2. Hagyja a többi alapértelmezett beállítást.
12. Válasszon **mentése**, majd **OK** kétszer.


## <a name="create-a-release-and-publish"></a>Egy kiadási létrehozása és közzététele
Most küldje le a webes üzembe helyezése egy új kiadási csomagot. Ebben a lépésben kommunikál az összes példányt, amely az üzembe helyezési csoport része, leküldi a webes ügynök a csomag telepítése, majd konfigurálja az IIS-t a frissített webalkalmazás futtatása.

1. Válassza ki a kiadási definíció **+ kiadás**, majd válassza ki **létrehozásához kiadási**.
2. Győződjön meg arról, hogy legújabb buildjének a legördülő listában kiválasztott, a **automatikus központi telepítési: kiadás létrehozása után**. Kattintson a **Létrehozás** gombra.
3. Egy kis szalagcím jelenik meg a kiadási definíció tetején például *kiadási "Release-1" létrejött*. A kiadási hivatkozásra.
4. Nyissa meg a **naplók** fülre, és tekintse meg a kibocsátási folyamat állapotát.
    
    ![A sikeres Team Services kiadási és webes telepítési csomag leküldéses](media/tutorial-vsts-iis-cicd/successful_release.png)

5. A kiadás befejezése után nyisson meg egy webböngészőt, és adja meg a virtuális gép nyilvános KVI címét. Az ASP.NET-alkalmazás fut.

    ![Az IIS virtuális gépen futó ASP.NET-webalkalmazás](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-the-whole-cicd-pipeline"></a>A teljes CI/CD-folyamat tesztelése
Az IIS-kiszolgálón futó webalkalmazás, és próbálja meg a teljes CI/CD-folyamat. A Visual Studio, és akkor indul el, a kód, a build véglegesítés, majd elindítja a frissített webes kiadásában a módosítás végrehajtása után telepítheti a csomagot az IIS:

1. A Visual Studióban nyissa meg a **Megoldáskezelőben** ablak.
2. Keresse meg és nyissa meg a *myWebApp |} Nézetek |} Kezdőlap |} Index.cshtml*
3. Olvassa el a 6. sort szerkesztése:

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. Mentse a fájlt.
5. Nyissa meg a **Team Explorer** ablakban válassza ki a *myWebApp* projektre, majd válassza a **módosítások**.
6. Adja meg egy véglegesítési üzenetet, például *tesztelés CI/CD-folyamat*, majd válassza ki **véglegesítési minden és szinkronizálási** a legördülő menüből.
7. A Team Services-munkaterületen egy új létrehozást akkor aktiválódik, a kód véglegesítésétől. 
    - Válasszon **Build & Release**, majd **buildek**. 
    - Válassza ki a builddefiníció, majd válassza ki a **várólistán & futó** hozhat létre, és tekintse meg a build előrehaladtával.
9. Ha a build sikeres, akkor aktiválódik, új kiadás.
    - Válasszon **Build & Release**, majd **kiadásokban** megtekintéséhez a web csomag az IIS virtuális gép leküldött központi telepítéséhez. 
    - Válassza ki a **frissítése** ikonra kattintva frissítheti az állapotot. Ha a *környezetek* oszlopban egy zöld pipa látható, a kiadás sikeresen telepítette az IIS.
11. A alkalmazni módosítások megtekintéséhez frissítse az IIS-webhely a böngészőben.

    ![CI/CD-folyamat az IIS virtuális gépen futó ASP.NET-webalkalmazás](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a>További lépések

Ez az oktatóanyag ASP.NET-webalkalmazás létrehozása a Team Services és a build konfigurálása, és új webalkalmazás üzembe helyezéséhez kiadási definíciók csomagok üzembe helyezése az IIS minden egyes kódvéglegesítéskor. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Team Services-projektbe egy ASP.NET-alkalmazás közzététele
> * Kód véglegesítése által aktivált builddefiníció létrehozása
> * IIS telepítése és konfigurálása az Azure-beli virtuális gépen
> * A Team Services üzembe helyezési csoport az IIS-példány hozzáadása
> * Hozzon létre egy kiadási definíció új webalkalmazás közzététele az IIS-csomagok üzembe helyezése
> * A CI/CD-folyamat tesztelése

Megtudhatja, hogyan telepítheti az SQL a következő oktatóanyaggal, amelyben tudatjuk a felhasználókkal&#92;IIS&#92;.NET verem Windows virtuális gépek párjai.

> [!div class="nextstepaction"]
> [SQL&#92;IIS&#92;.NET stack](tutorial-iis-sql.md)