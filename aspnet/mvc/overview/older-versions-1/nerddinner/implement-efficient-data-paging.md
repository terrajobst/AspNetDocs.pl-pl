---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementowanie wydajnego stronicowania danych | Microsoft Docs
author: microsoft
description: Krok 8 pokazuje, jak dodać obsługę stronicowania do naszego adresu URL/Dinners, aby zamiast tego wyświetlić tysiące obiadów jednocześnie. w... zostanie wyświetlonych tylko 10 nadchodzących obiadów.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 2d9a0dba381b71755ac626f76d52bc5bcb434447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601053"
---
# <a name="implement-efficient-data-paging"></a>Implementowanie wydajnego stronicowania danych

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 8 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 8 pokazuje, jak dodać obsługę stronicowania do naszego/Dinnersego adresu URL, tak aby zamiast obsłużyć tysiące obiadów jednocześnie. w tym czasie będzie wyświetlana tylko 10 nadchodzących obiadów i umożliwienie użytkownikom końcowym przechodzenia do tyłu i do przodu przez całą listę w zrozumiały sposób.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-8-paging-support"></a>NerdDinner krok 8: Obsługa stronicowania

Jeśli witryna zostanie zakończona pomyślnie, będzie miała tysiące nadchodzących obiadów. Musimy upewnić się, że nasz interfejs użytkownika skaluje się, aby obsłużyć wszystkie te obiady, i umożliwia użytkownikom ich przeglądanie. Aby to umożliwić, dodamy obsługę stronicowania do naszego */Dinnersego* adresu URL, tak aby zamiast obsłużyć tysiące obiadów na raz, będzie wyświetlane tylko 10 nadchodzących obiadów w czasie i umożliwienie użytkownikom końcowym przechodzenia do tyłu i do przodu przez całą listę w przyjazny sposób.

### <a name="index-action-method-recap"></a>Index () — Metoda akcji podsumowanie

Metoda akcji index () w klasie DinnersController obecnie wygląda następująco:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Gdy żądanie zostanie wysłane do adresu URL */Dinners* , pobiera listę wszystkich nadchodzących obiadów, a następnie renderuje listę wszystkich z nich:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iqueryablelttgt"></a>Informacje o interfejsie IQueryable&lt;T&gt;

*IQueryable&lt;t&gt;* jest interfejsem, który został wprowadzony przy użyciu LINQ w ramach programu .NET 3,5. Umożliwia zaawansowane scenariusze "wykonywanie odroczone", z których możemy skorzystać, aby zaimplementować obsługę stronicowania.

W naszym DinnerRepository zwracamy sekwencję&gt;ą&lt;w interfejsie IQueryable z naszej metody FindUpcomingDinners ():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

&gt; obiektu IQueryable&lt;, który został zwrócony przez naszą metodę FindUpcomingDinners () hermetyzuje zapytanie w celu pobrania obiektów obiadu z naszej bazy danych przy użyciu LINQ to SQL. Ważne jest, aby nie wykonywać zapytania względem bazy danych, dopóki nie spróbujemy uzyskać dostępu/iteracji danych w zapytaniu lub do momentu wywołania metody ToList — (). Kod wywołujący naszą metodę FindUpcomingDinners () może opcjonalnie dodać dodatkowe "łańcuchowe" operacje/filtry do obiektu IQueryable&lt;&gt; obiekt przed wykonaniem zapytania. Program LINQ to SQL jest następnie odpowiednio inteligentny, aby wykonać połączone zapytanie względem bazy danych, gdy dane są żądane.

Aby zaimplementować logikę stronicowania, możemy zaktualizować metodę akcji indeksu DinnersController (), tak aby stosowała dodatkowe operatory "Skip" i "Take" do zwróconego poziomu obiadu IQueryable&lt;&gt; sekwencję przed wywołaniem ToList — ():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Powyższy kod pomija pierwsze 10 przyszłych obiadów w bazie danych, a następnie zwraca 20 obiadów. LINQ to SQL jest wystarczająco inteligentny, aby utworzyć zoptymalizowane zapytanie SQL, które wykonuje tę pomijaną logikę w bazie danych SQL — a nie na serwerze sieci Web. Oznacza to, że nawet jeśli mamy miliony nadchodzących obiadów w bazie danych, w ramach tego żądania zostanie pobrany tylko 10.

### <a name="adding-a-page-value-to-the-url"></a>Dodawanie wartości "page" do adresu URL

Zamiast twardo kodowanie określonego zakresu stron, chcemy, aby nasze adresy URL zawierały parametr "page" wskazujący, który zakres obiadu jest żądany przez użytkownika.

