---
title: Meglévő Azure App Service csatlakozzon az Azure-adatbázis az MySQL
description: Utasításokat megfelelően kapcsolódás MySQL az Azure-adatbázishoz egy meglévő Azure App Service
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: ff4a28e2f9a0149016d0e47c24e4665ab2e0500d
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265503"
---
# <a name="connect-an-existing-azure-app-service-to-azure-database-for-mysql-server"></a>Egy meglévő Azure App Service csatlakozzon az Azure Database MySQL-kiszolgáló
Ez a témakör azt ismerteti, hogyan adatbázishoz való csatlakozáshoz egy meglévő Azure App Service szolgáltatásban az Azure MySQL-kiszolgáló.

## <a name="before-you-begin"></a>Előkészületek
Jelentkezzen be az [Azure portálra](https://portal.azure.com). MySQL-kiszolgáló Azure-adatbázis létrehozása. További információkért tekintse meg [Azure-adatbázis létrehozása a portálról MySQL kiszolgáló](quickstart-create-mysql-server-database-using-azure-portal.md) vagy [Azure-adatbázis létrehozása a parancssori felület használatával MySQL-kiszolgáló](quickstart-create-mysql-server-database-using-azure-cli.md).

Jelenleg nincsenek Azure-adatbázishoz egy Azure App Service szolgáltatásban való hozzáférésének engedélyezésére vonatkozó MySQL két megoldások. A két megoldás tartalmaz, amely kiszolgálószintű tűzfal-szabályok beállítása.

## <a name="solution-1---create-a-firewall-rule-to-allow-all-ips"></a>1 - megoldás hozzon létre egy tűzfalszabályt, hogy az összes IP-címek
Azure MySQL-adatbázis származó tűzfalat használ az adatok védelme érdekében biztonsági biztosít. Az Azure App Service az adatbázishoz való kapcsolódáskor Azure MySQL-kiszolgáló, ne feledje, hogy a kimenő IP-címek az App Service dinamikus jellegű. 

Az Azure App Service rendelkezésre állásának biztosítása érdekében javasoljuk, ezzel a megoldással minden IP-címek engedélyezése.

> [!NOTE]
> Microsoft dolgozik a hosszú távú megoldás elkerülése érdekében, amely lehetővé teszi az összes IP-címek az Azure szolgáltatások MySQL Azure adatbázishoz való kapcsolódáshoz.

1. A MySQL kiszolgálót a beállítások panelen elemcsoportban kattintson **kapcsolatbiztonsági** kapcsolatbiztonsági panel megnyitásához az Azure-adatbázis a MySQL.

   ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Adja meg **SZABÁLYNÉV**, **KEZDŐ IP-**, és **záró IP-Címnél**, és kattintson a **mentése**.
   - A szabálynév: engedélyezése – minden-IP-címek
   - IP elindítása: 0.0.0.0
   - Záró IP: 255.255.255.255

   ![Azure portál – az összes IP-címek hozzáadása](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-to-explicitly-allow-outbound-ips"></a>2 - megoldás hozzon létre egy tűzfalszabályt, explicit módon engedélyezi a kimenő IP-címek
A kimenő IP az Azure App Service explicit módon adhat hozzá.

1. Az App Service tulajdonságai panelen megtekintheti a **kimenő IP-cím**.

   ![Azure portál – nézet kimenő IP-címek](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. A MySQL-kapcsolat biztonsági panelen vegye fel a kimenő IP-címet egyenként.

   ![Azure portál – explicit IP-címek hozzáadása](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Ne felejtse el **mentése** a tűzfalszabályok között.

Bár az Azure App service megpróbálja adott idő alatt tartani az IP-címek állandó, vannak esetek, ahol az IP-címek megváltozhatnak. Például ez akkor fordulhat elő, amikor az alkalmazás újrahasznosítja azt vagy a méretezési művelet történik, vagy amikor új számítógépek hozzáadása a következő Azure regionális adatok adatközpontok növelhető a kapacitása. Ha az IP-címek módosítása, az alkalmazás sikerült leállásra abban az esetben, ha már nem tud kapcsolódni az a MySQL-kiszolgálóhoz. Az előző megoldásoknak a valamelyikét kiválasztásakor tartsa szem előtt a figyelmet.

## <a name="ssl-configuration"></a>SSL-beállítása
Azure MySQL-adatbázis SSL alapértelmezés szerint engedélyezve van. Ha az alkalmazás nem használ SSL az adatbázishoz való kapcsolódáshoz, majd szeretné az SSL letiltásához a MySQL-kiszolgálón. További SSL konfigurálásával kapcsolatos további információkért lásd: [SSL használatával MySQL az Azure-adatbázissal](howto-configure-ssl.md).

## <a name="next-steps"></a>További lépések
Kapcsolati karakterláncokkel kapcsolatos további információkért tekintse meg [kapcsolati karakterláncok](howto-connection-string.md).
