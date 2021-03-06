---
title: Azure Resource Manager-sablonok az Azure Backuphoz
description: Azure Backup PowerShell-minták
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 04/18/2018
ms.author: markgal
ms.custom: mvc
ms.openlocfilehash: 0aac49be397f5e1c86fa834d341399775fd71cfa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34607072"
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Azure Resource Manager-sablonok az Azure Backuphoz

A következő táblázat a helyreállítási tárakhoz és az Azure Backup funkcióihoz használható Azure Resource Manager-sablonokra mutató hivatkozásokat tartalmaz.

|   |   |
|---|---|
|**Helyreállítási tár** | |
| [Helyreállítási tár létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vault-create)| Recovery Services-tároló létrehozása. A tár használható az Azure Backup és az Azure Site Recovery szolgáltatásokkal. |
|**Virtuális gépek biztonsági mentése**| |
| [A Resource Manager által telepített virtuális gépek biztonsági mentése](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-backup-vms) | Használja a meglévő helyreállítási tárat és biztonsági mentési szabályzatot az ugyanabban az erőforráscsoportban lévő, Resource Manager által telepített virtuális gépek biztonsági mentéséhez.|
| [IaaS virtuális gépek biztonsági mentése helyreállítási tárba](https://github.com/Azure/azure-quickstart-templates/tree/master/201-recovery-services-backup-classic-resource-manager-vms) | Sablon a klasszikus és a Resource Manager által telepített virtuális gépek biztonsági mentéséhez. |
| [Heti biztonsági mentési szabályzat létrehozása IaaS virtuális gépekhez](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-weekly-backup-policy-create) | A sablon létrehoz egy helyreállítási tárat és egy heti biztonsági mentési szabályzatot a klasszikus és a Resource Manager által telepített virtuális gépek biztonsági mentéséhez.|
| [Napi biztonsági mentési szabályzat létrehozása IaaS virtuális gépekhez](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-daily-backup-policy-create) | A sablon létrehoz egy helyreállítási tárat és egy napi biztonsági mentési szabályzatot a klasszikus és a Resource Manager által telepített virtuális gépek biztonsági mentéséhez.|
| [Windows Server rendszerű virtuális gép telepítése engedélyezett biztonsági mentéssel](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-create-vm-and-configure-backup) | A sablon létrehoz egy Windows Server rendszerű virtuális gépet és helyreállítási tárat az alapértelmezett biztonsági mentési szabályzattal.|
|**Biztonsági mentési feladatok monitorozása** |  |
| [OMS Log Analytics használata az Azure Backup monitorozásához](https://github.com/Azure/azure-quickstart-templates/tree/master/101-backup-oms-monitoring) | A sablon telepíti az OMS-monitorozást az Azure Backuphoz, lehetővé téve a biztonsági mentési és visszaállítási feladatok, a biztonsági mentési riasztások és a helyreállítási tárban használt felhőtár monitorozását.|  
|   |   |

