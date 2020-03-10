---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Przegląd widoków ASP.NET MVC (C#) | Microsoft Docs
author: StephenWalther
description: Co to jest widok ASP.NET MVC i czym się różni od strony HTML? W tym samouczku Stephen Walther wprowadza do widoków i demonstruje, jak można t...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: b3f44aa9654a2a718381eaf9c856ca3e15ed1e27
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600318"
---
# <a name="aspnet-mvc-views-overview-c"></a>Omówienie widoków ASP.NET MVC (C#)

Autor [Stephen Walther](https://github.com/StephenWalther)

> Co to jest widok ASP.NET MVC i czym się różni od strony HTML? W tym samouczku Stephen Walther wprowadza do widoków i demonstruje, jak można wykorzystać możliwości wyświetlania danych i pomocników HTML w widoku.

Celem tego samouczka jest zapewnienie krótkiego wprowadzenia do ASP.NET widoków MVC, wyświetlania danych i pomocników HTML. Na końcu tego samouczka należy zrozumieć, jak tworzyć nowe widoki, przekazywać dane z kontrolera do widoku i używać pomocników HTML do generowania zawartości w widoku.

## <a name="understanding-views"></a>Informacje o widokach

W przypadku stron ASP.NET lub Active Server ASP.NET MVC nie obejmuje wszystkich elementów, które bezpośrednio odnoszą się do strony. W aplikacji ASP.NET MVC nie istnieje strona na dysku odpowiadająca ścieżce w adresie URL, który można wpisać na pasku adresu przeglądarki. Najbliższy element strony w aplikacji ASP.NET MVC to coś o nazwie *View*.

W aplikacji ASP.NET MVC żądania przeglądarki przychodzącej są mapowane na akcje kontrolera. Akcja kontrolera może zwrócić widok. Jednak akcja kontrolera może wykonać inny typ akcji, na przykład przekierowywanie do innej akcji kontrolera.

Lista 1 zawiera prosty kontroler o nazwie HomeController. HomeController uwidacznia dwie akcje kontrolera o nazwie index () i Details ().

**Lista 1 — HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

Możesz wywołać pierwszą akcję, indeks (), wpisując następujący adres URL na pasku adresu przeglądarki:

/Home/Index

Możesz wywołać drugą akcję, szczegóły (), wpisując ten adres w przeglądarce:

/Home/Details

Akcja index () zwraca widok. Większość akcji, które tworzysz, będzie zwracać widoki. Jednak akcja może zwracać inne typy wyników akcji. Na przykład akcja szczegóły () zwraca RedirectToActionResult, która przekierowuje żądanie przychodzące do akcji index ().

Akcja index () zawiera następujący pojedynczy wiersz kodu:

View();

Ten wiersz kodu zwraca widok, który musi znajdować się w następującej ścieżce na serwerze sieci Web:

\Views\Home\Index.aspx

Ścieżka do widoku jest wywnioskowana na podstawie nazwy kontrolera i nazwy akcji kontrolera.

Jeśli wolisz, możesz być jawne o widoku. Następujący wiersz kodu zwraca widok o nazwie Fred:

Widok (Fred);

Po wykonaniu tego wiersza kodu jest zwracany widok z następującej ścieżki:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Jeśli planujesz utworzyć testy jednostkowe dla aplikacji ASP.NET MVC, dobrym pomysłem jest jawne informacje o nazwach widoków. W ten sposób można utworzyć test jednostkowy, aby sprawdzić, czy oczekiwany widok został zwrócony przez akcję kontrolera.

## <a name="adding-content-to-a-view"></a>Dodawanie zawartości do widoku

Widok to standardowy dokument HTML (X), który może zawierać skrypty. Za pomocą skryptów można dodać zawartość dynamiczną do widoku.

Na przykład widok na liście 2 wyświetla bieżącą datę i godzinę.

**Lista 2 — \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

Zwróć uwagę, że treść strony HTML na liście 2 zawiera następujący skrypt:

&lt;% Response. Write (DateTime. Now);%&gt;

Używasz ograniczników skryptów &lt;% i%&gt; do oznaczania początku i końca skryptu. Ten skrypt jest zapisywana C#. Wyświetla bieżącą datę i godzinę, wywołując metodę Response. Write () w celu renderowania zawartości w przeglądarce. Ograniczniki skryptu &lt;% i%&gt; mogą służyć do wykonywania jednej lub kilku instrukcji.

Od momentu wywołania metody Response. Write (), firma Microsoft udostępnia skrót do wywołania metod Response. Write (). Widok w liście 3 używa ograniczników &lt;% = i%&gt; jako skrót do wywołania metody Response. Write ().

**Lista 3 — Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

Aby wygenerować zawartość dynamiczną w widoku, można użyć dowolnego języka platformy .NET. Zwykle używa się Visual Basic .NET lub C# do zapisywania kontrolerów i widoków.

## <a name="using-html-helpers-to-generate-view-content"></a>Generowanie zawartości widoku przy użyciu pomocników HTML

Aby ułatwić Dodawanie zawartości do widoku, możesz skorzystać z czegoś " *pomocnik HTML*". Pomocnik HTML, zazwyczaj jest metodą, która generuje ciąg. Pomocników HTML można użyć do wygenerowania standardowych elementów HTML, takich jak pola tekstowe, linki, listy rozwijane i pola listy.

Na przykład widok z listy 4 wykorzystuje trzy pomocnicy HTML — BeginForm (), pole tekstowe () i hasło () — aby wygenerować formularz logowania (patrz rysunek 1).

**Lista 4 — \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]

[![okno dialogowe Nowy projekt](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**Ilustracja 01**: standardowy formularz logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-views-overview-cs/_static/image2.png))

Wszystkie metody pomocników HTML są wywoływane we właściwości HTML widoku. Na przykład, możesz renderować pole tekstowe przez wywołanie metody html. TextBox ().

Zwróć uwagę, że używasz ograniczników skryptów &lt;% = i%&gt; podczas wywoływania zarówno pomocników html. TextBox (), jak i HTML. Password (). Ci pomocnicy po prostu zwracają ciąg. Musisz wywołać metodę Response. Write () w celu renderowania ciągu do przeglądarki.

Korzystanie z metod pomocników HTML jest opcjonalne. Zwiększają one życie, zmniejszając ilość kodu HTML i skryptów, które należy napisać. Widok na liście 5 renderuje dokładnie ten sam formularz co widok na liście 4 bez użycia pomocników HTML.

**Lista 5 — \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

Dostępna jest również opcja tworzenia własnych pomocników HTML. Na przykład można utworzyć metodę pomocnika GridView (), która wyświetla zestaw rekordów bazy danych w tabeli HTML automatycznie. W samouczku opisano **Tworzenie niestandardowych pomocników HTML**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Używanie danych widoku do przekazywania danych do widoku

Użyj widoku dane, aby przekazać dane z kontrolera do widoku. Pomyśl o wyświetlaniu danych, takich jak pakiet wysyłany za pomocą poczty e-mail. Wszystkie dane przekazywane z kontrolera do widoku muszą być wysyłane przy użyciu tego pakietu. Na przykład kontroler na liście 6 dodaje komunikat, aby wyświetlić dane.

**Lista 6 — ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

Właściwość ViewData kontrolera reprezentuje kolekcję par nazw i wartości. W liście 6 Metoda index () dodaje element do kolekcji danych widoku o nazwie Message o wartości Hello world!. Gdy widok jest zwracany przez metodę index (), dane widoku są automatycznie przesyłane do widoku.

Widok z listy 7 pobiera komunikat z danych widoku i renderuje komunikat do przeglądarki.

**Lista 7 — \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Należy zauważyć, że widok wykorzystuje metodę pomocnika html. Encode () podczas renderowania komunikatu. Pomocnik html. Encode () koduje znaki specjalne, takie jak &lt; i &gt;, do znaków, które są bezpieczne do wyświetlenia na stronie sieci Web. Zawsze podczas renderowania zawartości przesyłanej przez użytkownika do witryny sieci Web należy zakodować zawartość, aby zapobiec atakom z użyciem kodu JavaScript.

(Ponieważ utworzyliśmy komunikat wypróbujemy w ProductController, nie jest naprawdę konieczne zakodowanie komunikatu. Jednak dobrym wykonywaćem jest zawsze wywoływanie metody html. Encode () podczas wyświetlania zawartości pobranej z widoku danych w widoku.

W przypadku wystawiania 7 korzystamy z funkcji Wyświetl dane w celu przekazania prostego komunikatu ciągu z kontrolera do widoku. Można także użyć widoku dane do przekazania innych typów danych, takich jak kolekcja rekordów bazy danych, z kontrolera do widoku. Na przykład, jeśli chcesz wyświetlić zawartość tabeli bazy danych produktów w widoku, należy przekazać kolekcję rekordów bazy danych w widoku dane.

Dostępna jest również opcja przekazywania danych o jednoznacznie określonym typie z kontrolera do widoku. Ten temat jest omawiany w samouczku **Omówienie jednoznacznie wpisanych danych i widoków widoku**.

## <a name="summary"></a>Podsumowanie

Ten samouczek zawiera krótkie wprowadzenie do ASP.NET widoków MVC, wyświetlania danych i pomocników HTML. W pierwszej sekcji przedstawiono sposób dodawania nowych widoków do projektu. Wiesz, że musisz dodać widok do odpowiedniego folderu, aby wywołać go z określonego kontrolera. Następnie omówiono temat pomocników HTML. Wiesz, jak pomocniki HTML umożliwiają łatwe generowanie standardowej zawartości HTML. Na koniec wiesz już, jak korzystać z danych widoku do przekazywania danych z kontrolera do widoku.

> [!div class="step-by-step"]
> [Dalej](creating-custom-html-helpers-cs.md)
