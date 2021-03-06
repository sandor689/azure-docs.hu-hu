---
title: Az Azure HDInsight Hive hibaelhárítása
description: Az Apache Hive és az Azure HDInsight használatához kapcsolatos gyakori kérdésekre adott válaszok.
keywords: Az Azure HDInsight, a Hive, gyakori kérdések hibaelhárítási útmutató, gyakori kérdések
services: hdinsight
ms.service: hdinsight
author: dharmeshkakadia
ms.author: dharmeshkakadia
ms.topic: conceptual
ms.date: 11/2/2017
ms.openlocfilehash: 832fab6c4f183ddad512c5e6e4309d70938a316b
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600023"
---
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a>Az Azure HDInsight Hive hibaelhárítása

További tudnivalók feltett legnépszerűbb kérdésekre és azok megoldásait, az Apache Ambari az Apache Hive hasznos adatot használatakor.


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Hogyan egy Hive-metaadattár exportálása és importálása azt egy másik fürtön?


### <a name="resolution-steps"></a>A megoldás lépései

1. Csatlakozzon a HDInsight-fürthöz egy Secure Shell (SSH) ügyfél használatával. További információkért lásd: [További olvasnivaló](#additional-reading-end).

2. Futtassa a következő parancsot a HDInsight fürtön, amely alapján szeretne a metaadattár exportálása:

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  Ez a parancs létrehoz egy allatables.sql nevű fájlt.

3. A fájl alltables.sql átmásolása az új HDInsight-fürtöt, és futtassa a következő parancsot:

  ```apache
  hive -f alltables.sql
  ```

A kódot a megoldás lépései feltételezi, hogy az új fürtön adatelérési utak ugyanaz, mint az adatelérési utak a régi fürtön. Ha az adatelérési utak eltérő, manuálisan szerkesztheti a generált alltables.sql fájl módosult.

### <a name="additional-reading"></a>További olvasnivaló

- [Csatlakozás egy HDInsight-fürthöz SSH használatával](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a>Hogyan találom meg a Hive-naplók egy fürtön?

### <a name="resolution-steps"></a>A megoldás lépései

1. A HDInsight-fürthöz csatlakozhat SSH-val. További információkért lásd: **További olvasnivaló**.

2. Hive-ügyfél naplók megtekintéséhez használja a következő parancsot:

  ```apache
  /tmp/<username>/hive.log 
  ```

3. Hive-metaadattár naplók megtekintéséhez használja a következő parancsot:

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. Hiveserver-naplók megtekintéséhez használja a következő parancsot:

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a>További olvasnivaló

- [Csatlakozás egy HDInsight-fürthöz SSH használatával](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster"></a>Hogyan indítsa el a Hive-rendszerhéj-specifikus konfigurációk egy fürtön?

### <a name="resolution-steps"></a>A megoldás lépései

1. Adja meg a konfigurációs kulcs-érték pár, a Hive-rendszerhéj indításakor. További információkért lásd: [További olvasnivaló](#additional-reading-end).

  ```apache
  hive -hiveconf a=b 
  ```

2. Listázhatja a Hive-rendszerhéj az összes érvényes konfigurációt, használja a következő parancsot:

  ```apache
  hive> set;
  ```

  Például a következő parancs segítségével indítsa el a Hive-rendszerhéj a hibakeresési naplózás engedélyezve van a konzolon:

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a>További olvasnivaló

- [Hive-konfiguráció tulajdonságai](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Hogyan elemezheti a Tez DAG-adatokat egy fürt kritikus fontosságú elérési úton?


### <a name="resolution-steps"></a>A megoldás lépései
 
1. Elemezheti az Apache Tez irányított aciklikus diagramhoz (DAG) a fürt kritikus fontosságú grafikon, csatlakozzon a HDInsight-fürthöz SSH használatával. További információkért lásd: [További olvasnivaló](#additional-reading-end).

2. Egy parancssorban futtassa a következő parancsot:
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. Más elemzők, amelyek segítségével elemezheti a Tez DAG listájában, használja a következő parancsot:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  Meg kell adnia egy példa program első argumentumaként.

  A program érvényes nevek a következők:
    - **ContainerReuseAnalyzer**: tároló újbóli részleteket egy DAG nyomtatása
    - **CriticalPath**: keresse meg a kritikus fontosságú elérési útját egy DAG
    - **LocalityAnalyzer**: egy DAG helye részleteket nyomtatása
    - **ShuffleTimeAnalyzer**: egy DAG shuffle idő részleteiről elemzése
    - **SkewAnalyzer**: egy DAG torzulása részleteiről elemzése
    - **SlowNodeAnalyzer**: nyomtatása egy DAG a csomópont részletei
    - **SlowTaskIdentifier**: egy DAG a nyomtatási lassú feladat részletei
    - **SlowestVertexAnalyzer**: nyomtatása egy DAG a leglassabb csúcspont részletei
    - **SpillAnalyzer**: nyomtatás túlfolyó részleteket egy DAG
    - **TaskConcurrencyAnalyzer**: egy DAG egyidejűségi részlete nyomtatása
    - **VertexLevelCriticalPathAnalyzer**: a kritikus útvonalat csúcspont szintjén található egy DAG


### <a name="additional-reading"></a>További olvasnivaló

- [Csatlakozás egy HDInsight-fürthöz SSH használatával](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>Hogyan tölthetek le Tez DAG-adatok fürtből?


#### <a name="resolution-steps"></a>A megoldás lépései

A Tez DAG-adatok gyűjtésére két módja van:

- A parancssorból:
 
    A HDInsight-fürthöz csatlakozhat SSH-val. A parancssorban futtassa a következő parancsot:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- Az Ambari Tez nézet használata:
   
  1. Nyissa meg az Ambari. 
  2. Ugrás a Tez megtekintése (alatt a csempék ikonra a jobb felső sarokban). 
  3. Válassza ki a megtekinteni kívánt DAG.
  4. Válassza ki **adatletöltéshez**.

### <a name="additional-reading-end"></a>További olvasnivaló

[Csatlakozás egy HDInsight-fürthöz SSH használatával](hdinsight-hadoop-linux-use-ssh-unix.md)


### <a name="see-also"></a>Lásd még:
[Hibaelhárítás az Azure HDInsight segítségével](hdinsight-troubleshoot-guide.md)




