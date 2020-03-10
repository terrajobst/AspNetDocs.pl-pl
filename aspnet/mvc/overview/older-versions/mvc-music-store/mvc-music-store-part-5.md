---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Część 5: edytowanie formularzy i tworzenia szablonów | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 5 obejmuje edycję formularzy i tworzenia szablonów.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20b99cbe57b5dfa623205838a5929733a6c2d70d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559550"
---
# <a name="part-5-edit-forms-and-templating"></a>Część 5. Edycja formularzy i tworzenie szablonów

przez [Jan Galloway](https://github.com/jongalloway)

> Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.  
>   
> Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.
> 
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 5 obejmuje edycję formularzy i tworzenia szablonów.

W ostatnim rozdziale załadujemy dane z naszej bazy danych i ich wyświetlanie. W tym rozdziale umożliwimy również edytowanie danych.

## <a name="creating-the-storemanagercontroller"></a>Tworzenie StoreManagerController

Zaczniemy od utworzenia nowego kontrolera o nazwie **StoreManagerController**. W przypadku tego kontrolera będziemy korzystać z funkcji tworzenia szkieletów dostępnych w ramach aktualizacji narzędzi ASP.NET MVC 3. Ustaw opcje okna dialogowego Dodaj kontroler, jak pokazano poniżej.

![](mvc-music-store-part-5/_static/image1.png)

Po kliknięciu przycisku Dodaj zobaczysz, że mechanizm tworzenia szkieletu ASP.NET MVC 3 wykonuje dobrą ilość pracy:

- Tworzy nowy StoreManagerController z lokalną zmienną Entity Framework
- Dodaje folder Storemanager do folderu widoków projektu
- Dodaje do klasy albumu polecenie CREATE. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml i index. cshtml.

![](mvc-music-store-part-5/_static/image2.png)

Nowa klasa kontrolera Magazynumanager obejmuje akcje kontrolera CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie), które wiedzą, jak współpracować z klasą modelu albumu i korzystać z naszego kontekstu Entity Framework do uzyskiwania dostępu do bazy danych.

## <a name="modifying-a-scaffolded-view"></a>Modyfikowanie widoku szkieletowego

Należy pamiętać, że podczas gdy ten kod został wygenerowany dla nas, jest to standardowy kod ASP.NET MVC, podobnie jak w przypadku pisania w tym samouczku. Zachodzi konieczność zaoszczędzenia czasu poświęcanego na pisanie kodu standardowego kontrolera i ręczne utworzenie widoków o jednoznacznie określonym typie, ale nie jest to rodzaj wygenerowanego kodu, który może zostać wcześniej wyświetlony z ostrzeżeniami fatalne w komentarzach dotyczących sposobu nie może zmiany kodu. Jest to Twój kod, który powinien zostać zmieniony.

Zacznijmy od szybkiej edycji do widoku indeksu Magazynumanager (/Views/StoreManager/Index.cshtml). W tym widoku zostanie wyświetlona tabela zawierająca listę albumów w naszym sklepie z linkami Edytuj/szczegóły/Usuń i zawiera właściwości publiczne albumu. Usuniemy pole AlbumArtUrl, ponieważ nie jest ono bardzo przydatne w przypadku tego ekranu. W sekcji &lt;tabeli&gt; w kodzie widoku Usuń &lt;tej&gt; i &lt;TD&gt; elementy otaczające odwołania AlbumArtUrl, jak wskazano w wyróżnionych wierszach poniżej:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Zmodyfikowany kod widoku zostanie wyświetlony w następujący sposób:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Pierwsze spojrzenie na Menedżera sklepu

Teraz uruchom aplikację i przejdź do/StoreManager/. Spowoduje to wyświetlenie indeksu Menedżera sklepu, który został właśnie zmodyfikowany, pokazującego listę albumów w sklepie z linkami do edycji, szczegółów i usuwania.

![](mvc-music-store-part-5/_static/image3.png)

Kliknięcie linku edycji wyświetla formularz edycji z polami dla albumu, włącznie z listami rozwijanymi dla gatunku i wykonawcy.

![](mvc-music-store-part-5/_static/image4.png)

Kliknij link "z powrotem do listy" u dołu, a następnie kliknij link szczegóły dla albumu. Spowoduje to wyświetlenie szczegółowych informacji o pojedynczym albumie.

