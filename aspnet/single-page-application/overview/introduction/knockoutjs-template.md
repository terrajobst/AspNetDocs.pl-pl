---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Aplikacja jednostronicowa: szablon KnockoutJS | Microsoft Docs'
author: MikeWasson
description: Szablon odcinania
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 3a551db1caa9636eb7f2e04c287d3ef371263584
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578695"
---
# <a name="single-page-application-knockoutjs-template"></a>Aplikacja jednostronicowa: szablon KnockoutJS

według [Jan Wasson](https://github.com/MikeWasson)

> Odcinanie szablonu MVC jest częścią ASP.NET and Web Tools 2012,2
> 
> [Pobierz ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

Aktualizacja ASP.NET and Web Tools 2012,2 obejmuje szablon aplikacji jednostronicowej (SPA) dla ASP.NET MVC 4. Ten szablon został zaprojektowany, aby szybko rozpocząć tworzenie interaktywnych aplikacji sieci Web po stronie klienta.

"Aplikacja jednostronicowa" (SPA) to ogólny termin aplikacji sieci Web, który ładuje pojedynczą stronę HTML, a następnie automatycznie aktualizuje stronę, zamiast ładować nowe strony. Po początkowym załadowaniu strony SPA komunikuje się z serwerem przez żądania AJAX.

![](knockoutjs-template/_static/image1.png)

Technologia AJAX nie ma nic nowego, ale dzisiaj istnieją struktury języka JavaScript, które ułatwiają tworzenie i konserwowanie dużej zaawansowanej aplikacji SPA. Ponadto kod HTML 5 i CSS3 ułatwiają tworzenie bogatych interfejsów użytkownika.

Aby rozpocząć pracę, szablon SPA tworzy przykład aplikacji "Lista czynności do wykonania". W tym samouczku zajmiemy się szablonem. Najpierw przyjrzyjmy się aplikacji z listą zadań do wykonania, a następnie przeanalizować elementy technologii, które go działają.

## <a name="create-a-new-spa-template-project"></a>Utwórz nowy projekt szablonu SPA

Wymagania:

- Visual Studio 2012 lub Visual Studio Express 2012 dla sieci Web
- ASP.NET Web Tools 2012,2 Update. Tę aktualizację można zainstalować w [tym miejscu](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie startowej. Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web MVC 4 ASP.NET**. Nazwij projekt, a następnie kliknij przycisk **OK**.

![](knockoutjs-template/_static/image2.png)

W kreatorze **nowego projektu** wybierz pozycję **aplikacja jednostronicowa**.

![](knockoutjs-template/_static/image3.png)

Naciśnij klawisz F5, aby skompilować i uruchomić aplikację. Po pierwszym uruchomieniu aplikacji zostanie wyświetlony ekran logowania.

![](knockoutjs-template/_static/image4.png)

Kliknij link &quot;Utwórz konto&quot; i Utwórz nowego użytkownika.

![](knockoutjs-template/_static/image5.png)

Po zalogowaniu aplikacja tworzy domyślną listę zadań do zrobienia z dwoma elementami. Możesz kliknąć pozycję "Dodaj listę zadań do zrobienia", aby dodać nową listę.

![](knockoutjs-template/_static/image6.png)

Zmień nazwę listy, Dodaj elementy do listy i sprawdź je. Możesz również usunąć elementy lub usunąć całą listę. Zmiany są automatycznie utrwalane w bazie danych na serwerze (w rzeczywistości LocalDB w tym momencie, ponieważ aplikacja jest uruchamiana lokalnie).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architektura szablonu SPA

Ten diagram przedstawia główne bloki konstrukcyjne dla aplikacji.

![](knockoutjs-template/_static/image8.png)

Po stronie serwera ASP.NET MVC obsługuje kod HTML, a także obsługuje uwierzytelnianie oparte na formularzach.

Interfejs API sieci Web ASP.NET obsługuje wszystkie żądania, które odnoszą się do ToDoLists i ToDoItems, w tym pobieranie, tworzenie, aktualizowanie i usuwanie. Klient wymienia dane z interfejsem API sieci Web w formacie JSON.

Entity Framework (EF) to warstwa O/RM. Koryguje się między ASP.NET zorientowanym na obiekty a podstawową bazą danych. Baza danych używa LocalDB, ale można ją zmienić w pliku Web. config. Zazwyczaj można używać LocalDB do lokalnego tworzenia oprogramowania, a następnie wdrażać je w bazie danych SQL na serwerze przy użyciu programu EF Code-First Migration.

Po stronie klienta Biblioteka odcinania. js obsługuje aktualizacje stron z żądań AJAX. Odcinanie używa powiązania danych do synchronizowania strony z najnowszymi danymi. Dzięki temu nie trzeba pisać żadnego kodu, który przegląda dane JSON i aktualizuje DOM. Zamiast tego należy umieścić atrybuty deklaracyjne w kodzie HTML, które informują o odcinania sposobu prezentowania danych.

Dużą zaletą tej architektury jest oddzielenie warstwy prezentacji od logiki aplikacji. Część internetowego interfejsu API można utworzyć bez znajomości informacji o tym, jak będzie wyglądać Strona sieci Web. Po stronie klienta utworzysz "model widoku", aby reprezentować te dane, a model widoku używa odcinania do powiązania z kodem HTML. Pozwala to łatwo zmieniać Kod HTML bez zmiany modelu widoku. (Wyszukamy nieco odcinania później).

## <a name="models"></a>Modele

W projekcie programu Visual Studio folder modele zawiera modele, które są używane po stronie serwera. (Na stronie klienta znajdują się również modele i są one dostępne).

![](knockoutjs-template/_static/image9.png)

**TodoItem, TodoList**

Są to modele baz danych dla Code First Entity Framework. Zwróć uwagę, że te modele mają właściwości wskazujące na siebie. `ToDoList` zawiera kolekcję ToDoItems, a każda `ToDoItem` ma odwołanie z powrotem do jego nadrzędnej ToDoList. Właściwości te są nazywane właściwościami nawigacji i reprezentują relację "jeden do wielu" i "do".

Klasa `ToDoItem` używa również atrybutu **[ForeignKey]** , aby określić, że `ToDoListId` jest kluczem obcym w tabeli `ToDoList`. Oznacza to, że Dr dodaje do bazy danych ograniczenie klucza obcego.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Te klasy definiują dane, które zostaną wysłane do klienta. "DTO" oznacza "obiekt transferu danych". DTO definiuje sposób serializacji jednostek do formatu JSON. Ogólnie rzecz biorąc, istnieje kilka powodów, dla których należy używać DTO:

- Do kontrolowania, które właściwości są serializowane. DTO może zawierać podzestaw właściwości z modelu domeny. Można to zrobić ze względów bezpieczeństwa (w celu ukrycia poufnych danych) lub po prostu zmniejszyć ilość wysyłanych danych.
- Aby zmienić kształt danych — na przykład, aby spłaszczyć bardziej złożoną strukturę danych.
- Aby zachować logikę biznesową z DTO (separacja problemów).
- Jeśli z jakiegoś powodu nie można serializować modeli domeny. Na przykład odwołania cykliczne mogą spowodować problemy podczas serializacji obiektu. Istnieją sposoby obsługi tego problemu w interfejsie API sieci Web (zobacz [Obsługa cyklicznych odwołań do obiektów](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); Jednak użycie DTO po prostu pozwala uniknąć całkowitego problemu.

W szablonie SPA DTO zawiera te same dane, co modele domen. Jednak nadal są przydatne, ponieważ unikają odwołań cyklicznych ze wszystkich właściwości nawigacji i pokazują ogólny wzorzec DTO.

**AccountModels.cs**

Ten plik zawiera modele przynależności do lokacji. Klasa `UserProfile` definiuje schemat profilów użytkowników w bazie danych członkostwa. (W tym przypadku jedyne informacje to identyfikator użytkownika i nazwa użytkownika). Inne klasy modelu w tym pliku są używane do tworzenia formularzy rejestracji i logowania użytkownika.

## <a name="entity-framework"></a>Entity Framework

Szablon SPA używa EF Code First. W programie Code First Development definiujemy modele najpierw w kodzie, a następnie w celu utworzenia bazy danych program EF korzysta z modelu. Można również użyć EF z istniejącą bazą danych ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

Klasa `TodoItemContext` w folderze models pochodzi z **DbContext**. Ta klasa zawiera "Glue" między modelami i EF. `TodoItemContext` zawiera kolekcję `ToDoItem` i kolekcję `TodoList`. Aby wysłać zapytanie do bazy danych, wystarczy napisać zapytanie LINQ względem tych kolekcji. Na przykład Oto jak można wybrać wszystkie listy czynności do wykonania dla użytkownika "Alicja":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Możesz również dodać nowe elementy do kolekcji, zaktualizować elementy lub usunąć elementy z kolekcji i zachować zmiany w bazie danych.

## <a name="aspnet-web-api-controllers"></a>Kontrolery interfejsu API sieci Web ASP.NET

W interfejsie Web API ASP.NET kontrolery są obiektami, które obsługują żądania HTTP. Jak wspomniano, szablon SPA używa interfejsu API sieci Web, aby włączyć operacje CRUD na wystąpieniach `ToDoList` i `ToDoItem`. Kontrolery znajdują się w folderze controllers rozwiązania.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: obsługuje żądania HTTP dla elementów do wykonania
- `TodoListController`: obsługuje żądania HTTP dla list czynności do wykonania.

Te nazwy są istotne, ponieważ interfejs API sieci Web dopasowuje ścieżkę identyfikatora URI do nazwy kontrolera. (Aby dowiedzieć się, jak interfejs API sieci Web kieruje żądania HTTP do kontrolerów, zobacz [Routing in Web api ASP.NET](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Przyjrzyjmy się klasie `ToDoListController`. Zawiera pojedynczy element członkowski danych:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext` jest używany do komunikowania się z EF zgodnie z wcześniejszym opisem. Metody na kontrolerze implementują operacje CRUD. Interfejs API sieci Web mapuje żądania HTTP z klienta na metody kontrolera w następujący sposób:

| Żądanie HTTP | Controller — Metoda | Opis |
| --- | --- | --- |
| Pobierz /api/todo | `GetTodoLists` | Pobiera kolekcję list do wykonania. |
| Pobierz*Identyfikator* /API/TODO/ | `GetTodoList` | Pobiera listę czynności do wykonania według identyfikatora |
| WPROWADŹ*Identyfikator* /API/TODO/ | `PutTodoList` | Aktualizuje listę czynności do wykonania. |
| OPUBLIKUJ/api/zadań do wykonania | `PostTodoList` | Tworzy nową listę do wykonania. |
| Usuń*Identyfikator* /API/TODO/ | `DeleteTodoList` | Usuwa listę zadań do wykonania. |

Zwróć uwagę, że identyfikatory URI dla niektórych operacji zawierają symbole zastępcze dla wartości identyfikatora. Na przykład aby usunąć listę z IDENTYFIKATORem 42, identyfikator URI jest `/api/todo/42`.

Aby dowiedzieć się więcej o korzystaniu z interfejsu API sieci Web dla operacji CRUD, zobacz [Tworzenie internetowego interfejsu API, który obsługuje operacje CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Kod dla tego kontrolera jest dość prosty. Oto kilka interesujących punktów:

- Metoda `GetTodoLists` używa zapytania LINQ do filtrowania wyników według identyfikatora zalogowanego użytkownika. W ten sposób użytkownik widzi tylko te dane, które należą do niego. Należy również zauważyć, że instrukcja SELECT służy do konwertowania wystąpień `ToDoList` na wystąpienia `TodoListDto`.
- Metody PUT i POST sprawdzają stan modelu przed modyfikacją bazy danych. Jeśli **ModelState. IsValid** ma wartość false, te metody zwracają http 400, złe żądanie. Przeczytaj więcej na temat weryfikacji modelu w interfejsie Web API podczas [weryfikacji modelu](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Klasa kontrolera również jest uzupełniona atrybutem **[autoryzuje]** . Ten atrybut sprawdza, czy żądanie HTTP zostało uwierzytelnione. Jeśli żądanie nie zostanie uwierzytelnione, klient odbiera protokół HTTP 401, Brak autoryzacji. Przeczytaj więcej na temat uwierzytelniania [i autoryzacji w usłudze ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

Klasa `TodoController` jest bardzo podobna do `TodoListController`. Największą różnicą jest to, że nie definiuje żadnych metod GET, ponieważ klient uzyska elementy do wykonania wraz z każdą listą czynności do wykonania.

## <a name="mvc-controllers-and-views"></a>Kontrolery MVC i widoki

Kontrolery MVC znajdują się również w folderze controllers rozwiązania. `HomeController` renderuje główny kod HTML dla aplikacji. Widok dla kontrolera głównego jest zdefiniowany w widokach/Home/index. cshtml. Widok domu służy do renderowania różnej zawartości w zależności od tego, czy użytkownik jest zalogowany:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Gdy użytkownicy są zalogowani, zobaczą główny interfejs użytkownika. W przeciwnym razie zobaczy panel logowania. Zauważ, że to renderowanie warunkowe występuje po stronie serwera. Nigdy nie należy próbować ukrywać poufnej zawartości po&#8212;stronie klienta, co jest widoczne w odpowiedzi HTTP, która jest widoczna dla kogoś, kto ogląda nieprzetworzone wiadomości HTTP.

## <a name="client-side-javascript-and-knockoutjs"></a>Kod JavaScript po stronie klienta i odcinanie. js

Teraz przejdźmy po stronie serwera aplikacji do klienta programu. Szablon SPA używa kombinacji jQuery i separowania. js, aby utworzyć płynny, interaktywny interfejs użytkownika. Separowanie. js to biblioteka języka JavaScript, która ułatwia powiązanie kodu HTML z danymi. Separowanie. js używa wzorca o nazwie "Model-View-ViewModel".

- Model to dane domeny (listy zadań do zrobienia i zadania do wykonania).
- Widok jest dokumentem HTML.
- Model widoku to obiekt JavaScript, który zawiera dane modelu. Model widoku jest abstrakcją kodu interfejsu użytkownika. Nie ma informacji o reprezentacji HTML. Zamiast tego reprezentuje funkcje abstrakcyjne widoku, takie jak "Lista elementów do wykonania".

Widok jest powiązany z danymi z modelem widoku. Aktualizacje widoku-model są automatycznie odzwierciedlane w widoku. Powiązania działają również w innym kierunku. Zdarzenia w modelu DOM (takie jak kliknięcia) są powiązane z funkcjami w modelu widoku, które wyzwalają wywołania AJAX.

Szablon SPA organizuje kod JavaScript po stronie klienta w trzech warstwach:

- zadanie do wykonania. DataContext. js: wysyła żądania AJAX.
- do zrobienia. model. js: definiuje modele.
- do zrobienia. ViewModel. js: definiuje model widoku.

![](knockoutjs-template/_static/image11.png)

Te pliki skryptów znajdują się w folderze skrypty/aplikacja rozwiązania.

![](knockoutjs-template/_static/image12.png)

zadanie do **wykonania obsługuje wszystkie** wywołania AJAX do kontrolerów interfejsu API sieci Web. (Wywołanie AJAX do logowania jest zdefiniowane w innym miejscu w ajaxlogin. js).

do **zrobienia. model. js** definiuje modele po stronie klienta (przeglądarki) dla list zadań do wykonania. Istnieją dwie klasy modelu: todoItem i todoList.

Wiele właściwości w klasach modelu jest typu "ko. dostrzegalne". Observables to sposób odcinania. Z [dokumentacji odcinania](http://knockoutjs.com/documentation/introduction.html): zauważalny jest "obiekt JavaScript, który może powiadamiać subskrybentów o zmianach". Gdy wartość zauważalnych zmian, odcinanie aktualizuje wszystkie elementy HTML, które są powiązane z tymi observablesami. Na przykład todoItem ma observables dla tytułu i właściwości isdone:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Możesz również subskrybować kod. Na przykład Klasa todoItem subskrybuje zmiany we właściwościach "isdone" i "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Wyświetl model**

Model widoku jest zdefiniowany w ViewModel. js. Model widoku jest punktem centralnym, w którym aplikacja powiąże elementy strony HTML z danymi domeny. W szablonie SPA model widoku zawiera zauważalny tablicę todoLists. Poniższy kod w modelu widoku mówi odcinanie, aby zastosować powiązania:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>KOD HTML i powiązanie danych

Główny kod HTML strony jest zdefiniowany w widokach/Home/index. cshtml. Ze względu na to, że używamy powiązań danych, kod HTML jest tylko szablonem, co faktycznie jest renderowane. Odcinanie używa powiązań *deklaratywnych* . Elementy strony można powiązać z danymi, dodając do elementu atrybut "dane-powiązanie". Oto bardzo prosty przykład pochodzący z dokumentacji odcinania:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

W tym przykładzie odcinanie aktualizuje zawartość **&lt;span&gt;** elementu o wartości `myItems.count()`. Za każdym razem, gdy ta wartość ulegnie zmianie, odcinanie dokumentu.

Odcinanie zawiera różne typy powiązań. Poniżej przedstawiono niektóre powiązania używane w szablonie SPA:

- **foreach**: umożliwia przechodzenie przez pętlę i zastosowanie tego samego znacznika do każdego elementu na liście. Służy do renderowania list czynności do wykonania i elementów do wykonania. W elemencie **foreach**powiązania są stosowane do elementów listy.
- **widoczne**: służy do przełączania widoczności. Ukryj znaczniki, gdy kolekcja jest pusta, lub wprowadź komunikat o błędzie jako widoczny.
- **wartość**: służy do wypełniania wartości formularza.
- **kliknięcie**: wiąże zdarzenie kliknięcia z funkcją w modelu widoku.

## <a name="anti-csrf-protection"></a>Ochrona przed CSRF

Fałszerstwo żądania między lokacjami (CSRF) to atak polegający na tym, że złośliwa witryna wysyła żądanie do zagrożonej lokacji, w której użytkownik jest aktualnie zalogowany. Aby zapobiec atakom CSRF, ASP.NET MVC używa *tokenów chroniących przed fałszerstwem*, nazywanych również tokenami weryfikacji żądań. Pomysłem jest to, że serwer umieszcza losowo wygenerowany token w stronie sieci Web. Gdy klient przesyła dane do serwera, musi on zawierać tę wartość w komunikacie żądania.

Tokeny chroniące przed fałszerstwem działają, ponieważ złośliwa strona nie może odczytać tokenów użytkownika ze względu na zasady tego samego źródła. (Zasady tego samego pochodzenia uniemożliwiają dokumentom obsługiwanym przez dwie różne lokacje uzyskiwanie dostępu do zawartości każdej z nich.)

ASP.NET MVC zapewnia wbudowaną obsługę tokenów chroniących przed fałszerstwem za pomocą klasy ochrony przed [fałszerstwem](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) i atrybutu [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) . Obecnie ta funkcja nie jest wbudowana w interfejs API sieci Web. Jednak szablon SPA zawiera niestandardową implementację interfejsu API sieci Web. Ten kod jest zdefiniowany w klasie `ValidateHttpAntiForgeryTokenAttribute`, która znajduje się w folderze filters rozwiązania. Aby dowiedzieć się więcej na temat programu Anti-CSRF w interfejsie Web API, zobacz [zapobieganie atakom w ramach żądań między witrynami (CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Podsumowanie

Szablon SPA został zaprojektowany tak, aby można było szybko pisać nowoczesne, interaktywne aplikacje sieci Web. Używa biblioteki odcinania. js do oddzielenia prezentacji (znaczników HTML) z logiki danych i aplikacji. Natomiast odcinanie nie jest jedyną biblioteką JavaScript, której można użyć do utworzenia SPA. Jeśli chcesz poznać inne opcje, zapoznaj się z [szablonami Spa utworzonymi przez społeczność](../templates/index.md).