#### <a name="using-a-querystring-value"></a>Używanie wartości QueryString

Poniższy kod pokazuje, jak możemy zaktualizować metodę akcji index () w celu obsługi parametru QueryString i włączyć adresy URL, takie jak */Dinners? Page = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Powyższa metoda działania index () ma parametr o nazwie "page". Parametr jest zadeklarowany jako dopuszczający wartości null (czyli wartość int?). Oznacza to, że adres URL */Dinners? Page = 2* spowoduje przekazanie wartości "2" jako wartości parametru. Adres URL */Dinners* (bez wartości QueryString) spowoduje przekazanie wartości null.

Wartość strony jest mnożona przez rozmiar strony (w tym przypadku 10 wierszy), aby określić liczbę obiadów do pominięcia. Korzystamy z [ C# operatora "łączenia" o wartości null (??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) , który jest przydatny podczas pracy z typami dopuszczającymi wartość null. Powyższy kod przypisuje stronie wartość 0, jeśli parametr strony ma wartość null.

#### <a name="using-embedded-url-values"></a>Używanie wartości osadzonych adresów URL

Alternatywą dla użycia wartości QueryString jest osadzenie parametru Page w samym samym samym adresie URL. Na przykład: */Dinners/Page/2* lub */Dinners/2*. ASP.NET MVC zawiera wydajny aparat routingu URL, który ułatwia obsługę takich scenariuszy.

Możemy zarejestrować niestandardowe reguły routingu, które mapują dowolny przychodzący adres URL lub format adresu URL na dowolną klasę kontrolera lub metodę akcji. Wszystko, co należy zrobić, to otwarcie pliku Global. asax w naszym projekcie:

![](implement-efficient-data-paging/_static/image2.png)

A następnie zarejestrować nową regułę mapowania przy użyciu metody pomocnika plik maproute (), takiej jak pierwsze wywołanie tras. Plik maproute () poniżej:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Powyżej rejestrujemy nową regułę routingu o nazwie "UpcomingDinners". Wskazujemy, że ma format adresu URL "obiady/Strona/{Page}" – gdzie {Page} jest wartością parametru osadzoną w adresie URL. Trzeci parametr metody plik maproute () wskazuje, że należy zmapować adresy URL zgodne z tym formatem do metody akcji index () w klasie DinnersController.

Możemy użyć dokładnie tego samego kodu indeksu (), który był wcześniej z naszym scenariuszem QueryString — z wyjątkiem tego, że ten parametr "page" będzie pochodzący z adresu URL, a nie z ciągu QueryString:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Teraz, gdy uruchomimy aplikację i wpiszesz */Dinners* , zobaczymy pierwsze 10 nadchodzących obiadów:

![](implement-efficient-data-paging/_static/image3.png)

Po wpisaniu w */Dinners/Page/1* zobaczymy następną stronę obiadów:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Dodawanie interfejsu użytkownika nawigacji strony

Ostatnim krokiem, który należy wykonać w naszym scenariuszu stronicowania, będzie wdrożenie interfejsu użytkownika nawigacji "dalej" i "poprzedni" w naszym szablonie widoku, aby umożliwić użytkownikom łatwe pominięcie danych obiadu.

Aby poprawnie zaimplementować, należy znać łączną liczbę obiadów w bazie danych, a także liczbę stron danych, które są do niego przetłumaczyć. Następnie musimy obliczyć, czy aktualnie żądana wartość "page" znajduje się na początku, czy na końcu danych, i odpowiednio Pokazywanie lub ukrywanie interfejsu użytkownika "Previous" i "Next". Możemy zaimplementować tę logikę w ramach metody akcji index (). Alternatywnie możemy dodać do projektu klasę pomocnika, która hermetyzuje tę logikę w bardziej użyteczny sposób.

Poniżej znajduje się prosta Klasa pomocnika "PaginatedList", która pochodzi od klasy kolekcji&lt;T&gt; wbudowana w .NET Framework. Implementuje klasy kolekcji wielokrotnego użytku, która może służyć do stronicowania dowolnej sekwencji danych IQueryable. W naszej aplikacji NerdDinner będzie ona działać&lt;na Obiadie z&gt;ą, ale można ją łatwo wykorzystać w odniesieniu do IQueryable&lt;produktu&gt; lub IQueryable&lt;klienta&gt; wyniki w innych scenariuszach aplikacji:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Zwróć uwagę na to, jak ta wartość jest obliczana, a następnie ujawnia właściwości takie jak "PageIndex", "PageSize", "TotalCount" i "TotalPages". Udostępnia również dwie właściwości pomocnika "HasPreviousPage" i "HasNextPage", które wskazują, czy strona danych w kolekcji znajduje się na początku, czy na końcu oryginalnej sekwencji. Powyższy kod spowoduje uruchomienie dwóch zapytań SQL — pierwszy w celu pobrania licznika całkowitej liczby obiektów obiadu (nie zwraca obiektów — zamiast tego wykonuje instrukcję "SELECT COUNT", która zwraca liczbę całkowitą), a drugi do pobrania tylko wierszy dane, których potrzebujemy z bazy danych dla bieżącej strony danych.

Następnie możemy zaktualizować metodę pomocnika DinnersController. index () w celu utworzenia&gt;&lt;obiadu z naszego DinnerRepository. FindUpcomingDinners () i przekazać go do naszego szablonu widoku:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Następnie możemy zaktualizować szablon widoku \Views\Dinners\Index.aspx, aby dziedziczyć z ViewPage&lt;NerdDinner. Pomocnicys. PaginatedList&lt;obiad&gt;&gt; zamiast ViewPage&lt;IEnumerable&lt;, a następnie Dodaj następujący kod na dole naszego szablonu widoku, aby pokazać lub ukryć następny i poprzedni interfejs użytkownika nawigacji:&gt;&gt;

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Zwróć uwagę na to, jak używamy metody pomocnika html. RouteLink () do generowania hiperłączy. Ta metoda jest podobna do metody pomocnika html. ActionLink (), która została wcześniej użyta. Różnica polega na tym, że generujemy adres URL przy użyciu reguły routingu "UpcomingDinners" skonfigurowanej w naszym pliku Global. asax. Gwarantuje to, że będziemy generować adresy URL do naszej metody akcji index (), która ma format: */Dinners/Page/{Page}* — gdzie wartość {Page} to zmienna dostarczana powyżej na podstawie bieżącego wartości pageIndex.

A teraz po ponownym uruchomieniu aplikacji zobaczymy 10 obiadów w czasie w naszej przeglądarce:

![](implement-efficient-data-paging/_static/image5.png)

Mamy również &lt;&lt;&lt; i &gt;&gt;&gt; interfejs użytkownika nawigacji w dolnej części strony, który pozwala na pomijanie przekazywania danych do przodu i do tyłu przez dane za pomocą dostępnego dla aparatu wyszukiwania adresów URL:

![](implement-efficient-data-paging/_static/image6.png)

| **Temat po stronie: zrozumienie skutków platformy IQueryable&lt;T&gt;** |
| --- |
| Interfejs IQueryable&lt;T&gt; to bardzo wydajna funkcja, która umożliwia korzystanie z różnorodnych, odroczonych scenariuszy wykonywania (na przykład kwerend opartych na stronicowaniu i kompozycji). Podobnie jak w przypadku wszystkich zaawansowanych funkcji, należy zachować ostrożność przy ich użyciu i upewnić się, że nie są one nadmiarowe. Ważne jest, aby rozpoznać, że zwrócenie danych z usługi IQueryable&lt;T&gt; z repozytorium umożliwia wywoływanie kodu w celu dołączenia do niego powiązanych z nią metod operatora i dlatego należy wziąć udział w ostatecznym wykonaniu zapytania. Jeśli nie chcesz podawać kodu wywołującego tę możliwość, należy zwrócić wartość IList&lt;T&gt; lub IEnumerable&lt;T&gt; wyniki, które zawierają wyniki zapytania, które zostało już wykonane. W przypadku scenariuszy stronicowania wymaga to wypchnięcia rzeczywistej logiki stronicowania danych do wywoływanej metody repozytorium. W tym scenariuszu możemy zaktualizować metodę wyszukiwania FindUpcomingDinners () w celu uzyskania sygnatury, która zwróciła PaginatedList: PaginatedList&lt; obiad&gt; FindUpcomingDinners (int pageIndex, int pageSize) {} lub Wróć z powrotem do elementu IList&lt;obiad&gt;i użyj parametru out "totalCount", aby zwrócić łączną liczbę kolacji: IList&lt;obiad&gt; FindUpcomingDinners (int pageIndex, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Następny krok

Teraz przyjrzyjmy się sposobom dodawania obsługi uwierzytelniania i autoryzacji do naszej aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](re-use-ui-using-master-pages-and-partials.md)
> [dalej](secure-applications-using-authentication-and-authorization.md)
