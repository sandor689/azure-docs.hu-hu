---
title: Az Application Insights és a Log Analytics által használt IP-címek |} A Microsoft Docs
description: Az Application Insights által igényelt kiszolgálói tűzfal kivételek
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 44d989f8-bae9-40ff-bfd5-8343d3e59358
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 07/23/2018
ms.author: mbullwin
ms.openlocfilehash: 724cdb82f601805ffd93f1afd0c27983cc1ef96b
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389473"
---
# <a name="ip-addresses-used-by-application-insights-and-log-analytics"></a>Az Application Insights és a Log Analytics által használt IP-címek
A [Azure Application Insights](app-insights-overview.md) szolgáltatást használ egy IP-címek számát. Előfordulhat, hogy szeretne, ezeket a címeket, ha az alkalmazás, figyelt tűzfal mögött található.

> [!NOTE]
> Bár ezek a címek statikus, akkor lehet, hogy módosítania kell őket időről időre.
> 
> 

> [!TIP]
> Feliratkozás egy RSS-hírcsatorna hozzáadásával, ezen a lapon https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/application-insights/app-insights-ip-addresses.md.atom , a kedvenc RSS/ATOM olvasó kap értesítést a legutóbbi változtatásokat.
> 
> 

## <a name="outgoing-ports"></a>Kimenő portok
Meg kell nyitnia néhány kimenő portot a kiszolgálója tűzfalán, hogy az Application Insights SDK-t és/vagy Állapotfigyelőt adatokat küldeni a portálon:

| Cél | URL-cím | IP | Portok |
| --- | --- | --- | --- |
| Telemetria |dc.services.visualstudio.com<br/>dc.applicationinsights.microsoft.com |40.114.241.141<br/>104.45.136.42<br/>40.84.189.107<br/>168.63.242.221<br/>52.167.221.184<br/>52.169.64.244<br/>40.85.218.175<br/>104.211.92.54<br/>52.175.198.74 | 443 |
| Élő metrikastream |rt.services.visualstudio.com<br/>rt.applicationinsights.microsoft.com |23.96.28.38<br/>13.92.40.198 |443 |

## <a name="status-monitor"></a>A figyelő állapota
Állapot figyelő konfiguráció – csak módosításkor szükséges.

| Cél | URL-cím | IP | Portok |
| --- | --- | --- | --- |
| Konfiguráció |`management.core.windows.net` | |`443` |
| Konfiguráció |`management.azure.com` | |`443` |
| Konfiguráció |`login.windows.net` | |`443` |
| Konfiguráció |`login.microsoftonline.com` | |`443` |
| Konfiguráció |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| Konfiguráció |`auth.gfx.ms` | |`443` |
| Konfiguráció |`login.live.com` | |`443` |
| Telepítés |`packages.nuget.org` , `nuget.org`, `api.nuget.org`, `az320820.vo.msecnd.net` (NuGet-letöltések) | |`443` |

## <a name="availability-tests"></a>Rendelkezésre állási tesztek
Ez a cím, amelyről a lista [rendelkezésre állási webes tesztek](app-insights-monitor-web-app-availability.md) futnak. Ha azt szeretné, az alkalmazás webes tesztek futtatásához, de a webkiszolgáló korlátozódik bizonyos ügyfelek kiszolgálása, majd meg kell a bejövő forgalom engedélyezése, a rendelkezésre állási tesztelési kiszolgálók.

Nyissa meg a 80-as (http) és a 443-as (https), a bejövő forgalmat, ezek a címek (IP-címek hely szerint vannak csoportosítva):