![](mvc-music-store-part-5/_static/image5.png)

Ponownie kliknij link Wróć do listy, a następnie kliknij link Usuń. Spowoduje to wyświetlenie okna dialogowego potwierdzenia, pokazującego szczegóły dotyczące albumu i pytanie, czy na pewno chcemy go usunąć.

![](mvc-music-store-part-5/_static/image6.png)

Kliknięcie przycisku Usuń u dołu spowoduje usunięcie albumu i powrót do strony indeks, która pokazuje, że album został usunięty.

Nie zakończymy pracy z menedżerem sklepu, ale mamy do uruchomienia kontroler roboczy i kod wyświetlania CRUD operacji.

## <a name="looking-at-the-store-manager-controller-code"></a>Spojrzenie na kod kontrolera Menedżera sklepu

Kontroler Menedżera sklepu zawiera dobrą ilość kodu. Zacznijmy od góry do dołu. Kontroler zawiera kilka standardowych przestrzeni nazw dla kontrolera MVC, a także odwołanie do naszych nazw modeli. Kontroler ma prywatne wystąpienie MusicStoreEntities używane przez poszczególne akcje kontrolera na potrzeby dostępu do danych.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Akcje dotyczące indeksu i szczegółów Menedżera sklepu

Widok indeksu pobiera listę albumów, w tym informacje o gatunku i wykonawcach z odwołaniami do poszczególnych albumów, ponieważ zostały one wcześniej wyświetlone podczas pracy nad metodą przeglądania w sklepie. Widok indeksu jest następujący po odwołaniach do połączonych obiektów, aby można było wyświetlić nazwę gatunku każdego albumu i nazwę wykonawcy, aby kontroler był wydajny i badał zapytania dotyczące tych informacji w oryginalnym żądaniu.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Akcja kontrolera szczegółów kontrolera Sklepumanager działa dokładnie tak samo jak akcja szczegóły kontrolera sklepu, która została wcześniej zapisana — wykonuje zapytanie dotyczące albumu według identyfikatora przy użyciu metody Find (), a następnie zwraca je do widoku.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Metody tworzenia akcji

Metody tworzenia akcji są nieco inne niż te, które były już widoczne, ponieważ obsługują dane wejściowe formularza. Gdy użytkownik najpierw odwiedzi/StoreManager/Create/, będzie wyświetlany pusty formularz. Ta strona HTML będzie zawierać &lt;formularz&gt; element, który zawiera elementy listy rozwijanej i TextBox, gdzie mogą wprowadzać szczegóły albumu.

Po wypełnieniu przez użytkownika wartości formularza albumu można nacisnąć przycisk "Save" (Zapisz), aby przesłać te zmiany z powrotem do naszej aplikacji w celu zapisania ich w bazie danych. Gdy użytkownik naciśnie przycisk "Save" (Zapisz),&gt; formularz &lt;spowoduje przeprowadzenie żądania HTTP zwrotnego w adresie URL/StoreManager/Create/i przesłanie &lt;&gt; wartości w ramach POST protokołu HTTP.

ASP.NET MVC umożliwia łatwe rozdzielenie logiki tych dwóch scenariuszy wywołania adresu URL przez umożliwienie nam implementacji dwóch oddzielnych metod akcji "Create" w naszej klasie StoreManagerController — jeden do obsługi początkowego protokołu HTTP-GET w przejściu do adresu URL/StoreManager/Create/, a drugi do obsługi POST protokołu HTTP dla przesłanych zmian.

### <a name="passing-information-to-a-view-using-viewbag"></a>Przekazywanie informacji do widoku przy użyciu ViewBag

ViewBag wcześniej w tym samouczku, ale nie porozmawiasz z nim. ViewBag umożliwia nam przekazywanie informacji do widoku bez użycia obiektu modelu silnie określonego typu. W takim przypadku nasza akcja Edytuj kontroler HTTP-GET musi przekazywać listę gatunku i artystów do formularza w celu wypełnienia list rozwijanych, a Najprostszym sposobem, aby to zrobić, jako elementów ViewBag.

