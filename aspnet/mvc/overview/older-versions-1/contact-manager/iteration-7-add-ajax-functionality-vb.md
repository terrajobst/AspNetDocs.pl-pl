---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: 'Iteracja #7 — Dodawanie funkcji AJAX (VB) | Microsoft Docs'
author: microsoft
description: W siódmej iteracji poprawimy czas reakcji i wydajność naszej aplikacji przez dodanie obsługi technologii AJAX.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: cee2b6e7c7517a1e03ae26d5233fc438857a030c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601655"
---
# <a name="iteration-7--add-ajax-functionality-vb"></a>Iteracja #7 — Dodawanie funkcji AJAX (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> W siódmej iteracji poprawimy czas reakcji i wydajność naszej aplikacji przez dodanie obsługi technologii AJAX.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji Contact Management ASP.NET MVC (VB)

W tej serii samouczków tworzymy całą aplikację do zarządzania kontaktami od początku do końca. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazw, numerów telefonów i adresów e-mail — w celu uzyskania listy osób.

Aplikacja została utworzona przez wiele iteracji. W przypadku każdej iteracji stopniowo ulepszamy aplikację. Celem tej wielu iteracji jest umożliwienie zrozumienia przyczyny każdej zmiany.

- #1 iteracji — Utwórz aplikację. W pierwszej iteracji tworzymy Menedżera kontaktów w najprostszy sposób. Dodaliśmy obsługę podstawowych operacji bazy danych: Tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD).

- Iteracja #2 — Zwiększ wygląd aplikacji. W tej iteracji ulepszamy wygląd aplikacji, modyfikując domyślną stronę wzorcową widoku MVC ASP.NET i kaskadowy arkusz stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzeciej iteracji zostanie dodana podstawowa Walidacja formularza. Uniemożliwiamy użytkownikom przesyłanie formularza bez wykonywania wymaganych pól formularza. Sprawdzamy również adresy e-mail i numery telefonów.

- Iteracja #4 — możliwość swobodnego łączenia aplikacji. W tej czwartej iteracji wykorzystujemy kilka wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład Refaktoryzacja naszej aplikacji używa wzorca repozytorium i wzorca iniekcji zależności.

- #5 iteracji — Utwórz testy jednostkowe. W piątej iteracji upraszczamy obsługę i modyfikację naszej aplikacji przez dodanie testów jednostkowych. Tworzymy klasy modelu danych i kompilujemy testy jednostkowe dla naszych kontrolerów i logiki walidacji.

- Iteracja #6 — Użyj programowania opartego na testach. W tej szóstej iteracji Dodaliśmy nowe funkcje do naszej aplikacji, pisząc testy jednostkowe jako pierwsze i pisząc kod na testach jednostkowych. W tej iteracji dodamy grupy kontaktów.

- Iteracja #7 — Dodawanie funkcji AJAX. W siódmej iteracji poprawimy czas reakcji i wydajność naszej aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Ta iteracja

W tej iteracji aplikacji Contact Manager Refaktoryzacja naszej aplikacji korzysta z technologii AJAX. Dzięki wykorzystaniu technologii AJAX Nasza aplikacja nie odpowiada. Możemy uniknąć renderowania całej strony, gdy będziemy musieli zaktualizować tylko określony region na stronie.

Będziemy reprezentować nasz widok indeksu, aby nie musieli ponownie wyświetlać całej strony, gdy ktoś wybierze nową grupę kontaktów. Zamiast tego, gdy ktoś kliknie grupę kontaktów, wystarczy zaktualizować listę kontaktów i pozostawić resztę samej strony.

Zmienimy również sposób działania naszego linku usuwania. Zamiast wyświetlania oddzielnej strony potwierdzenia zostanie wyświetlone okno dialogowe potwierdzenia języka JavaScript. Jeśli potwierdzisz, że chcesz usunąć kontakt, operacja usuwania HTTP zostanie przeprowadzona na serwerze w celu usunięcia rekordu kontaktu z bazy danych.

Ponadto będziemy korzystać z technologii jQuery do dodawania efektów animacji do naszego widoku indeksu. Pokażemy animację, gdy nowa lista kontaktów zostanie pobrana z serwera.

Na koniec będziemy korzystać z obsługi ASP.NET AJAX Framework na potrzeby zarządzania historią przeglądarki. Utworzymy punkty historii za każdym razem, gdy wykonamy wywołanie AJAX w celu zaktualizowania listy kontaktów. Dzięki temu przyciski przeglądarki do tyłu i do przodu będą działały.

