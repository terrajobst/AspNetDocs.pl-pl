---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Obsługa wpisu formularza danych CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie) | Microsoft Docs
author: microsoft
description: Krok 5 pokazuje, jak utworzyć nasze klasy DinnersController przez włączenie obsługi edytowania, tworzenia i usuwania obiadów z nim.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: b3123af9a1477bc496a0d229d628510fc202b6d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580557"
---
# <a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Włączanie obsługi operacji CRUD (tworzenia, odczytu, aktualizacji i usuwania) w formularzach danych

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 5 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 5 pokazuje, jak utworzyć nasze klasy DinnersController przez włączenie obsługi edytowania, tworzenia i usuwania obiadów z nim.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner krok 5. Tworzenie, aktualizowanie i usuwanie scenariuszy formularzy

Wprowadziliśmy kontrolery i widoki oraz opisano, jak ich używać do implementowania obsługi list/szczegółów w przypadku obiadów w witrynie. Naszym następnym krokiem będzie dalsze przejęcie naszej klasy DinnersController i umożliwi obsługę edytowania, tworzenia i usuwania obiadów z nim.

### <a name="urls-handled-by-dinnerscontroller"></a>Adresy URL obsługiwane przez DinnersController

Wcześniej dodałeś metody akcji, aby DinnersController, że zaimplementowano obsługę dwóch adresów URL: */Dinners* i */Dinners/Details/[ID]* .

| **Adres URL** | **CZASOWNIK** | **Przeznaczenie** |
| --- | --- | --- |
| */Dinners/* | GET | Wyświetl listę HTML z nadchodzących obiadów. |
| */Dinners/Details/[ID]* | GET | Wyświetl szczegóły dotyczące określonego obiadu. |

Teraz dodamy metody akcji, aby zaimplementować trzy dodatkowe adresy URL: */Dinners/Edit/[ID]* , */Dinners/Create*i */Dinners/delete/[ID]* . Te adresy URL umożliwiają obsługę edytowania istniejących obiadów, tworzenia nowych obiadów i usuwania obiadów.

Będziemy obsługiwać interakcje HTTP GET i HTTP POST z tymi nowymi adresami URL. Żądania HTTP GET do tych adresów URL będą wyświetlały początkowy widok HTML danych (formularz wypełniony danymi obiadu w przypadku "Edytuj", pusty formularz w przypadku wystąpienia "Utwórz" i "Usuń"). Żądania POST protokołu HTTP dotyczące tych adresów URL będą zapisywać/aktualizować/usuwać dane z obiadu w naszych DinnerRepository (i z tego miejsca do bazy danych).

| **Adres URL** | **CZASOWNIK** | **Przeznaczenie** |
| --- | --- | --- |
| */Dinners/Edit/[ID]* | GET | Wyświetl edytowalny formularz HTML wypełniony danymi obiadu. |
| POST | Zapisz formularz zmiany dla określonego obiadu w bazie danych. |
| */Dinners/Create* | GET | Wyświetlanie pustego formularza HTML, który umożliwia użytkownikom definiowanie nowych obiadów. |
| POST | Utwórz nowy obiad i Zapisz go w bazie danych. |
| */Dinners/Delete/[ID]* | GET | Wyświetl ekran potwierdzania usuwania. |
| POST | Usuwa określony obiad z bazy danych. |

### <a name="edit-support"></a>Edytuj pomoc techniczną

Zacznijmy od zaimplementowania scenariusza "Edytuj".

#### <a name="the-http-get-edit-action-method"></a>Metoda akcji edycji HTTP-GET

Zaczniemy od zaimplementowania zachowania "GET" protokołu HTTP dla naszej metody działania Edit. Ta metoda zostanie wywołana, gdy zostanie żądany adres URL */Dinners/Edit/[ID]* . Nasza implementacja będzie wyglądać następująco:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Powyższy kod używa DinnerRepository do pobrania obiektu obiadu. Następnie renderuje szablon widoku przy użyciu obiektu obiadu. Ponieważ nie przekazał jawnie nazwy szablonu do metody pomocnika *View ()* , użyje domyślnej ścieżki na podstawie Konwencji do rozpoznania szablonu widoku:/views/Dinners/Edit.aspx.

Utwórzmy teraz ten szablon widoku. W tym celu należy kliknąć prawym przyciskiem myszy w metodzie edycji i wybrać polecenie menu kontekstowego "Dodaj widok":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

