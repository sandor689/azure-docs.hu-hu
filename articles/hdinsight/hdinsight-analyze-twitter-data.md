---
title: A HDInsight - Azure Hadoop-keretrendszerrel Twitter-adatok elemzése
description: Útmutató az egy adott szó előfordulási gyakoriságának megkeresése céljából a HDInsight a Hadoop a Twitter-adatok elemzése a Hive segítségével.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: 8ac97c14b4abaa1c07e8f982edb53e0acac3692f
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597620"
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Hive HDInsight használatával egy Twitter-adatok elemzése
Közösségi webhelyek egyik fő környezetbarát big-data bevezetésére. Nyilvános API-kat, mint például a Twitter által biztosított okai a hasznos adatok elemzéséhez és megértéséhez népszerű trendeket.
Ebben az oktatóanyagban, fog tweetek beszerzésére egy Twitter-streamelési API használatával, és hogyan Apache Hive az Azure HDInsight Twitter-felhasználók, akik a legtöbb tweeteket, amelyek egy bizonyos szót küldött listáját.

> [!IMPORTANT]
> A jelen dokumentumban leírt lépések egy Windows-alapú HDInsight-fürt szükséges. A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). Linux-alapú fürthöz adott lépésekért lásd: [elemzése Twitter-adatok használata a Hive a HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:

* **Egy munkaállomás** az Azure PowerShell telepítését és konfigurálását.

    Windows PowerShell-szkriptek végrehajtása, kell az Azure PowerShell futtatása rendszergazdaként, és a végrehajtási házirend beállítása *RemoteSigned*. Lásd: [futtatása Windows PowerShell-parancsfájlok][powershell-script].

    Windows PowerShell-parancsfájlok futtatásakor, mielőtt győződjön meg arról, hogy az Azure-előfizetés a következő parancsmag használatával csatlakozik:

    ```powershell
    Connect-AzureRmAccount
    ```

    Ha több Azure-előfizetéssel rendelkezik, az aktuális előfizetésben beállításához használja a következő parancsmagot:

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt. A jelen dokumentumban leírt lépések az új HDInsight-parancsmagokat használják, amelyek az Azure Resource Managerrel működnek.
    >
    > Kérjük, kövesse az alábbi cikkben leírt lépéseket az Azure PowerShell legújabb verziójának telepítéséhez: [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Az Azure PowerShell letöltése és konfigurálása). Ha vannak olyan parancsprogramjai, amelyeket módosítani kell az új, az Azure Resource Managerrel működő parancsmagok használatához, tekintse meg az alábbi cikket: [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Az Azure Resource Manager-alapú fejlesztési eszközökre való áttérés HDInsight-fürtök esetén).

* **Egy Azure HDInsight-fürt**. A fürtök kiépítése útmutatásért lásd: [HDInsight használatának első lépései] [ hdinsight-get-started] vagy [Provision HDInsight clusters][hdinsight-provision]. Az oktatóanyag későbbi részében szüksége lesz a fürt nevét.

Az alábbi táblázat a jelen oktatóanyagban használt fájlokat listázza:

| Fájlok | Leírás |
| --- | --- |
| /tutorials/Twitter/Data/tweets.txt |A forrásadatok a Hive-feladatokban. |
| /tutorials/Twitter/output |A Hive-feladat kimeneti mappába. Az alapértelmezett Hive feladat kimeneti fájl neve **000000_0**. |
| tutorials/Twitter/Twitter.hql |A HiveQL-parancsfájlt. |
| /tutorials/Twitter/JobStatus |A Hadoop-feladat állapotát. |

## <a name="get-twitter-feed"></a>Twitter-hírcsatorna lekérése
Ebben az oktatóanyagban használhatja a [Twitter-streamelési API-k][twitter-streaming-api]. Streamelési API-t fogja használni a meghatározott Twitter [állapotok/szűrés][twitter-statuses-filter].

> [!NOTE]
> 10 000 tweetet tartalmazó fájlt, és a Hive-szkriptfájl (a következő szakaszban ismertetett) fel lett töltve egy nyilvános Blob-tárolóban. Ez a szakasz kihagyhatja, ha azt szeretné használni a feltöltött fájlokat.

Tweetek adatok összetett beágyazott struktúrát tartalmaz a JavaScript Object Notation (JSON) formátumban tárolódik. Rengeteg kódsor írása egy hagyományos programozási nyelv használatával, helyett alakíthatja át a beágyazott struktúra egy Hive-táblába, hogy azt egy Structured Query Language (SQL) lekérdezhetők – például a HiveQL nevű nyelv.

Twitter hitelesített hozzáférést biztosítani az API OAuth-hitelesítést használ. OAuth-hitelesítési protokoll, amely lehetővé teszi a felhasználóknak az alkalmazások megosztása jelszavukat nélkül a felhasználók nevében eljárni jóváhagyásához. További információ található [oauth.net](http://oauth.net/) vagy a kiváló [Alapszintű útmutató az OAuth](http://hueniverse.com/oauth/) Hueniverse a.

OAuth használata az első lépés az új alkalmazás létrehozása a fejlesztői Twitter-webhelyen.

**Twitter-alkalmazás létrehozása**

1. Jelentkezzen be a [ https://apps.twitter.com/ ](https://apps.twitter.com/). Kattintson a **regisztráció** hivatkozásra, ha nem rendelkezik egy Twitter-fiókkal.
2. Kattintson a **új alkalmazás létrehozása**.
3. Adja meg **neve**, **leírás**, **webhely**. Meghatározhat egy URL-címe be a **webhely** mező. Az alábbi táblázat néhány mintaértékeket használhatja:

   | Mező | Érték |
   | --- | --- |
   |  Name (Név) |MyHDInsightApp |
   |  Leírás |MyHDInsightApp |
   |  Honlap |http://www.myhdinsightapp.com |
4. Ellenőrizze **Igen, elfogadom**, és kattintson a **Twitter-alkalmazás létrehozása**.
5. Kattintson a **engedélyek** fülre. Az alapértelmezett engedély **csak olvasható**. Ez a elegendő ehhez az oktatóanyaghoz.
6. Kattintson a **kulcsok és hozzáférési tokenek** fülre.
7. Kattintson a **saját hozzáférési token létrehozása**.
8. Kattintson a **teszt OAuth** az oldal jobb felső sarkában.
9. Írja le **fogyasztói kulcs**, **fogyasztói titkos kulcs**, **hozzáférési jogkivonat**, és **hozzáférési token titkos**. Az oktatóanyag későbbi részében szüksége lesz az értékeket.

Ebben az oktatóanyagban a Windows PowerShell használatával, hogy a webes szolgáltatás hívása. A más népszerű eszköz, hogy a webszolgáltatás-hívás [ *Curl*][curl]. A curl letölthető [Itt][curl-download].

> [!NOTE]
> A curl-parancs használatakor a Windows idézőjeleket helyett használjon szimpla idézőjelek között a beállítási értékeit.

**Twitter-üzenetek beolvasásához**

1. Nyissa meg a Windows PowerShell integrált parancsfájl-kezelési környezet (ISE). (Írja be a Windows 8 Start képernyő **PowerShell_ISE** majd **Windows PowerShell ISE-ben**. Lásd: [indítsa el a Windows PowerShell, a Windows 8 és Windows](https://docs.microsoft.com/en-us/powershell/scripting/setup/starting-windows-powershell?view=powershell-6)
2. Másolja a parancsfájl panelen a következő parancsfájlt:

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

    # Enter the OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    Connect-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
    Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

    Write-Host "Create block blob object ..." -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($containerName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    #end region

    # region - Format OAuth strings
    Write-Host "Format oauth strings ..." -ForegroundColor Green
    $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
    $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
    $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

    $signature = "POST&";
    $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
    $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
    $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
    $signature += [System.Uri]::EscapeDataString("track=" + $track);

    $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

    $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
    $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
    $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

    $oauth_authorization = 'OAuth ';
    $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
    $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
    $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
    $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
    $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
    $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
    $oauth_authorization += 'oauth_version="1.0"';

    $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
    #endregion

    #region - Read tweets
    Write-Host "Create HTTP web request ..." -ForegroundColor Green
    [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
    $request.Method = "POST";
    $request.Headers.Add("Authorization", $oauth_authorization);
    $request.ContentType = "application/x-www-form-urlencoded";
    $body = $request.GetRequestStream();

    $body.write($post_body, 0, $post_body.length);
    $body.flush();
    $body.close();
    $response = $request.GetResponse() ;

    Write-Host "Start stream reading ..." -ForegroundColor Green

    Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

    $inrec = $sReader.ReadLine()
    $count = 0
    while (($inrec -ne $null) -and ($count -le $lineMax))
    {
        if ($inrec -ne "")
        {
            Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

            $writeStream.WriteLine($inrec)
            $count ++
        }

        $inrec=$sReader.ReadLine()
    }
    #endregion

    #region - Write tweets to Blob storage
    Write-Host "Write to the destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Állítsa be az első 5-től 8 változókat a parancsfájl:

    Változó|Leírás
    ---|---
    $clusterName|Ez az, ahol futtatja az alkalmazást szeretné HDInsight-fürt nevére.
    $oauth_consumer_key|Ez az a Twitter-alkalmazás **fogyasztói kulcs** megírt korábban, a Twitter-alkalmazás létrehozásakor.
    $oauth_consumer_secret|Ez az a Twitter-alkalmazás **fogyasztói titkos kulcs** korábban megírt.
    $oauth_token|Ez az a Twitter-alkalmazás **hozzáférési jogkivonat** korábban megírt.
    $oauth_token_secret|Ez az a Twitter-alkalmazás **hozzáférési token titkos** korábban megírt.
    $destBlobName|Ez a kimeneti blob neve. Az alapértelmezett érték **tutorials/twitter/data/tweets.txt**. Ha megváltoztatja az alapértelmezett érték, szüksége lesz a Windows PowerShell-szkriptek megfelelően frissíteni.
    $trackString|A web service kapcsolatos ezeket a kulcsszavakat tartalmazó tweeteket ad vissza. Az alapértelmezett érték **Azure-felhő, HDInsight**. Ha megváltoztatja az alapértelmezett érték, ennek megfelelően frissíti a Windows PowerShell-parancsfájlok.
    $lineMax|Az érték határozza meg, hány tweeteket olvassa a parancsfájl. Körülbelül 3 percenként, majd olvassa el a 100 tweetet vesz igénybe. Beállíthatja, hogy több, de azt letölteni több időt vesz igénybe.

1. A szkriptek futtatásához nyomja le az **F5** billentyűt. Áthidaló megoldásként hibákba ütközne, ha az összes sort, és nyomja le az **F8**.
2. Meg kell jelennie "Teljes"! a kimenet végén. Hibaüzeneteket vörös színnel jelenik meg.

Egy érvényesítési eljárás szerint ellenőrizheti a kimeneti fájl **/tutorials/twitter/data/tweets.txt**, az Azure Blob Storage-egy Azure storage Explorerben vagy az Azure PowerShell használatával. A Windows PowerShell-példaszkript fájlok listázásához, lásd: [használja a Blob storage a HDInsight][hdinsight-storage-powershell].

## <a name="create-hiveql-script"></a>Hozzon létre HiveQL-parancsfájlt
Az Azure PowerShell használatával egyszerre több HiveQL utasítás egy futni, vagy csomagot egy parancsfájlt a HiveQL-utasítást. Ebben az oktatóanyagban létrehozhat egy HiveQL-parancsfájlt. A parancsfájl az Azure Blob storage-kell feltölteni. A következő szakaszban a parancsfájl fog futtatja az Azure PowerShell használatával.

> [!NOTE]
> A Hive-szkriptfájl és 10 000 tweetet tartalmazó fájl fel lett töltve egy nyilvános Blob-tárolóban. Ez a szakasz kihagyhatja, ha azt szeretné használni a feltöltött fájlokat.

A HiveQL-parancsfájlt hajtsa végre a következő lesz:

1. **Dobja el a tweets_raw táblát** abban az esetben, ha a tábla már létezik.
2. **A tweets_raw Hive-tábla létrehozásához**. Adatait tartalmazza az ideiglenes strukturált Hive-tábla további kinyerési, átalakítási és betöltési (ETL) feldolgozása. A partíciók információkért lásd: [Hive oktatóanyag][apache-hive-tutorial].
3. **Adatok betöltése** a forrásmappából, /tutorials/twitter/data. A beágyazott JSON-formátumban nagy tweetek adatkészlet most átalakítása ideiglenes Hive tábla struktúrába.
4. **Dobja el a tweetek táblát** abban az esetben, ha a tábla már létezik.
5. **A tweetek tábla létrehozásához**. Mielőtt a Hive használatával végezhet lekérdezéseket a tweetek adatkészletet, egy másik ETL-folyamat futtatásához szükséges. Az ETL-folyamat az adatok a "twitter_raw" táblában tárolt részletesebb tábla sémát határozza meg.
6. **Felülírás táblázat beillesztése**. Ez összetett Hive-szkriptet a Hadoop-fürt által elindít egy MapReduce-feladatok hosszú készletét. Az adatkészlet és a fürt méretét, függően ez eltarthat nagyjából 10 percet.
7. **INSERT felülírása directory**. Lekérdezés futtatása, és a egy fájlba az adatkészlet kimeneti. Ez a lekérdezés visszaad egy listát, Twitter-felhasználók, akik a legtöbb tweeteket szó küldött "Azure".

**Hozzon létre egy Hive-szkriptet, és töltse fel az Azure-bA**

1. Nyissa meg a Windows PowerShell ISE-ben.
2. Másolja a parancsfájl panelen a következő parancsfájlt:

    ```powershell
    #region - variables and constants
    $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
    $subscriptionID = "<Azure Subscription ID>"

    $sourceDataPath = "/tutorials/twitter/data"
    $outputPath = "/tutorials/twitter/output"
    $hqlScriptFile = "tutorials/twitter/twitter.hql"

    $hqlStatements = @"
    set hive.exec.dynamic.partition = true;
    set hive.exec.dynamic.partition.mode = nonstrict;

    DROP TABLE tweets_raw;
    CREATE EXTERNAL TABLE tweets_raw (
        json_response STRING
    )
    STORED AS TEXTFILE LOCATION '$sourceDataPath';

    DROP TABLE tweets;
    CREATE TABLE tweets
    (
        id BIGINT,
        created_at STRING,
        created_at_date STRING,
        created_at_year STRING,
        created_at_month STRING,
        created_at_day STRING,
        created_at_time STRING,
        in_reply_to_user_id_str STRING,
        text STRING,
        contributors STRING,
        retweeted STRING,
        truncated STRING,
        coordinates STRING,
        source STRING,
        retweet_count INT,
        url STRING,
        hashtags array<STRING>,
        user_mentions array<STRING>,
        first_hashtag STRING,
        first_user_mention STRING,
        screen_name STRING,
        name STRING,
        followers_count INT,
        listed_count INT,
        friends_count INT,
        lang STRING,
        user_location STRING,
        time_zone STRING,
        profile_image_url STRING,
        json_response STRING
    );

    FROM tweets_raw
    INSERT OVERWRITE TABLE tweets
    SELECT
        cast(get_json_object(json_response, '$.id_str') as BIGINT),
        get_json_object(json_response, '$.created_at'),
        concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
        substr (get_json_object(json_response, '$.created_at'),27,4)),
        substr (get_json_object(json_response, '$.created_at'),27,4),
        case substr (get_json_object(json_response, '$.created_at'),5,3)
            when "Jan" then "01"
            when "Feb" then "02"
            when "Mar" then "03"
            when "Apr" then "04"
            when "May" then "05"
            when "Jun" then "06"
            when "Jul" then "07"
            when "Aug" then "08"
            when "Sep" then "09"
            when "Oct" then "10"
            when "Nov" then "11"
            when "Dec" then "12" end,
        substr (get_json_object(json_response, '$.created_at'),9,2),
        substr (get_json_object(json_response, '$.created_at'),12,8),
        get_json_object(json_response, '$.in_reply_to_user_id_str'),
        get_json_object(json_response, '$.text'),
        get_json_object(json_response, '$.contributors'),
        get_json_object(json_response, '$.retweeted'),
        get_json_object(json_response, '$.truncated'),
        get_json_object(json_response, '$.coordinates'),
        get_json_object(json_response, '$.source'),
        cast (get_json_object(json_response, '$.retweet_count') as INT),
        get_json_object(json_response, '$.entities.display_url'),
        array(
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
        array(
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
        trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
        trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
        get_json_object(json_response, '$.user.screen_name'),
        get_json_object(json_response, '$.user.name'),
        cast (get_json_object(json_response, '$.user.followers_count') as INT),
        cast (get_json_object(json_response, '$.user.listed_count') as INT),
        cast (get_json_object(json_response, '$.user.friends_count') as INT),
        get_json_object(json_response, '$.user.lang'),
        get_json_object(json_response, '$.user.location'),
        get_json_object(json_response, '$.user.time_zone'),
        get_json_object(json_response, '$.user.profile_image_url'),
        json_response
    WHERE (length(json_response) > 500);

    INSERT OVERWRITE DIRECTORY '$outputPath'
    SELECT name, screen_name, count(1) as cc
        FROM tweets
        WHERE text like "%Azure%"
        GROUP BY name,screen_name
        ORDER BY cc DESC LIMIT 10;
    "@
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Connect-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing the Hive script file
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define the connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write the Hive script file to Blob storage
    Write-Host "Write to the destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Az első két változót állítsa be a parancsfájlt:

   | Változó | Leírás |
   | --- | --- |
   |  $clusterName |Adja meg a HDInsight-fürt neve, ahol szeretné az alkalmazás futtatásához. |
   |  $subscriptionID |Adja meg az Azure-előfizetés azonosítóját. |
   |  $sourceDataPath |A Hive-lekérdezések, olvassa el az adatokat az Azure Blob tárolási helyét. Nem kell módosítani a változót. |
   |  $outputPath |Az Azure Blob tárolási helye, ahol a Hive-lekérdezések kimenetei az eredményeket. Nem kell módosítani a változót. |
   |  $hqlScriptFile |A hely és a HiveQL-parancsfájlt a fájl neve. Nem kell módosítani a változót. |
4. A szkriptek futtatásához nyomja le az **F5** billentyűt. Áthidaló megoldásként hibákba ütközne, ha az összes sort, és nyomja le az **F8**.
5. Meg kell jelennie "Teljes"! a kimenet végén. Hibaüzeneteket vörös színnel jelenik meg.

Egy érvényesítési eljárás szerint ellenőrizheti a kimeneti fájl **/tutorials/twitter/twitter.hql**, az Azure Blob Storage-egy Azure storage Explorerben vagy az Azure PowerShell használatával. A Windows PowerShell-példaszkript fájlok listázásához, lásd: [használja a Blob storage a HDInsight][hdinsight-storage-powershell].

## <a name="process-twitter-data-by-using-hive"></a>Twitter-adatok feldolgozása a Hive használatával
Befejezte az összes előkészítési munkát. Ezzel a Hive-szkript meghívása és az eredményeket.

### <a name="submit-a-hive-job"></a>Egy Hive-feladat elküldése
A következő Windows PowerShell-parancsfájlt használja a Hive-szkript futtatásához. Állítsa be az első változót kell.

> [!NOTE]
> Az utolsó két szakaszok a tweeteket, és a HiveQL-parancsfájlt a feltöltött használandó $hqlScriptFile beállítása "/ tutorials/twitter/twitter.hql". Szeretne használni, amelyek meg lettek feltöltve egy nyilvános blob azokat, $hqlScriptFile beállítása "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of the following
$hqlScriptFile = "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
$hqlScriptFile = "/tutorials/twitter/twitter.hql"

$statusFolder = "/tutorials/twitter/jobstatus"
#endregion

$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value

$defaultBlobContainerName = $myCluster.DefaultStorageContainer

#region - Invoke Hive
Write-Host "Invoke Hive ... " -ForegroundColor Green

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display the standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace �\s*$� -replace �.*\s�
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-the-results"></a>Az eredmények ellenőrzése
A következő Windows PowerShell-parancsfájl használatával ellenőrizze a Hive-feladat kimenete. Az első két változókat kell.

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
$defaultBlobContainerName = $myCluster.DefaultStorageContainer

Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

Write-Host "Create a context object ... " -ForegroundColor Green
$storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
#endregion

#region - Download blob and display blob
Write-Host "Download the blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display the output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]
> A Hive-tábla \001 használja, mint a mezőhatárolóval. Az elválasztó nem áll a kimenetben látható.

Miután az elemzés eredménye lettek helyezve az Azure Blob storage-ban, exportálja az adatokat egy Azure SQL database vagy SQL server, az adatok exportálása Excelbe a Power Query használatával vagy az adatok az alkalmazás csatlakoztatása a Hive ODBC-illesztő használatával. További információkért lásd: [Sqoop használata a HDInsight-][hdinsight-use-sqoop], [HDInsight használatával repülőjáratok késési adatainak elemzése][hdinsight-analyze-flight-delay-data], [ Csatlakoztathatja az Excelt a HDInsight a Power Query][hdinsight-power-query], és [csatlakoztathatja az Excelt a HDInsight a Microsoft Hive ODBC-illesztő a][hdinsight-hive-odbc].

## <a name="next-steps"></a>További lépések
Ebben az oktatóanyagban úgy találtuk, hogyan alakíthatja az strukturálatlan JSON-adatkészlet egy strukturált, lekérdezésére, ismerje meg, és a twitteren adatok elemzése az Azure-on HDInsight használatával Hive-táblába. További tudnivalókért lásd:

* [HDInsight – első lépések][hdinsight-get-started]
* [HDInsight használatával repülőjáratok késési adatainak elemzése][hdinsight-analyze-flight-delay-data]
* [Csatlakoztathatja az Excelt a HDInsight a Power Query segítségével][hdinsight-power-query]
* [Az Excel összekapcsolása a HDInsight és a Microsoft Hive ODBC-illesztő][hdinsight-hive-odbc]
* [A Sqoop használata a HDInsightban][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]:hdinsight-hadoop-use-blob-storage.md#access-blobs-using-azure-powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-power-query]:hadoop/apache-hadoop-connect-excel-power-query.md
[hdinsight-hive-odbc]:hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md
