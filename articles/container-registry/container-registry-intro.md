---
title: Privát Docker-tárolójegyzékek az Azure-ban
description: Bevezetés az Azure Container Registry szolgáltatásba, amely felhőalapú, felügyelt és magán Docker-beállításjegyzékeket biztosít.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: overview
ms.date: 05/08/2018
ms.author: stevelas
ms.custom: mvc
ms.openlocfilehash: 394297e87ef03541725aad0689f11bca17c05ed9
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576300"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Az Azure-beli privát Docker-tárolójegyzékek bemutatása

Az Azure Container Registry egy felügyelt [Docker jegyzékszolgáltatás](https://docs.docker.com/registry/), amely a nyílt forráskódú Docker Registry 2.0 technológiára épül. Az Azure-beli tároló-beállításjegyzékek létrehozásával és fenntartásával tárolhatja és kezelheti privát [Docker-tárolóinak](https://www.docker.com/what-docker) rendszerképeit.

Az Azure-beli tároló-beállításjegyzékeit meglévő tárolófejlesztési és üzembe helyezési folyamataival együtt használhatja. A tárolórendszerképek Azure-ban történő létrehozásához használja az Azure Container Registry Buildet (ACR Build). Igény szerinti vagy teljesen automatizált összeállítás a forráskód véglegesítésével és az alapszintű rendszerkép frissítésével.

A Dockerrel és a tárolókkal kapcsolatos háttér-információkért lásd a [Docker áttekintő ismertetését](https://docs.docker.com/engine/docker-overview/).

## <a name="use-cases"></a>Használati esetek

Rendszerképek lekérése egy Azure-beli tároló-beállításjegyzékből különféle telepítési célokra:

* **Méretezhető előkészítési rendszerek**, amelyek tárolóalapú alkalmazásokat kezelnek gazdagépfürtökben (többek között [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) és [Kubernetes](http://kubernetes.io/docs/)).
* **Azure-szolgáltatások**, amelyek támogatják az alkalmazások építését és nagy mennyiségű alkalmazás futtatását, beleértve az [Azure Kubernetes Service (AKS)](../aks/index.yml), az [App Service](/app-service/index.md), a [Batch](../batch/index.yml), a [Service Fabric](/azure/service-fabric/) és egyéb szolgáltatásokat.

A fejlesztők emellett le is küldhetik a tároló-beállításjegyzékeket a tárolófejlesztési munkafolyamatok részeként. Például megcélozhat egy tároló-beállításjegyzéket egy olyan folyamatos integrációs és üzembe helyezési eszközből, mint a [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) vagy a [Jenkins](https://jenkins.io/).

Konfigurálja úgy az [ACR Build](#azure-container-registry-build) összeállítási feladatait, hogy az alapszintű rendszerképek frissítésekor a rendszer automatikusan újraépítse az alkalmazás-rendszerképeket. Az ACR Build használatával automatizálhatja a rendszerképek összeállítását, ha a csoport kódot véglegesít egy Git-adattárban. *Az ACR Build jelenleg előzetes verzióban érhető el.*

## <a name="key-concepts"></a>Fő fogalmak

* **Beállításjegyzék** – Létrehozhat egy vagy több tároló-beállításjegyzéket Azure-előfizetésében. A beállításjegyzékek három termékváltozatban – [Alapszintű, Standard és Prémium](container-registry-skus.md) – érhetők el. Mindhárom változat egyaránt támogatja a webhook-integrációt, az Azure Active Directoryval való beállításjegyzék-hitelesítést és a törlési funkciót. Hozzon létre egy beállításjegyzéket az üzemelő példányaival megegyező Azure-beli helyen, hogy kiaknázhassa tárolórendszerképei helyi, hálózatközeli tárolásának előnyeit. Haladó szintű replikációs és tárolórendszerkép-elosztási forgatókönyvekhez használja a Prémium szintű beállításjegyzékek [georeplikációs](container-registry-geo-replication.md) funkcióját. A teljes tartománynév `myregistry.azurecr.io` formában van.

  A tároló-beállításjegyzékhez való [hozzáférés szabályozása](container-registry-authentication.md) egy, az Azure Active Directory által támogatott [egyszerű szolgáltatással](../active-directory/develop/app-objects-and-service-principals.md) vagy a rendszergazdai fiókkal lehetséges. A beállításjegyzéken való hitelesítéshez futtassa a szabványos `docker login` parancsokat.

* **Tár** – A beállításjegyzékek egy vagy több tárat tartalmaznak, amelyek tárolórendszerképek csoportjai. Az Azure Container Registry támogatja a többszintű adattárnévtereket. A többszintű névterekkel csoportba rendezheti egy adott alkalmazáshoz vagy alkalmazások gyűjteményéhez kapcsolódó rendszerképek gyűjteményeit az egyes fejlesztői és üzemeltetői csoportok számára. Például:

  * A(z) `myregistry.azurecr.io/aspnetcore:1.0.1` egy, a teljes vállalatban elérhető rendszerképet jelöl
  * A(z) `myregistry.azurecr.io/warrantydept/dotnet-build` egy .NET-alkalmazások felépítéséhez használt rendszerképet jelöl, amely a jótállási részlegen van megosztva
  * A(z) `myregistry.azurecr.io/warrantydept/customersubmissions/web` egy, az ügyfélbeadványok alkalmazásban csoportosított webes rendszerképet jelöl, amely a jótállási részleg tulajdona

* **Rendszerkép** – Az adattárakban tárolt egyes rendszerképek egy-egy Docker-tároló csak olvasható pillanatfelvételei. Az Azure tároló-beállításjegyzékek Windows- és Linux-rendszerképeket is tartalmazhatnak. A rendszerképek neveit Ön határozza meg mindegyik tárolókörnyezetben. A rendszerképek szabványos [Docker-parancsokkal](https://docs.docker.com/engine/reference/commandline/) küldhetők le egy adattárba, vagy hívhatók elő onnan.

* **Tároló** – A tároló egy szoftveralkalmazást határoz meg annak függőségeivel együtt egy teljes fájlrendszerbe csomagolva, beleértve a kódot, a futtatókörnyezetet, a rendszereszközöket és a könyvtárakat. A Docker-tárolókat a tároló-beállításjegyzékekből előhívott Windows- vagy Linux-rendszerképek alapján futtathatja. Az egy gépen futó tárolók osztoznak az operációs rendszer kernelén. A Docker-tárolók teljes mértékben használhatók az összes nagyobb Linux-disztribúción, MacOS-en és Windowson is.

## <a name="azure-container-registry-build-preview"></a>Azure Container Registry Build (előzetes verzió)

Az [Azure Container Registry Build](container-registry-build-overview.md) (ACR Build) az Azure Container Registry egy szolgáltatáscsomagja, amely lehetővé teszi a Docker-tárolórendszerképek zökkenőmentes és hatékony összeállítását az Azure-ban. Az ACR Build használatával kiterjesztheti a felhőbe a belső fejlesztési ciklust a `docker build`-műveletek Azure-ba való áthelyezése által. Konfigurálhat összeállítási feladatokat a tároló operációs rendszerének és a keretrendszer javítási folyamatának automatizálására, valamint a rendszerképek automatikus összeállítására, ha a csoport kódot véglegesít a forráskezelőben.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## <a name="next-steps"></a>További lépések

* [Tároló-beállításjegyzék létrehozása az Azure Portalon](container-registry-get-started-portal.md)
* [Tároló beállításjegyzék létrehozása az Azure CLI-vel](container-registry-get-started-azure-cli.md)
* [Operációsrendszer- és keretrendszer-javítás az ACR Build használatával](container-registry-build-overview.md) (előzetes verzió)