ViewBag jest obiektem dynamicznym, co oznacza, że można wpisać ViewBag. foo lub ViewBag. YourNameHere bez pisania kodu, aby zdefiniować te właściwości. W takim przypadku kod kontrolera używa ViewBag. GenreId i ViewBag. ArtistId, tak aby wartości rozwijane przesłane z formularzem będą GenreId i ArtistId, które są właściwościami albumu, które będą ustawiane.

Te wartości rozwijane są zwracane do formularza przy użyciu obiektu SelectList, który jest zbudowany tylko do tego celu. Odbywa się to przy użyciu kodu w następujący sposób:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Jak widać w kodzie metody akcji, do utworzenia tego obiektu są używane trzy parametry:

- Lista elementów, które zostaną wyświetlone na liście rozwijanej. Zwróć uwagę, że nie jest to tylko ciąg — przekazujemy listę gatunku.
- Następny parametr, który jest przesyłany do SelectList jest wybraną wartością. W ten sposób SelectList wie, jak wstępnie wybrać element na liście. Będzie to łatwiejsze do zrozumienia, gdy zobaczymy formularz edycji, który jest bardzo podobny.
- Ostatnim parametrem jest właściwość, która ma zostać wyświetlona. W tym przypadku wskazuje, że właściwość Genre.Name jest pokazywana użytkownikowi.

Z tego względu akcja HTTP-GET Create jest bardzo prosta — dwa SelectLists są dodawane do ViewBag i żaden obiekt modelu nie jest przekazano do formularza (ponieważ nie został jeszcze utworzony).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Pomocników HTML do wyświetlania list rozwijanych w widoku tworzenia

Ponieważ porozmawiamy o sposobie przekazywania wartości listy rozwijanej do widoku, przyjrzyjmy się widokowi, aby zobaczyć, jak te wartości są wyświetlane. W widoku Kod (/Views/StoreManager/Create.cshtml) zobaczysz następujące wywołanie, aby wyświetlić listę rozwijaną gatunku.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Jest to znane jako pomocnik HTML — Metoda narzędziowa, która wykonuje typowe zadanie wyświetlania. Pomocniki HTML są bardzo przydatne w zachowaniu zwięzłego i czytelnego kodu widoku. Pomocnik html. DropDownList jest udostępniany przez ASP.NET MVC, ale w dalszej części będzie możliwe utworzenie własnych pomocników na potrzeby wyświetlania kodu, który zostanie ponownie użyty w naszej aplikacji.

Wywołanie html. DropDownList musi mieć pytania dotyczące dwóch rzeczy — miejsca, w którym ma zostać wyświetlona lista, i jaka wartość (jeśli istnieje) powinna być wstępnie wybrana. Pierwszy parametr, GenreId, informuje DropDownList o wyszukiwaniu wartości o nazwie GenreId w modelu lub ViewBag. Drugi parametr jest używany do wskazania wartości, która ma być wyświetlana jako początkowo wybrana na liście rozwijanej. Ponieważ ten formularz jest formularzem tworzenia, nie ma wartości, która ma zostać wybrana, a parametr String. Empty jest przenoszona.

### <a name="handling-the-posted-form-values"></a>Obsługa wartości opublikowanych formularzy

Jak opisano wcześniej, istnieją dwie metody akcji skojarzone z każdym formularzem. Pierwszy obsługuje żądanie HTTP-GET i wyświetla formularz. Drugi obsługuje żądanie HTTP-POST, które zawiera przesłane wartości formularza. Zauważ, że akcja kontrolera ma atrybut [HttpPost], który informuje ASP.NET MVC, że powinien on odpowiadać tylko na żądania HTTP-POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Ta akcja ma cztery obowiązki:

- 1. Odczytywanie wartości formularza
- 2. Sprawdź, czy wartości formularza przechodzą wszystkie reguły walidacji
- 3. Jeśli przesyłanie formularza jest prawidłowe, Zapisz dane i Wyświetl zaktualizowaną listę
- 4. Jeśli przesyłanie formularza jest nieprawidłowe, należy ponownie wyświetlić formularz z błędami walidacji

#### <a name="reading-form-values-with-model-binding"></a>Odczytywanie wartości formularza z powiązaniem modelu

