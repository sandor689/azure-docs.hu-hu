---
title: Az Azure Service Fabric-háló kapcsolatos gyakori kérdések |} A Microsoft Docs
description: További információk a gyakori kérdések és válaszok az Azure Service Fabric-háló.
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 06/25/2018
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: d0ae7fbb22f6d98662f83968158182d447a75394
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2018
ms.locfileid: "39501967"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Gyakori kérdések Service Fabric-háló
Az Azure Service Fabric Mesh egy teljes körűen felügyelt szolgáltatás, amely lehetővé teszi a fejlesztők számára a mikroszolgáltatás-alkalmazások üzembe helyezését a virtuális gépek, a tárolók és a hálózat kezelése nélkül. Ez a cikk gyakori kérdésekre adott válaszokat tartalmaz.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>Hogyan probléma jelentéséhez vagy tegyen fel kérdést?

Kérdések, válaszok a Microsoft mérnökei és a problémák jelentése a [service-fabric-háló-preview GitHub-adattárat](https://aka.ms/sfmeshissues).

## <a name="quota-and-cost"></a>Kvóta- és költség

**Az előzetes verzióra való részvételnek mennyibe kerül?**

Nem számítunk fel díjat a háló előzetes alkalmazások vagy a tárolók üzembe helyezése. Azonban értékekeit törli az erőforrások üzembe helyezése, és nem hagyhatja őket fut, kivéve, ha aktív tesztelés.

**A magok és a RAM-MAL számát kvóta korlátozva van?**

Igen, a kvóták minden egyes előfizetés esetében:

- Alkalmazások – 5 száma 
- Alkalmazás – 12 magok száma 
- Alkalmazásonkénti - 48 GB memória teljes Méretét 
- A hálózati bejövő és kimenő végpontok – 5 száma  
- Azure kötetek számát is csatlakoztatható - 10-es 
- Szolgáltatás replikák – 3 száma 
- 4 mag, 16 GB RAM a legnagyobb tárolót telepíthet korlátozódik.
- 0,5 mag, legfeljebb 6 magok lépésekben a tárolókat a részleges magok foglalhat.

**Hagyhatja éjjelente fut az alkalmazásom?**

Igen, akkor is, azonban, törölje az erőforrások üzembe helyezése, és nem hagyhatja őket arra biztatjuk fut, kivéve, ha aktív tesztelés. Ez a szabályzat a későbbiekben változhat, és ha vannak veszteségként törlődhetnek az erőforrásai.

## <a name="supported-container-os-images"></a>Támogatott operációs rendszer tárolórendszerképek
A következő operációs rendszer tárolórendszerképeket is használható, amikor a szolgáltatások üzembe helyezése.

- Windows - windowsservercore és nanoserver
    - Windows Server 2016
    - A Windows Server 1709-es verzió
- Linux
    - Nem ismert korlátozások

## <a name="features-gaps-and-known-issues"></a>Szolgáltatások hiányosságokat és ismert problémák

**Miután üzembe az alkalmazást, a hozzá társított hálózati erőforrás úgy tűnik, hogy IP-cím**

Mai késleltetés után hamarosan IP-címmel rendelkező egy ismert probléma van. A hálózati erőforrás állapotának ellenőrzéséhez tekintse meg a társított IP-cím, néhány perc múlva.

**Az alkalmazásom nem tudta elérni a megfelelő hálózati/köteterőforrás**

Az alkalmazásmodell kell érhetik el a társított erőforrás hálózatok és a kötetek teljes erőforrás-azonosítója használatával. Itt van ez néz ki a rövid útmutató minta:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

**Nem látom az aktuális alkalmazás modell támogatása olyan módon moje tajné kódy titkosításához**

Igen, titkos kulcsok titkosítása nem támogatott az aktuális privát előzetes verzióban érhető el. 

**DNS nem működik ugyanúgy, mint a Service Fabric fejlesztési fürtöt és a háló**

Nincs olyan ismert probléma, ahol szükség lehet a szolgáltatások eltérően a helyi fejlesztési fürt és az Azure-háló hivatkozhat. A helyi fejlesztési fürt használja {a(z) servicename szolgáltatáshoz}. {applicationName}. Az Azure Service Fabric tervezhetők használja {a(z) servicename szolgáltatáshoz}. Az Azure-háló jelenleg nem támogatja dns-feloldás alkalmazások között.

Egyéb ismert DNS problémái egy Service Fabric fejlesztési fürtöt futtat Windows 10-es, lásd itt: [Debug Windows-tárolók](/azure/service-fabric/service-fabric-how-to-debug-windows-containers).

**Ez a hiba jelenik meg a parancssori felület, a ImportError modul használata esetén: "sdk_no_wait" neve nem lehet importálni**

2.0.30-as-nál régebbi parancssori felület verziójának használatakor, előfordulhat, hogy ez a hibaüzenet-

```
cannot import name 'sdk_no_wait'
Traceback (most recent call last):
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\cli.py", line 193, in invoke cmd_result = self.invocation.execute(args)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 241, in execute self.commands_loader.load_arguments(command)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 201, in load_arguments self.command_table[command].load_arguments() # this loads the arguments via reflection
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 142, in load_arguments super(AzCliCommand, self).load_arguments()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\commands.py", line 76, in load_arguments cmd_args = self.arguments_loader()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 332, in default_arguments_loader op = handler or self.get_op_handler(operation)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 375, in get_op_handler op = import_module(mod_to_import)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\importlib_init_.py", line 126, in import_module return _bootstrap._gcd_import(name[level:], package, level)
File "", line 978, in _gcd_import
File "", line 961, in _find_and_load
File "", line 950, in _find_and_load_unlocked
File "", line 655, in _load_unlocked
File "", line 678, in exec_module
File "", line 205, in _call_with_frames_removed
File "C:\Users\annayak.azure\cliextensions\azure-cli-sbz\azext_sbz\custom.py", line 18, in 
from azure.cli.core.util import get_file_json, shell_safe_json_parse, sdk_no_wait
ImportError: cannot import name 'sdk_no_wait'.
```

**A CLI-bővítmény csomag telepítésekor egy eltérő terjesztési kapcsolatos hiba beolvasása**

Ez nem jelenti azt, hogy a bővítmény telepítése nem. Továbbra is kell tudni használni a CLI-parancsok hiba nélkül.

**Tudom a horizontális felskálázáshoz jelenik meg, hogy az összes tároló is érintett, beleértve a saját futó eszközök**

Ez a hiba, és rögzíteni kell a következő modul frissítéskor.

## <a name="next-steps"></a>További lépések

Service Fabric-háló kapcsolatos további információkért olvassa el a [áttekintése](service-fabric-mesh-overview.md).