W oknie dialogowym "Dodawanie widoku" będziemy informować, że przekazujemy obiekt obiadu do naszego szablonu widoku jako model i wybierzesz opcję tworzenia autoszkieletu szablonu "Edytuj":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Po kliknięciu przycisku "Dodaj" program Visual Studio doda nowy plik szablonu widoku "Edit. aspx" dla nas w katalogu "\Views\Dinners". Zostanie również otwarty nowy szablon widoku "Edytuj. aspx" w edytorze kodu — wypełniony początkową implementacją szkieletu "Edytuj", taką jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Wprowadźmy kilka zmian do domyślnej wygenerowanej szkieletu "Edytuj" i zaktualizuj szablon widoku do edycji, aby zawierał zawartość poniższą (co spowoduje usunięcie kilku właściwości, które nie powinny zostać ujawnione):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Po uruchomieniu aplikacji i poprosić o adres URL *"/Dinners/Edit/1"* zostanie wyświetlona następująca strona:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Znaczniki HTML wygenerowane przez nasz widok wyglądają jak poniżej. Jest to standardowy kod HTML — z &lt;formularz&gt;, który wykonuje żądanie HTTP POST na adres URL */Dinners/Edit/1* , gdy przycisk "zapisz" &lt;dane wejściowe typu "Prześlij"/&gt; jest wypychany. Typ danych wejściowych &lt;HTML "text"/&gt; został wyprowadzony dla każdej edytowanej właściwości:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Metody pomocnika html. BeginForm () i HTML. TextBox ()

Nasz szablon widoku "Edit. aspx" korzysta z kilku metod "pomocnika HTML": html. podsumowania walidacji (), html. BeginForm (), html. TextBox () i HTML. ValidationMessage (). Oprócz generowania znaczników HTML dla nas, te metody pomocnika zapewniają wbudowaną obsługę błędów i obsługę walidacji.

##### <a name="htmlbeginform-helper-method"></a>Metoda pomocnika html. BeginForm ()

Metoda pomocnika html. BeginForm () polega na tym, co dane wyjściowe w formularzu HTML &lt;&gt; elementu w naszym znaczniku. W naszym szablonie widoku Edit. aspx można zauważyć, że podczas korzystania z C# tej metody stosujemy instrukcję "Using". Otwarte nawiasy klamrowe wskazują początek &lt;formularza&gt; zawartość, a zamykający nawias klamrowy wskazuje koniec elementu&gt; &lt;formularzy:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Alternatywnie, jeśli okaże się, że instrukcja "Using" nie jest naturalna dla scenariusza takiego jak to, można użyć kombinacji html. BeginForm () i HTML. EndForm () (która jest taka sama):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Wywołanie metody html. BeginForm () bez żadnych parametrów spowoduje, że będzie on wyprowadzać element formularza, który wykonuje żądanie HTTP POST w adresie URL bieżącego żądania. To dlatego, że nasz widok do edycji generuje *&lt;formularz Action = "/Dinners/Edit/1" Metoda = "post"&gt;* elementu. Możemy również przekazywać jawne parametry do języka HTML. BeginForm (), jeśli chcemy ogłosić na innym adresie URL.

##### <a name="htmltextbox-helper-method"></a>Metoda pomocnika html. TextBox ()

Nasz widok edit. aspx używa metody pomocnika html. TextBox () do wyprowadzania &lt;input type = "text"/&gt; elementów:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Powyższa metoda html. TextBox () przyjmuje jeden parametr, który jest używany do określenia zarówno atrybutów identyfikatora, jak i nazwy elementu &lt;input type = "text"/&gt;, do danych wyjściowych, a także właściwości model do wypełniania wartości TextBox. Na przykład obiekt obiadu, który został przesłany do widoku edycji, miał wartość właściwości "title" ".NET futures", a więc nasze html. TextBox ("title") Metoda wywołania metody: *&lt;Input ID = "title" name = "title" Type = "text" value = ". NET futures"/&gt;* .

Alternatywnie można użyć pierwszego parametru html. TextBox () do określenia identyfikatora/nazwy elementu, a następnie jawnie przekazać wartość do użycia jako drugi parametr:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Często będziemy chcieć przeprowadzić niestandardowe formatowanie na wartości, która jest wyjściowa. Metoda statyczna String. Format () wbudowana w .NET jest przydatna dla tych scenariuszy. Nasz szablon widoku edytowania. aspx służy do formatowania wartości EventDate (która jest typu DateTime), tak aby nie pokazywała sekund przez czas:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Trzeci parametr do HTML. TextBox () może być opcjonalnie używany do wyprowadzania dodatkowych atrybutów HTML. Poniższy fragment kodu ilustruje sposób renderowania dodatkowego atrybutu size = "30" i atrybutu class = "mycssclass" w elemencie &lt;input type = "text"/&gt;. Zwróć uwagę na to, jak zmieniamy nazwę atrybutu klasy przy użyciu "@" character because "Class" jest zastrzeżonym słowem kluczowym C#w:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementowanie metody akcji edycji HTTP-POST

Mamy już zaimplementowaną metodę edycji HTTP-GET. Gdy użytkownik zażąda adresu URL */Dinners/Edit/1* , otrzymuje stronę HTML, podobną do następującej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Naciśnięcie przycisku "Zapisz" powoduje wysłanie formularza na adres URL */Dinners/Edit/1* i przesłanie wartości formularza&gt; danych wejściowych &lt;HTML przy użyciu zlecenia http post. Zaimplementujmy teraz zachowanie POST protokołu HTTP dla naszej metody działania Edit — która będzie obsługiwać zapisywanie obiadu.