```
Australia East
13.70.83.252
13.75.150.96
13.75.153.9
13.75.158.185
Brazil South
191.232.32.122
191.232.172.45
191.232.176.218
191.232.191.225
France South
52.136.140.221
52.136.140.222
52.136.140.223
52.136.140.226
France Central
52.143.140.242
52.143.140.246
52.143.140.247
52.143.140.249
40.89.137.100
40.89.142.126
40.89.131.237
40.89.136.180
East Asia
13.75.121.122
23.99.115.153
23.99.123.38
23.102.232.186
52.175.38.49
52.175.39.103
North Europe
13.74.184.101
13.74.185.160
40.69.200.198
52.164.224.46
52.169.12.203
52.169.14.11
52.169.237.149
52.178.183.105
40.112.90.148
40.112.94.212
104.46.15.57
40.115.125.114
Japan East
52.243.33.33
52.243.33.141
52.243.35.253
52.243.41.117
West Europe
52.174.166.113
52.174.178.96
52.174.31.140
52.174.35.14
52.178.104.23
52.178.109.190
52.178.111.139
52.233.166.221
23.100.10.236
23.100.6.155
52.232.113.84
51.144.113.219
UK South
51.140.79.229
51.140.84.172
51.140.87.211
51.140.105.74
UK West
51.141.25.219
51.141.32.101
51.141.35.167
51.141.54.177
51.140.240.239
51.140.205.236
51.140.245.132
51.140.203.56
Southeast Asia
52.187.29.7
52.187.179.17
52.187.76.248
52.187.43.24
52.163.57.91
52.187.30.120
West US
104.45.228.236
104.45.237.251
13.64.152.110
13.64.156.54
13.64.232.251
13.64.236.105
13.91.94.59
40.118.131.182
40.83.189.192
40.83.215.122
104.42.39.222
104.42.145.220
104.42.60.160
104.42.248.11
40.83.163.29
104.42.195.57
40.78.19.163
40.78.23.43
Central US
52.165.130.58
52.173.142.229
52.173.147.190
52.173.17.41
52.173.204.247
52.173.244.190
52.173.36.222
52.176.1.226
104.43.251.84
40.113.236.73
40.113.230.234
40.113.195.109
104.43.215.218
104.43.240.112
North Central US
23.96.247.139
23.96.249.113
52.162.124.242
52.162.126.139
52.162.241.147
52.162.246.222
52.162.247.136
52.237.153.231
52.237.154.216
52.237.156.14
52.237.157.218
52.237.157.37
65.52.205.196
23.100.75.146
65.52.63.179
157.55.143.58
South Central US
104.210.145.106
13.84.176.24
13.84.49.16
13.85.11.137
13.85.26.248
13.85.69.145
52.171.136.162
52.171.141.253
52.171.57.172
52.171.58.140
40.124.36.120
104.210.216.32
104.215.75.92
104.215.77.186
East US
13.82.218.95
13.90.96.71
13.90.98.52
13.92.137.70
40.85.187.235
40.87.61.61
52.168.8.247
52.170.38.79
52.170.80.61
52.179.9.26
104.41.133.69
137.117.103.13
40.114.75.45
40.121.8.31
168.62.41.234
168.62.168.66

```  

## <a name="application-insights-api"></a>Az Application Insights API
| Cél | URI | IP | Portok |
| --- | --- | --- | --- |
| API |api.applicationinsights.io<br/>api1.applicationinsights.io<br/>api2.applicationinsights.io<br/>api3.applicationinsights.io<br/>api4.applicationinsights.io<br/>api5.applicationinsights.io |13.82.26.252<br/>40.76.213.73 |80,443 |
| API-dokumentáció |dev.applicationinsights.io<br/>dev.applicationinsights.microsoft.com<br/>dev.aisvc.visualstudio.com<br/>www.applicationinsights.io<br/>www.applicationinsights.microsoft.com<br/>www.aisvc.visualstudio.com |13.82.24.149<br/>40.114.82.10 |80,443 |
| Belső API-t |aigs.aisvc.visualstudio.com<br/>aigs1.aisvc.visualstudio.com<br/>aigs2.aisvc.visualstudio.com<br/>aigs3.aisvc.visualstudio.com<br/>aigs4.aisvc.visualstudio.com<br/>aigs5.aisvc.visualstudio.com<br/>aigs6.aisvc.visualstudio.com |Dinamikus|443 |

