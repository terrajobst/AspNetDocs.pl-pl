---
uid: web-api/samples-list
title: Lista przykładów interfejsu API sieci Web — ASP.NET 4. x
author: rick-anderson
description: Lista przykładów interfejsu API sieci Web ASP.NET dla ASP.NET 4. x
ms.author: riande
ms.date: 09/18/2012
ms.custom: seoapril2019
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 24368fc80e7fc3c840f1f1f9accf13fa15a06c1b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598463"
---
# <a name="web-api-samples-list"></a>Lista przykładów wzorca Web API

## <a name="httpclient-samples"></a>Przykłady HttpClient

**Przykład tłumaczenia Bing** | [źródła vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/BingTranslateSample)

Pokazuje, jak wywołać [usługę Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) przy użyciu klasy **HttpClient** . Interfejs API usługi Microsoft Translator wymaga tokenu OAuth, który uzyskuje aplikacja, wysyłając żądanie do serwera tokenów platformy Azure dla każdego żądania do usługi Translator. Wynik serwera tokenów jest podawany do żądania wysłanego do usługi tłumaczenia. Przed uruchomieniem tego przykładu należy uzyskać [klucz aplikacji z witryny Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) i podać informacje w klasie przykładowej AccessTokenMessageHandler.

**Przykład usługi Google Maps** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [źródła vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/GoogleMapsSample)