Zaczniemy od dodania do naszego DinnersController przeciążonej metody akcji "Edytuj", która ma atrybut "AcceptVerbs", który wskazuje, że obsługuje scenariusze POST protokołu HTTP:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Gdy atrybut [AcceptVerbs] jest stosowany do przeciążonych metod akcji, ASP.NET MVC automatycznie obsługuje wysyłanie żądań do odpowiedniej metody działania w zależności od przychodzącego zlecenia HTTP. Żądania POST protokołu HTTP do adresów URL */Dinners/Edit/[ID]* przejdą do powyższej metody edycji, podczas gdy wszystkie inne żądania HTTP dotyczące */Dinners/Edit/[ID]* zostaną uwzględnione w pierwszej zaimplementowanej metodzie edycji (która nie ma atrybutu `[AcceptVerbs]`).

| **Temat po stronie: Dlaczego różni się za pośrednictwem czasowników HTTP?** |
| --- |
| Możesz zażądać — Dlaczego używamy jednego adresu URL i odróżniasz jego zachowanie za pośrednictwem zlecenia HTTP? Dlaczego nie mają już dwóch oddzielnych adresów URL do obsługi ładowania i zapisywania zmian edycji? Na przykład:/Dinners/Edit/[ID], aby wyświetlić początkowy formularz i/Dinners/Save/[ID], aby obsłużyć post formularza do jego zapisania? Minusem z publikowaniem dwóch odrębnych adresów URL to w przypadkach, w których wpisuje się do/Dinners/Save/2, a następnie musi ponownie wyświetlić formularz HTML z powodu błędu wejścia, użytkownik końcowy będzie miał adres URL/Dinners/Save/2 na pasku adresu przeglądarki (ponieważ był to adres URL w formularzu ogłoszonym). Jeśli użytkownik końcowy zakończył Tę stronę ponownie na liście ulubionych przeglądarki lub skopiuje/wklei adres URL i wyśle wiadomość e-mail do znajomego, spowoduje to zakończenie zapisywania adresu URL, który nie będzie działał w przyszłości (ponieważ ten adres URL zależy od wartości post). Przez udostępnienie pojedynczego adresu URL (na przykład:/Dinners/Edit/[ID]) i odróżnienie jego przetwarzania przez czasownik HTTP, jest bezpieczne dla użytkowników końcowych do zakładek na stronie edytowania i/lub wysłania adresu URL do innych osób. |

#### <a name="retrieving-form-post-values"></a>Pobieranie wartości post formularza

Istnieją różne sposoby uzyskiwania dostępu do opublikowanych parametrów formularza w ramach metody "Edit" protokołu HTTP POST. Jednym z prostych metod jest użycie właściwości Request w klasie podstawowej kontrolera w celu uzyskania dostępu do kolekcji formularzy i bezpośredniego pobrania opublikowanych wartości:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Powyższe podejście jest nieco pełne, chociaż, szczególnie po dodaniu logiki obsługi błędów.

Lepszym rozwiązaniem w tym scenariuszu jest wykorzystanie wbudowanej metody pomocnika *UpdateModel ()* w klasie podstawowej kontrolera. Obsługuje ona aktualizowanie właściwości obiektu przekazanego za pomocą parametrów formularza przychodzącego. Używa odbicia do określenia nazw właściwości w obiekcie, a następnie automatycznie konwertuje i przypisuje do nich wartości na podstawie wartości wejściowych przesłanych przez klienta.

Możemy użyć metody UpdateModel () w celu uproszczenia akcji edycji HTTP-POST przy użyciu tego kodu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Teraz możemy odwiedzać adres URL */Dinners/Edit/1* i zmienić tytuł naszego obiadu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Gdy klikniesz przycisk "Zapisz", wyślemy formularz do naszej akcji edycji, a zaktualizowane wartości zostaną utrwalone w bazie danych. Następnie zostanie przekierowany do adresu URL szczegółów dla obiadu (co spowoduje wyświetlenie nowo zapisanych wartości):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Obsługa błędów edycji

Nasze bieżące implementacje HTTP-POST działają prawidłowo — z wyjątkiem przypadków, gdy występują błędy.

Gdy użytkownik błędnie edytuje formularz, musimy upewnić się, że formularz jest ponownie wyświetlany z komunikatem o błędzie informacyjnym, który przeprowadzi te instrukcje. Obejmuje to przypadki, w których użytkownik końcowy zapisuje nieprawidłowe dane wejściowe (na przykład nieprawidłowo sformułowany ciąg daty), a także przypadki, w których format wejściowy jest prawidłowy, ale istnieje naruszenie reguły biznesowej. Gdy wystąpią błędy, formularz powinien zachować wprowadzone przez użytkownika dane wejściowe, aby nie musieli ręcznie wypełniać zmian. Ten proces powinien powtarzać się tyle razy, ile jest to konieczne do pomyślnego zakończenia formularza.

