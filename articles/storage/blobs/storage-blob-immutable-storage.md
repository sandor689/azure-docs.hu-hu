---
title: Az Azure Blob storage (előzetes verzió) nem módosítható storage |} A Microsoft Docs
description: Az Azure Storage kínál FÉREG (egyszer az írási, olvasási több) támogatása (objektum) a Blob storage, amely lehetővé teszi a felhasználók számára nem törölhető, nem módosítható állapotban tárolva az adatok egy megadott időszakkal.
services: storage
author: sangsinh
ms.service: storage
ms.topic: article
ms.date: 05/29/2018
ms.author: sangsinh
ms.component: blobs
ms.openlocfilehash: cfc25906e926e8dd6687eeccd311a38653772c4d
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398998"
---
# <a name="store-business-critical-data-in-azure-blob-storage-preview"></a>Üzleti szempontból kritikus fontosságú adatok Store az Azure Blob storage (előzetes verzió)

Nem módosítható storage (object) az Azure Blob Storage lehetővé teszi a felhasználók üzleti szempontból kritikus fontosságú adatokat tárolni a FÉREG (egyszer az írási, olvasási több) állapotú. Ebben az állapotban teszi az adatok nem törölhető és nem módosítható egy felhasználó által megadott időtartam alatt. Blobok is létrehozott, és olvassa el, de nem módosítható és nem törölhető, a megőrzési időtartam idejére.

## <a name="overview"></a>Áttekintés

Nem módosítható storage segíti a pénzügyi intézmények és a kapcsolódó iparágak--különösen átvitelszervező-kereskedő szervezetek--biztonságosan tárolhatja az adatokat.

Jellemző alkalmazási területek:

- **A jogszabályoknak való megfelelőség**: nem módosítható, az Azure Blob storage tárolási segít a szervezeteknek cím mp 17a-4(f), CFTC 1.31(d), finra az Egyesült és más szabályozások előírásainak betartását.

- **Biztonságos dokumentum megőrzési**: Blob storage biztosítja, hogy adatokat nem módosítható és nem törölhető bármely felhasználó, beleértve a fiók rendszergazdai jogosultságokkal rendelkező felhasználókat.

- **Jogi céllal zároltak közé**: az Azure Blob storage nem módosítható storage lehetővé teszi a felhasználók szempontjából kritikus fontosságú vagy rendőrségi vizsgálat hamisíthatatlan állapotban a kívánt időszakra bizalmas adatok tárolásához.

Nem módosítható storage lehetővé teszi, hogy:

- **Időalapú adatmegőrzési házirend támogatási**: tárolva az adatok egy megadott időszakkal házirendek beállítását a felhasználók.

- **Jogi célú visszatartási szabályzatot támogatási**: Ha a megőrzési időtartam nem ismert, a felhasználók is meg jogi célú visszatartással adatok immutably tárolásához, a jogi céllal zároltak közé történő törléséig.  Jogi célú visszatartás esetén a blobokat létre lehet hozni és be lehet olvasni, de nem lehet őket sem módosítani, sem törölni. Minden jogi céllal zároltak közé hozzárendelve egy felhasználó által definiált alfanumerikus kód, amely egy karakterlánc-azonosítót (például Ügyfélazonosítóként).

- **Az összes blob rétegek támogatás**: FÉREG házirendek az Azure Blob storage-rétegét független, és a alkalmazni kell az összes szint: gyakori és ritka elérésű, valamint az archivált adatok. Felhasználók adatok módosíthatatlansági fenntartásával tárolhatja a legtöbb költség optimalizált szint számítási feladatai az adatok.

- **Tároló szintű konfigurációs**: felhasználók konfigurálható az időalapú adatmegőrzési házirendek, és a jogi célú visszatartási címkék a tároló szintjén. Egyszerű tároló eszközszintű beállítások használatával a felhasználók létrehozhatják és időalapú adatmegőrzési; zárolása adatmegőrzési időköz; kiterjesztése Állítsa be, és törölje a jogi célú visszatartással; és továbbiakat. Ezeket a szabályzatokat alkalmazni a meglévő és az új tároló összes blobjának.