## <a name="why-use-ajax"></a>Dlaczego warto używać technologii AJAX?

Korzystanie z technologii AJAX ma wiele korzyści. Po pierwsze dodanie funkcji AJAX do aplikacji skutkuje lepszym interfejsem użytkownika. W normalnej aplikacji sieci Web cała strona musi być ogłaszana z powrotem na serwerze i za każdym razem, gdy użytkownik wykonuje akcję. Za każdym razem, gdy wykonujesz akcję, blokada i użytkownik muszą czekać, aż cała strona zostanie pobrana i ponownie wyświetlona.

Jest to nieakceptowalne środowisko w przypadku aplikacji klasycznych. Jednak tradycyjnie jesteśmy w trakcie tego nieprawidłowego środowiska użytkownika w przypadku aplikacji sieci Web, ponieważ nie wiemy, że możemy to poprawić. Uważamy, że było to ograniczenie aplikacji sieci Web, gdy, w rzeczywistości, było to tylko ograniczenie naszych Imaginations.

W aplikacji AJAX nie trzeba wprowadzać środowiska użytkownika w celu zatrzymania tylko w celu zaktualizowania strony. Zamiast tego można wykonać asynchroniczne żądanie w tle w celu zaktualizowania strony. Nie Wymuś, aby użytkownik czekał na aktualizację części strony.

Wykorzystując zalety technologii AJAX, możesz również zwiększyć wydajność aplikacji. Zastanów się, jak aplikacja Contact Manager działa teraz bez funkcji AJAX. Po kliknięciu grupy kontaktów należy ponownie wyświetlić cały widok indeksu. Lista kontaktów i list grup kontaktów musi być pobierana z serwera bazy danych. Wszystkie te dane muszą być przesyłane przez sieć z serwera sieci Web do przeglądarki sieci Web.

Po dodaniu funkcji AJAX do aplikacji można jednak uniknąć wyświetlania całej strony, gdy użytkownik kliknie grupę kontaktów. Nie potrzebujemy już grup kontaktów z bazy danych. Nie trzeba również wypchnąć całego widoku indeksu w sieci. Dzięki wykorzystaniu technologii AJAX zmniejszamy ilość pracy, jaką musi wykonać serwer bazy danych, i zmniejszamy ilość ruchu sieciowego wymaganego przez naszą aplikację.

## <a name="don-t-be-afraid-of-ajax"></a>Nie boisz AJAX

Niektórzy deweloperzy unikają korzystania ze AJAX, ponieważ obawiają się one o przeglądarki niskiego poziomu. Chcą mieć pewność, że aplikacje sieci Web będą nadal działały w przypadku uzyskania dostępu do przeglądarki, która nie obsługuje języka JavaScript. Ponieważ AJAX jest zależna od języka JavaScript, niektórzy deweloperzy nie mogą korzystać z technologii AJAX.

Jednak w przypadku starannego zaimplementowania technologii AJAX można tworzyć aplikacje, które współpracują z przeglądarkami międzypoziomowymi i niskiego poziomu. Nasza aplikacja z Menedżerem kontaktów będzie współpracować z przeglądarkami obsługującymi język JavaScript i przeglądarki, które nie.

Jeśli używasz aplikacji Contact Manager z przeglądarką obsługującą język JavaScript, będziesz mieć lepszy interfejs użytkownika. Na przykład po kliknięciu grupy kontaktów zostanie zaktualizowany tylko region strony z wyświetlonymi kontaktami.

Jeśli z drugiej strony, używasz aplikacji Contact Manager z przeglądarką, która nie obsługuje języka JavaScript (lub ma wyłączony kod JavaScript), będziesz mieć nieco mniej pożądane środowisko użytkownika. Na przykład po kliknięciu grupy kontaktów cały widok indeksu musi zostać opublikowany z powrotem do przeglądarki, aby wyświetlić pasującą listę kontaktów.

## <a name="adding-the-required-javascript-files"></a>Dodawanie wymaganych plików JavaScript

Będziemy musieli używać trzech plików JavaScript do dodawania funkcji AJAX do naszej aplikacji. Wszystkie trzy z tych plików są dołączone do folderu Scripts nowej aplikacji ASP.NET MVC.

