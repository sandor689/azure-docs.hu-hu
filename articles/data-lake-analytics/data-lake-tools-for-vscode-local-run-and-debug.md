---
title: Futtassa a U-SQL és hibakeresési helyileg Azure Data Lake Tools a Visual Studio Code
description: 'Útmutató: Azure Data Lake Tools for Visual Studio Code használatával futtatni, és elháríthatja a U-SQL feladatok helyileg.'
services: data-lake-analytics
ms.service: data-lake-analytics
author: jejiang
ms.author: jejiang
manager: kfile
editor: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.topic: conceptual
ms.date: 07/14/2017
ms.openlocfilehash: 850b13becb2137c9e881b2d6a657bbd06216e96e
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317146"
---
# <a name="run-u-sql-and-debug-locally-in-visual-studio-code"></a>Futtassa a U-SQL és helyi hibakeresése a Visual Studio Code
A cikkből megtudhatja, hogyan futtathat U-SQL feladatok egy helyi fejlesztési számítógép korai kódolási fázisok felgyorsítása érdekében, vagy helyileg a Visual Studio Code kód hibakereséséhez. Visual Studio Code Azure Data Lake eszköz vonatkozó utasításokért lásd: [használata Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md). 

## <a name="set-up-the-u-sql-local-run-environment"></a>A U-SQL helyi futtatási környezet beállítása

1. Válassza ki a Ctrl + Shift + P nyissa meg a parancs palettát, és írja be a **ADL: helyi futtatása csomag letöltése** letöltése a csomagok.  

   ![Töltse le az ADL LocalRun függőségi csomagok](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/downloadtheadllocalrunpackage.png)

2. Keresse meg a függőségi csomagok látható elérési útjáról a **kimeneti** ablaktáblán, majd telepítse BuildTools és Win10SDK 10240. Íme egy példa elérési útja:  
`C:\Users\xxx\AppData\Roaming\LocalRunDependency` 

   ![Keresse meg a függőségi csomagok](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1 telepítendő **BuildTools**, kattintson a LocalRunDependency mappában visualcppbuildtools_full.exe, majd kövesse a varázsló utasításait.   

    ![BuildTools telepítése](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2 telepítendő **Win10SDK 10240**, kattintson a LocalRunDependency/Win10SDK_10.0.10240_2 mappában sdksetup.exe, majd kövesse a varázsló utasításait.  

    ![Telepítse a Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. A környezeti változó beállítása. Állítsa be a **SCOPE_CPP_SDK** környezeti változót:  
`C:\Users\XXX\AppData\Roaming\LocalRunDependency\CppSDK_3rdparty`  


## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Indítsa el a helyi Futtatás szolgáltatást, és küldje el a U-SQL helyi fiók feladat 
Az első felhasználó használja **ADL: helyi futtatása csomag letöltése** letöltése a helyi futtatási csomagok, ha nem rendelkezik [U-SQL helyi futtatási környezet beállítása](#set-up-the-u-sql-local-run-environment).

1. Válassza ki a Ctrl + Shift + P nyissa meg a parancs palettát, és írja be a **ADL: helyi futtatása szolgáltatás indítása**.   
2. Válassza ki **elfogadás** először a Microsoft szoftverlicenc-szerződést fogadásához. 

   ![A Microsoft szoftverlicenc-feltételek elfogadása](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Az cmd-konzol megnyitása. A felhasználók első alkalommal meg kell adnia **3**, és keresse meg a bemeneti és kimeneti a helyi mappa elérési útját. Egyéb lehetőségek használhatja az alapértelmezett értékeket. 

   ![A Data Lake Tools for Visual Studio Code helyi futtatási cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Válassza ki a Ctrl + Shift + P nyissa meg a parancs palettát, adja meg **ADL: feladat elküldése**, majd válassza ki **helyi** elküldeni a feladatot a helyi fiókba.

   ![A Data Lake Tools for Visual Studio Code helyi kiválasztása](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. A feladat elküldése után megtekintheti a küldésének részleteit. A mintaküldési részletek megtekintéséhez válasszon ki **jobUrl** a a **kimeneti** ablak. A feladatállapot küldésének a cmd-konzolról is megtekintheti. Adja meg **7** a cmd-konzolon, ha szeretné tudni, hogy több feladat részleteit.

   ![A Data Lake Visual Studio Code helyi futtatáskor a kimeneti eszközei](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake Tools for Visual Studio Code helyi futtatáskor a cmd állapota](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>Egy helyi hibakeresési a U-SQL-feladat indítása  
Az első felhasználó:

1. Használjon **ADL: helyi futtatása csomag letöltése** letöltése a helyi futtatási csomagok, ha nem rendelkezik [U-SQL helyi futtatási környezet beállítása](#set-up-the-u-sql-local-run-environment).
2. Telepíti a .NET Core SDK 2.0 leírtak az üzenet mezőbe, ha nincs telepítve.
 
  ![Emlékeztető Dotnet telepíti](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. Telepítés C#, Visual Studio Code az üzenetablakban javasolt Ha nincs telepítve. Kattintson a **telepítése** a folytatáshoz, majd indítsa újra a VSCode.

    ![Emlékeztető telepítendő C#](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Hajtsa végre a helyi hibakeresési hajtsa végre az alábbi lépéseket:
  
1. Válassza ki a Ctrl + Shift + P nyissa meg a parancs palettát, és írja be a **ADL: helyi futtatása szolgáltatás indítása**. Az cmd-konzol megnyitása. Győződjön meg arról, hogy a **DataRoot** van beállítva.
2. A C# háttérkód állítható be töréspont.
3. Vissza a parancsprogram-szerkesztő, kattintson a jobb gombbal és válassza ki a **ADL: helyi hibakeresése**.
    
   ![A Data Lake Tools for Visual Studio Code helyi hibakeresési eredménye](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>További lépések
* [Az Azure Data Lake Tools for Visual Studio kód használata](data-lake-analytics-data-lake-tools-for-vscode.md)
* [U-SQL Python, R és az Azure Data Lake Analytics a VSCode a CSharp fejlesztése](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
* [A Data Lake Analytics PowerShell használatának első lépései](data-lake-analytics-get-started-powershell.md)
* [Ismerkedés a Data Lake Analytics az Azure portál használatával](data-lake-analytics-get-started-portal.md)
* [Használja a Data Lake Tools for Visual Studio a U-SQL-alkalmazások fejlesztésével](data-lake-analytics-data-lake-tools-get-started.md)
* [Használja a Data Lake Analytics(U-SQL) katalógus](data-lake-analytics-use-u-sql-catalog.md)
