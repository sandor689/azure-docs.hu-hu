
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>A Microsoft Graph API meghívása iOS-alkalmazásból

Ez az útmutató ismerteti, hogyan egy natív iOS-alkalmazás (Swift) segítségével meghívhatja a API-k, amelyek a Microsoft Azure Active Directory (Azure AD) v2.0-végpont a hozzáférési jogkivonatok igényelnek. Az útmutató bemutatja, hogyan hozzáférési tokenek beszerzése és a Microsoft Graph API és más API-hívásokat használni őket.

Miután végzett az útmutatóban szereplő gyakorlatok, az alkalmazás meghívhat egy védett API-t bármely vállalat vagy szervezet, amely rendelkezik az Azure ad-ben. Az alkalmazás például outlook.com, live.com, és egyéb személyes fiókok, valamint a munkahelyi vagy iskolai fiókok használatával is felvehető a védett API-hívások.

## <a name="prerequisites"></a>Előfeltételek
- XCode-verzió 8.x a jelen útmutatóban létrehozott minta megadása kötelező. Az XCode letöltheti a [iTunes webhely](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode letöltési URL-cím").
- A [Carthage](https://github.com/Carthage/Carthage) Függőségkezelő csomagkezelés szükség.

## <a name="how-this-guide-works"></a>Ez az útmutató működése

![Ez az útmutató működése](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.png)

Ebben az útmutatóban a mintaalkalmazás lehetővé teszi az IOS-es alkalmazás lekérdezése a Microsoft Graph API vagy a webes API-t, amely az Azure AD v2.0-végpont származó jogkivonatokat fogad el. Ebben a forgatókönyvben egy jogkivonatot a HTTP-kérelmekre szolgáltatással hozzáadott a **engedélyezési** fejléc. Token beszerzése és megújítása a Microsoft hitelesítési tár (MSAL) kezeli.


### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Token beszerzése a védett webes API-khoz való hozzáférés kezelése

Miután a felhasználó hitelesíti magát, a mintaalkalmazás kap jogkivonatot. A jogkivonatot a Microsoft Graph API vagy a webes API-k az Azure AD v2.0-végpont által védett lekérdezésére szolgál.

API-k, például a Microsoft Graph, egy hozzáférési jogkivonatot az adott erőforrások hozzáférésének engedélyezéséhez szükséges. Jogkivonatok szükségesek egy felhasználói profil olvasása, a felhasználó naptár elérésére, e-mail küldése és így tovább. Az alkalmazás hozzáférési tokent tud kérni az MSAL használatával, és adja meg az API-hatóköröket. A hozzáférési jogkivonatot a HTTP-re kerül **engedélyezési** fejlécet minden hívás, amely a védett erőforrás ellen.

Az MSAL gyorsítótárazását és hozzáférési jogkivonatok, frissíteni, így nem kell az alkalmazás kezeli.


## <a name="libraries"></a>Kódtárak

Ez az útmutató használja a következő könyvtárban:

|Részletes ismertetés|Leírás|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|IOS-hez készült Microsoft hitelesítési tár előzetes verzióját|

