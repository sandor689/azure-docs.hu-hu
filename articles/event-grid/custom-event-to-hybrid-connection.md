---
title: Egyéni Azure Event Grid-események küldése hibrid kapcsolatba | Microsoft Docs
description: Az Azure Event Grid és az Azure CLI segítségével közzétehet egy témakört, és feliratkozhat a kapcsolódó eseményre. Végpontként egy hibrid kapcsolatot használunk.
services: event-grid
keywords: ''
author: tfitzmac
ms.author: tomfitz
ms.date: 06/29/2018
ms.topic: tutorial
ms.service: event-grid
ms.openlocfilehash: 544f5210adbea6791f9224a1e2be0743ce9995d5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434146"
---
# <a name="route-custom-events-to-azure-relay-hybrid-connections-with-azure-cli-and-event-grid"></a>Egyéni események átvitele hibrid Azure Relay-kapcsolatokon keresztül az Azure CLI és az Event Grid segítségével

Az Azure Event Grid egy felhőalapú eseménykezelési szolgáltatás. A hibrid Azure Relay-kapcsolat az egyik támogatott eseménykezelő. A hibrid kapcsolatok eseménykezelőként való használatára akkor lehet szükség, ha olyan alkalmazásokból kell eseményeket kezelnie, amelyek nem rendelkeznek nyilvános végponttal. A vállalati hálózaton belül is lehetnek ilyen alkalmazások. Ebben a cikkben létrehozunk egy egyéni témakört az Azure CLI-vel, feliratkozunk a témakörre, majd elindítjuk az eseményt az eredmény megtekintéséhez. Az eseményeket hibrid kapcsolatokba küldjük.

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk feltételezi, hogy már rendelkezik egy hibrid kapcsolattal és egy figyelőalkalmazással. A hibrid kapcsolatok használatbavételéhez tekintse át a [Bevezetés a hibrid Relay-kapcsolatok használatába – .NET](../service-bus-relay/relay-hybrid-connections-dotnet-get-started.md) vagy a [Bevezetés a hibrid Relay-kapcsolatok használatába – Node](../service-bus-relay/relay-hybrid-connections-node-get-started.md) című témakört.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Az Event Grid-témakörök Azure-erőforrások, amelyeket egy Azure-erőforráscsoportba kell helyezni. Az erőforráscsoport egy olyan logikai gyűjtemény, amelyben a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.

Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#az-group-create) paranccsal. 

A következő példában létrehozunk egy *gridResourceGroup* nevű erőforráscsoportot a *westus2* helyen.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Egyéni témakör létrehozása

Az Event Grid-témakörök egy felhasználó által meghatározott végpontot biztosítanak, amelyben közzéteheti az eseményeket. Az alábbi példa az erőforráscsoportban létrehozza az egyéni témakört. A `<topic_name>` elemet a témakör egyedi nevére cserélje le. A témakör nevének egyedinek kell lennie, mert a nevet egy DNS-bejegyzés képviseli.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="subscribe-to-a-topic"></a>Feliratkozás témakörre

A témakörre való feliratkozással lehet tudatni az Event Griddel, hogy mely eseményeket kívánja nyomon követni. Az alábbi példa feliratkozik a létrehozott témakörre, és átadja a hibrid kapcsolat erőforrás-azonosítóját a végpontnak. A hibrid kapcsolat azonosítója a következő formátumot követi:

`/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Relay/namespaces/<relay-namespace>/hybridConnections/<hybrid-connection-name>`

Az alábbi szkript lekéri a Relay-névtér erőforrás-azonosítóját. Létrehozza a hibrid kapcsolat azonosítóját, és feliratkozik egy Event Grid-témakörre. A szkript a végpontot `hybridconnection` típusúra állítja be, és a hibrid kapcsolat azonosítóját használja végpontként.

```azurecli-interactive
relayname=<namespace-name>
relayrg=<resource-group-for-relay>
hybridname=<hybrid-name>

relayid=$(az resource show --name $relayname --resource-group $relayrg --resource-type Microsoft.Relay/namespaces --query id --output tsv)
hybridid="$relayid/hybridConnections/$hybridname"

az eventgrid event-subscription create \
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --name <event_subscription_name> \
  --endpoint-type hybridconnection \
  --endpoint $hybridid
```

## <a name="create-application-to-process-events"></a>Alkalmazás létrehozása események feldolgozásához

Olyan alkalmazásra van szüksége, amely eseményeket tud lekérni a hibrid kapcsolatból. Ezt a műveletet a [Microsoft Azure Event Grid hibrid kapcsolat C# fogyasztói mintája](https://github.com/Azure-Samples/event-grid-dotnet-hybridconnection-destination) hajtja végre. Már befejezte az előfeltételként felsorolt lépéseket.

1. Győződjön meg arról, hogy a Visual Studio 2017 15.5-ös vagy újabb verziója van telepítve.

1. Klónozza az adattárat a helyi gépre.

1. Töltse be a HybridConnectionConsumer projektet a Visual Studióban.

1. A Program.cs fájlban cserélje le a `<relayConnectionString>` és `<hybridConnectionName>` elemeket a továbbítási kapcsolat sztringjére és a létrehozott hibrid kapcsolatra.

1. Fordítsa és futtassa az alkalmazást a Visual Studióból.

## <a name="send-an-event-to-your-topic"></a>Esemény elküldése a témakörbe

Aktiváljunk egy eseményt, és lássuk, hogyan küldi el az üzenetet az Event Grid a végpontnak. Ez a cikk bemutatja, hogyan aktiválhatja az eseményt az Azure CLI használatával. Használhatja az [Event Grid közzétételi alkalmazást](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/tree/master/EventGridPublisher) is.

Először szükségünk lesz az egyéni témakör URL-címére és kulcsára. A `<topic_name>` helyett használja ismét a témakör nevét.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

A folyamat leegyszerűsítése érdekében használjon mintául szolgáló eseményadatokat, amelyeket elküldhet a témakörbe. Egy alkalmazás vagy Azure-szolgáltatás általában eseményadatokat küld el. A CURL egy olyan segédprogram, amely HTTP-kéréseket küld. Ebben a cikkben a CURL használatával küldjük el az eseményt a témakörbe.  Az alábbi példa három eseményt küld el az Event Grid-témakörnek:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

A figyelőalkalmazásnak meg kell kapnia az eseményüzenetet.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása
Ha tovább kívánja használni az eseményt, akkor ne törölje a cikkben létrehozott erőforrásokat. Ellenkező esetben a következő paranccsal törölheti a cikkben létrehozott erőforrásokat.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>További lépések

Most, hogy megismerkedett vele, hogyan hozhat létre témaköröket és eseményfeliratkozásokat, bővebben is tájékozódhat arról, hogy miben nyújthat segítséget az Event Grid:

- [Bevezetés az Event Grid használatába](overview.md)
- [Azure Blob Storage-események átirányítása egyéni webes végpontra](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Virtuális gépek módosításainak monitorozása az Azure Event Grid és a Logic Apps segítségével](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Big Data típusú adatok streamelése adattárházba](event-grid-event-hubs-integration.md)