- **Audit naplózás támogatást**: a tárolók az auditnapló tartalmazza. Zárolt időalapú adatmegőrzési házirendek, három naplók megőrzési időszak bővítmények legfeljebb öt időalapú adatmegőrzés parancsok jeleníti meg. Időalapú adatmegőrzés a napló tartalmazza a felhasználói azonosító, a parancs típusa, az időbélyegeket és a megőrzési időtartam. Jogi célú visszatartással a napló tartalmazza a felhasználói azonosító, a parancs típusa, az időbélyegzőket és jogi célú visszatartási címkék. Ez a napló őrizzük meg a tároló mp 17a-4(f) szabályozási előírások alapján élettartamát. A [Azure-tevékenységnapló](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) adatsík tevékenységek összes vezérlőelem átfogóbb naplóját jeleníti meg. Feladata a felhasználó, szabályozási és más célokra is szükség lehet, tartósan tárolja ezeket a naplókat.

Nem módosítható tárolás engedélyezve van az Azure összes nyilvános régióban.

## <a name="how-it-works"></a>Működés

Az Azure Blob storage nem módosítható storage támogatja a FÉREG vagy nem módosítható házirendek két típusa: időalapú adatmegőrzés és jogi célú visszatartással. Ezek nem módosítható a házirendek létrehozásával kapcsolatos részletekért lásd: a [bevezetés](#Getting-started) szakaszban.

Egy időalapú adatmegőrzési szabály vagy a jogi céllal zároltak közé egy tárolót az alkalmazása esetén az összes meglévő blobok helyezze át a nem módosítható (írási és törlési védett) állapot. A nem módosítható állapotban is át a tárolóba feltöltött összes új blobok.

> [!IMPORTANT]
> Időalapú adatmegőrzési kell *zárolva* kell lennie egy nem módosítható a BLOB (írási és törlési védett) állapot mp 17a-4(f) és egyéb szabályozásoknak való megfelelőséget. Azt javasoljuk, hogy egy ésszerű időn belül, általában 24 órán belül a házirend zárolása. Nem javasoljuk a *oldják a zárolást* állapot rövid távú funkció kísérletek kívül más célra.

Időalapú adatmegőrzési szabály alkalmazásakor a tárolón a tárolóban lévő összes BLOB időtartama nem módosítható állapotban marad a *hatékony* megőrzési ideje. A meglévő blobok esetében a tényleges adatmegőrzési időtartam megegyezik a blob létrehozási ideje és a felhasználó által megadott megőrzési intervallum különbözetével. 

Az új blobok esetében az adatmegőrzési időtartam egyenlő a felhasználó által megadott adatmegőrzési intervallummal. Mivel a felhasználók megváltoztathatják a megőrzési időtartam, nem módosítható tároló a tényleges megőrzési időtartam kiszámításához használja a legutóbbi érték a felhasználó által megadott megőrzési időtartam.

> [!TIP]
> Példa:
> 
> A felhasználó a megőrzési időtartam öt évig időalapú adatmegőrzési hoz létre.
>
> A meglévő blobot a tárolóban, testblob1, létrejött egy évre visszamenőleg. A tényleges megőrzési időszak testblob1 négy év meghatározva.
>
> Egy új blob, a testblob2, feltöltésre kerül a tárolóba. Az új blob hatékony megőrzési időtartama öt év meghatározva.

### <a name="legal-holds"></a>Jogi célú visszatartások

Megadva jogi célú visszatartás beállításakor minden meglévő és új blobok nem módosítható állapotban marad, amíg nem törlődik a jogi céllal zároltak közé. Set, és törölje a jogi célú visszatartással vonatkozó további információkért lásd: a [bevezetés](#Getting-started) szakaszban.

Egy tároló lehet megadva jogi célú visszatartás és a egy időalapú adatmegőrzési szabály egy időben. A tárolóban lévő összes BLOB állapotban marad a nem módosítható mindaddig, amíg minden jogi célú visszatartással nincsenek bejelölve, akkor is, ha azok hatékony megőrzési ideje lejárt. Ezzel szemben egy blob marad nem módosítható állapotban addig, amíg a tényleges megőrzési időtartam lejár, annak ellenére, hogy minden jogi célú visszatartással törölve lett.

Az alábbi táblázat a Művelettípusok blob, a másik nem módosítható forgatókönyvek esetén nem engedélyezett. További információkért lásd: a [Azure Blob Service API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) dokumentációját.

|Forgatókönyv  |BLOB állapota  |BLOB művelet nem engedélyezett  |
|---------|---------|---------|
|A blob tényleges adatmegőrzési időtartama még nem járt le és/vagy jogi célú visszatartás van érvényben     |Nem módosítható: törlés- és írásvédett         |Delete Container, Delete Blob, Put Blob1, Put Block, Put Block List, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|A blobon beállított tényleges megőrzési időtartam lejárt     |Csak írásvédett (a törlési műveletek engedélyezettek)         |Put Blob, Put Block, Put Block List, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|Minden jogi tárolja az üres, és időalapú adatmegőrzési szabályzat nem található a tárolóban     |Változtatható         |None         |
|Nincs FÉREG szabályzat jön létre (időalapú adatmegőrzés vagy jogi céllal zároltak közé)     |Változtatható         |None         |

> [!NOTE]
> Az első két helyzetben az előző táblázatban az első Put Blob és a egy blob létrehozásához szükséges tiltólista Put és Blokkelraktározási műveletek engedélyezettek. Minden további műveletek nem engedélyezettek.
>
> Csak a GPv2 és Blob storage-fiókok érhető el tárhely nem módosítható. Léteznie kell keresztül [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="pricing"></a>Díjszabás

Nem jár további költségekkel a szolgáltatást. Nem módosítható adatok rendszeres, mutable adatként ugyanúgy díjszabása. A díjszabás részleteiért tekintse meg a [Azure Storage díjszabását ismertető lapon](https://azure.microsoft.com/pricing/details/storage/blobs/).

### <a name="restrictions"></a>Korlátozások

A nyilvános előzetes verzió használatára a következő korlátozások érvényesek:

- *Éles, akár az üzleti szempontból kritikus fontosságú adatokat tárolja.*
- Minden előzetes és NDA korlátozások vonatkoznak.

## <a name="getting-started"></a>Első lépések

A legújabb verzióiban a [az Azure portal](http://portal.azure.com), [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), és [Azure PowerShell-lel](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May2018) nem módosítható storage támogatja az Azure Blob storage.

### <a name="azure-portal"></a>Azure Portal

1. Hozzon létre egy új tárolót, vagy válasszon ki egy már meglévőt a nem módosítható állapotban tartandó blobok tárolására.
 A tárolónak egy GPv2-tárfiókban kell lennie.
2. Válassza ki **hozzáférési szabályzat** a tároló beállításaiban. Válassza ki **+ szabályzat hozzáadása** alatt **nem módosítható blobtároló**.

    ![Tárolóbeállítások a portálon](media/storage-blob-immutable-storage/portal-image-1.png)

3. Időalapú adatmegőrzés engedélyezéséhez jelölje be **időalapú adatmegőrzés** a legördülő menüből.

    !["Időalapú adatmegőrzés" a "Házirend-típus" területen kijelölt](media/storage-blob-immutable-storage/portal-image-2.png)

4. Adja meg a megőrzési időtartam ideje napokban (legalább egy nap).

    !["A frissítés megőrzési időszak" mezőbe](media/storage-blob-immutable-storage/portal-image-5-retention-interval.png)

    Ahogy a képernyőképen látható, a házirend kezdeti állapota nem oldják a zárolást. A szolgáltatás kisebb visszatartási időköz tesztelése, és hajtsa végre a módosításokat a szabályzat előtt zárolni tudja. Zárolás elengedhetetlen az előírásoknak, például a 17a – 4/mp-ben.

5. A szabályzat zárolása. Kattintson a jobb gombbal a három pontra (**...** ), és a következő menü jelenik meg:

    !["Szabályzat zárolás" menüben](media/storage-blob-immutable-storage/portal-image-4-lock-policy.png)

    Válassza ki **zárolási szabályzat**, és a szabályzat állapotát zároltként jelenik meg. A szabály zárolása, miután nem lehet törölni, és csak a megőrzési időtartam bővítmények engedélyezett lesz.

6. Válassza ki ahhoz, hogy a jogi célú visszatartással, **+ szabályzat hozzáadása**. Válassza ki **megadva jogi célú visszatartás** a legördülő menüből.

    !["Jogi célú visszatartási" a "Szabályzat típusa" a menü](media/storage-blob-immutable-storage/portal-image-legal-hold-selection-7.png)

7. Hozzon létre egy vagy több címkét egy jogi céllal zároltak közé.

    !["A címke neve" mező mellett a házirend típusát](media/storage-blob-immutable-storage/portal-image-set-legal-hold-tags.png)

### <a name="azure-cli-20"></a>Azure CLI 2.0

Telepítse a [Azure CLI-bővítmény](http://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) használatával `az extension add -n storage-preview`.

Ha már rendelkezik a bővítmény telepítve van, a következő paranccsal ahhoz, hogy a tároló nem módosítható: `az extension update -n storage-preview`.

A szolgáltatás a következő parancs csoportokat tartalmazza: `az storage container immutability-policy` és `az storage container legal-hold`. Futtatás `-h` meg azokat a parancsokat.

### <a name="powershell"></a>PowerShell

[PowerShell-verzió 4.4.0-preview](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May20180) nem módosítható tárolást támogatja.
A funkció engedélyezéséhez kövesse az alábbi lépéseket:

1. Gondoskodjon arról, hogy a legújabb verziója telepítve van a PowerShellGet: `Install-Module PowerShellGet –Repository PSGallery –Force`.
2. Távolítsa el az Azure PowerShell korábbi telepítéseknek.
3. Telepítés AzureRM: `Install-Module AzureRM –Repository PSGallery –AllowClobber`. Az Azure hasonló módon telepítheti a tárházból.
4. Felügyeletisík-parancsmagok Storage előzetes verziójának telepítéséhez: `Install-Module -Name AzureRM.Storage -AllowPrerelease -Repository PSGallery -AllowClobber`.

A [minta PowerShell-kód](#sample-powershell-code) szakaszt a cikk későbbi részében a funkció használatát mutatja be.

## <a name="client-libraries"></a>Ügyfélkódtárak

A következő ügyfélkódtárak nem módosítható storage támogatja az Azure Blob storage:

- [.NET ügyféloldali kódtár verzió 7.2.0-preview és újabb verziók](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/7.2.0-preview)
- [NODE.js ügyféloldali kódtár 4.0.0-s verzió vagy újabb](https://www.npmjs.com/package/azure-arm-storage)
- [Python ügyféloldali kódtár 2.0.0-s verzió kiadásra jelölt verziójára frissít, 2 és újabb verziók](https://pypi.org/project/azure-mgmt-storage/2.0.0rc1/)

## <a name="supported-values"></a>Támogatott értékek

- A minimális megőrzési időtartam érték egy nap. A maximális 400 év meghatározva.
- A tárfiók zárolt nem módosítható szabályzatokkal tárolók maximális számának 1000.
- Egy storage-fiókok a tárolók a jogi céllal zároltak közé beállítású maximális számát az 1000.
- Egy tárolóban jogi céllal zároltak közé címkék maximális száma 10.
- A jogi céllal zároltak közé címke hossza legfeljebb 23 alfanumerikus karaktereket. A minimális hossz három karaktert.
- A tárolóhoz a maximális megőrzési időköz-bővítmények zárolt nem módosítható szabályzatok maximális számát az harmadik.
- Zárolt nem módosítható házirend a tárolóhoz legfeljebb öt időalapú adatmegőrzési szabályzat naplói és a egy legfeljebb 10 jogi tartsa házirend naplók megmaradnak a tároló időtartamára.

## <a name="faq"></a>GYIK

**A funkció csak blokkblobokat, vagy a lapon, és a hozzáfűző blobok is vonatkozik?**

Nem módosítható storage blob bármilyen is használható.  De azt javasoljuk, használja a leginkább a blokkblobok esetében. Ellentétben a blokkblobok a lap blobok és a hozzáfűző blobok kell létrehozni egy FÉREG tárolón kívül, és majd át lesznek másolva a. Másolását követően ezek a blobok egy FÉREG tárolóba nem további *hozzáfűzi* , egy hozzáfűző blob vagy egy lapblob módosítások engedélyezettek.

**Minden egyes alkalommal új tárfiókot kell létrehoznom a funkció használatához?**

Nem módosítható storage használható minden olyan meglévő GPv2-fiókok vagy az új storage-fiókok GPv2 fióktípus-e. Ez a funkció csak a Blob storage használatával érhető el.

**Mi történik, ha megpróbálok törölni egy tárolót, amelyre *zárolt* időalapú adatmegőrzési szabályzat vagy jogi célú visszatartás vonatkozik?**

A tároló törlése művelet sikertelen lesz, ha legalább egy blob zárolt időalapú adatmegőrzési szabály vagy a jogi céllal zároltak közé. Ez igaz, akkor is, ha az adatok [törölték](storage-blob-soft-delete.md). A tároló törlési művelete sikeres lesz, ha nincs benne aktív adatmegőrzési időtartammal és jogi célú visszatartással beállított blob. A tároló törlése előtt törölnie kell a blobokat. 

**Mi történik, ha megpróbálok törölni egy olyan WORM-tárolót tartalmazó tárfiókot, amelyre *zárolt* időalapú adatmegőrzési szabályzat vagy jogi célú visszatartás vonatkozik?**

A tárfiók törlése sikertelen lesz, ha legalább egy jogi célú visszatartással ellátott WORM-tárolót vagy aktív adatmegőrzési időtartammal rendelkező blobot tartalmaz.  A tárfiók törlése előtt törölnie kell az összes FÉREG tárolót. A tároló törléséhez információkért lásd: az előző kérdést.

**Áthelyezhetem az adatokat különböző blobrétegek között (gyakran, ritkán és alig használt rétegek), ha a blob nem módosítható állapotban van?**

Igen, a Blobszint beállítása parancs segítségével úgy helyezheti át az adatokat a blobrétegek között, hogy azok nem módosítható állapotban maradnak. Nem módosítható tárolási rétegek gyakori elérésű, ritka elérésű és ritka elérésű blob esetében támogatott.

**Mi történik, ha elmulasztok fizetni, és a megőrzési időtartam még nem járt le?**

Nemfizetés esetén normál adatmegőrzési házirendek az a Microsoft a szerződés feltételei érvényesek meghatározott.

**Létezik a funkció kipróbálására lehetőséget biztosító ingyenes próba- vagy türelmi időszak?**

Igen. Időalapú adatmegőrzési szabály létrehozásakor rendszer egy *oldják a zárolást* állapota. Ebben az állapotban is bármilyen kívánt módosítsa a megőrzési időtartam, például növelése vagy csökkentése és még akkor is törli a szabályzatot. A szabály zárolása, miután marad zárolt tartja, törlés megelőzése. Ekkor már a megőrzési időtartam csökkentésére sincs lehetőség. Határozottan javasoljuk, hogy használja a *oldják a zárolást* kizárólag kísérleti célokra állapotát és a egy 24 órás időtartamon belül a házirend zárolása. Ezen eljárások segítségével az mp-ben 17a-4(f) és más szabályozások előírásainak betartását.

**A szolgáltatás országos és kormányzati felhőkben is elérhető?**

Nem módosítható storage jelenleg csak a nyilvános Azure-régióban. Ha egy adott országos felhőbeli érdekli, e-mailt azurestoragefeedback@microsoft.com.

## <a name="sample-powershell-code"></a>Minta PowerShell-kód

A következő PowerShell-parancsfájlt referenciaként van. Ez a szkript létrehoz egy új tárfiókot és tárolót. Majd bemutatja, hogyan állítsa be és törölje a jogi célú visszatartással, hozzon létre és zárolhatja az időalapú adatmegőrzési (más néven vonatkozó módosíthatatlansági szabályzat) és kiterjesztése a megőrzési időtartam.

```powershell
$ResourceGroup = "<Enter your resource group>”
$StorageAccount = "<Enter your storage account name>"
$container = "<Enter your container name>"
$container2 = "<Enter another container name>”
$location = "<Enter the storage account location>"

# Log in to the Azure Resource Manager account
Login-AzureRMAccount
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Storage"

# Create your Azure resource group
New-AzureRmResourceGroup -Name $ResourceGroup -Location $location

# Create your Azure storage account
New-AzureRmStorageAccount -ResourceGroupName $ResourceGroup -StorageAccountName `
    $StorageAccount -SkuName Standard_LRS -Location $location -Kind Storage

# Create a new container
New-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Create Container 2 with a storage account object
$accountObject = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount
New-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2

# Get a container
Get-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Get a container with an account object
$containerObject = Get-AzureRmStorageContainer -StorageAccount $accountObject -Name $container

# List containers
Get-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount

# Remove a container (add -Force to dismiss the prompt)
Remove-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container2

# Remove a container with an account object
Remove-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2

# Remove a container with a container object
$containerObject2 = Get-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2
Remove-AzureRmStorageContainer -InputObject $containerObject2

# Set a legal hold
Add-AzureRmStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag tag1,tag2

# Set a legal hold with an account object
Add-AzureRmStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag tag3

# Set a legal hold with a container object
Add-AzureRmStorageContainerLegalHold -Container $containerObject -Tag tag4,tag5

# Clear a legal hold
Remove-AzureRmStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag tag2

# Clear a legal hold with an account object
Remove-AzureRmStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag tag3,tag5

# Clear a legal hold with a container object
Remove-AzureRmStorageContainerLegalHold -Container $containerObject -Tag tag4

# Create or update an immutability policy
## with an account name or container name

Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container -ImmutabilityPeriod 10

## with an account object
Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -ImmutabilityPeriod 1 -Etag $policy.Etag

## with a container object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 7

## with an immutability policy object
Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -ImmutabilityPeriod 5

# Get an immutability policy
Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container

# Get an immutability policy with an account object
Get-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container

# Get an immutability policy with a container object
Get-AzureRmStorageContainerImmutabilityPolicy -Container $containerObject

# Lock an immutability policy (add -Force to dismiss the prompt)
## with an immutability policy object

$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -force

## with an account name or container name
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

## with an account object
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -Etag $policy.Etag

## with a container object
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -Etag $policy.Etag -force

# Extend an immutability policy
## with an immutability policy object

$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container

$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy `
    $policy -ImmutabilityPeriod 11 -ExtendPolicy

## with an account name or container name
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -ImmutabilityPeriod 11 -Etag $policy.Etag -ExtendPolicy

## with an account object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -ImmutabilityPeriod 12 -Etag `
    $policy.Etag -ExtendPolicy

## with a container object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 13 -Etag $policy.Etag -ExtendPolicy

# Remove an immutability policy (add -Force to dismiss the prompt)
## with an immutability policy object
$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
Remove-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy

## with an account name or container name
Remove-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

## with an account object
Remove-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -Etag $policy.Etag

## with a container object
Remove-AzureRmStorageContainerImmutabilityPolicy -Container $containerObject `
    -Etag $policy.Etag
    
```
