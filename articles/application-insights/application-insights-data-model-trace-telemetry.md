---
title: Azure Application Insights Telemetria-adatmodell - nyomkövetési Telemetria |} Microsoft Docs
description: Application Insights – nyomkövetési telemetria tartozó adatmodell
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin; sergkanz
ms.openlocfilehash: d93ed9f292b6c05d0a3fb3202567f4024f62e35e
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/08/2018
---
# <a name="trace-telemetry-application-insights-data-model"></a>Nyomkövetési telemetria: Application Insights adatmodell

Nyomkövetési telemetria (a [Application Insights](app-insights-overview.md)) jelöli `printf` stílus nyomkövetési utasítás szöveget keres. `Log4Net`, `NLog`, és más szöveges naplófájl-bejegyzéseket az ilyen típusú példányok van lefordítva. A nyomkövetés nem lehet mérési egy bővíthetőségi.

## <a name="message"></a>Üzenet

Nyomkövetési üzenet.

Maximális hossz: 32768 karakter

## <a name="severity-level"></a>Súlyossági szint

Nyomkövetési súlyossági szint. Az érték lehet `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Egyéni tulajdonságok

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>További lépések

- [.NET nyomkövetési naplók megtekintése az Application Insights felfedezés](app-insights-asp-net-trace-logs.md).
- [Fedezze fel az Application Insights nyomkövetési naplók Java](app-insights-java-trace-logs.md).
- Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.
- [Egyéni nyomkövetési telemetria írása](app-insights-api-custom-events-metrics.md#tracktrace)
- Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.
