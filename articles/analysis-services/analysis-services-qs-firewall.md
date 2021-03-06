---
title: Rövid útmutató – Azure-beli Analysis Services-kiszolgáló tűzfalának konfigurálása | Microsoft Docs
description: Útmutatás tűzfal konfigurálásához egy Azure-beli Analysis Services-kiszolgálópéldányhoz.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c16a189e72edd94cf6fc60580d89dd4cfd1742e8
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443443"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Rövid útmutató: Kiszolgálótűzfal konfigurálása – Portál

Ez a rövid útmutató segítséget nyújt az Azure Analysis Services-kiszolgáló tűzfalának konfigurálásában. A kiszolgáló és az adatok védelmének fontos része egy tűzfal engedélyezése és IP-címtartományok konfigurálása csak a kiszolgálóhoz hozzáférő számítógépek számára.

## <a name="prerequisites"></a>Előfeltételek

- Analysis Services-kiszolgáló az előfizetésben. További tudnivalókért lásd: [Rövid útmutató: Kiszolgáló létrehozása – Portál](analysis-services-create-server.md) vagy [Rövid útmutató: Kiszolgáló létrehozása – PowerShell](analysis-services-create-powershell.md)
- Egy vagy több IP-címtartomány ügyfélszámítógépekhez (ha szükséges).

## <a name="log-in-to-the-azure-portal"></a>Bejelentkezés az Azure Portalra 

[Bejelentkezés a portálra](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Tűzfal konfigurálása

1. Kattintson a kiszolgálóra az Áttekintés oldal megnyitásához. 
2. A **BEÁLLÍTÁSOK** > **Tűzfal** > **Tűzfal engedélyezése** részben kattintson a **Be** lehetőségre.
3. A DirectQuery-hozzáférés engedélyezéséhez a Power BI szolgáltatásból a **Hozzáférés engedélyezése Power BI-ból** területen kattintson a **Be** lehetőségre.  
4. (Opcionális) Adjon meg egy vagy több IP-címtartományt. Adjon meg egy nevet, valamint egy kezdő és egy záró IP-címet mindegyik tartomány számára. 
5. Kattintson a **Save** (Mentés) gombra.

     ![Tűzfalbeállítások](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs rá szükség, törölje az IP-címtartományokat, vagy tiltsa le a tűzfalat.

## <a name="next-steps"></a>További lépések
Ebben a rövid útmutatóban megismerte, hogyan konfigurálhat tűzfalat a kiszolgálójához. Most, hogy tűfalas védelmet biztosított a kiszolgáló számára, hozzáadhat egy alapszintű minta adatmodellt a portálról. A mintamodell azért hasznos, mert segít megismerni a modell adatbázis-szerepkörök konfigurálását és az ügyfélkapcsolatok tesztelését. További tudnivalókért folytassa a mintamodell hozzáadását ismertető oktatóanyaggal.

> [!div class="nextstepaction"]
> [Oktatóanyag: Mintamodell hozzáadása a kiszolgálóhoz](analysis-services-create-sample-model.md)