## <a name="log-analytics-api"></a>Log Analytics API
| Cél | URI | IP | Portok |
| --- | --- | --- | --- |
| API |api.loganalytics.io<br/>*.api.loganalytics.io |Dinamikus |80,443 |
| API-dokumentáció |dev.loganalytics.io<br/>docs.loganalytics.io<br/>www.loganalytics.io |Dinamikus |80,443 |

## <a name="application-insights-analytics"></a>Application Insights-elemzési

| Cél | URI | IP | Portok |
| --- | --- | --- | --- |
| Analytics-portál | analytics.applicationinsights.io | Dinamikus | 80,443 |
| Tartalomkézbesítési hálózat (CDN) | applicationanalytics.azureedge.net | Dinamikus | 80,443 |
| Media CDN | applicationanalyticsmedia.azureedge.net | Dinamikus | 80,443 |

Megjegyzés: *. az Application Insights csapata a applicationinsights.io tartomány tulajdonosa.

## <a name="log-analytics-portal"></a>Log Analytics-portál

| Cél | URI | IP | Portok |
| --- | --- | --- | --- |
| Portál | portal.loganalytics.io | Dinamikus | 80,443 |
| Tartalomkézbesítési hálózat (CDN) | applicationanalytics.azureedge.net | Dinamikus | 80,443 |

Megjegyzés: *. a Log Analytics fejlesztőcsapata loganalytics.io tartomány tulajdonosa.

## <a name="application-insights-azure-portal-extension"></a>Application Insights az Azure portal bővítmény

| Cél | URI | IP | Portok |
| --- | --- | --- | --- |
| Application Insights-bővítmény | stamp2.app.insightsportal.visualstudio.com | Dinamikus | 80,443 |
| Application Insights-bővítmény CDN | insightsportal-prod2-cdn.aisvc.visualstudio.com<br/>insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com<br/>insightsportal-cdn-aimon.applicationinsights.io | Dinamikus | 80,443 |

## <a name="application-insights-sdks"></a>Application Insights SDK-k

| Cél | URI | IP | Portok |
| --- | --- | --- | --- |
| Application Insights node.js SDK CDN | az416426.vo.msecnd.net | Dinamikus | 80,443 |
| Application Insights Java SDK | aijavasdk.blob.core.windows.net | Dinamikus | 80,443 |

## <a name="profiler"></a>Profilkészítő

| Cél | URI | IP | Portok |
| --- | --- | --- | --- |
| Ügynök | agent.azureserviceprofiler.net<br/>*.agent.azureserviceprofiler.net | 51.143.96.206<br/>51.143.98.157<br/>52.161.8.88<br/>52.161.29.225<br/>52.178.149.106<br/>52.178.147.66<br/>40.68.32.221<br/>104.40.217.71<br/>52.230.124.46<br/>52.230.122.9 | 443
| Portál | gateway.azureserviceprofiler.net | Dinamikus | 443
| Storage | *.core.windows.net | Dinamikus | 443

## <a name="snapshot-debugger"></a>Pillanatkép-hibakereső

> [!NOTE]
> Profiler és a pillanatkép-hibakereső ossza meg ugyanazokat az IP-címeket.

| Cél | URI | IP | Portok |
| --- | --- | --- | --- |
| Ügynök | ppe.azureserviceprofiler.net<br/>*.ppe.azureserviceprofiler.net | 51.143.96.206<br/>51.143.98.157<br/>52.161.8.88<br/>52.161.29.225<br/>52.178.149.106<br/>52.178.147.66<br/>40.68.32.221<br/>104.40.217.71<br/>52.230.124.46<br/>52.230.122.9 | 443
| Portál | ppe.gateway.azureserviceprofiler.net | Dinamikus | 443
| Storage | *.core.windows.net | Dinamikus | 443