ASP.NET MVC zawiera pewne wbudowane funkcje, które umożliwiają obsługę błędów i szybkie wyświetlanie formularzy. Aby wyświetlić te funkcje w akcji, zaktualizujmy naszą metodę edycji akcji przy użyciu następującego kodu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Powyższy kod jest podobny do poprzedniej implementacji — z tą różnicą, że teraz zawijamy blok obsługi błędów try/catch wokół naszej pracy. Jeśli wystąpi wyjątek podczas wywoływania UpdateModel () lub gdy spróbujemy i zapiszemy DinnerRepository (co spowoduje wyjątek, jeśli obiekt obiadu, który próbujesz zapisać, jest nieprawidłowy z powodu naruszenia reguły w naszym modelu), nasz blok obsługi błędów catch zostanie wykonana. W ramach tego kroku przejdziemy przez wszystkie naruszenia reguł, które istnieją w obiekcie obiadu i dodają je do obiektu ModelState (co wkrótce omówię). Następnie ponownie wyświetli się widok.

Aby zobaczyć, jak to działa, spróbuj uruchomić aplikację, edytować obiad i zmienić jej wartość na pustą, EventDate "prawdziwa" i użyć BRYTYJSKIego numeru telefonu z wartością kraju USA. Po naciśnięciu przycisku "Zapisz" Nasza metoda HTTP POST nie będzie mogła zapisać obiadu (ponieważ wystąpiły błędy) i ponownie wyświetlić formularz:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Nasza aplikacja ma znośnego błąd. Elementy tekstowe z nieprawidłowymi danymi wejściowymi są wyróżnione na czerwono, a do użytkownika końcowego są wyświetlane komunikaty o błędach. Formularz zachowuje również dane wejściowe wprowadzone przez użytkownika, aby nie musieli w pełni wprowadzać żadnych danych.

Jak można zażądać, czy wystąpił błąd? W jaki sposób pola texttitle, EventDate i ContactPhone wyróżnią się w kolorze czerwonym i wiedzą, że dane wyjściowe wprowadzono pierwotnie wprowadzone wartości użytkownika? A jak komunikaty o błędach są wyświetlane na liście u góry? Dobrym komunikatem jest to, że nie było to spowodowane przez Magic, ponieważ użyto niektórych wbudowanych funkcji ASP.NET MVC, które ułatwiają sprawdzanie poprawności danych wejściowych i obsługę błędów.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Informacje o ModelState i metodach pomocników HTML weryfikacji

Klasy kontrolerów mają kolekcję właściwości "ModelState", która umożliwia wskazanie, że błędy istnieją w obiekcie modelu, który jest przesyłany do widoku. Wpisy błędów w kolekcji ModelState identyfikują nazwę właściwości modelu z problemem (na przykład: "title", "EventDate" lub "ContactPhone") i zezwalają na określenie przyjaznego dla człowieka komunikatu o błędzie (na przykład: "tytuł jest wymagany").

Metoda pomocnika *UpdateModel ()* automatycznie wypełnia kolekcję ModelState, gdy wystąpią błędy podczas próby przypisania wartości formularza do właściwości obiektu model. Na przykład właściwość EventDate obiektu obiadu jest typu DateTime. Gdy metoda UpdateModel () nie mogła przypisać wartości ciągu "fałszywe" do niego w powyższym scenariuszu, Metoda UpdateModel () dodała wpis do kolekcji ModelState wskazującą, że wystąpił błąd przypisania z tą właściwością.

Deweloperzy mogą również napisać kod, aby jawnie dodać wpisy błędów do kolekcji ModelState, tak jak to robimy w naszym bloku obsługi błędów "Catch", który wypełnia kolekcję ModelState z wpisami opartymi na aktywnych naruszeniach reguł w Obiekt obiadu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integracja pomocnika HTML z ModelState

Metody pomocników HTML — w tym HTML. TextBox () — Sprawdź kolekcję ModelState podczas renderowania danych wyjściowych. Jeśli istnieje błąd elementu, renderuje on wartość wprowadzoną przez użytkownika i klasę błędu CSS.

Na przykład w naszym widoku "Edytuj" używamy metody pomocnika html. TextBox () do renderowania EventDate naszego obiektu obiadu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Gdy widok został renderowany w scenariuszu błędu, Metoda html. TextBox () zaewidencjonuje kolekcję ModelState, aby sprawdzić, czy wystąpiły błędy związane z właściwością "EventDate" naszego obiektu obiadu. Po ustaleniu, że wystąpił błąd podczas renderowania przesłanych danych wejściowych użytkownika ("prawdziwa") jako wartość i Dodano klasę błędu CSS do &lt;input type = "TextBox"/&gt; znacznikiem, które wygenerowało:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Możesz dostosować wygląd klasy błędu CSS, aby wyglądało na to, jak chcesz. Domyślna Klasa błędów CSS — "Input-Validation-Error" — jest definiowana w arkuszu stylów *\content\site.css* i wygląda następująco:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Ta reguła CSS zawiera informacje o tym, co spowodowało nieprawidłowe elementy wejściowe, tak jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() Helper Method