Akcja kontrolera przetwarza przesyłanie formularza, który zawiera wartości dla GenreId i ArtistId (z listy rozwijanej) oraz wartości pól TextBox dla tytułu, ceny i AlbumArtUrl. Chociaż istnieje możliwość bezpośredniego dostępu do wartości formularzy, lepszym rozwiązaniem jest użycie funkcji powiązania modelu wbudowanych w ASP.NET MVC. Gdy akcja kontrolera przyjmuje typ modelu jako parametr, ASP.NET MVC podejmie próbę wypełnienia obiektu tego typu przy użyciu danych wejściowych formularza (a także wartości trasy i QueryString). Jest to możliwe dzięki szukaniu wartości, których nazwy pasują do właściwości obiektu modelu, np. podczas ustawiania wartości GenreId nowego obiektu albumu szuka danych wejściowych o nazwie GenreId. Podczas tworzenia widoków przy użyciu standardowych metod w ASP.NET MVC, formularze będą zawsze renderowane przy użyciu nazw właściwości jako nazw pól wejściowych, więc te nazwy pól będą się zgadzać.

#### <a name="validating-the-model"></a>Sprawdzanie poprawności modelu

Model jest sprawdzany z prostym wywołaniem metody ModelState. IsValid. Nie Dodaliśmy jeszcze żadnych reguł sprawdzania poprawności do naszej klasy albumu — wykonujemy tę czynność w następujący sposób. Ważne jest, aby to ModelStat. IsValid Check przywróci reguły sprawdzania poprawności, które zostały umieszczone w naszym modelu, więc przyszłe zmiany reguł walidacji nie będą wymagały żadnych aktualizacji kodu akcji kontrolera.

#### <a name="saving-the-submitted-values"></a>Zapisywanie przesłanych wartości

Jeśli przesyłanie formularza przejdzie walidację, czas na zapisanie wartości w bazie danych. Mając Entity Framework, to właśnie wymaga dodania modelu do kolekcji albumy i wywołania metody SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework generuje odpowiednie polecenia SQL, aby zachować wartość. Po zapisaniu danych przekierowujemy z powrotem do listy albumów, aby zobaczyć naszą aktualizację. Jest to realizowane przez zwrócenie RedirectToAction z nazwą akcji kontrolera, która ma zostać wyświetlona. W tym przypadku jest to metoda index.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Wyświetlanie nieprawidłowych przesłanych formularzy z błędami walidacji

W przypadku nieprawidłowych danych wejściowych formularza listy rozwijane są dodawane do ViewBag (podobnie jak w przypadku protokołu HTTP-GET), a powiązane wartości modelu są przesyłane z powrotem do widoku w celu wyświetlenia. Błędy walidacji są automatycznie wyświetlane przy użyciu pomocnika HTML @Html.ValidationMessageFor.

#### <a name="testing-the-create-form"></a>Testowanie formularza tworzenia

Aby przetestować tę wartość, uruchom aplikację i przejdź do/StoreManager/Create/— spowoduje to wyświetlenie pustego formularza, który został zwrócony przez StoreController tworzenia HTTP-GET.

Wypełnij pewne wartości, a następnie kliknij przycisk Utwórz, aby przesłać formularz.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Obsługa edycji

Para akcji Edytuj (HTTP-GET i HTTP-POST) jest bardzo podobna do metod tworzenia akcji, które właśnie oglądamy. Ponieważ scenariusz edycji obejmuje pracę z istniejącym albumem, Metoda Edytuj HTTP-GET ładuje album na podstawie parametru "ID" przekazanego za pośrednictwem trasy. Ten kod służący do pobierania albumu przez AlbumId jest taki sam, jak poprzednio oglądamy akcję kontrolera szczegółów. Podobnie jak w przypadku metody Create/HTTP-GET, wartości rozwijane są zwracane za pośrednictwem ViewBag. Dzięki temu możemy zwrócić album jako obiekt modelu do widoku (który jest silnie wpisanych do klasy albumu) podczas przekazywania dodatkowych danych (np. listy gatunków) za pośrednictwem ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

Akcja Edytuj HTTP-POST jest bardzo podobna do akcji tworzenia protokołu HTTP-POST. Jedyną różnicą jest to, że zamiast dodawania nowego albumu do bazy danych. Kolekcja albumów szuka bieżącego wystąpienia albumu przy użyciu bazy danych. Wpis (album) i ustawienie jego stanu na zmodyfikowano. Oznacza to, że Entity Framework modyfikowanie istniejącego albumu zamiast tworzenia nowego.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Możemy to przetestować, uruchamiając aplikację i przechodząc do/StoreManger/, a następnie klikając link Edytuj dla albumu.

