---
title: Kép keresési gyors üzembe helyezési SDK C# |} Microsoft Docs
description: A telepítő lemezkép keresési SDK C# Konzolalkalmazás.
titleSuffix: Azure cognitive services setup Image search SDK C# console application
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 01/29/2018
ms.author: v-gedod
ms.openlocfilehash: f58556f13bb25c3ea2ed9378c0669d649554a8c0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35349455"
---
# <a name="image-search-sdk-c-quickstart"></a>Kép keresési SDK C# gyors üzembe helyezés

A Bing kép keresési SDK tartalmazza a REST API-t a vonatkozó kérelmek és elemzési eredmények funkcióit. 

A [forráskód C# Bing kép keresési SDK minták](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch) érhető el a Git központ.

## <a name="application-dependencies"></a>Alkalmazásfüggőségek

Állítsa be a Konzolalkalmazás, a Bing kép keresési SDK használatával, keresse meg a `Manage NuGet Packages` lehetőséget a Visual Studio megoldáskezelőjében.  Adja hozzá a `Microsoft.Azure.CognitiveServices.Search.ImageSearch` csomag.

Telepíti a [kép keresési NuGet csomag](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch/1.2.0) is telepíti a függőségek, beleértve:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="image-search-client"></a>Kép keresési ügyfél
Egy példányának létrehozása a `ImageSearchAPI` ügyfelet, adja hozzá az irányelvek segítségével:
```
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;

```
Az ügyfél ezután példányosítható:
```
var client = new ImageSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));


```
Az ügyfél segítségével keresést a lekérdezés szövegének:
```
// Search for "Yosemite National Park"
var imageResults = client.Images.SearchAsync(query: "Canadian Rockies").Result;
Console.WriteLine("Search images for query \"canadian rockies\"");

```
A kép eredmények a korábbi lekérdezés által visszaadott elemezni:

```
if (imageResults.Value.Count > 0)
{
    var firstImageResult = imageResults.Value.First();

    Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
    Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");
    Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
    Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
}
else
{
    Console.WriteLine("Couldn't find image results!");
}

Console.WriteLine($"\r\nImage result total estimated matches: {imageResults.TotalEstimatedMatches}");

```

## <a name="complete-console-application"></a>Teljes Konzolalkalmazás

A következő Konzolalkalmazás végrehajtja a korábban meghatározott lekérdezést "Kanadai Rockies" keresse meg az eredményeket, majd nyomtassa ki az első kép insights token, miniatűr URL-címét, és a tartalom URL-címe:

```
using System;
using System.Collections.Generic;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
using System.Linq;

namespace ImageSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            var client = new ImageSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
            ImageResults(client);
            ImageDetail(client);
            ImageTrending(client);
            ImageSearchWithFilters(client);

            Console.WriteLine("Any key to exit...");
            Console.ReadKey();
        }

        // Searches for results on query (Canadian Rockies) and prints first image insights token, thumbnail url, and image content url.
        static void ImageResults(ImageSearchAPI client)
        {
            try
            {
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image results!");
                    }

                    Console.WriteLine($"\r\nImage result total estimated matches: {imageResults.TotalEstimatedMatches}");
                    Console.WriteLine($"Image result next offset: {imageResults.NextOffset}");

                    // Pivot suggestions
                    if (imageResults.PivotSuggestions.Count > 0)
                    {
                        var firstPivot = imageResults.PivotSuggestions.First();

                        Console.WriteLine($"\r\nPivot suggestion count: {imageResults.PivotSuggestions.Count}");
                        Console.WriteLine($"First pivot: {firstPivot.Pivot}");

                        if (firstPivot.Suggestions.Count > 0)
                        {
                            var firstSuggestion = firstPivot.Suggestions.First();

                            Console.WriteLine($"\r\nSuggestion count: {firstPivot.Suggestions.Count}");
                            Console.WriteLine($"First suggestion text: {firstSuggestion.Text}");
                            Console.WriteLine($"First suggestion web search url: {firstSuggestion.WebSearchUrl}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find suggestions!");
                        }
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find pivot suggestions!");
                    }

                    // Query expansions
                    if (imageResults.QueryExpansions.Count > 0)
                    {
                        var firstQueryExpansion = imageResults.QueryExpansions.First();

                        Console.WriteLine($"\r\nQuery expansion count: {imageResults.QueryExpansions.Count}");
                        Console.WriteLine($"First query expansion text: {firstQueryExpansion.Text}");
                        Console.WriteLine($"First query expansion search link: {firstQueryExpansion.SearchLink}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find query expansions!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }
        }
    }
}

```

## <a name="search-options"></a>Keresési beállítások

Az SDK különböző funkciókat fogunk bemutatni, a Bing keresési minták.  A következő funkciók hozzáadása a korábban meghatározott `ImageSrchSDK` osztály.

### <a name="search-using-a-filter"></a>A keresés szűrő használatával

Keressen a "studio ghibli", animált GIF és széles aspektus szűri képek, majd ellenőrizze a találatok száma, és nyomtassa ki insightsToken Miniatűr URL-cím és URL-cím első eredmény.

```
        public static void ImageSearchWithFilters(ImageSearchAPI client)
        {
            try
            {
                var imageResults = client.Images.SearchAsync(query: "studio ghibli", imageType: ImageType.AnimatedGif, aspect: ImageAspect.Wide).Result;
                Console.WriteLine("Search images for \"studio ghibli\" results that are animated gifs and wide aspect");

                if (imageResults == null)
                {
                    Console.WriteLine("Didn't see any image result data.");
                }
                else
                {
                    // First image result
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        Console.WriteLine($"First image insightsToken: {firstImageResult.ImageInsightsToken}");
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image web search url: {firstImageResult.WebSearchUrl}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

        }

```

### <a name="trending-images"></a>A világ legnépszerűbb képei

Keresse meg a trendekkel képek, és ellenőrizze a kategóriák és a csempéket.

```
        public static void ImageTrending(ImageSearchAPI client)
        {
            try
            {
                var trendingResults = client.Images.TrendingAsync().Result;
                Console.WriteLine("Search trending images");

                if (trendingResults == null)
                {
                    Console.WriteLine("Didn't see any trending image data.");
                }
                else
                {
                    // Categories
                    if (trendingResults.Categories?.Count > 0)
                    {
                        var firstCategory = trendingResults.Categories[0];
                        Console.WriteLine($"\r\nCategory count: {trendingResults.Categories.Count}");
                        Console.WriteLine($"First category title: {firstCategory.Title}");

                        // Tiles
                        if (firstCategory.Tiles?.Count > 0)
                        {
                            var firstTile = firstCategory.Tiles[0];
                            Console.WriteLine($"\r\nTile count: {firstCategory.Tiles.Count}");
                            Console.WriteLine($"First tile text: {firstTile.Query.Text}");
                            Console.WriteLine($"First tile url: {firstTile.Query.WebSearchUrl}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find tiles!");
                        }
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find categories!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

        }

```

### <a name="image-details"></a>Lemezkép adatait

Lemezképek keressen a "Degas", és keressen a lemezkép adatait az első kép.
```
        public static void ImageDetail(ImageSearchAPI client)
        {

            try
            {
                var imageResults = client.Images.SearchAsync(query: "degas").Result;

                var firstImage = imageResults?.Value?.FirstOrDefault();

                if (firstImage != null)
                {
                    var modules = new List<string>() { ImageInsightModule.All };
                    var imageDetail = client.Images.DetailsAsync(query: "degas", insightsToken: firstImage.ImageInsightsToken, modules: modules).Result;
                    Console.WriteLine($"\r\nSearch detail for image insightsToken={firstImage.ImageInsightsToken}");

                    if (imageDetail != null)
                    {
                        // Insights token
                        Console.WriteLine($"Expected image insights token: {imageDetail.ImageInsightsToken}");

                        // Best representative query
                        if (imageDetail.BestRepresentativeQuery != null)
                        {
                            Console.WriteLine($"\r\nBest representative query text: {imageDetail.BestRepresentativeQuery.Text}");
                            Console.WriteLine($"Best representative query web search url: {imageDetail.BestRepresentativeQuery.WebSearchUrl}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find best representative query!");
                        }

                        // Caption 
                        if (imageDetail.ImageCaption != null)
                        {
                            Console.WriteLine($"\r\nImage caption: {imageDetail.ImageCaption.Caption}");
                            Console.WriteLine($"Image caption data source url: {imageDetail.ImageCaption.DataSourceUrl}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image caption!");
                        }

                        // Pages including the image
                        if (imageDetail.PagesIncluding?.Value?.Count > 0)
                        {
                            var firstPage = imageDetail.PagesIncluding.Value[0];
                            Console.WriteLine($"\r\nPages including count: {imageDetail.PagesIncluding.Value.Count}");
                            Console.WriteLine($"First page content url: {firstPage.ContentUrl}");
                            Console.WriteLine($"First page name: {firstPage.Name}");
                            Console.WriteLine($"First page date published: {firstPage.DatePublished}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find any pages including this image!");
                        }

                        // Related searches 
                        if (imageDetail.RelatedSearches?.Value?.Count > 0)
                        {
                            var firstRelatedSearch = imageDetail.RelatedSearches.Value[0];
                            Console.WriteLine($"\r\nRelated searches count: {imageDetail.RelatedSearches.Value.Count}");
                            Console.WriteLine($"First related search text: {firstRelatedSearch.Text}");
                            Console.WriteLine($"First related search web search url: {firstRelatedSearch.WebSearchUrl}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find any related searches!");
                        }

                        // Visually similar images
                        if (imageDetail.VisuallySimilarImages?.Value?.Count > 0)
                        {
                            var firstVisuallySimilarImage = imageDetail.VisuallySimilarImages.Value[0];
                            Console.WriteLine($"\r\nVisually similar images count: {imageDetail.RelatedSearches.Value.Count}");
                            Console.WriteLine($"First visually similar image name: {firstVisuallySimilarImage.Name}");
                            Console.WriteLine($"First visually similar image content url: {firstVisuallySimilarImage.ContentUrl}");
                            Console.WriteLine($"First visually similar image size: {firstVisuallySimilarImage.ContentSize}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find any related searches!");
                        }

                        // Image tags
                        if (imageDetail.ImageTags?.Value?.Count > 0)
                        {
                            var firstTag = imageDetail.ImageTags.Value[0];
                            Console.WriteLine($"\r\nImage tags count: {imageDetail.ImageTags.Value.Count}");
                            Console.WriteLine($"First tag name: {firstTag.Name}");
                        }
                        else
                        {
                            Console.WriteLine("\r\nCouldn't find any related searches!");
                        }
                    }
                    else
                    {
                        Console.WriteLine("\r\nCouldn't find detail about the image!");
                    }
                }
                else
                {
                    Console.WriteLine("\r\nCouldn't find image results!");
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }
        }

```

## <a name="next-steps"></a>További lépések

[Kognitív services .NET SDK-minták](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)