Metoda pomocnika html. ValidationMessage () może służyć do wyprowadzania komunikatu o błędzie ModelState skojarzonego z określoną właściwością modelu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Powyższe dane wyjściowe kodu: *&lt;span class = "pole-Walidacja-Error"&gt; wartość "prawdziwa" jest nieprawidłowa&lt;/span&gt;*

Metoda pomocnika html. ValidationMessage () obsługuje również drugi parametr, który umożliwia deweloperom przesłonięcie wyświetlanego komunikatu tekstowego błędu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Powyższy kod wyprowadza: *&lt;span class = "pole-Walidacja-Error"&gt;\*&lt;/span&gt;* zamiast domyślnego tekstu błędu, gdy występuje błąd dla właściwości EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Metoda pomocnika html. podsumowania walidacji ()

Metoda pomocnika html. podsumowania walidacji () może służyć do renderowania podsumowującego komunikatu o błędzie, dołączonego &lt;ul&gt;&lt;li/&gt;&lt;/ul&gt; listę wszystkich szczegółowych komunikatów o błędach w kolekcji ModelState:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Metoda pomocnika html. podsumowania walidacji () przyjmuje opcjonalny parametr ciągu, który definiuje skrócony komunikat o błędzie wyświetlany powyżej listy szczegółowych błędów:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Opcjonalnie możesz użyć CSS, aby przesłonić, jak wygląda Lista błędów.

#### <a name="using-a-addruleviolations-helper-method"></a>Użycie metody pomocnika AddRuleViolations

Nasza początkowa implementacja protokołu HTTP-POST edycji użyła instrukcji foreach w bloku catch, aby wykonać pętlę przed naruszenia reguły obiektu obiadu i dodać je do kolekcji ModelState kontrolera:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Możemy sprawić, aby kod był nieco bardziej czytelny poprzez dodanie klasy "ControllerHelpers" do projektu NerdDinner i zaimplementowanie w niej metody rozszerzenia "AddRuleViolations", która dodaje metodę pomocnika do klasy ASP.NET MVC ModelStateDictionary. Ta metoda rozszerzania może hermetyzować logikę niezbędną do wypełnienia ModelStateDictionary z listą błędów RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Następnie możemy zaktualizować metodę akcji edycji HTTP-POST, aby użyć tej metody rozszerzenia do wypełnienia kolekcji ModelState z naszymi naruszeniami reguł obiadu.

#### <a name="complete-edit-action-method-implementations"></a>Ukończ implementacje implementacji metod akcji

Poniższy kod implementuje całą logikę kontrolera wymaganą przez nasz scenariusz edycji:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Warto pamiętać o naszej implementacji edycji, ponieważ nie ma żadnej z naszych klas kontrolera ani naszego szablonu widoku, aby nie wiedzieć niczego, co jest wymuszane na podstawie reguły sprawdzania poprawności lub reguł firmy realizowanych przez nasz model obiadu. W przyszłości możemy dodać do naszego modelu dodatkowe reguły i nie trzeba wprowadzać żadnych zmian w kodzie w naszym kontrolerze lub widoku w celu ich obsługi. Dzięki temu można łatwo rozwijać wymagania dotyczące aplikacji w przyszłości przy minimalnych zmianach w kodzie.

### <a name="create-support"></a>Utwórz pomoc techniczną

Zakończono implementację zachowania "Edit" klasy DinnersController. Teraz przejdźmy do wdrożenia na nim obsługi "Utwórz", co umożliwi użytkownikom dodawanie nowych obiadów.

#### <a name="the-http-get-create-action-method"></a>Metoda działania HTTP-GET Create

Zaczniemy od zaimplementowania zachowania "GET" protokołu HTTP dla naszej metody akcji Create. Ta metoda zostanie wywołana, gdy ktoś odwiedzi adres URL */Dinners/Create* . Nasza implementacja wygląda następująco:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Powyższy kod tworzy nowy obiekt obiadu i przypisuje jego właściwość EventDate jako tydzień w przyszłości. Następnie renderuje widok, który jest oparty na nowym obiekcie obiadu. Ponieważ nie przekazałeś jawnie nazwy do metody pomocnika *View ()* , użyje domyślnej ścieżki na podstawie Konwencji do rozpoznania szablonu widoku:/views/Dinners/Create.aspx.

