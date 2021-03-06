---
title: Azure Service Fabric tárolószolgáltatások csoportosan felügyelt szolgáltatásfiók beállítása |} Microsoft Docs
description: Ismerje meg, ha folytatni szeretné az Azure Service Fabric-beli tárolója csoportosan felügyelt szolgáltatásfiók beállítása.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: subramar
ms.openlocfilehash: e4cd0b42e21609f88edc28c8fd7b5c433d56b3c1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209093"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>A Service Fabric futó Windows-tárolók csoportosan felügyelt szolgáltatásfiók beállítása

Csoportosan felügyelt szolgáltatásfiók (felügyelt szolgáltatásfiókok. csoport), a hitelesítő adatok megadását fájlját beállítása (`credspec`) helyezkedik el a fürt összes csomópontján. A fájl az összes olyan csomóponton, a Virtuálisgép-bővítmény használatával lehet másolni.  A `credspec` fájl tartalmaznia kell a csoportosan felügyelt fiók adatait. További információ a `credspec` fájl című [szolgáltatásfiókok](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). A hitelesítő adatok megadását és a `Hostname` címke meg van határozva a az alkalmazás jegyzékében. A `Hostname` címke meg kell egyeznie a tárolóban fut, a csoportosan felügyelt szolgáltatásfiók fióknevet.  A `Hostname` címke lehetővé teszi, hogy a tárolót, hogy hitelesítse magát a Kerberos-hitelesítés tartományban más szolgáltatásokhoz.  Adja meg a minta a `Hostname` és a `credspec` az alkalmazás a jegyzékben megjelenik-e a a következő kódrészletet:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
A következő lépésben olvassa el a következő cikkeket:

* [A Service Fabric Windows Server 2016 egy Windows-tároló telepítése](service-fabric-get-started-containers.md)
* [Telepítse a Service Fabric Linux egy Docker-tároló](service-fabric-get-started-containers-linux.md)
