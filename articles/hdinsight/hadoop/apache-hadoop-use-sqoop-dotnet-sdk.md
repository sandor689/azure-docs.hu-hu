---
title: Sqoop-feladatok futtatása a .NET-keretrendszer és a HDInsight - az Azure használatával
description: Útmutató a HDInsight .NET SDK használata futtatása Sqoop-importálás és exportálása egy Hadoop-fürtöt és a egy Azure SQL database között.
keywords: sqoop feladat
editor: jasonwhowell
services: hdinsight
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: 19c275de80b872fe214e45a52de7d6fb283daf41
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595144"
---
# <a name="run-sqoop-jobs-by-using-net-sdk-for-hadoop-in-hdinsight"></a>Sqoop-feladatok futtatása a hadoop együttes használata a HDInsight .NET SDK használatával
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Ismerje meg, hogyan használhatja az Azure HDInsight .NET SDK Sqoop-feladatok futtatása a HDInsight egy HDInsight-fürt és a egy Azure SQL database vagy SQL Server-adatbázis közötti exportálására és importálásra.

> [!NOTE]
> Bár ebben a cikkben ismertetett mindkettővel egy Windows-alapú vagy Linux-alapú HDInsight-fürt, csak a Windows ügyfél működnek. Egyéb módszerek kiválasztásához, ez a cikk tetején lapon választómezőt használja.
> 

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez a következő elemet kell rendelkeznie:

* A HDInsight Hadoop-fürt. További információkért lásd: [hozzon létre egy fürtöt és a egy SQL-adatbázis](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="use-sqoop-on-hdinsight-clusters-with-the-net-sdk"></a>A Sqoop használata a HDInsight-fürtökön a .NET SDK használatával
A HDInsight .NET SDK-val .NET-ügyfélkönyvtárak, biztosít, így könnyebben működik a HDInsight-fürtökkel a .NET használatával. Ebben a szakaszban hozzon létre egy C# konzolalkalmazást a hivesampletable exportálása az Azure SQL Database tábla, amely ebben az oktatóanyagban korábban létrehozott.

## <a name="submit-a-sqoop-job"></a>Sqoop feladat elküldése

1. Hozzon létre egy C# konzolalkalmazást a Visual Studióban.

2. A Visual Studio Csomagkezelő konzolról a csomag importálása a következő NuGet-parancs futtatásával:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job

3. Használja a következő kódot a Program.cs fájlban:
   
        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
   
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
   
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
   
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
   
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }

4. A program futtatásához jelölje ki a **F5** kulcsot. 

## <a name="limitations"></a>Korlátozások
Linux-alapú HDInsight mutat be a következő korlátozások vonatkoznak:

* Tömeges exportálását:, amellyel a Microsoft SQL Server vagy az Azure SQL Database-adatok exportálása a Sqoop-összekötő jelenleg nem támogatja a tömeges beszúrás.

* Kötegelés: használatával a `-batch` mikor váltson végrehajt a beszúrások, a sqoop használatával hajt végre több beszúrás helyett a beszúrási műveletek kötegelése.

## <a name="next-steps"></a>További lépések
Most már megtanulhatta, hogyan használható a sqoop használatával. További tudnivalókért lásd:

* [Az Oozie használata a HDInsight](../hdinsight-use-oozie.md): Oozie-munkafolyamatokkal használata Sqoop műveletét.
* [HDInsight használatával repülőjáratok késési adatainak elemzése](../hdinsight-analyze-flight-delay-data.md): késleltetheti az adatok elemzéséhez, repülési Hive használata, és majd egy Azure SQL database-adatok exportálása a Sqoop.
* [Adatok feltöltése a HDInsight](../hdinsight-upload-data.md): keresse meg a HDInsight vagy Azure Blob storage-ba történő feltöltéséhez más módszerekkel.

