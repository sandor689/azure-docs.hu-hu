---
title: Az Azure Functions Twilio-kötés
description: Megtudhatja, hogyan használhatja a Twilio-kötések az Azure Functions használatával.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: az Azure functions, függvények, eseményfeldolgozás, dinamikus számítás, kiszolgáló nélküli architektúra
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/09/2018
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 580dd0409c2210de786723736128d489e5a93aa9
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345830"
---
# <a name="twilio-binding-for-azure-functions"></a>A Twilio-kötést az Azure Functions

Ez a cikk azt ismerteti, hogyan a szöveges üzenetek küldéséhez a [Twilio](https://www.twilio.com/) kötések az Azure Functions szolgáltatásban. Az Azure Functions támogatja a kimeneti kötések a Twilióból.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Csomagok – 1.x függvények

A Twilio-kötések szerepelnek a [Microsoft.Azure.WebJobs.Extensions.Twilio](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) NuGet-csomag verziója 1.x. A csomag forráskódja a [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.Twilio/) GitHub-adattárban.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Csomagok – 2.x függvények

A Twilio-kötések szerepelnek a [Microsoft.Azure.WebJobs.Extensions.Twilio](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) NuGet-csomag verziója 3.x. A csomag forráskódja a [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/) GitHub-adattárban.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example---functions-1x"></a>– Példa 1.x függvények

Tekintse meg az adott nyelvű példa:

* [C#](#c-example)
* [C# script (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-példa

A következő példa bemutatja egy [C#-függvény](functions-dotnet-class-library.md) , amely egy üzenetsor által aktivált Ha szöveges üzenetet küld.

```cs
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

Ez a példa a `TwilioSms` attribútum metódus visszaadott értéke. Az attribútum a használata helyett egy `out SMSMessage` paraméter vagy egy `ICollector<SMSMessage>` vagy `IAsyncCollector<SMSMessage>` paraméter.

### <a name="c-script-example"></a>C#-szkript példa

Az alábbi példa bemutatja egy Twilio kimeneti kötés történő egy *function.json* fájl és a egy [C#-szkriptfüggvény](functions-reference-csharp.md) , amely a kötés használja. A függvényt használ egy `out` paraméter szöveges üzenet küldése.

Íme kötési adatait a *function.json* fájlt:

Példa function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Itt látható C#-szkriptkódot:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

Kimenő aszinkron kódban paraméterek nem használhatók. Íme egy aszinkron C# szkriptet kód példa:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

### <a name="javascript-example"></a>JavaScript-példa

Az alábbi példa bemutatja egy Twilio kimeneti kötés történő egy *function.json* fájl és a egy [JavaScript-függvény](functions-reference-node.md) , amely a kötés használja.

Íme kötési adatait a *function.json* fájlt:

Példa function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

A következő JavaScript-kódot:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="example---functions-2x"></a>Példa – Functions 2.x

Tekintse meg az adott nyelvű példa:

* [2.x C#](#2x-c-example)
* [2.x C#-szkript (.csx)](#2x-c-script-example)
* [2.x JavaScript](#2x-javascript-example)

### <a name="2x-c-example"></a>2.x C#-példa

A következő példa bemutatja egy [C#-függvény](functions-dotnet-class-library.md) , amely egy üzenetsor által aktivált Ha szöveges üzenetet küld.

```cs
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static CreateMessageOptions Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"))
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

Ez a példa a `TwilioSms` attribútum metódus visszaadott értéke. Az attribútum a használata helyett egy `out CreateMessageOptions` paraméter vagy egy `ICollector<CreateMessageOptions>` vagy `IAsyncCollector<CreateMessageOptions>` paraméter.

### <a name="2x-c-script-example"></a>példaszkript 2.x C#

Az alábbi példa bemutatja egy Twilio kimeneti kötés történő egy *function.json* fájl és a egy [C#-szkriptfüggvény](functions-reference-csharp.md) , amely a kötés használja. A függvényt használ egy `out` paraméter szöveges üzenet küldése.

Íme kötési adatait a *function.json* fájlt:

Példa function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Itt látható C#-szkriptkódot:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;

public static void Run(string myQueueItem, out CreateMessageOptions message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least
    // initialize the CreateMessageOptions variable.
    message = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"));

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

Kimenő aszinkron kódban paraméterek nem használhatók. Íme egy aszinkron C# szkriptet kód példa:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;

public static async Task Run(string myQueueItem, IAsyncCollector<CreateMessageOptions> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least
    // initialize the CreateMessageOptions variable.
    CreateMessageOptions smsText = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"));

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

### <a name="2x-javascript-example"></a>2.x JavaScript-példa

Az alábbi példa bemutatja egy Twilio kimeneti kötés történő egy *function.json* fájl és a egy [JavaScript-függvény](functions-reference-node.md) , amely a kötés használja.

Íme kötési adatait a *function.json* fájlt:

Példa function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

A következő JavaScript-kódot:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="attributes"></a>Attribútumok

A [C#-osztálykódtárakat](functions-dotnet-class-library.md), használja a [TwilioSms](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) attribútum.

Konfigurálható attribútum-tulajdonságokkal kapcsolatos információkért lásd: [konfigurációs](#configuration). Íme egy `TwilioSms` attribútum példa egy podpis metody:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(
    AccountSidSetting = "TwilioAccountSid", 
    AuthTokenSetting = "TwilioAuthToken", 
    From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    ...
}
 ```

Egy teljes példa: [C#-példa](#c-example).

## <a name="configuration"></a>Konfiguráció

A következő táblázat ismerteti a megadott kötés konfigurációs tulajdonságaiban a *function.json* fájlt, és a `TwilioSms` attribútum.

|Function.JSON tulajdonság | Attribútum tulajdonsága |Leírás|
|---------|---------|----------------------|
|**type**|| Meg kell `twilioSms`.|
|**direction**|| Meg kell `out`.|
|**name**|| A Twilio SMS-üzenetben a függvény kódját a használt változó neve. |
|**accountSid**|**AccountSid**| Ezt az értéket kell beállítani a Twilio-fiók Sid tartalmazó alkalmazásbeállítás neve.|
|**authToken**|**Sablonokból**| Ezt az értéket kell beállítani a Twilio hitelesítési jogkivonatot tartalmazó alkalmazásbeállítás neve.|
|**A**|**Címzett**| Ez az érték a telefonszámot, amelyet elküld a SMS szöveg értéke.|
|**A**|**A**| Ez az érték a telefonszámot, amelyet az SMS szöveges is küld értéke.|
|**Törzs**|**Törzs**| Ez az érték használható az SMS-üzenetben keményen code, ha nem szeretne dinamikusan állítsa a függvény a kódban. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [Tudjon meg többet az Azure functions eseményindítók és kötések](functions-triggers-bindings.md)