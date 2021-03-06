---
title: Az Azure Diagnostics bővítmény áttekintése
description: Az Azure diagnosztikai eszköz használata a hibakeresés, a teljesítmény méréséhez, figyelés, a cloud services, virtual machines és a service fabric forgalomelemzés
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 07/13/2018
ms.author: robb
ms.component: diagnostic-extension
ms.openlocfilehash: b00d774ec59755288b8660d238c7b8dfc9a89eab
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089893"
---
# <a name="what-is-azure-diagnostics-extension"></a>Mi az Azure Diagnostics bővítmény
Az Azure Diagnostics bővítmény az ügynök, amely lehetővé teszi az üzembe helyezett alkalmazás diagnosztikai adatgyűjtés Azure-ban. A diagnosztikai bővítmény számos különféle forrásból származó is használhatja. Jelenleg csak Azure-Felhőszolgáltatás (klasszikus) webes és feldolgozói szerepkörök, virtuális gépek, a virtuálisgép-méretezési csoportok és a Service Fabric. Más Azure-szolgáltatásokkal rendelkezik diagnosztikai különböző módszereket. Lásd: [áttekintése az Azure-ban figyelési](monitoring-overview.md). 

## <a name="linux-agent"></a>Linux-ügynök
A [a bővítmény verziója Linux](../virtual-machines/linux/diagnostic-extension.md) Linux rendszerű virtuális gépek számára elérhető. A gyűjtött és viselkedés érdekében eltérhetnek a Windows-verziót. 

## <a name="data-you-can-collect"></a>Adatokat gyűjthet
Az Azure Diagnostics bővítmény gyűjthet adatokat a következő típusú:

| Adatforrás | Leírás |
| --- | --- |
| Teljesítményszámlálók |Operációs rendszer és az egyéni teljesítményszámlálók |
| Alkalmazásnaplók |Az alkalmazás által írt nyomkövetési üzenetek |
| Windows-eseménynaplók |A Windows-esemény naplózása rendszer küldött adatok |
| .NET-eseményforrás |Kód írása az események a .NET használatával [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) osztályban |
| IIS-naplók |IIS-webhelyek kapcsolatos információk |
| Jegyzékalapú ETW |Esemény-nyomkövetés a Windows események a folyamatok által generált. (1) |
| összeomlási memóriaképek, |Egy alkalmazás összeomlása esetén a folyamat állapotával kapcsolatos információk |
| egyéni hibanaplók, |Az alkalmazás vagy szolgáltatás által létrehozott naplók |
| Az Azure diagnosztikai infrastruktúra naplói |Saját maga diagnosztikai információk |

(1) az ETW-szolgáltatók listájának lekéréséhez futtassa `c:\Windows\System32\logman.exe query providers` a konzolablakban a gépen, amelyet szeretne adatainak összegyűjtése. 

## <a name="data-storage"></a>Adattárolás
A bővítmény tárolja az adatait egy [Azure Storage-fiók](azure-diagnostics-storage.md) megadott. 

Emellett elküldheti az, hogy [Application Insights](../application-insights/app-insights-cloudservices.md). Egy másik lehetőség, hogy adatfolyamként való [Eseményközpont](../event-hubs/event-hubs-what-is-event-hubs.md), amely azután lehetővé teszi, hogy küldje el a nem Azure-beli figyelés szolgáltatásokhoz. 


## <a name="versioning-and-configuration-schema"></a>Verziókezelés és konfigurációs séma
Lásd: [Azure Diagnostics-verzióelőzmények és séma](azure-diagnostics-versioning-history.md).


## <a name="next-steps"></a>További lépések
Válassza ki, melyik szolgáltatás próbált meg a diagnosztikai adatok gyűjtéséhez és a következő cikkek az első lépésekhez. Általános Azure diagnostics hivatkozásokkal az egyes feladatok referenciaként.

## <a name="cloud-services-using-azure-diagnostics"></a>A cloud Services, Azure Diagnostics használatával
* Ha a Visual Studiót használja, lásd: [nyomon követése a Cloud Services-alkalmazás használatát a Visual Studio](../vs-azure-tools-debug-cloud-services-virtual-machines.md) a kezdéshez. Egyéb esetben lásd:
* [Azure Diagnostics-felhőszolgáltatások figyelése](../cloud-services/cloud-services-how-to-monitor.md)
* [A Cloud Services-alkalmazás az Azure Diagnostics beállítása](../cloud-services/cloud-services-dotnet-diagnostics.md)

Az összetettebb témákra lásd:

* [Az Azure Diagnostics használatával az Application insights segítségével a Cloud Services](../application-insights/app-insights-cloudservices.md)
* [A folyamat egy Cloud Services-alkalmazás az Azure Diagnostics segítségével nyomon követése](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [A Cloud Services diagnosztika beállítása a PowerShell használatával](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines"></a>Virtuális gépek
* Ha a Visual Studiót használja, lásd: [Visual Studio használata az Azure Virtual Machines nyomkövetési](../vs-azure-tools-debug-cloud-services-virtual-machines.md) a kezdéshez. Egyéb esetben lásd:
* [Egy Azure virtuális gépen az Azure Diagnostics beállítása](../virtual-machines-dotnet-diagnostics.md)

Az összetettebb témákra lásd:

* [Diagnosztika az Azure Virtual Machinesben beállítása a PowerShell használatával](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Windows virtuális gép létrehozása figyelési és diagnosztikai funkciókkal, az Azure Resource Manager-sablon használatával](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric"></a>Service Fabric
Első lépések [Service Fabric-alkalmazás figyelése](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). További Service Fabric diagnosztikai számos cikkek érhetők el a bal oldali navigációs fában után jelenik meg ebben a cikkben.

## <a name="general-articles"></a>Általános tartalmú cikkek
* Ismerje meg, hogyan [teljesítményszámlálók használata az Azure Diagnostics](../cloud-services/diagnostics-performance-counters.md).
* Ha problémája akad diagnosztika indítása, vagy tekintse meg az adatok keresése az Azure storage-táblák, [Azure Diagnostics hibaelhárítása](azure-diagnostics-troubleshooting.md)