Jeśli planujesz używanie technologii AJAX na wielu stronach w aplikacji, warto uwzględnić wymagane pliki JavaScript na stronie wzorcowej widoku aplikacji. Dzięki temu pliki JavaScript zostaną dołączone do wszystkich stron w aplikacji automatycznie.

Dodaj następujący kod JavaScript w tagu &lt;głównego&gt; strony wzorcowej widoku:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refaktoryzacja widoku indeksu w celu użycia technologii AJAX

Niech Rozpocznij, modyfikując nasz widok indeksu, aby kliknięcie grupy kontaktów tylko zaktualizuje region widoku, w którym są wyświetlane kontakty. Czerwona rubryka na rysunku 1 zawiera region, który chcemy zaktualizować.

[![aktualizowanie tylko kontaktów](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Ilustracja 01**. aktualizowanie tylko kontaktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-7-add-ajax-functionality-vb/_static/image2.png))

Pierwszym krokiem jest oddzielenie części widoku, który chcemy zaktualizować asynchronicznie do oddzielnej częściowej (Wyświetl kontrolkę użytkownika). Sekcja widoku indeks, w którym jest wyświetlana tabela kontaktów, została przeniesiona do części listy 1.

**Lista 1 — Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Zwróć uwagę, że część z listy 1 ma inny model niż widok indeksu. Atrybut *Inherits* w dyrektywie &lt;% @ Page%&gt; określa, że częściowo dziedziczy z klasy ViewUserControl&lt;Group&gt;.

Zaktualizowany widok indeksu znajduje się na liście 2.

**Lista 2 — Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Istnieją dwie rzeczy, na które należy zauważyć zaktualizowany widok na liście 2. Najpierw należy zauważyć, że cała zawartość przeniesiona do częściowej jest zastępowana wywołaniem html. RenderPartial (). Metoda html. RenderPartial () jest wywoływana podczas pierwszego żądania widoku indeksu w celu wyświetlenia początkowego zestawu kontaktów.

Następnie Zwróć uwagę, że plik HTML. ActionLink () używany do wyświetlania grup kontaktów został zastąpiony przez AJAX. ActionLink (). Wartość AJAX. ActionLink () jest wywoływana z następującymi parametrami:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

Pierwszy parametr reprezentuje tekst, który ma być wyświetlany dla łącza, drugi parametr reprezentuje wartości trasy, a trzeci parametr reprezentuje opcje AJAX. W takim przypadku użyjemy opcji UpdateTargetId AJAX, aby wskazać znacznik div&gt; HTML &lt;, który chcemy zaktualizować po zakończeniu żądania AJAX. Chcemy zaktualizować tag &lt;DIV&gt; z nową listą kontaktów.

Zaktualizowana Metoda index () kontrolera Contact znajduje się na liście 3.

**Lista 3-Controllers\ContactController.vb (Metoda index)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

Zaktualizowana akcja index () warunkowo zwraca jedną z dwóch rzeczy. Jeśli akcja index () jest wywoływana przez żądanie AJAX, kontroler zwraca częściowy. W przeciwnym razie akcja index () zwraca cały widok.

Zwróć uwagę, że akcja index () nie musi zwracać ilości danych, gdy są wywoływane przez żądanie AJAX. W kontekście normalnego żądania akcja indeksu zwraca listę wszystkich grup kontaktów i wybranej grupy kontaktów. W kontekście żądania AJAX akcja index () zwróci tylko wybraną grupę. Technologia AJAX oznacza mniejszą ilość pracy na serwerze bazy danych.

Nasz zmodyfikowany widok indeksu działa w przypadku przeglądarek międzypoziomowych i niskiego poziomu. Jeśli klikniesz grupę kontaktów, a przeglądarka obsługuje kod JavaScript, zostanie zaktualizowany tylko region widoku zawierający listę kontaktów. Jeśli z drugiej strony przeglądarka nie obsługuje języka JavaScript, cały widok zostanie zaktualizowany.

Nasz zaktualizowany widok indeksu ma jeden problem. Po kliknięciu grupy kontaktów wybrana grupa nie zostanie wyróżniona. Ponieważ lista grup jest wyświetlana poza regionem, który jest aktualizowany w trakcie żądania AJAX, po prawej stronie nie zostanie wyróżniona. Naprawimy ten problem w następnej sekcji.

## <a name="adding-jquery-animation-effects"></a>Dodawanie efektów animacji jQuery

