---
title: Ismerkedés a hibrid Azure Relay-kapcsolatok websocketjeivel a Node-ban | Microsoft Docs
description: Node.js-konzolalkalmazást hozhat létre a hibrid Azure Relay-kapcsolatok websocketjeihez.
services: service-bus-relay
documentationcenter: node
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 05/02/2018
ms.author: sethm
ms.openlocfilehash: 1e0b76b96029e1a7ed84f1c8cd895090e8acbc6f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38671002"
---
# <a name="get-started-with-relay-hybrid-connections-websockets-in-node"></a>Ismerkedés a hibrid Relay-kapcsolatok websocketjeivel a Node-ban

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Ez az oktatóanyag bevezetést nyújt a [hibrid Azure Relay-kapcsolatok](relay-what-is-it.md#hybrid-connections) Websockets funkciójának használatába, és bemutatja egy olyan ügyfélalkalmazás Node.js használatával való létrehozását, amely Websockets-üzeneteket küld egy kapcsolódó figyelőalkalmazásnak.

## <a name="what-will-be-accomplished"></a>Az oktatóanyag célja

A hibrid kapcsolatokhoz egy ügyfélre és egy kiszolgáló-összetevőre is szükség van, így ebben az oktatóanyagban két konzolalkalmazást hozunk létre. A lépések a következők:

1. Relay-névtér létrehozása az Azure Portal használatával.
2. Hibrid kapcsolat létrehozása az Azure Portal használatával.
3. Kiszolgálói konzolalkalmazás írása üzenetfogadási céllal.
4. Ügyfél-konzolalkalmazás írása üzenetküldési céllal.

## <a name="prerequisites"></a>Előfeltételek

1. [Node.js](https://nodejs.org/en/).
2. Azure-előfizetés.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Névtér létrehozása az Azure Portal használatával

Ha a Relay-névteret már létrehozta, folytassa a [Hibrid kapcsolat létrehozása az Azure Portal használatával](#2-create-a-hybrid-connection-using-the-azure-portal) szakasszal.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Hibrid kapcsolat létrehozása az Azure Portal használatával

Ha már rendelkezik egy létrehozott hibrid kapcsolattal, folytassa a [Kiszolgálói alkalmazás létrehozása](#3-create-a-server-application-listener) szakasszal.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Kiszolgálói alkalmazás (figyelő) létrehozása

Írjon egy Node.js-konzolalkalmazást az üzenetek figyeléséhez és a Relaytől való fogadásához.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Ügyfélalkalmazás létrehozása (küldő)

Írjon egy Node.js konzolalkalmazást az üzenetek a Relay számára való küldéséhez.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Az alkalmazások futtatása

1. Futtassa a kiszolgálóalkalmazást: egy Node.js-parancssorba írja be a következőt: `node listener.js`.
2. Futtassa az ügyfélalkalmazást: egy Node.js parancssorba írja be a `node sender.js` parancsot, majd írjon be szöveget.
3. Győződjön meg arról, hogy az alkalmazás konzolja kiírja a szöveget, amely az ügyfélalkalmazásban lett megadva.

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Gratulálunk, végpontok közötti hibrid kapcsolatok alkalmazást hozott létre a Node.js használatával.

## <a name="next-steps"></a>További lépések

* [Relay – gyakori kérdések](relay-faq.md)
* [Névtér létrehozása](relay-create-namespace-portal.md)
* [Ismerkedés a .NET-tel](relay-hybrid-connections-dotnet-get-started.md)
* [Bevezetés a Node használatába](relay-hybrid-connections-node-get-started.md)

