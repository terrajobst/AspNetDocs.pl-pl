---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Routing adresów URL | Microsoft Docs
author: Erikre
description: Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 66b727b69ca4f9a3d35b67f492f9a554146e09ef
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74590710"
---
# <a name="url-routing"></a>Routing adresów URL

Autor [Erik Reitan](https://github.com/Erikre)

[Pobierz program Wingtip zabawki (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013 dla sieci Web. Projekt Visual Studio 2013 [z C# kodem źródłowym](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny do tej serii samouczków.

W tym samouczku zmodyfikujesz przykładową aplikację Wingtip zabawki, aby obsługiwać Routing adresów URL. Funkcja routingu umożliwia aplikacji sieci Web korzystanie z przyjaznych, łatwiejszych do zapamiętania adresów URL i lepszych obsługi przez aparaty wyszukiwania. Ten samouczek jest oparty na poprzednim samouczku "członkostwo i administracja" i jest częścią serii samouczków Wingtip.

## <a name="what-youll-learn"></a>Dowiesz się:

- Jak zarejestrować trasy dla aplikacji formularzy sieci Web ASP.NET.
- Jak dodać trasy do strony sieci Web.
- Jak wybrać dane z bazy danych do obsługi tras.

## <a name="aspnet-routing-overview"></a>ASP.NET routing — Omówienie

Routing adresów URL umożliwia skonfigurowanie aplikacji do akceptowania adresów URL żądań, które nie są mapowane na pliki fizyczne. Adres URL żądania jest po prostu adresem URL, który użytkownik wprowadza do przeglądarki, aby znaleźć stronę w witrynie sieci Web. Funkcja routingu służy do definiowania adresów URL, które są semantycznie zrozumiałe dla użytkowników i które mogą pomóc w optymalizacji aparatu wyszukiwania (wyszukiwarka).

Domyślnie szablon formularzy sieci Web zawiera [przyjazne adresy url ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Większość podstawowych zadań routingu zostanie wdrożona za pomocą *przyjaznych adresów URL*. Jednak w tym samouczku dodasz dostosowane możliwości routingu.

Przed przystąpieniem do dostosowywania routingu adresów URL Przykładowa aplikacja Wingtip zabawki może połączyć się z produktem, korzystając z następującego adresu URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Dostosowując Routing adresów URL, aplikacja Przykładowa Wingtip zabawki będzie łączyć się z tym samym produktem przy użyciu łatwiejszy do odczytania adresu URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Rozsyłan

Trasa jest wzorcem adresu URL, który jest mapowany do procedury obsługi. Program obsługi może być plikiem fizycznym, takim jak plik aspx w aplikacji formularzy sieci Web. Program obsługi może być również klasą, która przetwarza żądanie. Aby zdefiniować trasę, należy utworzyć wystąpienie klasy Route przez określenie wzorca adresu URL, programu obsługi i opcjonalnie nazwy trasy.

Dodaj trasę do aplikacji, dodając obiekt `Route` do właściwości static `Routes` klasy `RouteTable`. Właściwość Routes jest obiektem `RouteCollection`, który przechowuje wszystkie trasy dla aplikacji.

### <a name="url-patterns"></a>Wzorce adresów URL

Wzorzec adresu URL może zawierać wartości literałów i symbole zastępcze zmiennych (określane jako parametry adresu URL). Literały i symbole zastępcze znajdują się w segmentach adresu URL, które są rozdzielane znakami ukośnika (`/`).

Po utworzeniu żądania do aplikacji sieci Web adres URL jest analizowany do segmentów i symboli zastępczych, a wartości zmiennych są przekazywane do programu obsługi żądań. Ten proces jest podobny do sposobu, w jaki dane w ciągu zapytania są analizowane i przesyłane do procedury obsługi żądania. W obu przypadkach informacje o zmiennych są zawarte w adresie URL i przesyłane do procedury obsługi w postaci par klucz-wartość. W przypadku ciągów zapytań zarówno klucze, jak i wartości są w adresie URL. W przypadku tras klucze są nazwami zastępczymi zdefiniowanymi we wzorcu adresu URL, a tylko wartości są w adresie URL.

W wzorcu adresu URL definiuje się symbole zastępcze, umieszczając je w nawiasach klamrowych (`{` i `}`). Można zdefiniować więcej niż jeden symbol zastępczy w segmencie, ale symbole zastępcze muszą być oddzielone wartością literału. Na przykład `{language}-{country}/{action}` jest prawidłowym wzorcem trasy. Jednakże `{language}{country}/{action}` nie jest prawidłowym wzorcem, ponieważ nie ma wartości literału ani ogranicznika między symbolami zastępczymi. W związku z tym routing nie może określić, gdzie należy rozdzielić wartość symbolu zastępczego języka z wartości symbolu zastępczego kraju.

### <a name="mapping-and-registering-routes"></a>Mapowanie i rejestrowanie tras

Zanim będzie można uwzględnić trasy do stron przykładowej aplikacji Wingtip zabawki, należy zarejestrować trasy podczas uruchamiania aplikacji. Aby zarejestrować trasy, zmodyfikuj procedurę obsługi zdarzeń `Application_Start`.

1. W **Eksplorator rozwiązań**programu Visual Studio Znajdź i otwórz plik *Global.asax.cs* .
2. Dodaj kod wyróżniony żółtym do pliku *Global.asax.cs* w następujący sposób:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Po uruchomieniu przykładowej aplikacji Wingtip zabawki zostanie wywołana procedura obsługi zdarzeń `Application_Start`. Na końcu tego programu obsługi zdarzeń wywoływana jest metoda `RegisterCustomRoutes`. Metoda `RegisterCustomRoutes` dodaje każdą trasę, wywołując metodę `MapPageRoute` obiektu `RouteCollection`. Trasy są definiowane przy użyciu nazwy trasy, adresu URL trasy i fizycznego adresu URL.

Pierwszy parametr ("`ProductsByCategoryRoute`") jest nazwą trasy. Służy do wywoływania trasy, gdy jest to konieczne. Drugi parametr ("`Category/{categoryName}`") definiuje przyjazny zastępczy adres URL, który może być dynamiczny w oparciu o kod. Ta trasa jest używana podczas wypełniania kontrolki danych za pomocą łączy, które są generowane na podstawie danych. Trasa jest pokazywana w następujący sposób:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Drugi parametr trasy zawiera wartość dynamiczną określoną przez nawiasy klamrowe (`{ }`). W tym przypadku `categoryName` jest zmienną, która zostanie użyta do określenia właściwej ścieżki routingu.

> [!NOTE] 
> 
> **Optional**
> 
> Łatwiej jest zarządzać kodem, przenosząc metodę `RegisterCustomRoutes` do oddzielnej klasy. W folderze *logiki* utwórz oddzielną klasę `RouteActions`. Przenieś powyższą metodę `RegisterCustomRoutes` z pliku *Global.asax.cs* do nowej klasy `RoutesActions`. Użyj klasy `RoleActions` i metody `createAdmin` jako przykładu wywołania metody `RegisterCustomRoutes` z pliku *Global.asax.cs* .

Może być również zauważone wywołanie metody `RegisterRoutes` przy użyciu obiektu `RouteConfig` na początku procedury obsługi zdarzeń `Application_Start`. To wywołanie jest wykonywane w celu zaimplementowania domyślnego routingu. Został on uwzględniony jako domyślny kod podczas tworzenia aplikacji przy użyciu szablonu formularzy sieci Web programu Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Pobieranie i używanie danych tras

Jak wspomniano powyżej, można zdefiniować trasy. Kod, który został dodany do programu obsługi zdarzeń `Application_Start` w pliku *Global.asax.cs* , ładuje możliwe do zdefiniowania trasy.

### <a name="setting-routes"></a>Ustawianie tras

Trasy wymagają dodania dodatkowego kodu. W tym samouczku użyjesz powiązania modelu, aby pobrać obiekt `RouteValueDictionary`, który jest używany podczas generowania tras przy użyciu danych z kontrolki danych. Obiekt `RouteValueDictionary` będzie zawierać listę nazw produktów należących do określonej kategorii produktów. Dla każdego produktu tworzony jest link na podstawie danych i trasy.

#### <a name="enable-routes-for-categories-and-products"></a>Włączanie tras dla kategorii i produktów

Następnie zaktualizujesz aplikację w taki sposób, aby używała `ProductsByCategoryRoute` do określenia właściwej trasy do uwzględnienia dla każdego łącza kategorii produktów. Zostanie również zaktualizowana strona *ProductList. aspx* , która będzie zawierać link kierowany dla każdego produktu. Linki będą wyświetlane tak, jakby znajdowały się przed zmianą, ale linki będą teraz używały routingu adresów URL.

1. W **Eksplorator rozwiązań**Otwórz stronę *site. Master* , jeśli nie została jeszcze otwarta.
2. Zaktualizuj formant **ListView** o nazwie "`categoryList`" zmianami wyróżnionymi kolorem żółtym, tak aby znaczniki pojawiły się w następujący sposób:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. W **Eksplorator rozwiązań**Otwórz stronę *ProductList. aspx* .
4. Zaktualizuj element `ItemTemplate` strony *ProductList. aspx* przy użyciu opcji aktualizacje wyróżnione kolorem żółtym, aby znaczniki pojawiły się w następujący sposób:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Otwórz kod związany z *ProductList.aspx.cs* i Dodaj następujący obszar nazw jako wyróżniony na żółto:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Zastąp metodę `GetProducts` kodu (*ProductList.aspx.cs*) następującym kodem:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Dodawanie kodu do szczegółów produktu

Teraz zaktualizuj kod w tle (*ProductDetails.aspx.cs*) dla strony *ProductDetails. aspx* , aby użyć danych tras. Należy zauważyć, że nowa metoda `GetProduct` również akceptuje wartość ciągu zapytania w przypadku, gdy użytkownik ma link z zakładką, który używa starszej nieprzyjaznego adresu URL bez routingu.

1. Zastąp metodę `GetProduct` kodu (*ProductDetails.aspx.cs*) następującym kodem:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Uruchamianie aplikacji

Teraz możesz uruchomić aplikację, aby zobaczyć zaktualizowane trasy.

1. Naciśnij klawisz **F5** , aby uruchomić przykładową aplikację Wingtip zabawki.  
 Zostanie otwarta przeglądarka i zostanie wyświetlona strona *default. aspx* .
2. Kliknij link **Products (produkty** ) w górnej części strony.  
 Wszystkie produkty są wyświetlane na stronie *ProductList. aspx* . Następujący adres URL (z numerem portu) jest wyświetlany dla przeglądarki:  
    `https://localhost:44300/ProductList`
3. Następnie kliknij link kategorii **samochody** w górnej części strony.  
 Na stronie *ProductList. aspx* są wyświetlane tylko samochody. Następujący adres URL (z numerem portu) jest wyświetlany dla przeglądarki:  
    `https://localhost:44300/Category/Cars`
4. Kliknij link zawierający nazwę pierwszego samochodu wymienionego na stronie ("**wymienialny samochód**"), aby wyświetlić szczegółowe informacje o produkcie.  
 Następujący adres URL (z numerem portu) jest wyświetlany dla przeglądarki:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Następnie wprowadź następujący adres URL bez routingu (przy użyciu numeru portu) do przeglądarki:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Kod nadal rozpoznaje adres URL, który zawiera ciąg zapytania, w przypadku, gdy użytkownik ma link oznaczony zakładką.

## <a name="summary"></a>Podsumowanie

W tym samouczku dodano trasy dla kategorii i produktów. Wiesz już, jak trasy mogą być zintegrowane z kontrolkami danych, które używają powiązania modelu. W następnym samouczku zostanie zaimplementowana globalna obsługa błędów.

## <a name="additional-resources"></a>Dodatkowe materiały

[Przyjazne adresy URL ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Wdróż bezpieczną aplikację ASP.NET Web Forms z członkostwem, uwierzytelnianiem OAuth i SQL Database, aby Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure — bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Poprzednie](membership-and-administration.md)
> [dalej](aspnet-error-handling.md)