Zwykle po kliknięciu linku na stronie sieci Web można użyć paska postępu przeglądarki do wykrycia, czy przeglądarka aktywnie pobiera zaktualizowaną zawartość. Podczas wykonywania żądania AJAX, z drugiej strony, pasek postępu przeglądarki nie pokazuje żadnego postępu. Może to spowodować, że użytkownicy są nerw. Skąd wiadomo, czy przeglądarka została zamrożona?

Istnieje kilka sposobów, które można wskazać użytkownikowi, który jest wykonywany podczas wykonywania żądania AJAX. Jednym z metod jest wyświetlenie prostej animacji. Na przykład można wyciszyć region, gdy żądanie AJAX zacznie się i zanika w regionie po zakończeniu żądania.

Użyjemy biblioteki jQuery, która jest dołączona do Microsoft ASP.NET platformy MVC, aby utworzyć efekty animacji. Zaktualizowany widok indeksu znajduje się na liście 4.

**Lista 4 — Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Należy zauważyć, że zaktualizowany widok indeksu zawiera trzy nowe funkcje języka JavaScript. Pierwsze dwie funkcje używają platformy jQuery do zanikania i zanikania na liście kontaktów po kliknięciu nowej grupy kontaktów. Trzecia funkcja wyświetla komunikat o błędzie, gdy żądanie AJAX powoduje błąd (na przykład limit czasu sieci).

Pierwsza funkcja wymaga również wyróżniania wybranej grupy. Klasa = wybrany atrybut jest dodawany do elementu nadrzędnego (element LI) klikniętego elementu. Ponownie program jQuery ułatwia wybranie odpowiedniego elementu i dodanie klasy CSS.

Te skrypty są powiązane z łączami do grup z pomocą parametru AjaxOptions AJAX. ActionLink (). Zaktualizowane wywołanie metody AJAX. ActionLink () wygląda następująco:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Dodawanie obsługi historii przeglądarki

Zwykle po kliknięciu linku w celu zaktualizowania strony Historia przeglądarki zostanie zaktualizowana. W ten sposób można kliknąć przycisk Wstecz przeglądarki, aby wrócić do poprzedniego stanu strony. Jeśli na przykład klikniesz grupę kontaktowa znajomych, a następnie klikniesz grupę kontaktów służbowych, możesz kliknąć przycisk Wstecz przeglądarki, aby przejść z powrotem do stanu strony, gdy została wybrana grupa kontaktowa znajomych.

Niestety, wykonanie żądania AJAX nie aktualizuje automatycznie historii przeglądarki. Jeśli klikniesz grupę kontaktów, a lista pasujących kontaktów zostanie pobrana z żądaniem AJAX, Historia przeglądarki nie zostanie zaktualizowana. Nie można użyć przycisku Wstecz przeglądarki, aby przejść z powrotem do grupy kontaktów po wybraniu nowej grupy kontaktów.

Jeśli chcesz, aby użytkownicy mogli korzystać z przycisku Wstecz w przeglądarce po przeprowadzeniu żądań AJAX, musisz wykonać nieco więcej pracy. Musisz skorzystać z funkcji zarządzania historią przeglądarki wbudowanej w architekturę ASP.NET AJAX.

Historia ASP.NET AJAX Browser, należy wykonać trzy czynności:

1. Włącz historię przeglądarki, ustawiając właściwość enableBrowserHistory na wartość true.
2. Zapisuj punkty historii, gdy stan widoku zmienia się, wywołując metodę addHistoryPoint ().
3. Rekonstrukcja stanu widoku po wywołaniu zdarzenia nawigacji.

Zaktualizowany widok indeksu znajduje się na liście 5.

**Lista 5 — Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

W liście 5 jest włączona Historia przeglądarki w funkcji pageInit (). Funkcja pageInit () służy również do konfigurowania programu obsługi zdarzeń dla zdarzenia nawigacji. Zdarzenie nawigacji jest wywoływane za każdym razem, gdy przycisk przeglądarki do przodu lub wstecz powoduje zmianę stanu strony.

Metoda beginContactList () jest wywoływana po kliknięciu grupy kontaktów. Ta metoda tworzy nowy punkt historyczny przez wywołanie metody addHistoryPoint (). Identyfikator klikniętej grupy kontaktów został dodany do historii.

Identyfikator grupy jest pobierany z atrybutu expando w łączu grupy kontaktów. Link jest renderowany przy użyciu następującego wywołania AJAX. ActionLink ().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

Ostatni parametr przesłany do AJAX. ActionLink () dodaje atrybut expand o nazwie GroupID do linku (małe litery dla zgodności XHTML).