Utwórzmy teraz ten szablon widoku. Można to zrobić, klikając prawym przyciskiem myszy w ramach metody tworzenia akcji i wybierając polecenie menu kontekstowego "Dodaj widok". W oknie dialogowym "Dodawanie widoku" będziemy informować, że przekazujemy obiekt obiadu do szablonu widoku i wybierzesz opcję autoszkieletu szablonu "Create":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Po kliknięciu przycisku "Dodaj" program Visual Studio zapisze nowy widok "Create. aspx" oparty na tworzeniu szkieletu w katalogu "\Views\Dinners" i otworzy go w środowisku IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Wprowadźmy kilka zmian w domyślnym pliku szkieletu "Create", który został wygenerowany dla nas, i zmodyfikuj go tak, aby wyglądał jak poniżej:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Teraz, gdy uruchamiamy naszą aplikację i uzyskuję dostęp do adresu URL *"/Dinners/Create"* w przeglądarce, będzie on renderować interfejs użytkownika, na przykład poniżej, z naszej implementacji akcji tworzenia:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementowanie metody akcji Create protokołu HTTP-POST

Mamy zaimplementowaną wersję protokołu HTTP-GET dla naszej metody akcji Create. Gdy użytkownik kliknie przycisk "Zapisz", wykonuje formularz post w adresie URL */Dinners/Create* i prześle wartości formularza&gt; danych wejściowych w formacie &lt;HTML przy użyciu zlecenia http post.

Zaimplementujmy teraz zachowanie POST protokołu HTTP dla naszej metody akcji Create. Zaczniemy od dodania do naszego DinnersController przeciążonej metody akcji "Create", która ma atrybut "AcceptVerbs", który wskazuje, że obsługuje scenariusze POST protokołu HTTP:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Istnieją różne sposoby uzyskiwania dostępu do opublikowanych parametrów formularza w ramach metody "Create" w ramach protokołu HTTP-POST.

Jednym z rozwiązań jest utworzenie nowego obiektu obiadu, a następnie użycie metody pomocnika *UpdateModel ()* (podobnie jak w przypadku akcji Edytuj), aby wypełnić ją wartościami zaksięgowanymi formularza. Następnie możemy dodać go do naszego DinnerRepository, zachować go w bazie danych i przekierować użytkownika do naszej akcji szczegółowej, aby wyświetlić nowo utworzony obiad przy użyciu poniższego kodu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternatywnie możemy użyć metody akcji Create (), która przyjmuje obiekt obiadu jako parametr metody. ASP.NET MVC automatycznie utworzy nowy obiekt obiadu dla nas, wypełni jego właściwości przy użyciu wejść formularzy i przekaże go do naszej metody działania:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Nasza metoda działania powyżej sprawdza, czy obiekt obiadu został pomyślnie wypełniony przy użyciu formularza post wartości, sprawdzając Właściwość ModelState. IsValid. Spowoduje to zwrócenie wartości false, jeśli wystąpią problemy z konwersją danych wejściowych (na przykład ciąg "fałszywe" dla właściwości EventDate), a w przypadku problemów z tą metodą akcji ponownie jest wyświetlany formularz.

