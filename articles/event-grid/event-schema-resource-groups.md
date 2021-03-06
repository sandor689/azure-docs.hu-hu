---
title: Az Azure Event Grid erőforrás csoport eseménysémája
description: Ismerteti a szolgáltatást az Azure Event Grid erőforrás-csoport események tulajdonságait
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 08/02/2018
ms.author: tomfitz
ms.openlocfilehash: 407d9fd5b6f4d554af37b60edf12422f8816ac00
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495322"
---
# <a name="azure-event-grid-event-schema-for-resource-groups"></a>Az erőforráscsoportok az Azure Event Grid eseménysémája

Ez a cikk a tulajdonságok és a séma biztosít erőforrás-csoport események. Eseménysémák szeretné megismerni, lásd: [Azure Event Grid-esemény séma](event-schema.md).

Az Azure-előfizetések és -erőforráscsoportok gridre bocsáthatja ki az azonos esemény típusú. Az esemény típusú erőforrások módosításához kapcsolódó. Az elsődleges különbség, hogy erőforráscsoportok gridre bocsáthatja ki az eseményeket az erőforrások az erőforráscsoporton belül, és az Azure-előfizetések eseményeire a PowerShell erőforrások küldik az előfizetésből.

Erőforrás-események PUT, PATCH, jön létre, és törlési műveletek küldött `management.azure.com`. POST és a GET műveletek nem hozható létre eseményeket. Az adatsík küldött műveletek (például `myaccount.blob.core.windows.net`) hozzon létre eseményeket.

Amikor előfizet egy erőforráscsoport eseményeire, a végpont kapja meg az erőforráscsoport összes eseményt. Az események eseményt szeretne látni, például egy virtuális gép frissítése, de események, amelyek esetleg nem fontos számunkra, például az írt új bejegyzést az üzembe helyezési előzmények tartalmazhatnak. Az összes esemény fogadása a végpont és kód írása, amely feldolgozza a kezelni kívánt eseményeket, vagy beállíthatja a szűrőt az esemény-előfizetés létrehozásakor.

Események programozott módon kezelje, rendezheti események megtekintésével a `operationName` értéket. Például az esemény-végpont lehet, hogy csak feldolgozni a műveletek eseményeit, amelyek egyenlőnek kell lennie `Microsoft.Compute/virtualMachines/write` vagy `Microsoft.Storage/storageAccounts/write`.

Az esemény áll, akkor az erőforrás, amely a művelet céljaként megadott erőforrás-Azonosítóját. Erőforrás események szűréséhez állítsa be, adja meg, hogy az erőforrás létrehozásakor az esemény-előfizetés AZONOSÍTÓJÁT. Mintaszkriptek, lásd: [az előfizetés és erőforráscsoport - PowerShell szűrő](scripts/event-grid-powershell-resource-group-filter.md) vagy [az előfizetés és erőforráscsoport - Azure CLI-szűrő](scripts/event-grid-cli-resource-group-filter.md). Szűrés erőforrástípus szerint, használja az értéket a következő formátumban: `/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Compute/virtualMachines`

## <a name="available-event-types"></a>Rendelkezésre álló események típusai

Erőforráscsoportok gridre bocsáthatja ki felügyeleti események Azure Resource Manager, például egy virtuális gép létrehozásakor, vagy a tárfiókot törölték.

| Esemény típusa | Leírás |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Meg, ha egy erőforrás létrehozása vagy a frissítés sikeres. |
| Microsoft.Resources.ResourceWriteFailure | Jelenik meg, ha egy erőforrás-létrehozás vagy frissítés művelet sikertelen lesz. |
| Microsoft.Resources.ResourceWriteCancel | Meg, ha egy erőforrás létrehozása vagy frissítési művelet meg lett szakítva. |
| Microsoft.Resources.ResourceDeleteSuccess | Ha egy erőforrás-törlési művelet sikeres következik be. |
| Microsoft.Resources.ResourceDeleteFailure | Jelenik meg, ha egy erőforrás-törlési művelet sikertelen lesz. |
| Microsoft.Resources.ResourceDeleteCancel | Jelenik meg, ha egy erőforrás-törlési művelet meg lett szakítva. Ez az esemény történik, ha a sablon központi telepítés meg lett szakítva. |

## <a name="example-event"></a>Példa esemény

Az alábbi példa bemutatja a sémában egy **ResourceWriteSuccess** esemény. Ugyanazzal a sémával használt **ResourceWriteFailure** és **ResourceWriteCancel** eltérő értékek az események `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-07-19T18:38:04.6117357Z",
  "id": "4db48cba-50a2-455a-93b4-de41a3b5b7f6",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/write",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "{expiration}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/write",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
}]
```

Az alábbi példa bemutatja a sémában egy **ResourceDeleteSuccess** esemény. Ugyanazzal a sémával használt **ResourceDeleteFailure** és **ResourceDeleteCancel** eltérő értékek az események `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2018-07-19T19:24:12.763881Z",
  "id": "19a69642-1aad-4a96-a5ab-8d05494513ce",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/delete",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "262800",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "DELETE",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2018-02-01"
    },
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
}]
```

## <a name="event-properties"></a>Esemény tulajdonságai

Egy esemény a következő legfelső szintű adatokat tartalmaz:

| Tulajdonság | Típus | Leírás |
| -------- | ---- | ----------- |
| témakör | sztring | A forrás teljes erőforrás elérési útja. Ez a mező nem írható. Event Grid biztosítja ezt az értéket. |
| Tulajdonos | sztring | Az esemény tárgya közzétevő által megadott elérési útja. |
| eventType | sztring | Ehhez eseményre adatforráshoz regisztrált esemény típusok egyikét. |
| eventTime | sztring | Az esemény akkor jön létre az idő alapján a szolgáltató UTC idő. |
| id | sztring | Az esemény egyedi azonosítója. |
| adatok | objektum | Erőforrás-csoport eseményadatokat. |
| dataVersion | sztring | Az adatobjektum sémaverziója. A közzétevő a sémaverziót határozza meg. |
| metadataVersion | sztring | Az esemény-metaadatok sémaverziója. Event Grid sémáját, a legfelső szintű tulajdonságait határozza meg. Event Grid biztosítja ezt az értéket. |

Az objektum a következő tulajdonságokkal rendelkezik:

| Tulajdonság | Típus | Leírás |
| -------- | ---- | ----------- |
| Engedélyezési | objektum | A kért hitelesítést biztosít a műveletet. |
| jogcímek | objektum | A jogcímek tulajdonságait. További információkért lásd: [JWT-specifikáció](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html). |
| correlationId | sztring | A hibaelhárítási művelet azonosítója. |
| Törzsparaméterei | objektum | A művelet részleteit. Ez az objektum csak van hozzáadva, amikor egy meglévő erőforrás frissítése vagy töröl egy erőforrást. |
| ResourceProvider | sztring | Az erőforrás-szolgáltató a művelet végrehajtása. |
| resourceUri | sztring | A műveletet az erőforrás URI azonosítója. |
| operationName | sztring | A végrehajtott műveletet. |
| status | sztring | A művelet állapota. |
| subscriptionId | sztring | Az erőforrás előfizetés-azonosítója. |
| tenantId | sztring | Az erőforrás Bérlőazonosítója. |

## <a name="next-steps"></a>További lépések

* Azure Event Grid bemutatása, lásd: [Mi az Event Grid?](overview.md)
* Az Azure Event Grid-előfizetés létrehozásával kapcsolatos további információkért lásd: [Event Grid-előfizetés séma](subscription-creation-schema.md).
