---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Używanie kontrolerów i widoków do implementowania interfejsu użytkownika listy/szczegółów | Microsoft Docs
author: microsoft
description: Krok 4 pokazuje, jak dodać kontroler do aplikacji, która wykorzystuje nasz model, aby udostępnić użytkownikom listę/szczegóły środowiska nawigacji...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 74319fe5ea4c79b50140834349e2fdf86420cfbb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600745"
---
# <a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Implementowanie interfejsu użytkownika typu lista/szczegóły przy użyciu kontrolerów i widoków

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 4 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 4 pokazuje, jak dodać kontroler do aplikacji, która korzysta z naszego modelu, aby udostępnić użytkownikom listę/szczegóły środowiska nawigacji dla obiadów w naszej witrynie NerdDinner.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner krok 4: kontrolery i widoki

W przypadku tradycyjnych platform sieci Web (klasycznych technologii ASP, PHP, ASP.NET Web Forms itp.) przychodzące adresy URL są zazwyczaj mapowane na pliki na dysku. Na przykład: żądanie dotyczące adresu URL, takiego jak "/Products.aspx" lub "/Products.php", może być przetwarzane przez plik "Products. aspx" lub "Products. php".

Sieci Web platformy MVC mapują adresy URL na kod serwera w nieco inny sposób. Zamiast mapować przychodzące adresy URL na pliki, zamiast tego mapują adresy URL na metody klasy. Klasy te są nazywane "kontrolerami" i są odpowiedzialne za przetwarzanie przychodzących żądań HTTP, obsługę danych wejściowych użytkownika, pobieranie i zapisywanie danych oraz określanie odpowiedzi na odesłanie do klienta (wyświetlanie kodu HTML, pobieranie pliku, przekierowanie do innego Adres URL itd.

Teraz, gdy Utworzyliśmy podstawowy model naszej aplikacji NerdDinner, następnym krokiem będzie dodanie kontrolera do aplikacji, która korzysta z niej w celu udostępnienia użytkownikom informacji o wykorzystaniu i nawigacji w przypadku obiadów w naszej witrynie.

### <a name="adding-a-dinnerscontroller-controller"></a>Dodawanie kontrolera DinnersController

Rozpocznie się po kliknięciu prawym przyciskiem myszy folderu "controllers" w naszym projekcie sieci Web, a następnie wybraniu polecenia menu **kontrolera Add-&gt;** (można także wykonać to polecenie, wpisując CTRL-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Spowoduje to wyświetlenie okna dialogowego "Dodawanie kontrolera":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Zmienimy nazwę nowego kontrolera "DinnersController" i klikniesz przycisk "Dodaj". Program Visual Studio doda następnie plik DinnersController.cs w katalogu \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Zostanie również otwarta nowa klasa DinnersController w edytorze kodu.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Dodawanie metod akcji index () i Details () do klasy DinnersController

Chcemy umożliwić odwiedzającym korzystanie z naszej aplikacji, aby przeglądać listę nadchodzących obiadów i zezwolić im na kliknięcie dowolnego obiadu na liście, aby zobaczyć szczegółowe informacje na jego temat. W tym celu należy opublikować następujące adresy URL z naszej aplikacji:

| **Adres URL** | **Przeznaczenie** |
| --- | --- |
| */Dinners/* | Wyświetl listę HTML z nadchodzących obiadów |
| */Dinners/Details/[ID]* | Wyświetl szczegóły dotyczące określonego obiadu wskazywanego przez parametr "ID" osadzony w adresie URL, który będzie pasować do DinnerIDego obiadu w bazie danych. Na przykład:/Dinners/Details/2 wyświetli stronę HTML ze szczegółowymi informacjami na temat obiadu, którego wartość DinnerID wynosi 2. |

Opublikujemy wstępne implementacje tych adresów URL, dodając dwie publiczne "metody akcji" do naszej klasy DinnersController, jak poniżej:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Następnie uruchomimy aplikację NerdDinner i użyjemy naszej przeglądarki, aby je wywoływać. Wpisanie adresu URL *"/Dinners/"* spowoduje uruchomienie metody *index ()* i wysłanie następującej odpowiedzi:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Wpisanie adresu URL *"/Dinners/Details/2"* spowoduje uruchomienie naszej metody *Details ()* i wysłanie następującej odpowiedzi:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Być może zastanawiasz się, jak ASP.NET MVC, aby utworzyć naszą klasę DinnersController i wywoływać te metody? Aby zrozumieć, że Przyjrzyjmy się, jak działa Routing.

### <a name="understanding-aspnet-mvc-routing"></a>Zrozumienie routingu ASP.NET MVC

ASP.NET MVC zawiera wydajny aparat routingu URL, który zapewnia dużą elastyczność kontroli nad sposobem mapowania adresów URL na klasy kontrolerów. Pozwala nam na całkowite dostosowanie sposobu, w jaki ASP.NET MVC wybiera klasę kontrolera do utworzenia, którą metodę należy wywołać, a także skonfigurować różne sposoby, aby zmienne mogły być automatycznie analizowane przy użyciu adresu URL/QueryString i przekazane do metody jako argumenty parametrów. Zapewnia elastyczną, aby całkowicie zoptymalizować lokację pod kątem funkcji optymalizacji aparatu wyszukiwania, a także opublikować dowolną strukturę adresów URL z aplikacji.

Domyślnie nowe projekty ASP.NET MVC są dostarczane ze wstępnie skonfigurowanym zestawem reguł routingu adresów URL, które są już zarejestrowane. Dzięki temu można łatwo zacząć korzystać z aplikacji bez konieczności jawnego konfigurowania jakichkolwiek elementów. Rejestracje domyślnych reguł routingu można znaleźć w klasie "aplikacja" naszych projektów, którą można otworzyć przez dwukrotne kliknięcie pliku "Global. asax" w katalogu głównym naszego projektu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Domyślne reguły routingu ASP.NET MVC są rejestrowane w metodzie "RegisterRoutes" tej klasy:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"Trasy. Plik maproute () "wywołanie metody powyżej rejestruje domyślną regułę routingu, która mapuje przychodzące adresy URL na klasy kontrolerów przy użyciu formatu adresu URL:"/{Controller}/{Action}/{ID} "— gdzie" Controller "jest nazwą klasy kontrolera do wystąpienia," Akcja "jest nazwą metody publicznej do wywołania, a" ID "jest opcjonalnym parametrem osadzonym w adresie URL, który może być przekazaniem jako argument do metody. Trzeci parametr przesłany do wywołania metody "plik maproute ()" jest zestawem wartości domyślnych, które są używane dla wartości kontrolera/akcji/identyfikatora w przypadku, gdy nie występują w adresie URL (Controller = "Strona główna", Akcja = "indeks", identyfikator = "").

Poniżej znajduje się tabela, która pokazuje, w jaki sposób różne adresy URL są mapowane przy użyciu domyślnej reguły trasy "<em>/{controllers}/{Action}/{ID}"</em>:

| **Adres URL** | **Klasa kontrolera** | **Action — Metoda** | **Parametry zakończone** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Szczegóły (identyfikator) | id=2 |
| */Dinners/Edit/5* | DinnersController | Edytuj (identyfikator) | id=5 |
| */Dinners/Create* | DinnersController | Utwórz () | N/D |
| */Dinners* | DinnersController | Index() | N/D |
| */Home* | HomeController | Index() | N/D |
| */* | HomeController | Index() | N/D |

W ostatnich trzech wierszach są wyświetlane wartości domyślne (Controller = Home, Action = index, ID = ""). Ponieważ metoda "index" jest zarejestrowana jako domyślna nazwa akcji, jeśli nie jest określona, adresy URL "/Dinners" i "/Home" powodują wywoływanie metody akcji index () dla ich klas kontrolera. Ponieważ kontroler "Home" jest zarejestrowany jako kontroler domyślny, jeśli nie jest określony, adres URL "/" powoduje utworzenie HomeController oraz metodę akcji index (), która ma zostać wywołana.

Jeśli nie chcesz, aby te domyślne reguły routingu adresów URL były łatwe do zmiany — wystarczy je edytować w powyższej metodzie RegisterRoutes. Mimo że nasza aplikacja NerdDinner nie zmienia żadnych domyślnych reguł routingu adresów URL, zamiast tego będziemy korzystać z nich w stanie takim, w jakim się znajdują.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Korzystanie z DinnerRepository z naszej DinnersController

Zastąpmy teraz bieżącą implementację metod akcji index () DinnersController () i Details () z implementacjami korzystającymi z naszego modelu.

Użyjemy wcześniej wbudowanej klasy DinnerRepository, aby zaimplementować zachowanie. Zaczniemy od dodania instrukcji "Using", która odwołuje się do przestrzeni nazw "NerdDinner. Models", a następnie zadeklaruj wystąpienie naszego DinnerRepository jako pole w naszej klasie DinnerController.

W dalszej części tego rozdziału wprowadzimy koncepcję "iniekcji zależności" i pokazano inny sposób, aby nasze kontrolery uzyskały odwołanie do DinnerRepository, które umożliwią lepsze testowanie jednostek — ale po prostu utworzymy wystąpienie naszego DinnerRepository w tekście poniżej.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Teraz jesteśmy gotowi do wygenerowania odpowiedzi HTML z powrotem przy użyciu podanych obiektów modelu danych.

### <a name="using-views-with-our-controller"></a>Używanie widoków z naszym kontrolerem

Chociaż można napisać kod w naszych metodach działania, aby złożyć HTML, a następnie użyć metody pomocnika *Response. Write ()* w celu wysłania jej z powrotem do klienta, to podejście stanie się dość nieporęczny szybko. Znacznie lepszym rozwiązaniem jest tylko wykonywanie logiki aplikacji i danych w naszych metodach działania DinnersController, a następnie przekazywanie danych potrzebnych do renderowania odpowiedzi HTML do oddzielnego szablonu "widok", który jest odpowiedzialny za wyprowadzanie reprezentacji HTML z niego. Jak zobaczymy na chwilę, szablon "widok" jest plikiem tekstowym, który zwykle zawiera kombinację znaczników HTML i osadzonego kodu renderowania.

Rozdzielenie naszej logiki kontrolera od naszego renderowania widoku przynosi wiele korzyści. W szczególności pomaga wymusić wyraźne "oddzielenie obaw" między kodem aplikacji i formatowaniem interfejsu użytkownika/kodem renderowania. Dzięki temu można znacznie łatwiej przetestować logikę aplikacji w celu odizolowania od logiki renderowania interfejsu użytkownika. Ułatwia to późniejsze modyfikowanie szablonów renderowania interfejsu użytkownika bez konieczności wprowadzania zmian w kodzie aplikacji. Może to ułatwić deweloperom i projektantom współpracę wspólnie z projektami.

Możemy zaktualizować naszą klasę DinnersController, aby wskazać, że chcemy użyć szablonu widoku w celu wysłania odpowiedzi z powrotem do interfejsu użytkownika HTML, zmieniając sygnatury metod naszych dwóch metod akcji, z których ma zostać zwrócony typ "void", aby zamiast tego był typem zwracanym "ActionResult". Następnie możemy wywołać metodę pomocnika *View ()* w klasie podstawowej kontrolera w celu zwrócenia z powrotem obiektu "ViewResult", takiego jak poniżej:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

Sygnatura metody pomocnika *widoku ()* , z której korzystamy powyżej, wygląda jak poniżej:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Pierwszy parametr do metody pomocnika *View ()* jest nazwą pliku szablonu widoku, który ma być używany do renderowania odpowiedzi html. Drugi parametr jest obiektem modelu, który zawiera dane, których wymaga szablon widoku w celu renderowania odpowiedzi HTML.

W naszej metodzie działania indeksu () wywoływana jest metoda pomocnika *widoku ()* i wskazująca, że chcemy renderować listę z obiadów HTML przy użyciu szablonu widoku "index" (indeks). Przekazujemy szablon widoku z sekwencją obiektów obiadu, aby wygenerować listę z:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

W naszej metodzie akcji () podjęto próbę pobrania obiektu obiadu przy użyciu identyfikatora podanego w adresie URL. Jeśli zostanie znaleziony prawidłowy obiad, wywołamy metodę pomocnika *View ()* , wskazując, że chcemy użyć szablonu widoku "Szczegóły" w celu renderowania pobranego obiektu obiadu. Jeśli zażądano nieprawidłowego obiadu, zostanie wyświetlony komunikat o błędzie informujący o tym, że obiad nie istnieje przy użyciu szablonu widoku "NotFound" (i przeciążonej wersji metody pomocnika *View ()* , która po prostu przyjmuje nazwę szablonu):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Zaimplementujmy teraz szablony widoków "NotFound", "Details" i "index".

### <a name="implementing-the-notfound-view-template"></a>Implementowanie szablonu widoku "NotFound"

Zaczniemy od implementacji szablonu widoku "NotFound" — który wyświetla przyjazny komunikat o błędzie informujący o tym, że nie można znaleźć żądanego obiadu.

Utworzymy nowy szablon widoku poprzez rozmieszczenie kursora tekstu w ramach metody akcji kontrolera, a następnie kliknięcie prawym przyciskiem myszy i wybranie polecenia menu "Dodaj widok" (można również wykonać to polecenie, wpisując CTRL-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Spowoduje to wyświetlenie okna dialogowego "Dodawanie widoku", takiego jak poniżej. Domyślnie okno dialogowe spowoduje wstępne wypełnienie nazwy widoku, który zostanie utworzony w celu dopasowania do nazwy metody akcji, w której znajduje się kursor, gdy okno dialogowe zostało uruchomione (w tym przypadku "Szczegóły"). Ponieważ chcemy najpierw zaimplementować szablon "NotFound", przesłonimy tę nazwę widoku i ustawimy ją jako "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Po kliknięciu przycisku "Dodaj" program Visual Studio utworzy nowy szablon widoku "NotFound. aspx" dla nas w katalogu "\Views\Dinners" (który również zostanie utworzony, jeśli katalog jeszcze nie istnieje):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Zostanie również otwarty nasz nowy szablon widoku "NotFound. aspx" w edytorze kodu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Widok szablony domyślnie ma dwa "regiony zawartości", w których można dodać zawartość i kod. Po pierwsze pozwala nam dostosować "tytuł" strony HTML wysłanej z powrotem. Druga pozwala na dostosowanie "głównej zawartości strony HTML wysłanej z powrotem.

Aby zaimplementować nasz szablon widoku "NotFound", dodamy zawartość podstawową:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Możemy to zrobić w przeglądarce. Aby to umożliwić, zażądaj adresu URL *"/Dinners/Details/9999"* . Odnosi się to do obiadu, który nie istnieje w bazie danych i spowoduje, że metoda akcji DinnersController. Details () będzie renderować nasz szablon widoku "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

W tym celu należy zauważyć, że na powyższym zrzucie ekranu jest to, że nasz podstawowy szablon widoku dziedziczy zbiór kodu HTML, który otacza główną zawartość na ekranie. Wynika to z faktu, że nasz szablon widoku używa szablonu "Strona główna", który umożliwia nam stosowanie spójnego układu we wszystkich widokach w witrynie. Będziemy omawiać, jak strony wzorcowe pracują więcej w dalszej części tego samouczka.

### <a name="implementing-the-details-view-template"></a>Implementowanie szablonu widoku "Szczegóły"

Teraz zaimplementujmy szablon widoku "Szczegóły", który będzie generował kod HTML dla pojedynczego modelu obiadu.

Zajmiemy się tym rozmieszczeniem kursora tekstu w metodzie akcji Details, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie menu "Dodaj widok" (lub naciśnij klawisze CTRL-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Spowoduje to wyświetlenie okna dialogowego "Dodawanie widoku". Zachowamy domyślną nazwę widoku ("Szczegóły"). Zaznaczmy również pole wyboru "Utwórz widok z silną nazwą" w oknie dialogowym, a następnie wybierz (przy użyciu listy rozwijanej ComboBox) nazwę typu modelu przekazywanego z kontrolera do widoku. W tym widoku przekazujemy obiekt obiadu (w pełni kwalifikowana nazwa tego typu to: "NerdDinner. models. obiad"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

W przeciwieństwie do poprzedniego szablonu, w przypadku wybrania opcji utworzenia "pustego widoku" ten czas zostanie wybrany automatycznie "szkieletem" dla widoku przy użyciu szablonu "Szczegóły". Możemy to potwierdzić, zmieniając listę rozwijaną "Wyświetl zawartość" w oknie dialogowym powyżej.

"Rusztowanie" spowoduje wygenerowanie początkowej implementacji naszego szablonu widoku szczegółów na podstawie obiektu obiadu, który przekazujemy do niego. Zapewnia to łatwy sposób na szybkie rozpoczęcie pracy nad implementacją szablonu widoku.

Po kliknięciu przycisku "Dodaj" program Visual Studio utworzy nowy plik szablonu widoku "Szczegóły. aspx" dla nas w katalogu "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Zostanie również otwarty nasz nowy szablon widoku "Szczegóły. aspx" w edytorze kodu. Będzie zawierać wstępną implementację szkieletu widoku szczegółów na podstawie modelu obiadu. Aparat tworzenia szkieletów używa odbicia platformy .NET, aby przyjrzeć się właściwościom publicznym narażonym na klasę przekazaną i dodać odpowiednią zawartość w oparciu o każdy znaleziony typ:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Możemy zażądać adresu URL *"/Dinners/Details/1"* , aby sprawdzić, jak wygląda implementacja szkieletu "Szczegóły" w przeglądarce. Użycie tego adresu URL spowoduje wyświetlenie jednego z obiadów, które zostały ręcznie dodane do naszej bazy danych po jej utworzeniu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Pozwala to na szybkie uruchamianie i wykonywanie wstępnych implementacji naszego widoku szczegółów. aspx. Możemy następnie przejść i dopasować go, aby dostosować interfejs użytkownika do naszego zadowolenia.

Gdy sprawdzimy szablon szczegóły. aspx dokładniej, zobaczymy, że zawiera on statyczny kod HTML, a także osadzony kodu renderowania. &lt;%%&gt; kod wykonywania kodu Nuggets, gdy renderuje szablon widoku i &lt;% =%&gt; kod Nuggets wykonywania zawartego w nich kodu, a następnie Renderuj wynik do strumienia wyjściowego szablonu.

Możemy napisać kod w naszym widoku, który uzyskuje dostęp do obiektu modelu "obiad", który został przesłany przez nasz kontroler przy użyciu właściwości "model" o jednoznacznie określonym typie. Program Visual Studio udostępnia nam pełny kod — IntelliSense podczas uzyskiwania dostępu do tej właściwości "model" w edytorze:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Wprowadźmy kilka dostosowań, aby źródło dla naszego końcowego szablonu widoku szczegółów wyglądało jak poniżej:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Gdy będziemy ponownie uzyskiwać dostęp do adresu URL *"/Dinners/Details/1"* , będzie on teraz renderowany w następujący sposób:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementowanie szablonu widoku "index"

Teraz zaimplementujmy szablon widoku "index", który spowoduje wygenerowanie listy przyszłych obiadów. Aby to zrobić, zmienimy kursor tekstowy w ramach metody akcji index, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie menu "Dodaj widok" (lub naciśnij klawisze CTRL-M, Ctrl-V).

W oknie dialogowym "Dodawanie widoku" zachowasz szablon widoku o nazwie "index" i wybierzesz pole wyboru "Utwórz widok o jednoznacznie określonym typie". Tym razem wybierzemy automatyczne wygenerowanie szablonu widoku "list" i wybranie "NerdDinner. models. obiad" jako typu modelu przekazana do widoku (który wskazuje, że tworzysz szkielet "list", spowoduje to, że w oknie dialogowym Dodawanie widoku zostanie założono, że przekazywanie sekwencji obiektów obiadu z naszego kontrolera do widoku):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Po kliknięciu przycisku "Dodaj" program Visual Studio utworzy nowy plik szablonu widoku "index. aspx" dla nas w katalogu "\Views\Dinners". W tym celu zostanie wyświetlona wstępna implementacja szkieletu, która zawiera tabelę HTML zawierającą uroczyste obiady do widoku.

Po uruchomieniu aplikacji i otrzymaniu dostępu do adresu URL *"/Dinners/"* zostanie wyrenderowana nasza lista obiadów, takich jak:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

Powyższe rozwiązanie tabeli umożliwia nam tworzenie układu przypominającego siatkę naszych danych obiadu — co nie jest dość potrzebne dla naszej listy obiadów z klientem. Możemy zaktualizować szablon widoku index. aspx i zmodyfikować go w taki sposób, aby wyświetlał mniejszą liczbę kolumn danych, i użyć elementu &lt;ul&gt;, aby renderować je zamiast tabeli przy użyciu poniższego kodu:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Używamy słowa kluczowego "var" w powyższej instrukcji foreach, ponieważ pętla każdego obiadu w naszym modelu. Osoby nieznające C# 3,0 mogą myśleć, że użycie "var" oznacza, że obiekt obiadu jest opóźniony. Zamiast tego oznacza, że kompilator używa wnioskowania typu względem właściwości "model" o jednoznacznie określonym typie, czyli typu "IEnumerable&lt;obiad&gt;") i kompilowania lokalnej zmiennej "obiad" jako typu obiadu, co oznacza, że w blokach kodu zostanie wypełniona pełna funkcja IntelliSense i sprawdzanie czasu kompilacji:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Po trafieniu na adres URL */Dinners* w naszej przeglądarce nasz zaktualizowany widok wygląda teraz następująco:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Jest to dokładniejsze wyszukiwanie, ale nie jest jeszcze w całości. Ostatnim krokiem jest umożliwienie użytkownikom końcowym klikania poszczególnych obiadów na liście i wyświetlania szczegółowych informacji o nich. Zaimplementujemy to przez renderowanie elementów hiperłączy HTML, które łączą się z metodą akcji szczegółów w naszym DinnersController.

Można generować te hiperłącza w naszym widoku indeksu na jeden z dwóch sposobów. Pierwszym z nich jest ręczne utworzenie kodu HTML &lt;&gt; elementy, takie jak poniżej, gdzie osadzamy &lt;%%&gt; bloków w &lt;&gt; elementu HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Alternatywnym podejściem, którego można użyć, jest skorzystanie z wbudowanej metody pomocnika "html. ActionLink ()" w ramach ASP.NET MVC, która obsługuje Programistyczne tworzenie kodu HTML &lt;element&gt;, który łączy się z inną metodą akcji na kontrolerze:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Pierwszy parametr do pliku HTML. ActionLink () — metoda pomocnika to tekst linku do wyświetlenia (w tym przypadku tytuł obiadu), drugi parametr to nazwa akcji kontrolera, do której chcemy wygenerować łącze (w tym przypadku Metoda Details), a trzeci parametr to zestaw parametrów do wysłania do akcji (zaimplementowany jako typ anonimowy przy użyciu nazwy właściwości/wartości). W tym przypadku określamy parametr "ID" obiadu, z którym chcemy się połączyć, a ponieważ domyślna reguła routingu adresu URL w ASP.NET MVC to "{Controller}/{Action}/{id}" metoda pomocnika html. ActionLink () generuje następujące dane wyjściowe:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

W naszym widoku index. aspx będziemy używać metody pomocnika html. ActionLink () i mieć każdy obiad na liście w linku do odpowiedniego adresu URL szczegółów:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

A teraz, gdy trafimy adres URL */Dinners* , nasz list obiadu wygląda następująco:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Po kliknięciu dowolnego obiadu na liście będziemy przeglądać szczegółowe informacje na jego temat:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Nazewnictwo oparte na konwencji i struktura katalogów \Views

ASP.NET aplikacje MVC domyślnie wykorzystują strukturę nazewnictwa katalogu na podstawie Konwencji podczas rozpoznawania szablonów widoków. Dzięki temu deweloperzy nie muszą w pełni kwalifikować ścieżki lokalizacji podczas odwoływania się do widoków z poziomu klasy kontrolera. Domyślnie ASP.NET MVC szuka pliku szablonu widoku znajdującego się w katalogu * \Views\[ControllerName\*] pod aplikacją.

Na przykład pracujemy nad klasą DinnersController — która jawnie odwołuje się do trzech szablonów widoków: "index", "Details" i "NotFound". ASP.NET MVC będzie domyślnie szukać tych widoków w katalogu *\Views\Dinners* pod naszym katalogiem głównym aplikacji:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Zwróć uwagę na to, że istnieją trzy klasy kontrolera w projekcie (DinnersController, HomeController i elementu AccountController — ostatnie dwa zostały dodane domyślnie podczas tworzenia projektu), a istnieją trzy podkatalogi (po jednym dla każdej kontroler) w katalogu \Views.

Widoki z odwołaniami z kontrolerów głównych i kont automatycznie rozwiązują szablony widoków z odpowiednich katalogów *\Views\Home* i *\Views\Account* . Podkatalog *\Views\Shared* umożliwia przechowywanie szablonów widoków, które są ponownie używane na wielu kontrolerach w aplikacji. Gdy ASP.NET MVC próbuje rozpoznać szablon widoku, najpierw sprawdzi w określonym katalogu *\Views\[Controller]* i jeśli nie może znaleźć szablonu widoku, będzie wyglądał w katalogu *\Views\Shared* .

Gdy przyjdzie do nazewnictwa poszczególnych szablonów widoków, zalecaną wskazówką jest udostępnienie szablonu widoku tej samej nazwy co metoda działania, która spowodowała renderowanie. Na przykład powyżej nasza metoda działania "index" używa widoku "index", aby renderować wynik widoku, a metoda akcji "Szczegóły" używa widoku "Szczegóły", aby renderować wyniki. Dzięki temu można łatwo szybko sprawdzić, który szablon jest skojarzony z poszczególnymi akcjami.

Deweloperzy nie muszą jawnie określać nazwy szablonu widoku, gdy szablon widoku ma taką samą nazwę jak Metoda akcji wywoływana na kontrolerze. Zamiast tego można przekazać obiekt modelu do metody pomocnika "View ()" (bez określenia nazwy widoku), a ASP.NET MVC automatycznie wywnioskuje, że chcemy użyć *\Views\[ControllerName]\[ActionName]* Wyświetl szablon na dysku, aby go renderować.

Dzięki temu możemy czyścić kod kontrolera nieco trochę i uniknąć duplikowania nazwy dwukrotnie w naszym kodzie:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Powyższym kodem jest wszystko, co jest konieczne do zaimplementowania w celu uzyskania szczegółowych informacji o tym, jak to działa.

#### <a name="next-step"></a>Następny krok

Teraz masz wbudowaną funkcję przeglądania obiadu.

Teraz Włącz obsługę edycji formularzy danych w programie CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie).

> [!div class="step-by-step"]
> [Poprzednie](build-a-model-with-business-rule-validations.md)
> [dalej](provide-crud-create-read-update-delete-data-form-entry-support.md)