Gdy użytkownik trafi przycisk przeglądarki do tyłu lub do przodu, zdarzenie nawigacji jest wywoływane i wywoływana jest metoda nawigowania (). Ta metoda aktualizuje kontakty wyświetlane na stronie w celu dopasowania jej do stanu strony odpowiadającej punktowi historii przeglądarki przekazaniu do metody nawigacji.

## <a name="performing-ajax-deletes"></a>Wykonywanie operacji usuwania AJAX

Obecnie w celu usunięcia kontaktu należy kliknąć łącze Usuń, a następnie kliknąć przycisk Usuń wyświetlany na stronie potwierdzenia usunięcia (patrz rysunek 2). Wydaje się to na przykład wiele żądań stron, które ułatwiają wykonywanie czegoś prostego, takiego jak usuwanie rekordu bazy danych.

[![strony potwierdzenia usunięcia](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Ilustracja 02**. strona potwierdzenia usunięcia ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-7-add-ajax-functionality-vb/_static/image4.png))

Zachęcamy do pomijania strony potwierdzenia usunięcia i usunięcia kontaktu bezpośrednio z widoku indeksu. Należy unikać tego Temptation, ponieważ to podejście spowoduje otwarcie aplikacji pod kątem luk w zabezpieczeniach. Ogólnie rzecz biorąc, nie chcesz wykonywać operacji HTTP GET podczas wywoływania akcji, która modyfikuje stan aplikacji sieci Web. W przypadku usuwania, należy wykonać operację usuwania http lub jeszcze lepszy.

Link usuwania znajduje się w częściowej ContactList. Zaktualizowana wersja ContactList częściowej jest zawarta w aukcji 6.

**Lista 6 — Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

Link usuwania jest renderowany przy użyciu następującego wywołania metody AJAX. ImageActionLink ():

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> AJAX. ImageActionLink () nie jest standardową częścią struktury ASP.NET MVC. AJAX. ImageActionLink () jest niestandardowymi metodami pomocniczymi zawartymi w projekcie programu Contact Manager.

Parametr AjaxOptions ma dwie właściwości. Najpierw Właściwość Confirm służy do wyświetlania okna dialogowego potwierdzenia języka JavaScript. Po drugie, właściwość HttpMethod służy do wykonywania operacji usuwania HTTP.

Lista 7 zawiera nową akcję AjaxDelete (), która została dodana do kontrolera Contact.

**Lista 7-Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

Akcja AjaxDelete () jest dekoracyjna z atrybutem AcceptVerbs. Ten atrybut uniemożliwia Wywoływanie akcji poza operacją HTTP inną niż operacja usuwania HTTP. W szczególności nie można wywołać tej akcji przy użyciu protokołu HTTP GET.

Po usunięciu rekordu bazy danych należy wyświetlić zaktualizowaną listę kontaktów, które nie zawierają usuniętego rekordu. Metoda AjaxDelete () zwraca częściową ContactList i zaktualizowaną listę kontaktów.

## <a name="summary"></a>Podsumowanie

W tej iteracji dodaliśmy funkcję AJAX do naszej aplikacji Contact Manager. Używamy technologii AJAX, aby zwiększyć czas odpowiedzi i wydajność naszej aplikacji.

Po pierwsze, Refaktoryzacja widoku indeksu, tak aby kliknięcie grupy kontaktów nie aktualizuje całego widoku. Zamiast tego kliknięcie grupy kontaktów aktualizuje tylko listę kontaktów.

Następnie używamy efektów animacji jQuery do zanikania i zanikania na liście kontaktów. Dodawanie animacji do aplikacji Ajax może służyć do udostępnienia użytkownikom aplikacji równorzędnym paskiem postępu przeglądarki.

Dodaliśmy również obsługę historii przeglądarki do naszej aplikacji AJAX. Wszyscy użytkownicy włączyli przyciski przeglądarki Wstecz i dalej, aby zmienić stan widoku indeksu.

Na koniec utworzyliśmy link usuwania obsługujący operacje usuwania HTTP. Po przeprowadzeniu usuwania AJAX użytkownicy umożliwiają użytkownikom usuwanie rekordów bazy danych bez konieczności żądania dodatkowej strony potwierdzenia usunięcia przez użytkownika.

> [!div class="step-by-step"]
> [Wstecz](iteration-6-use-test-driven-development-vb.md)