![](mvc-music-store-part-5/_static/image9.png)

Zostanie wyświetlony formularz Edycja pokazywany przez metodę Edytuj HTTP-GET. Wypełnij pewne wartości, a następnie kliknij przycisk Zapisz.

![](mvc-music-store-part-5/_static/image10.png)

Spowoduje to opublikowanie formularza, zapisanie wartości i zwrócenie do listy albumów z informacją o tym, że wartości zostały zaktualizowane.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Obsługa usuwania

Usunięcie jest zgodne z tym samym wzorcem, co Edytuj i Utwórz, przy użyciu jednej akcji kontrolera do wyświetlania formularza potwierdzenia i innej akcji kontrolera do obsługi przesłania formularza.

Akcja protokołu HTTP-GET Delete Controller jest dokładnie taka sama jak w poprzedniej akcji kontrolera. szczegóły Menedżera sklepu.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Wyświetlamy formularz, który jest silnie wpisany do typu albumu, przy użyciu szablonu usuwania zawartości.

![](mvc-music-store-part-5/_static/image12.png)

W obszarze usuwanie szablonu są wyświetlane wszystkie pola dla modelu, ale możemy uprościć, że jest to dość bit. Zmień kod widoku w/Views/StoreManager/Delete.cshtml na następujący.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Spowoduje to wyświetlenie uproszczonego potwierdzania usuwania.

![](mvc-music-store-part-5/_static/image13.png)

Kliknięcie przycisku Usuń powoduje, że formularz zostanie ogłoszony ponownie na serwerze, który wykonuje akcję DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Nasza akcja w ramach kontrolera usuwania HTTP-POST wykonuje następujące akcje:

- 1. Ładuje album według identyfikatora
- 2. Usuwa album i zapisuje zmiany
- 3. Przekierowuje do indeksu, pokazując, że album został usunięty z listy

Aby to przetestować, uruchom aplikację i przejdź do/StoreManager. Wybierz album z listy i kliknij link Usuń.

![](mvc-music-store-part-5/_static/image14.png)

Spowoduje to wyświetlenie ekranu potwierdzania usuwania.

![](mvc-music-store-part-5/_static/image15.png)

Kliknięcie przycisku Usuń powoduje usunięcie albumu i zwrócenie do strony indeksu Menedżera sklepu, która pokazuje, że album został usunięty.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Używanie niestandardowego pomocnika HTML do obcinania tekstu

Mamy jeden potencjalny problem z naszą stroną indeksu Menedżera sklepu. Nasz tytuł albumu i właściwości nazwy wykonawcy mogą być wystarczająco długie, aby mogły zgłosić nasze formatowanie tabeli. Utworzymy niestandardowy pomocnik HTML, aby umożliwić nam łatwe obcinanie tych i innych właściwości w naszych widokach.

![](mvc-music-store-part-5/_static/image17.png)

Składnia @helper Razor ułatwia tworzenie własnych funkcji pomocnika do użycia w widokach. Otwórz widok/Views/StoreManager/Index.cshtml i Dodaj następujący kod bezpośrednio po wierszu @model.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Ta metoda pomocnika przyjmuje ciąg i maksymalną dozwoloną długość. Jeśli podany tekst jest krótszy niż określona długość, pomocnik będzie wyprowadzał go jako-is. Jeśli jest dłuższa, obcina tekst i renderuje "..." dla reszty.

Teraz możemy użyć naszego pomocnika obcinania, aby upewnić się, że właściwości tytułu albumu i wykonawcy mają mniej niż 25 znaków. Pełny kod widoku przy użyciu naszego nowego pomocnika obcinania pojawia się poniżej.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Teraz, gdy przeglądasz adres URL/StoreManager/, albumy i tytuły będą przechowywane poniżej naszych maksymalnych długości.

![](mvc-music-store-part-5/_static/image18.png)

Uwaga: pokazuje prosty przypadek tworzenia i używania pomocnika w jednym widoku. Aby dowiedzieć się więcej na temat tworzenia pomocników, których możesz użyć w całej lokacji, zobacz mój wpis w blogu: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-4.md)
> [dalej](mvc-music-store-part-6.md)
