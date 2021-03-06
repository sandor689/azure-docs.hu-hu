---
title: Rövid útmutató – Windows rendszerű virtuális gép létrehozása az Azure Portalon | Microsoft Docs
description: Ez a rövid útmutató a Windows rendszerű virtuális gépek az Azure Portallal történő létrehozását ismerteti.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/03/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f5a92d421bbf7bfe485252c148d5f64ae2fb8e23
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916115"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-the-azure-portal"></a>Rövid útmutató: Windows rendszerű virtuális gép létrehozása az Azure Portalon

Az Azure-beli virtuális gépek (VM-ek) létrehozhatók az Azure Portal segítségével. Ez a módszer egy böngészőalapú felhasználói felületet biztosít a virtuális gépek és a társított erőforrások létrehozásához. Ez a rövid útmutató bemutatja, hogyan helyezhet üzembe az Azure Portal segítségével Windows Server 2016 rendszerű virtuális gépeket (VM-eket) az Azure-ban. A virtuális gép működésének ellenőrzéséhez ezután RDP-kapcsolaton keresztül csatlakozzon a géphez, és telepítse az IIS-webkiszolgálót.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

## <a name="sign-in-to-azure"></a>Bejelentkezés az Azure-ba

Jelentkezzen be az Azure Portalra a https://portal.azure.com webhelyen.

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása

1. Válassza az Azure Portal bal felső sarkában az **Erőforrás létrehozása** lehetőséget.

2. Az Azure Marketplace-erőforrások fölött lévő keresési mezőben keressen a **Windows Server 2016 Datacenter** elemre, és válassza ki, majd válassza a **Létrehozás** lehetőséget.

3. Adja meg a virtuális gép nevét (például *myVM*), a lemez típusát hagyja az *SSD* értéken, majd adjon meg egy felhasználónevet (például *azureuser*). A jelszónak legalább 12 karakter hosszúságúnak kell lennie, [az összetettségre vonatkozó követelmények teljesülése mellett](faq.md#what-are-the-password-requirements-when-creating-a-vm).

    ![Írja be a virtuális gép alapvető adatait a portálpanelen](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)

5. Válassza az **Új létrehozása** lehetőséget egy erőforráscsoport létrehozásához, majd adjon meg egy nevet (például *myResourceGroup*). Válassza ki a **Helyet**, majd kattintson az **OK** gombra.

4. Válasszon méretet a virtuális gép számára. Szűrhet például *számítási típus* vagy *lemeztípus* alapján. A virtuális gép ajánlott mérete a *D2s_v3*. Kattintson a **Kiválasztás** elemre a méret kiválasztása után.

    ![Képernyőkép a virtuális gépek méreteivel](./media/quick-create-portal/create-windows-vm-portal-sizes.png)

5. A **Beállítások** lap **Hálózat** > **Hálózati biztonsági csoport** > **Nyilvános bejövő portok** területén válassza a **HTTP** és az **RDP (3389)** lehetőséget a legördülő listából. Tartsa meg az alapértelmezett értékeket a többi beállításnál, majd kattintson az **OK** gombra.

6. Az Összefoglalás lapon válassza a **Létrehozás** lehetőséget a virtuális gép üzembe helyezésének megkezdéséhez.

7. A virtuális gép rögzítve lesz az Azure Portal irányítópultján. Az üzembe helyezés befejeztével a virtuális gép összefoglalás panelje automatikusan megnyílik.

## <a name="connect-to-virtual-machine"></a>Csatlakozás virtuális géphez

Hozzon létre egy távoli asztali kapcsolatot a virtuális géppel. Ezek az utasítások ismertetik, hogyan csatlakozhat a virtuális gépéhez egy Windows rendszerű gépről. Mac rendszerben szüksége van egy RDP-kliensre, mint például a Mac App Store áruházban elérhető [távoli asztali ügyfélre](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12).

1. Kattintson a **Csatlakozás** gombra a virtuális gép tulajdonságainak lapján. 

    ![Csatlakozás az Azure-beli virtuális gépekhez a portálról](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png)
    
2. A **Csatlakozás virtuális géphez** oldalon tartsa meg az alapértelmezett beállításokat a DNS-név alapján a 3389-es porton keresztül való csatlakozáshoz, és kattintson az **RDP-fájl letöltése** parancsra.

2. Nyissa meg a letöltött RDP-fájlt, és kattintson a **Csatlakozás** gombra, amikor a rendszer erre kéri. 

3. A **Windows rendszerbiztonság** ablakban válassza a **További lehetőségek**, majd a **Másik fiók használata** elemet. Írja be a felhasználónevet *virtuális_gép_neve*\\*felhasználónév* formátumban, és adja meg a virtuális géphez létrehozott jelszót, majd kattintson az **OK** gombra.

4. A bejelentkezés során egy figyelmeztetés jelenhet meg a tanúsítvánnyal kapcsolatban. A kapcsolat létrehozásához kattintson az **Igen** vagy **Folytatás** gombra.

## <a name="install-web-server"></a>Webkiszolgáló telepítése

A virtuális gép működésének ellenőrzéséhez telepítse az IIS-webkiszolgálót. Nyisson meg egy PowerShell-parancssort a virtuális gépen, és futtassa a következő parancsot:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Ha elkészült, zárja be a virtuális géphez nyitott RDP-kapcsolatot.


## <a name="view-the-iis-welcome-page"></a>Az IIS kezdőlapjának megtekintése

A portálon válassza ki a virtuális gépet, majd a virtuális gép áttekintési területén használja a **Kattintson a másoláshoz** gombot az IP-cím jobb oldalán annak másolásához és a böngészőlapra való beillesztéshez. Megnyílik az alapértelmezett IIS-kezdőlap, amelynek így kell kinéznie:

![Alapértelmezett IIS-webhely](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs rájuk szükség, törölheti az erőforráscsoportot, a virtuális gépet és az összes kapcsolódó erőforrást. Ehhez válassza ki a virtuális gép erőforráscsoportját, kattintson a **Törlés** elemre, majd erősítse meg a törölni kívánt erőforráscsoport nevét.

## <a name="next-steps"></a>További lépések

Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, megnyitott egy hálózati portot a webes forgalom számára, valamint telepített egy alapszintű webkiszolgálót. Ha bővebb információra van szüksége az Azure-alapú virtuális gépekkel kapcsolatban, lépjen tovább a Windows rendszerű virtuális gépekről szóló oktatóanyagra.

> [!div class="nextstepaction"]
> [Windowsos virtuális gépek az Azure-ban – oktatóanyagok](./tutorial-manage-vm.md)
