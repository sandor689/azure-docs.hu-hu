---
title: Az Azure API Management házirend-kifejezések |} Microsoft Docs
description: További tudnivalók az Azure API Management kifejezés.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 5943357bc421bbae0caef7f0acd7aa3364813826
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34597518"
---
# <a name="api-management-policy-expressions"></a>API-felügyeleti házirend-kifejezések
Ez a cikk ismerteti, amelyek házirend kifejezések szintaxisa a C# 7. Minden egyes kifejezés hozzáfér a implicit módon megadott [környezetben](api-management-policy-expressions.md#ContextVariables) változó és egy engedélyezett [részhalmaza](api-management-policy-expressions.md#CLRTypes) .NET-keretrendszer típusú.  

További információk:

- Adja meg a környezetben információkat által a háttérszolgáltatáshoz témakörben talál. Használja a [állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) és [be HTTP-fejléc](api-management-transformation-policies.md#SetHTTPheader) szabályzatai segítségével automatikusan megadhatja ezt az információt.
- Használata a [érvényesítése JWT](api-management-access-restriction-policies.md#ValidateJWT) házirend előre a műveletek hozzáférés hitelesítése a token jogcímei alapján.   
- Használata egy [API Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) megtekintéséhez a házirendek kiértékelése hogyan nyomkövetés, ezért ezen eredmények.  
- A kifejezések használata a [lekérni a gyorsítótár](api-management-caching-policies.md#GetFromCache) és [tároló, a gyorsítótár](api-management-caching-policies.md#StoreToCache) szabályzatainak használatával konfigurálhatja az API Management válasz gyorsítótárazását. Egy időtartamot, amely megfelel a háttérszolgáltatáshoz a biztonsági másolat szolgáltatás által megadott válaszok gyorsítótárazása `Cache-Control` direktívát.  
- Lásd: hogyan hajthat végre a tartalom szűrést. Adatok eltávolítása a háttér-a kapott válasz a [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) és [állítsa be a szervezet](api-management-transformation-policies.md#SetBody) házirendek. 
- A házirend-utasításoknál letöltéséhez lásd: a [api-felügyeleti-minták/házirendek](https://github.com/Azure/api-management-samples/tree/master/policies) github-tárház.  
  
  
##  <a name="Syntax"></a> Szintaxis  
 Egyetlen utasításként kifejezések parancsfájlblokkjában találhatók `@(expression)`, ahol `expression` van egy megfelelően formázott C# kifejezés utasításaként.  
  
 Több utasításból álló kifejezések parancsfájlblokkjában találhatók `@{expression}`. Több utasításból álló kifejezések belül a programkód összes útvonalán végén szerepelnie kell egy `return` utasítást.  
  
##  <a name="PolicyExpressionsExamples"></a> Példák  
  
```  
@(true)  
  
@((1+1).ToString())  
  
@("Hi There".Length)  
  
@(Regex.Match(context.Response.Headers.GetValueOrDefault("Cache-Control",""), @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value)  
  
@(context.Variables.ContainsKey("maxAge") ? int.Parse((string)context.Variables["maxAge"]) : 3600)  
  
@{   
  string value;   
  if (context.Request.Headers.TryGetValue("Authorization", out value))   
  {   
    return Encoding.UTF8.GetString(Convert.FromBase64String(value));  
  }   
  else   
  {   
    return null;  
  }  
}  
```  
  
##  <a name="PolicyExpressionsUsage"></a> Használat  
 Kifejezések használhatók attribútum vagy szöveges értékek bármely API Management [házirendek](api-management-policies.md) (kivéve, ha a házirend-hivatkozás határozza meg, ellenkező esetben).  
  
> [!IMPORTANT]
>  Házirend-kifejezések használata esetén nincs a házirend-kifejezések csak korlátozott vizsgálata során a házirend lett meghatározva. Az átjáró futásidőben, futásidejű hiba eredményez házirend kifejezések által létrehozott kivételek kifejezések végrehajtása.  
  
##  <a name="CLRTypes"></a> .NET-keretrendszer típusok engedélyezett a házirend-kifejezések  
 A következő táblázat felsorolja a .NET-keretrendszer típusok és azok tagjait, házirend-kifejezések engedélyezett.  
  
|CLR-típus|Támogatott módszerek|  
|--------------|-----------------------|  
|Newtonsoft.Json.Linq.Extensions|Módszer közül mindegyik támogatott|  
|Newtonsoft.Json.Linq.JArray|Módszer közül mindegyik támogatott|  
|Newtonsoft.Json.Linq.JConstructor|Módszer közül mindegyik támogatott|  
|Newtonsoft.Json.Linq.JContainer|Módszer közül mindegyik támogatott|  
|Newtonsoft.Json.Linq.JObject|Módszer közül mindegyik támogatott|  
|Newtonsoft.Json.Linq.JProperty|Módszer közül mindegyik támogatott|  
|Newtonsoft.Json.Linq.JRaw|Módszer közül mindegyik támogatott|  
|Newtonsoft.Json.Linq.JToken|Módszer közül mindegyik támogatott|  
|Newtonsoft.Json.Linq.JTokenType|Módszer közül mindegyik támogatott|  
|Newtonsoft.Json.Linq.JValue|Módszer közül mindegyik támogatott|  
|System.Collections.Generic.IReadOnlyCollection<T\>|Összes|  
|System.Collections.Generic.IReadOnlyDictionary<TKey,  TValue>|Összes|  
|System.Collections.Generic.ISet<TKey, TValue>|Összes|  
|System.Collections.Generic.KeyValuePair<TKey,  TValue>|Kulcs értéke|  
|System.Collections.Generic.List<TKey, TValue>|Összes|  
|System.Collections.Generic.Queue<TKey, TValue>|Összes|  
|System.Collections.Generic.Stack<TKey, TValue>|Összes|  
|System.Convert|Összes|  
|System.DateTime|Összes|  
|System.DateTimeKind|UTC szerint|  
|System.DateTimeOffset|Összes|  
|System.Decimal|Összes|  
|System.Double|Összes|  
|System.Guid|Összes|  
|System.IEnumerable < T\>|Összes|  
|System.IEnumerator<T\>|Összes|  
|System.Int16|Összes|  
|System.Int32|Összes|  
|System.Int64|Összes|  
|System.Linq.Enumerable<T\>|Módszer közül mindegyik támogatott|  
|System.Math|Összes|  
|System.MidpointRounding|Összes|  
|System.Nullable<T\>|Összes|  
|System.Random|Összes|  
|System.SByte|Összes|  
|System.Security.Cryptography. HMACSHA384|Összes|  
|System.Security.Cryptography. HMACSHA512|Összes|  
|System.Security.Cryptography.HashAlgorithm|Összes|  
|System.Security.Cryptography.HMAC|Összes|  
|System.Security.Cryptography.HMACMD5|Összes|  
|System.Security.Cryptography.HMACSHA1|Összes|  
|System.Security.Cryptography.HMACSHA256|Összes|  
|System.Security.Cryptography.KeyedHashAlgorithm|Összes|  
|System.Security.Cryptography.MD5|Összes|  
|System.Security.Cryptography.RNGCryptoServiceProvider|Összes|  
|System.Security.Cryptography.SHA1|Összes|  
|System.Security.Cryptography.SHA1Managed|Összes|  
|System.Security.Cryptography.SHA256|Összes|  
|System.Security.Cryptography.SHA256Managed|Összes|  
|System.Security.Cryptography.SHA384|Összes|  
|System.Security.Cryptography.SHA384Managed|Összes|  
|System.Security.Cryptography.SHA512|Összes|  
|System.Security.Cryptography.SHA512Managed|Összes|  
|System.Single|Összes|  
|System.String|Összes|  
|System.StringSplitOptions|Összes|  
|System.Text.Encoding|Összes|  
|System.Text.RegularExpressions.Capture|Index, hossz, érték|  
|System.Text.RegularExpressions.CaptureCollection|Count elem|  
|System.Text.RegularExpressions.Group|Rögzíti, sikeres|  
|System.Text.RegularExpressions.GroupCollection|Count elem|  
|System.Text.RegularExpressions.Match|Üres, csoportok, eredménye|  
|System.Text.RegularExpressions.Regex|(Konstruktor) IsMatch, egyezik, megegyezik, cserélje le a következőket:|  
|System.Text.RegularExpressions.RegexOptions|Lefordított, az IgnoreCase, IgnorePatternWhitespace, többsoros, None, RightToLeft, Singleline|  
|System.TimeSpan|Összes|  
|System.Tuple|Összes|  
|System.UInt16|Összes|  
|System.UInt32|Összes|  
|System.UInt64|Összes|  
|System.Uri|Összes|  
|System.Xml.Linq.Extensions|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XAttribute|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XCData|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XComment|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XContainer|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XDeclaration|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XDocument|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XDocumentType|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XElement|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XName|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XNamespace|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XNode|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XNodeDocumentOrderComparer|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XNodeEqualityComparer|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XObject|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XProcessingInstruction|Módszer közül mindegyik támogatott|  
|System.Xml.Linq.XText|Módszer közül mindegyik támogatott|  
|System.Xml.XmlNodeType|Összes|  
  
##  <a name="ContextVariables"></a> Környezeti változó  
 Nevű változó `context` implicit módon érhető el minden házirend [kifejezés](api-management-policy-expressions.md#Syntax). A tagok adja meg a megfelelő adatokat a `\request`. Összes a `context` tagjai csak olvasható.  
  
|Környezeti változó|Engedélyezett metódusok, tulajdonságok és értékei|  
|----------------------|-------------------------------------------------------|  
|A környezetben|API: IApi<br /><br /> Környezet<br /><br /> Eltelt: TimeSpan - Timestamp értéket és aktuális időpontja között eltelt idő<br /><br /> LastError<br /><br /> Művelet<br /><br /> Product<br /><br /> Kérés<br /><br /> Kérelemazonosító: Guid - egyedi azonosítóját<br /><br /> Válasz<br /><br /> Előfizetés<br /><br /> Időbélyeg: DateTime - időpont, amikor a kérelem érkezett<br /><br /> Nyomkövetés: bool - azt jelzi, hogy nyomkövetés be- és kikapcsolása <br /><br /> Felhasználó<br /><br /> Változók: IReadOnlyDictionary < karakterlánc, objektum ><br /><br /> "void" Trace(message: string)|  
|context.Api|Azonosító: karakterlánc<br /><br /> IsCurrentRevision: logikai<br /><br />  Name: karakterlánc<br /><br /> Elérési út: karakterlánc<br /><br /> Változat: karakterlánc<br /><br /> ServiceUrl: IUrl<br /><br /> Verzió: karakterlánc |  
|a környezetben. Központi telepítés|A régióban: karakterlánc<br /><br /> Szolgáltatásnév: karakterlánc<br /><br /> Certificates: IReadOnlyDictionary<string, X509Certificate2>|  
|context.LastError|Forrás: karakterlánc<br /><br /> OK: karakterlánc<br /><br /> Üzenet: karakterlánc<br /><br /> Hatókör: karakterlánc<br /><br /> A szakasz: karakterlánc<br /><br /> Elérési út: karakterlánc<br /><br /> PolicyId: karakterlánc<br /><br /> További információ a környezetben. Hiba, lásd: [hibakezelés](api-management-error-handling-policies.md).|  
|a környezetben. Művelet|Azonosító: karakterlánc<br /><br /> Módszer: karakterlánc<br /><br /> Name: karakterlánc<br /><br /> UrlTemplate: karakterlánc|  
|a környezetben. A termék|API-k: IEnumerable < IApi\><br /><br /> ApprovalRequired: logikai<br /><br /> Csoportok: IEnumerable < IGroup\><br /><br /> Azonosító: karakterlánc<br /><br /> Name: karakterlánc<br /><br /> Állapot: enum ProductState {NotPublished, közzétett}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: logikai|  
|a környezetben. Kérelem|Törzs: IMessageBody<br /><br /> Certificate: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Fejlécek: IReadOnlyDictionary < string, string [] ><br /><br /> IP-cím: karakterlánc<br /><br /> MatchedParameters: IReadOnlyDictionary<string, string><br /><br /> Módszer: karakterlánc<br /><br /> OriginalUrl:IUrl<br /><br /> URL-cím: IUrl|  
|karakterlánc-környezetben. Request.Headers.GetValueOrDefault (fejléc neve: karakterlánc, alapértelmezett érték: karakterlánc)|fejléc neve: karakterlánc<br /><br /> DefaultValue érték: karakterlánc<br /><br /> Vesszővel tagolt kérelem fejléc értékei adja vissza vagy `defaultValue` Ha a fejléc nem található.|  
|a környezetben. Válasz|Törzs: IMessageBody<br /><br /> Fejlécek: IReadOnlyDictionary < string, string [] ><br /><br /> StatusCode: int<br /><br /> StatusReason: karakterlánc|  
|karakterlánc-környezetben. Response.Headers.GetValueOrDefault (fejléc neve: karakterlánc, alapértelmezett érték: karakterlánc)|fejléc neve: karakterlánc<br /><br /> DefaultValue érték: karakterlánc<br /><br /> A fejléc értékei vesszővel tagolt választ ad vissza, vagy `defaultValue` Ha a fejléc nem található.|  
|context.Subscription|CreatedTime: DateTime<br /><br /> EndDate: DateTime?<br /><br /> Azonosító: karakterlánc<br /><br /> Kulcs: karakterlánc<br /><br /> Name: karakterlánc<br /><br /> PrimaryKey: karakterlánc<br /><br /> Másodlagos kulcs: karakterlánc<br /><br /> A StartDate: DateTime?|  
|context.User|E-mailek: karakterlánc<br /><br /> Utónév: karakterlánc<br /><br /> Csoportok: IEnumerable < IGroup\><br /><br /> Azonosító: karakterlánc<br /><br /> Identitások: IEnumerable < IUserIdentity\><br /><br /> Vezetéknév: karakterlánc<br /><br /> Megjegyzés: karakterlánc<br /><br /> RegistrationDate: DateTime|  
|IApi|Azonosító: karakterlánc<br /><br /> Name: karakterlánc<br /><br /> Elérési út: karakterlánc<br /><br /> Protokollok: IEnumerable < karakterlánc\><br /><br /> ServiceUrl: IUrl<br /><br /> SubscriptionKeyParameterNames: ISubscriptionKeyParameterNames|  
|IGroup|Azonosító: karakterlánc<br /><br /> Name: karakterlánc|  
|IMessageBody|Mint < T\>(preserveContent: bool = false): Ha T: karakterlánc, JObject, JToken, JArray, XNode, XElement, XDocument<br /><br /> A `context.Request.Body.As<T>` és `context.Response.Body.As<T>` módszerek használhatók egy kérelem-válasz olvasása a megadott típusú üzenettörzs `T`. Alapértelmezés szerint a használja az eredeti üzenet törzsének adatfolyam és válik elérhetetlenné után adja vissza. Azzal, hogy a szervezet adatfolyam másolatát működik metódus, amely elkerüléséhez állítsa a `preserveContent` paramétert `true`. Nyissa meg [Itt](api-management-transformation-policies.md#SetBody) lásd például.|  
|IUrl|Állomás: karakterlánc<br /><br /> Elérési út: karakterlánc<br /><br /> Port: int<br /><br /> Query: IReadOnlyDictionary<string, string[]><br /><br /> Lekérdezési karakterlánc: karakterlánc<br /><br /> Rendszer: karakterlánc|  
|IUserIdentity|Azonosító: karakterlánc<br /><br /> Szolgáltató: karakterlánc|  
|ISubscriptionKeyParameterNames|Fejléc: karakterlánc<br /><br /> Lekérdezés: karakterlánc|  
|karakterlánc-IUrl.Query.GetValueOrDefault (queryParameterName: karakterlánc, alapértelmezett érték: karakterlánc)|queryParameterName: karakterlánc<br /><br /> DefaultValue érték: karakterlánc<br /><br /> Vesszővel tagolt lekérdezés paramétereinek értékét adja vissza vagy `defaultValue` Ha a paraméter nem található.|  
|T környezetben. Variables.GetValueOrDefault < T\>(variableName: karakterlánc, defaultValue: T)|variableName: karakterlánc<br /><br /> defaultValue: T<br /><br /> Írja be a típusúvá változó értékét adja vissza `T` vagy `defaultValue` Ha a változó nem található.<br /><br /> Ez a metódus kivételt jelez, ha a megadott típus nem egyezik a tényleges típus, a visszaadott változó.|  
|BasicAuthCredentials AsBasic(input: this string)|bemeneti: karakterlánc<br /><br /> Ha a bemeneti paraméter érvényes HTTP-hitelesítés alapszintű engedélyezési kérelem fejléc értéke tartalmaz, a metódus visszaadja típusú objektum `BasicAuthCredentials`; ellenkező esetben a metódus null értéket ad vissza.|  
|logikai TryParseBasic (bemenet: a karakterlánc, az eredmény: BasicAuthCredentials kimenő)|bemeneti: karakterlánc<br /><br /> eredmény: BasicAuthCredentials kimenő<br /><br /> Ha a bemeneti paraméter értéke érvénytelen egyszerű HTTP-hitelesítés engedélyezési a kérelem fejlécében a metódus visszaadja `true` és az eredmény paraméter típusú értéket tartalmaz `BasicAuthCredentials`; ellenkező esetben a metódus visszaadja `false`.|  
|BasicAuthCredentials|Jelszó: karakterlánc<br /><br /> UserId: string|  
|Jwt AsJwt(input: this string)|bemeneti: karakterlánc<br /><br /> A bemeneti paraméter értéke érvényes JWT jogkivonat, ha a metódus visszaadja típusú objektum `Jwt`; ellenkező esetben a metódus visszaadja `null`.|  
|logikai TryParseJwt (bemenet: a karakterlánc, az eredmény: Jwt kimenő)|bemeneti: karakterlánc<br /><br /> eredmény: Jwt-kimenő<br /><br /> Ha a bemeneti paraméter értéke érvénytelen JWT jogkivonat, a metódus visszaadja `true` és az eredmény paraméter típusú értéket tartalmaz `Jwt`; ellenkező esetben a metódus visszaadja `false`.|  
|Jwt-t|Algoritmus: karakterlánc<br /><br /> A célközönség: IEnumerable < karakterlánc\><br /><br /> Claims: IReadOnlyDictionary<string, string[]><br /><br /> ExpirationTime: DateTime?<br /><br /> Azonosító: karakterlánc<br /><br /> Kibocsátó: karakterlánc<br /><br /> NotBefore: DateTime?<br /><br /> Tulajdonos: karakterlánc<br /><br /> Típus: karakterlánc|  
|string Jwt.Claims.GetValueOrDefault(claimName: string, defaultValue: string)|claimName: karakterlánc<br /><br /> DefaultValue érték: karakterlánc<br /><br /> Vesszővel elválasztott értéket ad vissza a jogcímérték vagy `defaultValue` Ha a fejléc nem található.|
|byte [] titkosítása (bemeneti: a byte [], alg: karakterlánc, a kulcs: byte [], a iv:byte[])|bemenet – egyszerű szöveges titkosításához<br /><br />ALG - szimmetrikus titkosítási algoritmus neve<br /><br />kulcs - titkosítási kulcs<br /><br />IV-inicializáló vektort.<br /><br />Vissza titkosított egyszerű szöveg.|
|byte [] titkosítása (bemeneti: a byte [], alg: System.Security.Cryptography.SymmetricAlgorithm)|bemenet – egyszerű szöveges titkosításához<br /><br />ALG - titkosítási algoritmus<br /><br />Vissza titkosított egyszerű szöveg.|
|byte [] titkosítása (bemeneti: a byte [], alg: System.Security.Cryptography.SymmetricAlgorithm, kulcs: byte [], iv:byte[])|bemenet – egyszerű szöveges titkosításához<br /><br />ALG - titkosítási algoritmus<br /><br />kulcs - titkosítási kulcs<br /><br />IV-inicializáló vektort.<br /><br />Vissza titkosított egyszerű szöveg.|
|byte [] visszafejtése (bemeneti: a byte [], alg: karakterlánc, a kulcs: byte [], a iv:byte[])|bemenet - rejtjelzésben szöveg vissza kell fejteni<br /><br />ALG - szimmetrikus titkosítási algoritmus neve<br /><br />kulcs - titkosítási kulcs<br /><br />IV-inicializáló vektort.<br /><br />Egyszerű szövegként adja vissza.|
|byte [] visszafejtése (bemeneti: a byte [], alg: System.Security.Cryptography.SymmetricAlgorithm)|bemenet - rejtjelzésben szöveg vissza kell fejteni<br /><br />ALG - titkosítási algoritmus<br /><br />Egyszerű szövegként adja vissza.|
|byte [] visszafejtése (bemeneti: a byte [], alg: System.Security.Cryptography.SymmetricAlgorithm, kulcs: byte [], iv:byte[])|visszafejtendő bemeneti - bemeneti - rejtjelzésben szöveg<br /><br />ALG - titkosítási algoritmus<br /><br />kulcs - titkosítási kulcs<br /><br />IV-inicializáló vektort.<br /><br />Egyszerű szövegként adja vissza.|


## <a name="next-steps"></a>További lépések

Házirendek használata további információkért lásd:

+ [Az API Management házirendek](api-management-howto-policies.md)
+ [Átalakítás API-k](transform-api.md)
+ [Házirend-hivatkozás](api-management-policy-reference.md) házirend-utasításoknál és a beállítások teljes listáját
+ [Házirend-minták](policy-samples.md)   
