1. Másolja a telepítőt egy helyi mappába (például C:\Temp) a kiszolgálón, amely számára védelmet kíván. Futtassa az alábbi parancsokat rendszergazdaként parancsot a parancssorba:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. A mobilitási szolgáltatás telepítéséhez futtassa a következő parancsot:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. Most már az ügynök regisztrálva kell lennie a konfigurációs kiszolgálóval.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>A mobilitási szolgáltatás telepítőjének parancssori argumentumok

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Paraméter|Típus|Leírás|Lehetséges értékek|
|-|-|-|-|
|/ Role|Kötelező|Megadja, hogy a mobilitási szolgáltatás (MS) telepítve kell lennie, vagy MasterTarget (MT) telepítve kell lennie.|MS </br> FŐ CÉLKISZOLGÁLÓ|
|/InstallLocation|Optional|Hely, ahol a mobilitási szolgáltatás telepítve van.|A számítógép bármely mappája|
|És platformok|Kötelező|Meghatározza a platformot, amelyre telepítve van a mobilitási szolgáltatást. </br> </br>- **VMware**: használja ezt az értéket, ha egy virtuális gépen futó mobilitási szolgáltatás telepítése *VMware vSphere ESXi-gazdagépek*, *Hyper-V-gazdagépek*, és *fizikai kiszolgálók*. </br> - **Azure**: használja ezt az értéket, ha az ügynök telepítése az Azure IaaS virtuális gépen. | VMware </br> Azure|
|/ Csendes|Optional|Megadja, hogy a telepítő futtatásához csendes módban.| –|

>[!TIP]
> A telepítési naplókban találhatók % ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log.

#### <a name="mobility-service-registration-command-line-arguments"></a>A mobilitási szolgáltatás regisztrációs parancssori argumentumok

```
Usage :
UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Paraméter|Típus|Leírás|Lehetséges értékek|
  |-|-|-|-|
  |/CSEndPoint |Kötelező|A konfigurációs kiszolgáló IP-címe| Bármely érvényes IP-cím|
  |/PassphraseFilePath|Kötelező|A hozzáférési kód helyét |Bármely érvényes UNC vagy helyi elérési útja|


>[!TIP]
> Az ügynök konfigurációs naplók % ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log területen találhatók.