Program używa **HttpClient** do pobierania mapy REDMOND, WA z [interfejsu API usługi Google Maps](https://developers.google.com/maps/), zapisuje ją jako plik lokalny i otwiera domyślną przeglądarkę obrazów.

**Przykładowy klient usługi Twitter** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [źródła vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/TwitterSample)

Pokazuje, jak napisać prostego klienta usługi Twitter przy użyciu **HttpClient**. Przykład używa **HttpMessageHandler** , aby wstawić informacje uwierzytelniania OAuth do **HttpRequestMessage**wychodzącego. Wynik z usługi Twitter jest odczytywany przy użyciu JSON.NET. Przed uruchomieniem tego przykładu należy uzyskać [klucz aplikacji z serwisu Twitter](https://dev.twitter.com/)i wypełnić informacje w klasie przykładowej OAuthMessageHandler.

**Przykład banku światowego** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | źródła danych [vs 2010 Source](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net40) | [a 2012 Source](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net45)

Pokazuje, jak pobrać dane z witryny danych banku światowego przy użyciu JSON.NET, aby przeanalizować wynik.

## <a name="web-api-samples"></a>Przykłady interfejsu API sieci Web

**Wprowadzenie ze źródłem ASP.NET Web API** | [vs 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Pokazuje, jak utworzyć podstawowy interfejs API sieci Web, który obsługuje żądania HTTP GET. Zawiera kod źródłowy samouczka, który został [pierwszy ASP.NET internetowy interfejs API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Scenariusze JavaScript ASP.NET Web API — komentarze** | [vs 2012 Source](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Pokazuje, jak używać interfejsu API sieci Web ASP.NET do tworzenia interfejsów API sieci Web obsługujących klientów przeglądarki i można je łatwo wywoływać za pomocą jQuery.

Źródło programu **Contact Manager** | [a 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Ten przykład używa interfejsu API sieci Web ASP.NET w celu utworzenia prostej aplikacji z Menedżerem kontaktów. Aplikacja składa się z internetowego interfejsu API programu Contact Manager, który jest używany przez aplikację ASP.NET MVC i aplikację Windows Phone do wyświetlania listy kontaktów i zarządzania nią.

**Przykład wsadowy** | [szczegółowy opis](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [źródła vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/BatchSample)

Pokazuje, jak zaimplementować przetwarzanie wsadowe HTTP w ASP.NET. Przetwarzanie wsadowe polega na umieszczeniu wielu żądań HTTP w ramach jednej treści jednostki wieloczęściowej MIME, która jest następnie wysyłana do serwera jako POST protokołu HTTP. Żądania są przetwarzane pojedynczo, a odpowiedzi są umieszczane w innej treści jednostki wieloczęściowej MIME, która jest zwracana do klienta.

**Przykładowy | kontrolera zawartości** — [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) [2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net40) | źródłowej | vs [2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net45) Source

Pokazuje, jak odczytywać i zapisywać jednostki żądań i odpowiedzi asynchronicznie przy użyciu strumieni. Przykładowy kontroler ma dwie akcje: Akcja PUT, która odczytuje treść jednostki żądania asynchronicznie i zapisuje ją w pliku lokalnym oraz akcję GET, która zwraca zawartość pliku lokalnego.

**Przykład niestandardowego programu rozpoznawania zestawów** | [vs 2012 Source](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomAssemblyResolverSample)

Pokazuje, jak zmodyfikować interfejs API sieci Web ASP.NET w celu obsługi odnajdywania kontrolerów z dynamicznie załadowanego zestawu biblioteki. Przykład implementuje niestandardowy **IAssembliesResolver** , który wywołuje implementację domyślną, a następnie dodaje zestaw biblioteki do domyślnych wyników.

**Niestandardowy opis przykładowego programu formatującego typ nośnika** | [szczegóły](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [źródła vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomMediaTypeFormatterSample)

Pokazuje, jak utworzyć niestandardowy program formatujący typ nośnika przy użyciu klasy bazowej **BufferedMediaTypeFormatter** . Ta klasa bazowa jest przeznaczona dla programu formatującego, który głównie używa synchronicznych operacji odczytu i zapisu. Oprócz wyświetlania programu formatującego typu nośnika, przykład pokazuje, jak podłączyć go przez zarejestrowanie go jako część **HttpConfiguration** dla aplikacji. Należy zauważyć, że można również użyć klasy bazowej **MediaTypeFormatter** bezpośrednio dla programu formatującego, który głównie używa asynchronicznych operacji odczytu i zapisu.

**Przykład powiązania parametru niestandardowego** | [szczegółowy opis](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [źródła vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomParameterBinding)

Pokazuje, jak dostosować proces powiązań parametrów, który jest procesem, który określa, jak informacje z żądania są powiązane z parametrami akcji. W tym przykładzie kontroler główny ma cztery akcje:

1. BindPrincipal pokazuje, jak powiązać parametr IPrincipal z niestandardowym podmiotem ogólnym, a nie z komunikatu HTTP GET;
2. BindCustomComplexTypeFromUriOrBody pokazuje, jak powiązać parametr typu złożonego, który może pochodzić z treści komunikatu lub z identyfikatora URI żądania HTTP POST.
3. BindCustomComplexTypeFromUriWithRenamedProperty pokazuje, jak powiązać parametr typu złożonego z właściwością o zmienionej nazwie, która pochodzi od identyfikatora URI żądania wiadomości POST protokołu HTTP.
4. PostMultipleParametersFromBody pokazuje, jak powiązać wiele parametrów z treści dla wiadomości POST;

**Przykład przekazywania plików** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [źródła vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/FileUploadSample)

Pokazuje, jak przekazywać pliki do **ApiController** przy użyciu wieloczęściowego przekazywania plików MIME oraz jak skonfigurować powiadomienia o postępie za pomocą **HttpClient** przy użyciu **ProgressNotificationHandler**. Kontroler odczytuje zawartość pliku HTML asynchroniczne przekazywanie i zapisuje co najmniej jedną część treści do pliku lokalnego. Odpowiedź zawiera informacje na temat przekazanego pliku (lub plików).

**Przykład przekazywania plików do magazynu obiektów blob platformy Azure** | [szczegółowy opis](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [źródła programu VS 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/AzureBlobsFileUploadSample)

Ten przykład jest podobny do przykładowego przekazywania plików, ale zamiast zapisywania przekazanych plików na dysku lokalnym, asynchronicznie przekazuje pliki do [magazynu obiektów blob platformy Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) przy użyciu [zestawu Windows Azure SDK dla platformy .NET](https://www.windowsazure.com/develop/net/). Zapewnia również mechanizm tworzenia listy obiektów BLOB znajdujących się obecnie w [kontenerze BLOB Storage platformy Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Możesz wypróbować przykład uruchomienia dla **emulatora usługi Azure Storage** , który jest dostarczany z zestawem Azure SDK. Jeśli masz [konto usługi Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), możesz również uruchomić usługę Real Storage.

**Przykład potoku obsługi komunikatów Http** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [źródła vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/HttpMessageHandlerPipelineSample)

Pokazuje, jak połączyć wystąpienia **HttpMessageHandler** zarówno na kliencie (**HttpClient**), jak i na serwerze (ASP.NET Web API). W przykładzie ta sama procedura obsługi jest używana zarówno na kliencie, jak i na serwerze. Chociaż jest to rzadki, że dokładna procedura obsługi będzie działać w obu miejscach, model obiektów jest taki sam na stronie klienta i serwera.

**Przykład przekazywania danych JSON** | [źródła vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/JsonUploadSample)

Pokazuje, jak przekazywać i pobierać dane JSON do i z **ApiController**. Przykład korzysta z minimalnej **ApiController** i uzyskuje do niego dostęp przy użyciu **HttpClient**.

**Przykład zestawu połączonych** danych | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [źródła vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MashupSample)

Pokazuje, w jaki sposób asynchronicznie uzyskiwać dostęp do wielu lokacji zdalnych z poziomu akcji **ApiController** . Za każdym razem, gdy akcja zostanie osiągnięta, żądania są wykonywane asynchronicznie, dzięki czemu nie są blokowane żadne wątki.

**Przykład śledzenia pamięci** | [szczegółowy opis](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [źródła vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MemoryTracingSample)

Ten przykładowy projekt tworzy pakiet NuGet, który zainstaluje niestandardowy moduł zapisujący śledzenia w pamięci w aplikacjach interfejsu API sieci Web ASP.NET.

**Przykładowy MongoDB** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [źródła vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MongoSample)

Pokazuje, jak używać MongoDB jako trwałego magazynu dla **ApiController**przy użyciu wzorca repozytorium.

**Przykładowy procesor treści odpowiedzi** | [vs 2012 Source](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ResponseEntityProcessorSample)

Pokazuje, w jaki sposób skopiować jednostkę odpowiedzi (czyli treść odpowiedzi HTTP) do pliku lokalnego przed przesłaniem do klienta i wykonać dodatkowe przetwarzanie na tym pliku asynchronicznie. Przykład implementuje element **HttpMessageHandler** , który otacza jednostkę odpowiedzi, tak że oba zapisy zapisują się w danych wyjściowych jako normalne i do pliku lokalnego.

**Przekazywanie przykładu metody XDocument** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [źródła vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/UploadXDocumentSample)

Pokazuje, w jaki sposób przekazać metodę XDocument do **ApiController** przy użyciu **PushStreamContent** i **HttpClient**.

**Przykład walidacji** | [źródła vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ValidationSample)

Pokazuje, w jaki sposób można użyć atrybutów walidacji w modelach w ASP.NET WebAPI, aby zweryfikować zawartość żądania HTTP. Pokazuje, jak oznaczyć właściwości zgodnie z potrzebami, jak używać atrybutów niestandardowych zdefiniowanych przez platformę i niestandardowe do dodawania adnotacji do modelu oraz jak zwracać odpowiedzi na błędy dla nieprawidłowych Stanów modelu.

**Przykładowy formularz sieci Web** | [szczegółowy opis](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [źródła vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/WebFormSample)

Pokazuje ApiController dodany do projektu formularzy sieci Web.

**[Przykład RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs to prosta aplikacja śledzenia błędów, która pokazuje, jak używać interfejsu API sieci Web ASP.NET i nowej biblioteki klienta HTTP do tworzenia systemu opartego na nośniku. Przykład obejmuje implementacje klienta i serwera przy użyciu interfejsu API sieci Web ASP.NET. Serwer używa niestandardowego programu formatującego Razor do generowania reprezentacji zasobów. Przykład zawiera również serwer Node. js w celu zilustrowania korzyści, które wynikają z używania projektu pośredniego do oddzielenia klientów i serwerów.
