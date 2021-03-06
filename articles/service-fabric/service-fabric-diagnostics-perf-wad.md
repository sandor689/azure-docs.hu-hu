---
title: Az Azure Service Fabric - teljesítmény figyelése a Windows Azure Diagnostics bővítmény |} A Microsoft Docs
description: Windows Azure Diagnostics használata az Azure Service Fabric-fürtök a teljesítményszámlálók adatainak összegyűjtése.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: srrengar
ms.openlocfilehash: f99206fe673f69c78bf130026207ed58344ccea5
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324425"
---
# <a name="performance-monitoring-with-the-windows-azure-diagnostics-extension"></a>A Windows Azure Diagnostics bővítményt az alkalmazásteljesítmény-figyelés

Ez a dokumentum ismerteti a rendszerteljesítmény-számlálók a Windows-fürtök a Windows Azure Diagnostics (WAD) bővítményével gyűjteményét beállításához szükséges lépéseket. Linux-fürtöket, állítsa be a [OMS-ügynök](service-fabric-diagnostics-oms-agent.md) a csomópontok a teljesítményszámlálók adatainak összegyűjtése. 

 > [!NOTE]
> Az alábbi lépéseket az Ön számára a fürtön a WAD-bővítményt kell telepíteni. Ha nem állította be, látogasson el [esemény összesítésére és a Windows Azure Diagnostics segítségével gyűjteményt](service-fabric-diagnostics-event-aggregation-wad.md).  

## <a name="collect-performance-counters-via-the-wadcfg"></a>A WadCfg keresztül teljesítményszámlálók gyűjtése

Teljesítményszámlálók adatainak összegyűjtése a WAD-n keresztül, akkor kell módosítania a konfiguráció megfelelő a fürt Resource Manager-sablon. Hajtsa végre a következő lépésekkel adhatja hozzá egy teljesítményszámláló gyűjtése a sablonhoz, és a egy Resource Manager-erőforrás frissítése.

1. Keresse meg a WAD-konfigurációt a fürt sablonban – keresés `WadCfg`. Fog rendeléseket teljesítményszámlálókat ad a `DiagnosticMonitorConfiguration`.

2. A konfiguráció beállításához adja hozzá a következő szakaszban, a teljesítményszámlálók adatainak összegyűjtése a `DiagnosticMonitorConfiguration`. 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    A `scheduledTransferPeriod` határozza meg, hogyan konfigurálta az gyűjtött a számlálók értékeit átkerülnek az Azure storage-táblához, és bármely gyakori a fogadó. 

3. A teljesítményszámlálók, amelyet szeretne gyűjteni a felvétele a `PerformanceCounterConfiguration` , amely az előző lépésben lett deklarálva. Minden egyes gyűjteni kívánt teljesítményszámláló van definiálva egy `counterSpecifier`, `sampleRate`, `unit`, `annotation`, és minden kapcsolódó `sinks`.

Íme egy példa a számláló a konfigurációval a *teljes processzoridő* (mennyi ideig feldolgozó műveletekhez használatban volt a CPU) és *Service Fabric Actors megpróbálkoznimásodpercenként*, egy a Service Fabric egyéni teljesítményszámlálók. Tekintse meg [Reliable Actor-teljesítményszámlálók](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters) és [Reliable Service teljesítményszámlálók](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters) Service Fabric – teljesítményszámlálók egyéni teljes listáját.

 ```json
 "WadCfg": {
         "DiagnosticMonitorConfiguration": {
           "overallQuotaInMB": "50000",
           "EtwProviders": {
             "EtwEventSourceProviderConfiguration": [
               {
                 "provider": "Microsoft-ServiceFabric-Actors",
                 "scheduledTransferKeywordFilter": "1",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableActorEventTable"
                 }
               },
               {
                 "provider": "Microsoft-ServiceFabric-Services",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableServiceEventTable"
                 }
               }
             ],
             "EtwManifestProviderConfiguration": [
               {
                 "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                 "scheduledTransferLogLevelFilter": "Information",
                 "scheduledTransferKeywordFilter": "4611686018427387904",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricSystemEventTable"
                 }
               }
             ]
           },
           "PerformanceCounters": {
                 "scheduledTransferPeriod": "PT1M",
                 "PerformanceCounterConfiguration": [
                     {
                         "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                         "sampleRate": "PT1M",
                         "unit": "Percent",
                         "annotation": [
                         ],
                         "sinks": ""
                     },
                     {
                         "counterSpecifier": "\\Service Fabric Actor Method(*)\\Invocations/Sec",
                         "sampleRate": "PT1M",
                     }
                 ]
             }
         }
       },
  ```

 A számláló a mintavételi gyakoriság igényeknek megfelelően módosíthatók. A formátum akkor `PT<time><unit>`, ha azt szeretné, hogy másodpercenként gyűjtött számláló, majd kell beállítania a `"sampleRate": "PT15S"`.

 >[!NOTE]
 >Bár használhatja `*` teljesítményszámlálókat hasonlóképpen nevesített csoportok megadásához számlálókat küldése egy fogadó keresztül (az Application Insightsba) szükséges, hogy azok külön-külön deklarált. 

4. Miután hozzáadta a megfelelő teljesítményszámlálókat kell gyűjteni, a fürt erőforrásai frissíteni, hogy ezek a módosítások megjelennek a futó fürt szeretne. Mentse a módosított `template.json` , és nyissa meg a powershellt. A fürt használatával frissítheti `New-AzureRmResourceGroupDeployment`. A hívás szükség van az az erőforráscsoport, a frissített sablon fájlt, és a paramétereket tartalmazó fájlt, és kérni fogja, hogy a megfelelő módosításokat frissített erőforrások Resource Manager. Miután bejelentkezett a fiókjába, és a megfelelő előfizetéshez tartozik, használja a következő parancsot a frissítés futtatásához:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. A frissítés befejeződése után WAD bevezetéséről (közötti 15-45 percet vesz igénybe), érdemes lehet gyűjtése a teljesítményszámlálók, és elküldi azokat a storage-fiókban WADPerformanceCountersTable nevű táblázat a fürthöz társított. Tekintse meg az Application Insights által a teljesítményszámlálók [a Resource Manager-sablon hozzáadása a mesterséges Intelligencia fogadó](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template).

## <a name="next-steps"></a>További lépések
* A fürt több teljesítményszámlálót gyűjt. Lásd: [teljesítmény-mérőszámok](service-fabric-diagnostics-event-generation-perf.md) listája számlálókat kell gyűjteni.
* [Használat monitorozása és diagnosztizálása egy Windows virtuális gép és az Azure Resource Manager-sablonokkal](../virtual-machines/windows/extensions-diagnostics-template.md) módosításokat továbbá az `WadCfg`, beleértve a diagnosztikai adatok küldése további tárfiókok konfigurálásáról.