Jeśli wartości wejściowe są prawidłowe, Metoda akcji próbuje dodać i zapisać nowy obiad do DinnerRepository. Ta praca jest zawijana w bloku try/catch i ponownie wyświetla formularz w przypadku naruszeń reguł firmy (co może spowodować wystąpienie wyjątku metody dinnerRepository. Save ().

Aby wyświetlić to zachowanie obsługi błędów w działaniu, możemy zażądać adresu URL */Dinners/Create* i uzupełnić szczegóły dotyczące nowego obiadu. Nieprawidłowe dane wejściowe lub wartości spowodują, że formularz tworzenia zostanie wyświetlony ponownie z błędami wyróżnionymi poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Zwróć uwagę, w jaki sposób nasz formularz jest uznawany za dokładnie te same reguły sprawdzania poprawności i reguł firmy co nasz formularz edycji. Wynika to z faktu, że nasze reguły weryfikacji i biznesowe zostały zdefiniowane w modelu i nie zostały osadzone w interfejsie użytkownika lub kontrolerze aplikacji. Oznacza to, że możemy później zmienić/przystąpić nasze reguły weryfikacji lub reguł firmy w jednym miejscu i zastosować je w całej naszej aplikacji. Nie trzeba zmieniać żadnego kodu w ramach naszych metod edycji ani tworzenia akcji, aby automatycznie przestrzegać nowych reguł lub modyfikacji istniejących.

Po naprawieniu wartości wejściowych zostanie ponownie kliknięty przycisk "Zapisz", dodanie do DinnerRepository powiedzie się, a nowy obiad zostanie dodany do bazy danych. Następnie zostanie przekierowany do adresu URL */Dinners/Details/[ID]* — w którym zostaną wyświetlone szczegółowe informacje na temat nowo utworzonego obiadu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Usuń pomoc techniczną

Dodajmy teraz obsługę "usuwania" do naszego DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Metoda akcji usuwania HTTP-GET

Zaczniemy od implementacji zachowania HTTP GET dla naszej metody akcji usuwania. Ta metoda zostanie wywołana, gdy ktoś odwiedzi adres URL */Dinners/delete/[ID]* . Poniżej przedstawiono implementację:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Metoda akcji próbuje pobrać obiad do usunięcia. Jeśli istnieje obiad, renderuje widok oparty na obiekcie obiadu. Jeśli obiekt nie istnieje (lub został już usunięty) zwraca widok, który renderuje utworzony wcześniej szablon widoku "NotFound" dla naszej metody akcji "Details".

Możemy utworzyć szablon widoku "Delete", klikając prawym przyciskiem myszy metodę usuwania akcji i wybierając polecenie menu kontekstowego "Dodaj widok". W oknie dialogowym "Dodawanie widoku" będziemy informować, że przekazujemy obiekt obiadu do naszego szablonu widoku jako model i wybierzesz opcję utworzenia pustego szablonu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Po kliknięciu przycisku "Dodaj" program Visual Studio doda nowy plik szablonu widoku "Delete. aspx" dla nas w katalogu "\Views\Dinners". Dodamy do szablonu kod HTML i szablon w celu zaimplementowania ekranu potwierdzenia usuwania, jak pokazano poniżej:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Powyższy kod wyświetla tytuł obiadu, który ma zostać usunięty, i wyprowadza &lt;formularz&gt; elementu, który wysyła wpis do adresu URL/Dinners/Delete/[ID], jeśli użytkownik końcowy kliknie w nim przycisk "Usuń".

W przypadku uruchamiania naszej aplikacji i uzyskiwania dostępu do adresu URL *"/Dinners/delete/[ID]"* dla prawidłowego obiektu obiadu jest renderowany następujący interfejs użytkownika:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Temat po stronie: Dlaczego wykonujemy wpis?** |
| --- |
| Możesz zadawać pytania — dlaczego udało nam się utworzyć &lt;formularz&gt; na naszym ekranie potwierdzenia usunięcia? Dlaczego nie tylko używać standardowego hiperlinku do łączenia z metodą akcji, która wykonuje rzeczywistą operację usuwania? Przyczyną jest to, że chcemy zachować ostrożność przed przeszukiwaniem sieci Web i wyszukiwarkami odnajdywania naszych adresów URL, a nieumyślnie powoduje usunięcie danych po ich zakończeniu. Adresy URL przy użyciu protokołu HTTP-GET są uznawane za bezpieczne dla użytkowników w celu uzyskania dostępu/przeszukiwania i dlatego nie są zgodne z WPISami HTTP. Dobrą regułą jest upewnienie się, że zawsze można wprowadzać destrukcyjne lub modyfikujące dane operacji za żądaniami POST protokołu HTTP. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementowanie metody akcji HTTP-POST Delete

Dostępna jest teraz funkcja HTTP-GET z zaimplementowaną metodą akcji usuwania, która wyświetla ekran potwierdzenia usuwania. Gdy użytkownik końcowy kliknie przycisk "Usuń", spowoduje wykonanie formularza post w adresie URL */Dinners/Dinner/[ID]* .

Zaimplementujmy teraz zachowanie protokołu HTTP "POST" metody usuwania akcji przy użyciu poniższego kodu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Wersja HTTP-POST naszej metody akcji usuwania próbuje pobrać obiekt obiadu do usunięcia. Jeśli nie można go znaleźć (ponieważ został już usunięty), renderuje nasz szablon "NotFound". Jeśli wykryje obiad, usunie go z DinnerRepository. Następnie renderuje szablon "usunięty".

Aby zaimplementować szablon "usunięty", kliknij prawym przyciskiem myszy w metodzie akcji i wybierz menu kontekstowe "Dodaj widok". Zostanie wyświetlona nazwa naszego widoku "usunięty" i będzie on mieć pusty szablon (a nie przyjmuje obiektu modelu o jednoznacznie określonym typie). Następnie dodamy do niej zawartość HTML:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Teraz, gdy uruchamiamy naszą aplikację i uzyskuję dostęp do adresu URL *"/Dinners/delete/[ID]"* dla prawidłowego obiektu obiadu, zobaczymy ekran potwierdzający usunięcie Twojego obiadu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Po kliknięciu przycisku "Usuń" spowoduje to wykonanie polecenia HTTP POST w adresie URL */Dinners/delete/[ID]* , co spowoduje usunięcie obiadu z naszej bazy danych i wyświetlenie szablonu widoku "usunięte":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Zabezpieczenia powiązań modelu

Omawiamy dwa różne sposoby używania wbudowanych funkcji powiązania modelu ASP.NET MVC. Pierwsze użycie metody UpdateModel () do aktualizowania właściwości w istniejącym obiekcie modelu, a drugi przy użyciu obsługi ASP.NET MVC do przekazywania obiektów modelu w jako parametry metody akcji. Obie te techniki są bardzo wydajne i niezwykle użyteczne.

Ta moc również wiąże się z odpowiedzialnością IT. Ważne jest, aby zawsze to informacje o zabezpieczeniach przy akceptowaniu wszelkich danych wejściowych użytkownika. jest to również prawdziwe w przypadku powiązania obiektów z danymi wejściowymi. Należy zachować ostrożność zawsze Koduj wartości wprowadzone przez użytkownika, aby zapobiec atakom w języku HTML i kodzie JavaScript oraz zachować ostrożność w przypadku ataków iniekcji SQL (Uwaga: używamy LINQ to SQL aplikacji, która automatycznie koduje parametry, aby zapobiec typy ataków). Nigdy nie należy polegać wyłącznie na weryfikacji po stronie klienta i zawsze stosować walidację po stronie serwera, aby chronić przed hakerami próbującymi wysyłać nieprawdziwych wartości.

Jednym z dodatkowych elementów zabezpieczeń, które należy wziąć pod uwagę podczas korzystania z funkcji powiązania ASP.NET MVC, jest zakres obiektów, które są powiązane. W związku z tym należy upewnić się, że rozumiesz implikacje zabezpieczeń właściwości, które można powiązać, i upewnij się, że tylko te właściwości mają być aktualizowane przez użytkownika końcowego, który ma zostać zaktualizowany.

Domyślnie metoda UpdateModel () podejmie próbę zaktualizowania wszystkich właściwości obiektu modelu, które pasują do wartości parametrów formularza przychodzącego. Podobnie obiekty przekazane jako parametry metody akcji również domyślnie mogą mieć wszystkie właściwości ustawiane za pośrednictwem parametrów formularza.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Blokowanie powiązania dla poszczególnych użycia

Zasady powiązań można zablokować na podstawie użycia, dostarczając jawną "dołączaną listę" właściwości, które można zaktualizować. Można to zrobić przez przekazanie dodatkowego parametru tablicy ciągów do metody UpdateModel () podobnej do poniższego:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Obiekty przenoszone jako parametry metody akcji obsługują również atrybut [bind], który umożliwia określenie "include list" dozwolonych właściwości, które można określić w następujący sposób:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Blokowanie powiązania w poziomie na podstawie typu

Można także zablokować reguły powiązań dla poszczególnych typów. Dzięki temu można określić reguły powiązań raz, a następnie zastosować je we wszystkich scenariuszach (w tym scenariusze parametrów UpdateModel i metody akcji) dla wszystkich kontrolerów i metod akcji.

Można dostosować reguły powiązań dla poszczególnych typów, dodając atrybut [bind] do typu lub przez zarejestrowanie go w pliku Global. asax aplikacji (przydatne w scenariuszach, w których nie jest używany typ). Następnie można użyć właściwości dołączania i wykluczania powiązania, aby kontrolować właściwości, które można powiązać z konkretną klasą lub interfejsem.

Użyjemy tej techniki dla klasy obiadu w naszej aplikacji NerdDinner i dodamy do niej atrybut [bind], który ogranicza listę właściwości możliwych do powiązania do następujących elementów:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Zwróć uwagę, że nie zezwalasz na manipulowanie kolekcją RSVP za pośrednictwem powiązania ani nie zezwolimy na ustawienie właściwości DinnerID lub HostedBy za pośrednictwem powiązania. Ze względów bezpieczeństwa będziemy w zamian manipulować tylko tymi określonymi właściwościami za pomocą jawnego kodu w naszych metodach działania.

### <a name="crud-wrap-up"></a>Zawijanie CRUD

ASP.NET MVC zawiera szereg wbudowanych funkcji, które ułatwiają implementowanie scenariuszy księgowania formularzy. Używamy wielu z tych funkcji, aby zapewnić obsługę interfejsu użytkownika CRUD na DinnerRepository.

Korzystamy z podejścia ukierunkowanego na model do implementowania naszej aplikacji. Oznacza to, że cała Walidacja i logika reguł firmy są zdefiniowane w naszej warstwie modelu — a nie w obrębie naszych kontrolerów lub widoków. Żadna z naszych klas kontrolerów ani szablonów widoków nie wie o konkretnych regułach biznesowych, które są wymuszane przez naszą klasę modelu obiadu.

Dzięki temu nasza architektura aplikacji będzie przejrzysta i łatwiejsza do przetestowania. W przyszłości możemy dodać do naszej warstwy reguły biznesowe i *nie trzeba wprowadzać żadnych zmian w kodzie* w naszym kontrolerze lub widoku w celu ich obsługi. Pozwala to na zapewnienie elastyczności w celu rozdzielenia i zmiany naszej aplikacji w przyszłości.

Nasze DinnersController teraz włączają aukcje/szczegóły obiadu, a także obsługę tworzenia, edytowania i usuwania. Kompletny kod klasy można znaleźć poniżej:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Następny krok

Mamy teraz podstawowe CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie) obsługują implementację w ramach naszej klasy DinnersController.

Teraz przyjrzyjmy się sposobom korzystania z klas ViewData i ViewModel w celu zapewnienia jeszcze bardziej zaawansowanego interfejsu użytkownika w naszych formularzach.

> [!div class="step-by-step"]
> [Poprzednie](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [dalej](use-viewdata-and-implement-viewmodel-classes.md)
