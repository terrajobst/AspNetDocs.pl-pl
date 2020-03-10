---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Wprowadzenie do stron sieci Web ASP.NET — podstawowe informacje o formularzach HTML | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku przedstawiono podstawowe informacje dotyczące sposobu tworzenia formularza wejściowego i sposobu obsługi danych wejściowych użytkownika podczas korzystania z ASP.NET stron sieci Web (Razor). A teraz...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f57661077ec3bb13f3d4ec41b130bda4d2fb9070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574285"
---
# <a name="introducing-aspnet-web-pages---html-form-basics"></a>Wprowadzenie do stron sieci Web ASP.NET — podstawy formularzy HTML

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym samouczku przedstawiono podstawowe informacje dotyczące sposobu tworzenia formularza wejściowego i sposobu obsługi danych wejściowych użytkownika podczas korzystania z ASP.NET stron sieci Web (Razor). Teraz, gdy już masz bazę danych, będziesz używać swoich umiejętności, aby umożliwić użytkownikom znalezienie określonych filmów w bazie danych. Przyjęto założenie, że została ukończona seria przez [wprowadzenie do wyświetlania danych przy użyciu stron sieci Web ASP.NET](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Zawartość:
> 
> - Jak utworzyć formularz przy użyciu standardowych elementów HTML.
> - Jak odczytać dane wejściowe użytkownika w formularzu.
> - Jak utworzyć zapytanie SQL, które selektywnie pobiera dane przy użyciu wyszukiwanego terminu przez użytkownika.
> - Jak mieć pola na stronie "Zapamiętaj" wprowadzone przez użytkownika.
>   
> 
> Omówione funkcje/technologie:
> 
> - Obiekt `Request`.
> - Klauzula `Where` SQL.

## <a name="what-youll-build"></a>Co będziesz kompilować

W poprzednim samouczku została utworzona baza danych, do której dodano dane, a następnie użyto pomocnika `WebGrid`, aby wyświetlić dane. W tym samouczku dodasz pole wyszukiwania, które umożliwia znalezienie filmów określonego gatunku lub tytuł zawiera wszelkie wprowadzane słowa. (Na przykład można znaleźć wszystkie filmy, których gatunek jest "Action" lub którego tytuł zawiera "Harry" lub "Adventure").

Gdy skończysz korzystać z tego samouczka, zobaczysz stronę podobną do tej:

![Strona filmów z gatunkiem i wyszukiwaniem tytułu](form-basics/_static/image1.png)

Część wystawiania strony jest taka sama jak w ostatnim samouczku &mdash; siatki. Różnica polega na tym, że siatka będzie zawierać tylko te filmy, które zostały wyszukane.

## <a name="about-html-forms"></a>Informacje o formularzach HTML

(Jeśli masz doświadczenie w tworzeniu formularzy HTML i z różnicą między `GET` i `POST`, możesz pominąć tę sekcję).

Formularz zawiera elementy danych wejściowych użytkownika &mdash; pola tekstowe, przyciski, przyciski radiowe, pola wyboru, listy rozwijane itd. Użytkownicy wypełniają te kontrolki lub dokonają wyboru, a następnie przesyłają formularz, klikając przycisk.

Podstawowa składnia HTML formularza ilustruje następujący przykład:

[!code-html[Main](form-basics/samples/sample1.html)]

Po uruchomieniu tego znacznika na stronie tworzy prosty formularz, który wygląda podobnie do poniższej ilustracji:

![Podstawowa postać HTML zgodna z renderowaniem w przeglądarce](form-basics/_static/image2.png)

Element `<form>` obejmuje elementy HTML do przesłania. (Łatwa do zaniechania należy dodać elementy do strony, a następnie zapomnieć, aby umieścić je wewnątrz elementu `<form>`. W takim przypadku nic nie zostanie przesłane. Atrybut `method` informuje przeglądarkę o sposobie przesyłania danych wejściowych użytkownika. Ta wartość jest ustawiana na `post` w przypadku przeprowadzania aktualizacji na serwerze lub `get`, jeśli właśnie pobierasz dane z serwera.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **Bezpieczeństwo zlecenia GET, POST i HTTP**
> 
> HTTP, protokół używany przez przeglądarki i serwery do wymiany informacji, jest niezwykle prosty w swoich podstawowych operacjach. Przeglądarki używają tylko kilku czasowników do żądania serwerów. Podczas pisania kodu dla sieci Web warto zrozumieć te czasowniki oraz sposób ich używania przez przeglądarkę i serwer. Najczęściej używane czasowniki są następujące:
> 
> - `GET`. Przeglądarka używa tego zlecenia do pobrania elementu z serwera. Na przykład po wpisaniu adresu URL w przeglądarce przeglądarka wykonuje `GET` operacji, aby zażądać żądanej strony. Jeśli strona zawiera grafikę, przeglądarka wykonuje dodatkowe operacje `GET`, aby pobrać obrazy. Jeśli operacja `GET` musi przekazać informacje do serwera, informacje są przekazywane jako część adresu URL w ciągu zapytania.
> - `POST`. Przeglądarka wysyła żądanie `POST` w celu przesłania danych do dodania lub zmiany na serwerze. Na przykład zlecenie `POST` służy do tworzenia rekordów w bazie danych lub zmieniania istniejących. W większości przypadków, gdy wypełnisz formularz i klikniesz przycisk Prześlij, przeglądarka wykonuje operację `POST`. W operacji `POST` dane przesyłane do serwera są w treści strony.
> 
> Istotną różnicą między tymi zleceniami jest to, że operacja `GET` nie powinna być zmieniana na serwerze, czy też w nieco bardziej abstrakcyjny sposób operacja `GET` nie spowoduje zmiany stanu na serwerze. Możesz wykonać `GET` operacji na tych samych zasobach tyle razy, ile chcesz, i te zasoby nie zmienią się. (`GET` operacja jest często określana jako "bezpieczna" lub do korzystania z warunków technicznych jest *idempotentne*). Z kolei oczywiście żądanie `POST` zmienia coś na serwerze za każdym razem, gdy wykonujesz operację.
> 
> Dwa przykłady ułatwią zilustrowanie tego rozróżnienia. Podczas wyszukiwania przy użyciu aparatu, takiego jak Bing lub Google, należy wypełnić formularz, który składa się z jednego pola tekstowego, a następnie kliknąć przycisk wyszukiwania. Przeglądarka wykonuje `GET` operacji z wartością wprowadzoną w polu przekazaną w ramach adresu URL. Używanie `GET` operacji dla tego typu formularza jest prawidłowy, ponieważ operacja wyszukiwania nie zmienia żadnych zasobów na serwerze, po prostu pobiera informacje.
> 
> Teraz Rozważmy proces porządkowania elementu w trybie online. Wypełnij pola szczegóły zamówienia, a następnie kliknij przycisk Prześlij. Ta operacja będzie `POST` żądanie, ponieważ operacja spowoduje zmiany na serwerze, takie jak nowy rekord zamówienia, zmiana informacji o koncie i prawdopodobnie wiele innych zmian. W przeciwieństwie do operacji `GET` nie można powtórzyć żądania `POST` — w przypadku, gdy żądanie zostało wysłane ponownie, zostanie wygenerowane nowe zamówienie na serwerze. (W takich przypadkach witryny sieci Web często ostrzegają, aby nie klikać przycisku przesyłania więcej niż jeden raz lub wyłączyli przycisk Prześlij, aby nie ponownie przesłać formularza przypadkowo).
> 
> W ramach tego samouczka użyjesz operacji `GET` i `POST` operacji do pracy z formularzami HTML. W każdym przypadku wyjaśnimy, dlaczego używane zlecenie są odpowiednie.
> 
> (Aby dowiedzieć się więcej na temat czasowników HTTP, zobacz artykuł [definicje metod](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) w witrynie W3C).

Większość elementów wejściowych użytkownika jest `<input>` elementów HTML. Wyglądają tak jak `<input type="type" name="name">,` gdzie *Type* wskazuje rodzaj kontroli danych wejściowych użytkownika. Te elementy są typowymi:

- Pole tekstowe: `<input type="text">`
- Pole wyboru: `<input type="check">`
- Przycisk radiowy: `<input type="radio">`
- Przycisk: `<input type="button">`
- Przycisk Prześlij: `<input type="submit">`

Można również użyć elementu `<textarea>` do utworzenia wielowierszowego pola tekstowego i elementu `<select>`, aby utworzyć listę rozwijaną lub przewijaną listę. (Aby uzyskać więcej informacji na temat elementów formularza HTML, zobacz [formularze HTML i dane wejściowe](http://www.w3schools.com/html/html_forms.asp) w witrynie w3schools).

Atrybut `name` jest bardzo istotny, ponieważ jest to sposób, w jaki otrzymasz wartość elementu później, jak zobaczysz szybko.

Interesująca część to to, co ty, programista strony, na dane wejściowe użytkownika. Nie ma wbudowanego zachowania skojarzonego z tymi elementami. Zamiast tego należy pobrać wartości wprowadzone lub wybrane przez użytkownika i zrobić coś z nimi. To już poznasz w tym samouczku.

> [!TIP] 
> 
> **Formularze HTML5 i dane wejściowe**
> 
> Jak wiadomo, kod HTML jest w stanie przejścia, a Najnowsza wersja (HTML5) obejmuje obsługę bardziej intuicyjnych sposobów wprowadzania informacji przez użytkowników. Na przykład w języku HTML5 użytkownik (deweloper strony) może powiedzieć stronie, że użytkownik ma wprowadzić datę. Przeglądarka może następnie automatycznie wyświetlać kalendarz, a nie wymagać od użytkownika ręcznego wprowadzania daty. Jednak program HTML5 jest nowy i nie jest jeszcze obsługiwany we wszystkich przeglądarkach.
> 
> Strony sieci Web ASP.NET obsługują dane wejściowe HTML5 w tym zakresie, w jakim działa przeglądarka użytkownika. Aby uzyskać pomysł dotyczący nowych atrybutów elementu `<input>` w języku HTML5, zobacz [atrybut typu danych wejściowych&gt; HTML &lt;](http://www.w3schools.com/html/html_form_input_types.asp) w witrynie w3schools.

## <a name="creating-the-form"></a>Tworzenie formularza

W programie WebMatrix w obszarze roboczym **pliki** Otwórz stronę *filmy. cshtml* .

Po tagu `</h1>` zamykającym i przed tagiem otwierającym `<div>` wywołania `grid.GetHtml` Dodaj następujący znacznik:

[!code-html[Main](form-basics/samples/sample2.html)]

Ten znacznik tworzy formularz, który ma pole tekstowe o nazwie `searchGenre` i przycisk Prześlij. Pole tekstowe i przycisk Prześlij są ujęte w `<form>` elementu, którego atrybut `method` jest ustawiony na `get`. (Pamiętaj, że jeśli nie umieścisz pola tekstowego i przycisk Prześlij wewnątrz elementu `<form>`, nic nie zostanie przesłane po kliknięciu przycisku). Tutaj możesz użyć zlecenia `GET`, ponieważ tworzysz formularz, który nie wprowadza żadnych zmian na serwerze — tylko spowoduje to wyszukanie. (W poprzednim samouczku użyto metody `post`, która umożliwia przesyłanie zmian na serwerze. Zobaczysz to w następnym samouczku ponownie.

Uruchom stronę. Mimo że nie zdefiniowano żadnego zachowania dla formularza, można zobaczyć, co wygląda następująco:

![Strona filmów z polem wyszukiwania dla gatunku](form-basics/_static/image3.png)

Wprowadź wartość w polu tekstowym, na przykład "komedia". Następnie kliknij pozycję **kryterium wyszukiwania**.

Zwróć uwagę na adres URL strony. Ponieważ atrybut `method` elementu `<form>` jest ustawiany na `get`, wprowadzona wartość jest teraz częścią ciągu zapytania w adresie URL, na przykład:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Odczytywanie wartości formularza

Strona zawiera już kod, który pobiera dane bazy danych i wyświetla wyniki w postaci siatki. Teraz musisz dodać kod, który odczytuje wartość pola tekstowego, aby można było uruchomić zapytanie SQL zawierające termin wyszukiwania.

Ponieważ ustawiamy metodę `get`, można odczytać wartość wprowadzoną w polu tekstowym przy użyciu kodu w następujący sposób:

`var searchTerm = Request.QueryString["searchGenre"];`

Obiekt `Request.QueryString` (Właściwość `QueryString` obiektu `Request`) zawiera wartości elementów, które zostały przesłane jako część operacji `GET`. Właściwość `Request.QueryString` zawiera *kolekcję* (listę) wartości, które są przesyłane w formularzu. Aby uzyskać indywidualną wartość, należy określić nazwę żądanego elementu. Dlatego należy mieć atrybut `name` w elemencie `<input>` (`searchTerm`), który tworzy pole tekstowe. (Aby uzyskać więcej informacji na temat obiektu `Request`, zobacz [pasek boczny](#BKMK_TheRequestObject) później).

Jest to bardzo proste, aby odczytać wartość pola tekstowego. Ale jeśli użytkownik nie wprowadził niczego w polu tekstowym, ale zamiast tego kliknął opcję **Wyszukaj** , możesz zignorować to kliknięcie, ponieważ nie ma niczego do wyszukania.

Poniższy kod jest przykładem, który pokazuje, jak zaimplementować te warunki. (Nie musisz jeszcze dodawać tego kodu; możesz to zrobić w chwilę).

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Test jest podzielony w dół w następujący sposób:

- Pobierz wartość `Request.QueryString["searchGenre"]`, a mianowicie wartość wprowadzoną w elemencie `<input>` o nazwie `searchGenre`.
- Sprawdź, czy jest on pusty przy użyciu metody `IsEmpty`. Ta metoda jest standardowym sposobem określenia, czy element (na przykład element formularza) zawiera wartość. Jednak naprawdę należy zadbać o to, aby *nie* były puste...
- Dodaj operator `!` przed testem `IsEmpty`. (Operator `!` oznacza wartość logiczną NOT).

W zwykłym języku angielskim cały warunek `if` jest tłumaczony na następujące: *Jeśli element searchGenre formularza nie jest pusty, a następnie...*

Ten blok ustawia etap tworzenia zapytania, które korzysta z wyszukiwanego terminu. Należy to zrobić w następnej sekcji.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **Obiekt żądania**
> 
> Obiekt `Request` zawiera wszystkie informacje wysyłane przez przeglądarkę do aplikacji po zażądaniu lub przesłaniu strony. Ten obiekt zawiera wszystkie informacje udostępniane przez użytkownika, takie jak wartości pól tekstowych lub pliki do przekazania. Obejmuje ona również wszystkie informacje dodatkowe, takie jak pliki cookie, wartości w ciągu zapytania adresu URL (jeśli istnieją), ścieżka pliku strony, która jest uruchomiona, typ przeglądarki używanej przez użytkownika, listę języków ustawionych w przeglądarce i wiele innych.
> 
> Obiekt `Request` jest *kolekcją* (listą) wartości. Otrzymasz pojedynczą wartość z kolekcji, określając jej nazwę:
> 
> `var someValue = Request["name"];`
> 
> Obiekt `Request` rzeczywiście uwidacznia kilka podzestawów. Na przykład:
> 
> - `Request.Form` daje wartości z elementów wewnątrz przesłanego `<form>` elementu, jeśli żądanie jest żądaniem `POST`.
> - `Request.QueryString` daje tylko wartości w ciągu zapytania adresu URL. (W adresie URL, na przykład `http://mysite/myapp/page?searchGenre=action&page=2`, sekcja `?searchGenre=action&page=2` adresu URL jest ciągiem zapytania).
> - Kolekcja `Request.Cookies` zapewnia dostęp do plików cookie wysłanych przez przeglądarkę.
> 
> Aby uzyskać wartość, którą znasz, możesz użyć `Request["name"]`. Alternatywnie można użyć bardziej konkretnych wersji `Request.Form["name"]` (dla żądań `POST`) lub `Request.QueryString["name"]` (dla żądań `GET`). Oczywiście *Nazwa* to nazwa elementu, który ma zostać pobrany.
> 
> Nazwa elementu, który ma zostać pobrany, musi być unikatowa w obrębie używanej kolekcji. Dlatego, że obiekt `Request` dostarcza podzestawy, takich jak `Request.Form` i `Request.QueryString`. Załóżmy, że strona zawiera element formularza o nazwie `userName` i zawiera *również* plik cookie o nazwie `userName`. Jeśli uzyskasz `Request["userName"]`, jest to niejednoznaczne, czy chcesz, aby wartość formularza lub plik cookie. Jeśli jednak uzyskasz `Request.Form["userName"]` lub `Request.Cookie["userName"]`, masz wyraźne informacje o tym, którą wartość uzyskać.
> 
> Dobrym sposobem jest zastosowanie i użycie podzbioru interesujących Cię `Request`, takich jak `Request.Form` lub `Request.QueryString`. W przypadku prostych stron, które tworzysz w tym samouczku, prawdopodobnie nie spowoduje to w praktyce żadnej różnicy. Jednak podczas tworzenia bardziej skomplikowanych stron przy użyciu jawnej wersji `Request.Form` lub `Request.QueryString` mogą pomóc uniknąć problemów, które mogą wystąpić, gdy strona zawiera formularz (lub wiele formularzy), pliki cookie, wartości ciągów zapytań itd.

## <a name="creating-a-query-by-using-a-search-term"></a>Tworzenie zapytania przy użyciu terminu wyszukiwania

Teraz, gdy wiesz, jak uzyskać termin wyszukiwania wprowadzony przez użytkownika, możesz utworzyć zapytanie, które go używa. Należy pamiętać, że aby pobrać wszystkie elementy filmu z bazy danych, używasz zapytania SQL, które wygląda następująco:

`SELECT * FROM Movies`

Aby uzyskać tylko niektóre filmy, należy użyć zapytania zawierającego klauzulę `Where`. Ta klauzula umożliwia ustawienie warunku, w którym wiersze są zwracane przez zapytanie. Oto przykład:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Format podstawowy jest `WHERE column = value`. Możesz użyć różnych operatorów oprócz tylko `=`, takich jak `>` (większe niż), `<` (mniejsze niż), `<>` (nie równe), `<=` (mniejsze niż lub równe) itp., w zależności od tego, czego szukasz.

W przypadku, gdy zastanawiasz się, w instrukcji SQL nie jest rozróżniana wielkość liter &mdash; `SELECT` jest taka sama jak `Select` (lub nawet `select`). Jednak osoby często używają słów kluczowych w instrukcji SQL, takich jak `SELECT` i `WHERE`, aby ułatwić ich odczytywanie.

### <a name="passing-the-search-term-as-a-parameter"></a>Przekazywanie terminu wyszukiwania jako parametru

Wyszukiwanie określonego gatunku jest wystarczająco proste (`WHERE Genre = 'Action'`), ale chcesz mieć możliwość wyszukiwania dowolnego gatunku wprowadzonego przez użytkownika. W tym celu utworzysz jako kwerendę SQL, która zawiera symbol zastępczy dla wartości do wyszukania. To polecenie będzie wyglądać następująco:

`SELECT * FROM Movies WHERE Genre = @0`

Symbol zastępczy jest znakiem `@`, po którym następuje zero. Jak można odgadnąć, zapytanie może zawierać wiele symboli zastępczych i mieć nazwę `@0`, `@1`, `@2`itd.

Aby skonfigurować zapytanie i faktycznie przekazać je do wartości, należy użyć następującego kodu:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Ten kod jest podobny do tego, co zostało już zrobione w celu wyświetlenia danych w siatce. Jedyne różnice:

- Zapytanie zawiera symbol zastępczy (`WHERE Genre = @0"`).
- Zapytanie jest umieszczane w zmiennej (`selectCommand`); wcześniej zapytanie zostało przesłane bezpośrednio do metody `db.Query`.
- Po wywołaniu metody `db.Query` należy przekazać zarówno zapytanie, jak i wartość do użycia dla symbolu zastępczego. (Jeśli zapytanie miało wiele symboli zastępczych, należy przekazać je wszystkie jako oddzielne wartości do metody).

Jeśli umieścisz wszystkie te elementy razem, uzyskasz następujący kod:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Ważne!** Używanie symboli zastępczych (takich jak `@0`) do przekazywania wartości do polecenia SQL jest *niezwykle ważne* dla bezpieczeństwa. Sposób, w jaki zobaczysz, przy użyciu symboli zastępczych dla danych zmiennych, jest jedynym sposobem konstruowania poleceń SQL.
> 
> Nie należy tworzyć instrukcji języka SQL przez umieszczenie razem (łączenie) tekstu literału i wartości pobieranych od użytkownika. Połączenie danych wprowadzonych przez użytkownika do instrukcji SQL powoduje otwarcie witryny programu w celu uzyskania ataku polegającego na *iniekcji kodu SQL* , gdy złośliwy użytkownik przesyła wartości do strony, która nadaje się do złamania bazy danych. (Więcej informacji można znaleźć w artykule [iniekcja kodu SQL](https://msdn.microsoft.com/library/ms161953.aspx) w witrynie MSDN).

## <a name="updating-the-movies-page-with-search-code"></a>Aktualizowanie strony filmów przy użyciu kodu wyszukiwania

Teraz można zaktualizować kod w pliku *Films. cshtml* . Aby rozpocząć, Zastąp kod w bloku kodu w górnej części strony tym kodem:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

Różnica polega na tym, że kwerenda została umieszczona w zmiennej `selectCommand`, co zostanie przekazane do `db.Query` później. Umieszczenie instrukcji SQL w zmiennej umożliwia zmianę instrukcji, co jest wykonywane w celu przeprowadzenia wyszukiwania.

Zostały również usunięte te dwa wiersze, które zostaną umieszczone później:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Nie chcesz jeszcze uruchamiać zapytania (oznacza to, że Wywołaj `db.Query`) i nie chcesz już inicjować pomocnika `WebGrid`. Te czynności należy wykonać po rozpoczęciu wykonywania instrukcji języka SQL.

Po tym zapisaniu można dodać nową logikę do obsługi wyszukiwania. Ukończony kod będzie wyglądać podobnie do poniższego. Zaktualizuj kod na stronie, aby pasował do tego przykładu:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

Ta strona działa teraz tak. Za każdym razem, gdy strona jest uruchamiana, kod otwiera bazę danych, a zmienna `selectCommand` jest ustawiona na instrukcję SQL, która pobiera wszystkie rekordy z tabeli `Movies`. Kod inicjuje również zmienną `searchTerm`.

Jeśli jednak bieżące żądanie zawiera wartość dla elementu `searchGenre`, kod ustawia `selectCommand` na inne zapytanie — a mianowicie, na taki, który zawiera klauzulę `Where`, aby wyszukać gatunek. Ustawia również `searchTerm` do dowolnego elementu, który został przekazano do pola wyszukiwania (co może nie mieć wartości Nothing).

Bez względu na to, która instrukcja SQL znajduje się w `selectCommand`, kod następnie wywołuje `db.Query`, aby uruchomić zapytanie, przekazując je w `searchTerm`. Jeśli nie ma nic w `searchTerm`, nie ma znaczenia, ponieważ w takim przypadku nie istnieje parametr do przekazania wartości do `selectCommand` mimo to.

Na koniec kod inicjuje pomocnika `WebGrid` przy użyciu wyników zapytania, podobnie jak przed.

Możesz zobaczyć, że przez umieszczenie instrukcji SQL i wyszukiwanego terminu w zmiennych, dodaliśmy elastyczność do kodu. Jak widać w dalszej części tego samouczka, możesz użyć tej podstawowej platformy i kontynuować dodawanie logiki dla różnych typów wyszukiwania.

## <a name="testing-the-search-by-genre-feature"></a>Testowanie funkcji wyszukiwania przez gatunek

W programie WebMatrix Uruchom stronę *filmy. cshtml* . Zostanie wyświetlona strona z polem tekstowym dla gatunku.

Wprowadź gatunek, który został wprowadzony dla jednego z rekordów testu, a następnie kliknij przycisk **Wyszukaj**. Tym razem zobaczysz listę tylko tych filmów, które pasują do tego gatunku:

![Strona filmów po wyszukiwaniu gatunku "Comedies"](form-basics/_static/image4.png)

Wprowadź inny gatunek i wyszukaj go ponownie. Spróbuj wprowadzić gatunek, używając wielkich liter lub wersalików, aby zobaczyć, że wyszukiwanie nie uwzględnia wielkości liter.

## <a name="remembering-what-the-user-entered"></a>"Zapamiętywanie" zawartości wprowadzonej przez użytkownika

Być może zauważono, że po wprowadzeniu gatunku i kliknięciu **gatunku wyszukiwania**zobaczysz listę dla tego gatunku. Jednak pole tekstowe wyszukiwania było puste &mdash; innymi słowy, nie pamiętasz, co zostało wprowadzone.

Ważne jest, aby zrozumieć, dlaczego takie zachowanie występuje. Po przesłaniu strony przeglądarka wysyła żądanie do serwera sieci Web. Gdy ASP.NET pobiera żądanie, tworzy nowe wystąpienie strony, uruchamia w nim kod, a następnie ponownie renderuje stronę do przeglądarki. W efekcie Strona nie wie, że pracujesz tylko z poprzednią wersją. Wszystkie informacje wiedzą, że otrzymały żądanie, które zawierało pewne dane formularza.

Za każdym razem, gdy zażądasz strony &mdash;, czy po raz pierwszy, czy przez przesłanie &mdash; otrzymujesz nową stronę. Serwer sieci Web nie ma pamięci ostatniego żądania. Ani nie jest ASP.NET, ani nie obsługuje przeglądarki. Jedyne połączenie między tymi osobnymi wystąpieniami strony to wszystkie dane przesyłane między nimi. Jeśli zostanie przesłana Strona, na przykład nowe wystąpienie strony może pobrać dane formularza, które zostały wysłane przez poprzednie wystąpienie. (Inny sposób przekazywania danych między stronami polega na użyciu plików cookie).

Formalna Metoda opisywania tej sytuacji polega na tym, że strony sieci Web są *bezstanowe*. Serwery sieci Web i same strony i elementy na stronie nie zachowują żadnych informacji o poprzednim stanie strony. Sieć Web została zaprojektowana w ten sposób, ponieważ utrzymywanie stanu poszczególnych żądań szybko wyczerpuje zasoby serwerów sieci Web, które często obsługują tysiące, nawet setki tysięcy z żądań na sekundę.

Dlatego też pole tekstowe było puste. Po przesłaniu strony ASP.NET utworzone nowe wystąpienie strony i uruchomiono je za pomocą kodu i znaczników. W tym kodzie nie wprowadzono żadnych informacji, które poinformowały ASP.NET o wprowadzeniu wartości do pola tekstowego. Tak więc ASP.NET nie zrobił niczego i pole tekstowe zostało renderowane bez wartości.

Istnieje już prosty sposób na obejście tego problemu. Gatunek wprowadzony w polu tekstowym *jest* dostępny w kodzie, &mdash; jest `Request.QueryString["searchGenre"]`.

Zaktualizuj znaczniki dla pola tekstowego, aby atrybut `value` pobiera swoją wartość z `searchTerm`, jak w poniższym przykładzie:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Na tej stronie można również ustawić atrybut `value` na zmienną `searchTerm`, ponieważ ta zmienna również zawiera wprowadzony gatunek. Jednak użycie obiektu `Request` do ustawienia atrybutu `value`, jak pokazano poniżej, jest standardowym sposobem wykonania tego zadania. (Przy założeniu, że nawet chcesz zrobić to &mdash; w niektórych sytuacjach, możesz chcieć renderować stronę *bez* wartości w polach. Wszystko to zależy od tego, co się dzieje z Twoją aplikacją.

> [!NOTE]
> Nie możesz "zapamiętać" wartości pola tekstowego, które jest używane na potrzeby haseł. Może to być Luka w zabezpieczeniach umożliwiająca użytkownikom Wypełnienie pola hasła przy użyciu kodu.

Uruchom ponownie stronę, wprowadź gatunek i kliknij pozycję **Szukaj gatunku**. Tym razem nie tylko zobaczysz wyniki wyszukiwania, ale pole tekstowe zapamiętuje dane wprowadzone w ostatnim momencie:

![Strona wskazująca, że w polu tekstowym zapamiętano Poprzedni wpis](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Wyszukiwanie dowolnego wyrazu w tytule

Możesz teraz wyszukać dowolny gatunek, ale możesz również wyszukać tytuł. Przed rozpoczęciem wyszukiwania trudno uzyskać tytuł, dlatego możesz wyszukiwać słowo, które pojawia się w dowolnym miejscu w tytule. Aby to zrobić w programie SQL, należy użyć operatora `LIKE` i składni podobnej do następujących:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

To polecenie pobiera wszystkie filmy, których tytuły zawierają "Adventure". Korzystając z operatora `LIKE`, należy uwzględnić symbol wieloznaczny `%` w ramach wyszukiwanego terminu. `LIKE 'adventure%'` wyszukiwania to "zaczynające się od" Adventure "". (Technicznie oznacza "ciąg" Adventure ", po którym następuje wszystko") ". Podobnie termin wyszukiwania `LIKE '%adventure'` oznacza "coś, a po nim ciąg" Adventure "", który jest innym sposobem na "zakończenie" Adventure "".

Termin wyszukiwania `LIKE '%adventure%'` oznacza "" "z" Adventure "w dowolnym miejscu w tytule". (Technicznie, "wszystkie elementy w tytule, po którym następuje" Adventure ", po którym następuje" ").

Wewnątrz elementu `<form>` Dodaj następujące znaczniki bezpośrednio pod tagiem zamykającym `</div>` dla wyszukiwania gatunku (tuż przed zamykającym elementem `</form>`):

[!code-html[Main](form-basics/samples/sample10.html)]

Kod do obsługi tego wyszukiwania jest podobny do kodu dla wyszukiwania gatunku, z tą różnicą, że musisz złożyć wyszukiwanie `LIKE`. W bloku kodu w górnej części strony Dodaj następujący blok `if` tuż po bloku `if` dla wyszukiwania gatunku:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Ten kod używa tej samej logiki, która była wcześniej, z tą różnicą, że wyszukiwanie używa operatora `LIKE`, a kod umieszcza "`%`" przed upływem terminu i po nim.

Zwróć uwagę, jak łatwo dodać kolejne wyszukiwanie do strony. Wszystkie musiały zostać:

- Utwórz blok `if` przetestowany w celu sprawdzenia, czy odpowiednie pole wyszukiwania ma wartość.
- Ustaw zmienną `selectCommand` na nową instrukcję języka SQL.
- Ustaw zmienną `searchTerm` na wartość, która ma zostać przekazana do zapytania.

Oto kompletny blok kodu, który zawiera nową logikę wyszukiwania tytułu:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Oto podsumowanie informacji o tym kodzie:

- Zmienne `searchTerm` i `selectCommand` są inicjowane u góry. Te zmienne są ustawiane na odpowiedni termin wyszukiwania (jeśli istnieje) i odpowiednie polecenie SQL na podstawie tego, co użytkownik wykonuje na stronie. Domyślne wyszukiwanie to prosty przypadek uzyskiwania wszystkich filmów z bazy danych.
- W testach dla `searchGenre` i `searchTitle`kod ustawia `searchTerm` na wartość, którą chcesz wyszukać. Te bloki kodu są również ustawiane `selectCommand` na odpowiednie polecenie SQL dla tego wyszukiwania.
- Metoda `db.Query` jest wywoływana tylko raz, przy użyciu dowolnego polecenia SQL znajduje się w `selectedCommand` i niezależnie od wartości w `searchTerm`. Jeśli nie ma wyszukiwanego terminu (bez gatunku i bez tytułu), wartość `searchTerm` jest pustym ciągiem. Jednak nie ma znaczenia, ponieważ w takim przypadku zapytanie nie wymaga parametru.

## <a name="testing-the-title-search-feature"></a>Testowanie funkcji wyszukiwania tytułów

Teraz możesz przetestować ukończoną stronę wyszukiwania. Uruchom *filmy. cshtml*.

Wprowadź gatunek i kliknij pozycję **kryterium wyszukiwania**. Siatka wyświetla filmy tego gatunku, takie jak przed.

Wprowadź wyraz tytułu i kliknij przycisk **Wyszukaj tytuł**. Siatka wyświetla filmy, które mają ten wyraz w tytule.

![Strona filmów po wyszukiwaniu wyrazu "The" w tytule](form-basics/_static/image6.png)

Pozostaw puste pola tekstowe i kliknij jeden z przycisków. Siatka wyświetla wszystkie filmy.

## <a name="combining-the-queries"></a>Łączenie zapytań

Możesz zauważyć, że wyszukiwania, które można wykonywać, są wyłącznie na wyłączność. Nie można wyszukać tytułu i gatunku w tym samym czasie, nawet jeśli oba pola wyszukiwania zawierają wartości. Na przykład nie można wyszukać wszystkich filmów akcji, których tytuł zawiera ciąg "Adventure". (Jako że strona jest teraz zakodowana, jeśli wprowadzisz wartości dla obu gatunków i tytułów, wyszukiwanie tytułu będzie miało pierwszeństwo). Aby utworzyć wyszukiwanie, które łączy warunki, należy utworzyć zapytanie SQL, które ma składnię podobną do następującej:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

I musisz uruchomić zapytanie przy użyciu instrukcji podobnej do następującej (w przybliżeniu mówiąc):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Tworzenie logiki zezwalającej na wiele permutacji kryteriów wyszukiwania może uzyskać nieco, jak widać. W związku z tym zatrzymamy się tutaj.

## <a name="coming-up-next"></a>Przyszłe przejście

W następnym samouczku utworzysz stronę korzystającą z formularza, aby umożliwić użytkownikom dodawanie filmów do bazy danych.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Ukończ listę dla strony filmu (Zaktualizowano przy użyciu wyszukiwania)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Dodatkowe materiały

- [Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Klauzula WHERE języka SQL](http://www.w3schools.com/sql/sql_where.asp) w witrynie w3schools
- Artykuł [definicji metod](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) w witrynie W3C

> [!div class="step-by-step"]
> [Poprzednie](displaying-data.md)
> [dalej](entering-data.md)
