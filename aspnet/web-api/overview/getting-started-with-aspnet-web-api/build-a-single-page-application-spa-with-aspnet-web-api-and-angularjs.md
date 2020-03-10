---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Wskazówki dotyczące laboratorium: Tworzenie aplikacji jednostronicowej (SPA) za pomocą interfejsu API sieci Web ASP.NET i języka kątowego. js — ASP.NET 4. x'
author: rick-anderson
description: 'Kod krok po kroku: kompilowanie aplikacji jednostronicowej (SPA) za pomocą interfejsu API sieci Web ASP.NET i języka kątowego. js dla ASP.NET 4. x.'
ms.author: riande
ms.date: 09/30/2015
ms.custom: seoapril2019
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 86833a890da759e489dd11dc9afb128a9b7a75e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557044"
---
# <a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Ćwiczenia praktyczne: tworzenie aplikacji jednostronicowej przy użyciu wzorca ASP.NET Web API i platformy Angular.js

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

W tym ćwiczeniu w laboratorium pokazano, jak utworzyć aplikację jednostronicową (SPA) za pomocą interfejsu API sieci Web ASP.NET i języka kątowego. js dla ASP.NET 4. x.

W tym ćwiczeniu można wykorzystać te technologie do implementacji quizu pojedynek maniaków komputerowych, kwizyą witrynę sieci Web na podstawie koncepcji SPA. Najpierw należy zaimplementować warstwę usługi z interfejsem API sieci Web ASP.NET, aby udostępnić wymagane punkty końcowe do pobrania pytań quizu i przechowywania odpowiedzi. Następnie utworzysz bogaty i dynamiczny interfejs użytkownika przy użyciu efektów transformacji AngularJS i CSS3.

W tradycyjnych aplikacjach sieci Web klient (przeglądarka) inicjuje komunikację z serwerem przez żądanie strony. Następnie serwer przetwarza żądanie i wysyła kod HTML strony do klienta programu. W kolejnych interakcjach ze stroną — np. użytkownik nawiguje do linku lub przesyła formularz z danymi — nowe żądanie jest wysyłane do serwera, a przepływ zostanie uruchomiony ponownie: serwer przetwarza żądanie i wysyła nową stronę do przeglądarki w odpowiedzi na nowe żądanie akcji Ed przez klienta.
> 
> W przypadku aplikacji jednostronicowych (aplikacji jednostronicowych) cała strona jest ładowana do przeglądarki po początkowym żądaniu, ale kolejne interakcje odbywają się przez żądania AJAX. Oznacza to, że przeglądarka musi zaktualizować tylko część strony, która została zmieniona; nie ma potrzeby ponownego ładowania całej strony. Metoda SPA pozwala skrócić czas reakcji aplikacji na działania użytkownika, co spowodowało bardziej płynne działanie.
> 
> Architektura SPA wiąże się z pewnymi wyzwaniami, które nie są obecne w tradycyjnych aplikacjach sieci Web. Jednak pojawiające się technologie, takie jak ASP.NET Web API, struktury JavaScript, takie jak AngularJS i nowe funkcje stylu udostępniane przez CSS3, ułatwiają projektowanie i kompilowanie aplikacji jednostronicowych.
> 
> 
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Tworzenie usługi internetowego interfejsu API ASP.NET w celu wysyłania i odbierania danych JSON
- Tworzenie interfejsu użytkownika, który odpowiada, za pomocą AngularJS
- Ulepszanie środowiska interfejsu użytkownika za pomocą transformacji CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Następujące czynności są wymagane do wykonania tego laboratorium praktycznego:

- [Visual Studio Express 2013 dla sieci Web](https://www.microsoft.com/visualstudio/) lub nowszego

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

Aby można było uruchomić ćwiczenia w tym ćwiczeniu, należy najpierw skonfigurować środowisko.

1. Otwórz Eksploratora Windows i przejdź do folderu **źródłowego** laboratorium.
2. Kliknij prawym przyciskiem myszy pozycję **Setup. cmd** i wybierz polecenie **Uruchom jako administrator** , aby uruchomić proces instalacji, który skonfiguruje środowisko i zainstaluje fragmenty kodu programu Visual Studio dla tego laboratorium.
3. Jeśli zostanie wyświetlone okno dialogowe Kontrola konta użytkownika, potwierdź akcję, aby wykonać operację.

> [!NOTE]
> Upewnij się, że wszystkie zależności dla tego laboratorium zostały sprawdzone przed uruchomieniem Instalatora.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Używanie fragmentów kodu

W całym dokumencie laboratoryjnym pojawi się monit o wstawienie bloków kodu. Dla wygody większość tego kodu jest udostępniana jako fragmenty Visual Studio Code, do których można uzyskać dostęp z poziomu Visual Studio 2013, aby uniknąć konieczności ręcznego dodawania go.

> [!NOTE]
> Każdemu z nich towarzyszy rozpoczęcie rozwiązanie znajdujące się w folderze **BEGIN** w ćwiczeniu, który umożliwia wykonywanie poszczególnych czynności niezależnie od innych. Należy pamiętać, że fragmenty kodu dodawane podczas wykonywania nie są dostępne w tych rozwiązaniach i mogą nie działać do czasu ukończenia ćwiczenia. Wewnątrz kodu źródłowego dla ćwiczenia znajdziesz również folder **końcowy** zawierający rozwiązanie programu Visual Studio z kodem, który wynika z wykonania czynności w odpowiednim ćwiczeniu. Te rozwiązania można wykorzystać jako wskazówkę, jeśli potrzebujesz dodatkowej pomocy podczas pracy w ramach tego praktycznego laboratorium.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Tworzenie internetowego interfejsu API](#Exercise1)
2. [Tworzenie interfejsu SPA](#Exercise2)

Szacowany czas wykonywania tego laboratorium: **60 minut**

> [!NOTE]
> Po pierwszym uruchomieniu programu Visual Studio należy wybrać jedną z wstępnie zdefiniowanych kolekcji ustawień. Każda wstępnie zdefiniowana kolekcja jest zaprojektowana tak, aby była zgodna z konkretnym stylem deweloperskim i określa układy okien, zachowanie edytora, fragmenty kodu IntelliSense i opcje okna dialogowego. Procedury przedstawione w tym laboratorium opisują akcje niezbędne do wykonania danego zadania w programie Visual Studio, gdy jest używana **Ogólna kolekcja ustawień deweloperskich** . W przypadku wybrania innej kolekcji ustawień dla środowiska programistycznego mogą wystąpić różnice w czynnościach, które należy wziąć pod uwagę.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Ćwiczenie 1: Tworzenie internetowego interfejsu API

Jedną z kluczowych części SPA jest warstwa usługi. Jest on odpowiedzialny za przetwarzanie wywołań AJAX wysyłanych przez interfejs użytkownika i zwracanie danych w odpowiedzi na to wywołanie. Pobrane dane powinny być prezentowane w formacie do odczytu maszynowego, aby można było je analizować i zużywać przez klienta.

Struktura interfejsu API sieci Web jest częścią stosu ASP.NET i została zaprojektowana tak, aby ułatwić implementowanie usług HTTP, ogólnie wysyłając i odbierać dane w formacie JSON lub XML za pośrednictwem interfejsu API RESTful. W tym ćwiczeniu utworzysz witrynę sieci Web, która będzie hostować aplikację quizu pojedynek maniaków komputerowych, a następnie zaimplementujemy usługę zaplecza w celu udostępnienia i utrwalenia danych quizu przy użyciu internetowego interfejsu API ASP.NET.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Zadanie 1 — Tworzenie początkowego projektu dla quizu pojedynek maniaków komputerowych

W tym zadaniu rozpocznie się tworzenie nowego projektu ASP.NET MVC z obsługą interfejsu API sieci Web ASP.NET opartego na jednym typie projektu **ASP.NET** , który jest dostarczany z programem Visual Studio. **Jedna ASP.NET** łączy wszystkie technologie ASP.NET i pozwala na ich mieszanie i dopasowanie odpowiednio do potrzeb. Następnie należy dodać klasy modelu Entity Framework i inicjatora bazy danych, aby wstawić pytania quizu.

1. Otwórz **Visual Studio Express 2013 dla sieci Web** i wybierz pozycję **plik | Nowy projekt...** , aby rozpocząć nowe rozwiązanie.

    ![Tworzenie nowego projektu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "Tworzenie nowego projektu")

    *Tworzenie nowego projektu*
2. W oknie dialogowym **Nowy projekt** wybierz pozycję **aplikacja sieci Web ASP.NET** w obszarze **Wizualizacja C# | Karta Sieć Web** . Upewnij się, że **.NET Framework 4,5** jest zaznaczone, nadaj jej nazwę *GeekQuiz*, wybierz **lokalizację** i kliknij przycisk **OK**.

    ![Tworzenie nowego projektu aplikacji sieci Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "Tworzenie nowego projektu aplikacji sieci Web ASP.NET")

    *Tworzenie nowego projektu aplikacji sieci Web ASP.NET*
3. W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **MVC** i wybierz opcję **internetowy interfejs API** . Upewnij się również, że opcja **uwierzytelniania** jest ustawiona na **konta poszczególnych użytkowników**. Kliknij przycisk **OK** , aby kontynuować.

    ![Tworzenie nowego projektu za pomocą szablonu MVC, w tym składników Web API Components](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Tworzenie nowego projektu za pomocą szablonu MVC, w tym składników Web API Components*
4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **modele** projektu **GeekQuiz** i wybierz polecenie **Dodaj | Istniejący element.** ...

    ![Dodawanie istniejącego elementu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "Dodawanie istniejącego elementu")

    *Dodawanie istniejącego elementu*
5. W oknie dialogowym **Dodaj istniejący element** przejdź do folderu **Source/Assets/models** i zaznacz wszystkie pliki. Kliknij pozycję **Add** (Dodaj).

    ![Dodawanie zasobów modelu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "Dodawanie zasobów modelu")

    *Dodawanie zasobów modelu*

    > [!NOTE]
    > Dodając te pliki, dodajesz model danych, kontekst bazy danych Entity Framework i inicjator bazy danych dla aplikacji quizu pojedynek maniaków komputerowych.
    > 
    > **Entity Framework (EF)** to Mapowanie obiektowo-relacyjne (ORM), które umożliwia tworzenie aplikacji do uzyskiwania dostępu do danych przez Programowanie przy użyciu modelu aplikacji koncepcyjnej zamiast programowania bezpośrednio przy użyciu relacyjnego schematu magazynu. Więcej informacji na temat Entity Framework można znaleźć [tutaj](../../../entity-framework.md).
    > 
    > Poniżej znajduje się opis właśnie dodanych klas:
    > 
    > - **TriviaOption:** reprezentuje pojedynczą opcję skojarzoną z pytaniem quizu
    > - **TriviaQuestion:** reprezentuje pytanie quizu i uwidacznia skojarzone opcje za pomocą właściwości **Options**
    > - **TriviaAnswer:** reprezentuje opcję wybraną przez użytkownika w odpowiedzi na pytanie quizu
    > - **TriviaContext:** reprezentuje kontekst bazy danych Entity Framework aplikacji quizu pojedynek maniaków komputerowych. Ta klasa pochodzi z **DContext** i uwidacznia właściwości **nieogólnymi** , które reprezentują kolekcje jednostek opisanych powyżej.
    > - **TriviaDatabaseInitializer:** implementacja inicjatora Entity Framework dla klasy **TriviaContext** , która dziedziczy z **CreateDatabaseIfNotExists**. Domyślnym zachowaniem tej klasy jest utworzenie bazy danych, jeśli nie istnieje, wstawianie jednostek określonych w metodzie **inicjatora** .
6. Otwórz plik **Global.asax.cs** i Dodaj następującą instrukcję using.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Dodaj następujący kod na początku **aplikacji\_Uruchom** metodę, aby ustawić **TriviaDatabaseInitializer** jako inicjator bazy danych.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Zmodyfikuj kontroler **Home** , aby ograniczyć dostęp do uwierzytelnionych użytkowników. W tym celu Otwórz plik **HomeController.cs** wewnątrz folderu **controllers** i Dodaj atrybut **Autoryzuj** do definicji klasy **HomeController** .

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > Filtr **Autoryzuj** sprawdza, czy użytkownik jest uwierzytelniony. Jeśli użytkownik nie jest uwierzytelniony, zwraca kod stanu HTTP 401 (nieautoryzowany) bez wywoływania akcji. Filtr można zastosować globalnie, na poziomie kontrolera lub na poziomie poszczególnych akcji.
9. Teraz można dostosować układ stron sieci Web i znakowanie. Aby to zrobić, Otwórz plik **\_Layout. cshtml** w **widokach | Folder udostępniony** i zaktualizuj zawartość **&lt;tytułu&gt;** elementu, zastępując *moją aplikację ASP.NET* za pomocą *quizu pojedynek maniaków komputerowych*.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. W tym samym pliku zaktualizuj pasek nawigacyjny, usuwając linki *informacje* i *kontakt* oraz zmieniając nazwę linku *macierzystego* do *odtworzenia*. Ponadto Zmień nazwę linku *nazwy aplikacji* na *Quiz pojedynek maniaków komputerowych*. Kod HTML na pasku nawigacyjnym powinien wyglądać podobnie do poniższego kodu.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Zaktualizuj stopkę strony układu, zastępując *moją aplikację ASP.NET* za pomocą *quizu pojedynek maniaków komputerowych*. W tym celu Zastąp zawartość **&lt;stopki&gt;** elementu następującym wyróżnionym kodem.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Zadanie 2 — Tworzenie interfejsu API sieci Web TriviaController

W poprzednim zadaniu utworzono początkową strukturę aplikacji sieci Web quizu pojedynek maniaków komputerowych. Teraz utworzysz prostą usługę interfejsu API sieci Web, która współdziała z modelem danych quizu i uwidacznia następujące działania:

- **Pobierz/API/Trivia**: pobiera następne pytanie z listy quizu, na które udzielono odpowiedzi uwierzytelnionego użytkownika.
- **Post/API/Trivia**: przechowuje odpowiedź quizu określoną przez uwierzytelnionego użytkownika.

Użyjesz narzędzi do tworzenia szkieletów ASP.NET dostarczonych przez program Visual Studio, aby utworzyć linię bazową dla klasy kontrolera interfejsu API sieci Web.

1. Otwórz plik **WebApiConfig.cs** w folderze **Start\_** . Ten plik definiuje konfigurację usługi internetowego interfejsu API, na przykład sposób mapowania tras na akcje kontrolera interfejsu API sieci Web.
2. Dodaj następującą instrukcję using na początku pliku.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Dodaj następujący wyróżniony kod do metody **register** , aby globalnie skonfigurować program formatujący dla danych JSON pobranych przez metody akcji interfejsu API sieci Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver** automatycznie konwertuje nazwy właściwości na *notacji CamelCase* , która jest ogólną Konwencją dla nazw właściwości w języku JavaScript.
4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **controllers (kontrolery** ) projektu **GeekQuiz** i wybierz polecenie **Dodaj | Nowy element szkieletowy..** .

    ![Tworzenie nowego elementu szkieletowego](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "Tworzenie nowego elementu szkieletowego")

    *Tworzenie nowego elementu szkieletowego*
5. W oknie dialogowym **Dodawanie szkieletu** upewnij się, że w okienku po lewej stronie jest wybrany **wspólny** węzeł. Następnie wybierz w środkowym okienku **kontroler Web API 2 — pusty** szablon i kliknij przycisk **Dodaj**.

    ![Wybieranie pustego szablonu interfejsu Web API 2](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Wybieranie pustego szablonu interfejsu Web API 2")

    *Wybieranie pustego szablonu interfejsu Web API 2*

    > [!NOTE]
    > Tworzenie **szkieletów ASP.NET** jest strukturą generowania kodu dla aplikacji sieci Web ASP.NET. Visual Studio 2013 obejmuje wstępnie zainstalowane generatory kodu dla projektów MVC i Web API. Należy używać szkieletów w projekcie, aby szybko dodać kod, który współdziała z modelami danych w celu skrócenia czasu potrzebnego do opracowania standardowych operacji na danych.
    > 
    > Proces tworzenia szkieletu gwarantuje również, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład, jeśli zaczniesz od pustego projektu ASP.NET, a następnie używasz szkieletu do dodawania kontrolera interfejsu API sieci Web, wymagane pakiety i odwołania internetowego interfejsu API sieci Web są dodawane do projektu automatycznie.
6. W oknie dialogowym **Dodawanie kontrolera** wpisz *TriviaController* w polu tekstowym **nazwa kontrolera** , a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie kontrolera kwizy](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Dodawanie kontrolera kwizy")

    *Dodawanie kontrolera kwizy*
7. Plik **TriviaController.cs** jest następnie dodawany do folderu **controllers** projektu **GeekQuiz** , zawierającego pustą klasę **TriviaController** . Dodaj następujące instrukcje using na początku pliku.

    (Fragment kodu- *AspNetWebApiSpa-Ex1-TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Dodaj następujący kod na początku klasy **TriviaController** , aby zdefiniować, zainicjować i usunąć wystąpienie **TriviaContext** w kontrolerze.

    (Fragment kodu- *AspNetWebApiSpa-Ex1-TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > Metoda **Dispose** elementu **TriviaController** wywołuje metodę **Dispose** wystąpienia **TriviaContext** , która zapewnia, że wszystkie zasoby używane przez obiekt kontekstu są zwalniane, gdy wystąpienie **TriviaContext** zostanie usunięte lub nie zostało pobrane jako elementy bezużyteczne. Obejmuje to zamknięcie wszystkich połączeń bazy danych otwartych przez Entity Framework.
9. Dodaj następującą metodę pomocnika na końcu klasy **TriviaController** . Ta metoda pobiera następujące pytanie quizu z bazy danych, do której ma zostać udzielona odpowiedź określony użytkownik.

    (Fragment kodu- *AspNetWebApiSpa-Ex1-TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Dodaj następującą metodę **Get** Action do klasy **TriviaController** . Ta metoda akcji wywołuje metodę pomocnika **NextQuestionAsync** zdefiniowaną w poprzednim kroku w celu pobrania następnego pytania dla uwierzytelnionego użytkownika.

    (Fragment kodu- *AspNetWebApiSpa-Ex1-TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Dodaj następującą metodę pomocnika na końcu klasy **TriviaController** . Ta metoda zapisuje określoną odpowiedź w bazie danych i zwraca wartość logiczną wskazującą, czy odpowiedź jest poprawna.

    (Fragment kodu- *AspNetWebApiSpa-Ex1-TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Dodaj następującą metodę **post** Action do klasy **TriviaController** . Ta metoda akcji kojarzy odpowiedź z uwierzytelnionym użytkownikiem i wywołuje metodę pomocnika **StoreAsync** . Następnie wysyła odpowiedź z wartością logiczną zwracaną przez metodę pomocnika.

    (Fragment kodu- *AspNetWebApiSpa-Ex1-TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Zmodyfikuj kontroler interfejsu API sieci Web, aby ograniczyć dostęp do użytkowników uwierzytelnionych przez dodanie atrybutu **Autoryzuj** do definicji klasy **TriviaController** .

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Zadanie 3 — Uruchamianie rozwiązania

W tym zadaniu sprawdzisz, że usługa internetowego interfejsu API, która została utworzona w poprzednim zadaniu, działa zgodnie z oczekiwaniami. Będziesz używać **Narzędzia deweloperskie F12** programu Internet Explorer, aby przechwytywać ruch sieciowy i sprawdzać pełną odpowiedź z usługi internetowego interfejsu API.

> [!NOTE]
> Upewnij **się, że** na pasku narzędzi programu Visual Studio jest wybrany program **Internet Explorer** .
> 
> ![Opcja programu Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)

1. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie. Strona **logowania** powinna zostać wyświetlona w przeglądarce.

    > [!NOTE]
    > Po uruchomieniu aplikacji jest wyzwalana domyślna trasa MVC, która domyślnie jest mapowana na akcję **index** klasy **HomeController** . Ponieważ **HomeController** jest ograniczony do użytkowników uwierzytelnionych (należy pamiętać, że ta klasa została zapisana z atrybutem **Autoryzuj** w ćwiczeniu 1) i nie ma jeszcze uwierzytelnionego użytkownika, aplikacja przekierowuje oryginalne żądanie do strony logowania.

    ![Uruchamianie rozwiązania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "Uruchamianie rozwiązania")

    *Uruchamianie rozwiązania*
2. Kliknij pozycję **zarejestruj** , aby utworzyć nowego użytkownika.

    ![Rejestrowanie nowego użytkownika](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "Rejestrowanie nowego użytkownika")

    *Rejestrowanie nowego użytkownika*
3. Na stronie **Rejestr** wprowadź **nazwę użytkownika** i **hasło**, a następnie kliknij pozycję **zarejestruj**.

    ![Strona rejestracji](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "Strona rejestracji")

    *Strona rejestracji*
4. Aplikacja rejestruje nowe konto, a użytkownik zostaje uwierzytelniony i przekierowany z powrotem do strony głównej.

    ![Użytkownik jest uwierzytelniany](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "Użytkownik uwierzytelniony")

    *Użytkownik jest uwierzytelniany*
5. W przeglądarce naciśnij klawisz **F12** , aby otworzyć panel **Narzędzia deweloperskie** . Naciśnij **klawisze CTRL + 4** lub kliknij ikonę **sieci** , a następnie kliknij przycisk Zielona strzałka, aby rozpocząć przechwytywanie ruchu sieciowego.

    ![Inicjowanie przechwytywania sieci Web API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "Inicjowanie przechwytywania sieci Web API")

    *Inicjowanie przechwytywania sieci Web API*
6. Dołącz **interfejs API/kwizy** do adresu URL na pasku adresu przeglądarki. Teraz sprawdzisz szczegóły odpowiedzi z metody **Get** Action w **TriviaController**.

    ![Pobieranie następnych danych pytań za pomocą interfejsu API sieci Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Pobieranie następnych danych pytań za pomocą interfejsu API sieci Web")

    *Pobieranie następnych danych pytań za pomocą interfejsu API sieci Web*

    > [!NOTE]
    > Po zakończeniu pobierania zostanie wyświetlony monit o wykonanie akcji z pobranym plikiem. Pozostaw otwarte okno dialogowe, aby móc oglądać zawartość odpowiedzi za pomocą okna narzędzia deweloperów.
7. Teraz sprawdzimy treść odpowiedzi. Aby to zrobić, kliknij kartę **szczegóły** , a następnie kliknij pozycję **treść odpowiedzi**. Możesz sprawdzić, czy pobrane dane są obiektami z **opcjami** właściwości (czyli listą obiektów **TriviaOption** ), **identyfikatorem** i **tytułem** odpowiadającym klasie **TriviaQuestion** .

    ![Wyświetlanie treści odpowiedzi interfejsu API sieci Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Wyświetlanie treści odpowiedzi interfejsu API sieci Web")

    *Wyświetlanie treści odpowiedzi interfejsu API sieci Web*
8. Wróć do programu Visual Studio i naciśnij klawisze **Shift + F5** , aby zatrzymać debugowanie.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Ćwiczenie 2: Tworzenie interfejsu SPA

W tym ćwiczeniu najpierw utworzysz część frontonu internetowego pojedynek maniaków komputerowych quiz, skupiając się na interakcji aplikacji jednostronicowej przy użyciu **AngularJS**. Następnie można ulepszyć środowisko użytkownika przy użyciu usługi CSS3, aby wykonywać rozbudowane animacje i zapewnić wizualny efekt przełączenia kontekstu podczas przejścia z jednego pytania do następnego.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Zadanie 1 — Tworzenie interfejsu SPA przy użyciu AngularJS

W tym zadaniu będziesz używać **AngularJS** do implementowania strony klienta aplikacji quizu pojedynek maniaków komputerowych. **AngularJS** to struktura języka JavaScript "open source", która rozszerza aplikacje oparte na przeglądarce z możliwością *kontrolera modelu* (MVC), ułatwiając rozwój i testowanie.

Zacznij od zainstalowania AngularJS z konsoli Menedżera pakietów programu Visual Studio. Następnie utworzysz kontroler, aby zapewnić zachowanie aplikacji quizu pojedynek maniaków komputerowych i widoku, aby renderować pytania quizu i odpowiedzi za pomocą aparatu szablonów AngularJS.

> [!NOTE]
> Aby uzyskać więcej informacji na temat AngularJS, zobacz [[http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).

1. Otwórz **Visual Studio Express 2013 dla sieci Web** i Otwórz rozwiązanie **GeekQuiz. sln** znajdujące się w folderze **Source/Ex2-CreatingASPAInterface/BEGIN** . Alternatywnie możesz kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.
2. Otwórz **konsolę Menedżera pakietów** z **narzędzi** > **Menedżera pakietów NuGet**. Wpisz następujące polecenie, aby zainstalować pakiet NuGet **AngularJS. Core** .

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **scripts** projektu **GeekQuiz** i wybierz polecenie **Dodaj | Nowy folder**. Nadaj nazwę **aplikacji** folderu i naciśnij klawisz **Enter**.
4. Kliknij prawym przyciskiem myszy folder **aplikacji** , który właśnie został utworzony, a następnie wybierz pozycję **Dodaj | Plik JavaScript**.

    ![Tworzenie nowego pliku JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Tworzenie nowego pliku JavaScript*
5. W oknie dialogowym **Określanie nazwy dla elementu** wpisz polecenie *Quiz-Controller* w polu tekstowym **Nazwa elementu** i kliknij przycisk **OK**.

    ![Nadawanie nazwy nowemu plikowi JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Nadawanie nazwy nowemu plikowi JavaScript*
6. W pliku **Quiz-Controller. js** Dodaj następujący kod, aby zadeklarować i zainicjować kontroler AngularJS **QuizCtrl** .

    (Fragment kodu- *AspNetWebApiSpa-Ex2-AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Funkcja konstruktora kontrolera **QuizCtrl** oczekuje wstrzykiwanego parametru o nazwie **$SCOPE**. Początkowy stan zakresu należy skonfigurować w funkcji konstruktora przez dołączenie właściwości do obiektu **$SCOPE** . Właściwości zawierają **model widoku**i będą dostępne dla szablonu, gdy kontroler zostanie zarejestrowany.
    > 
    > Kontroler **QuizCtrl** jest zdefiniowany wewnątrz modułu o nazwie **QuizApp**. Moduły są jednostkami pracy, które umożliwiają przerwanie działania aplikacji w osobnych składnikach. Główne zalety korzystania z modułów polegają na tym, że kod jest łatwiejszy do zrozumienia i ułatwia testowanie jednostkowe, możliwość ponownego wykorzystania i łatwość utrzymania.
7. Teraz dodasz zachowanie do zakresu w celu reagowania na zdarzenia wyzwalane w tym widoku. Dodaj następujący kod na końcu kontrolera **QuizCtrl** , aby zdefiniować funkcję **nextQuestion** w obiekcie **$SCOPE** .

    (Fragment kodu- *AspNetWebApiSpa-Ex2-AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Ta funkcja pobiera następne pytanie z internetowego interfejsu API **kwizy** utworzonego w poprzednim ćwiczeniu i dołącza dane pytania do obiektu **$SCOPE** .
8. Wstaw poniższy kod na końcu kontrolera **QuizCtrl** , aby zdefiniować funkcję **sendAnswer** w obiekcie **$SCOPE** .

    (Fragment kodu- *AspNetWebApiSpa-Ex2-AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Ta funkcja wysyła odpowiedź wybraną przez użytkownika do internetowego interfejsu API **kwizy** i zapisuje wynik — tj. Jeśli odpowiedź jest poprawna lub nie — w obiekcie **$SCOPE** .
    > 
    > Funkcje **nextQuestion** i **sendAnswer** z powyższych funkcji używają obiektu AngularJS **$http** , aby przeprowadzić abstrakcyjną komunikację z interfejsem API sieci Web za pośrednictwem obiektu JavaScript XMLHttpRequest z przeglądarki. AngularJS obsługuje inną usługę, która zapewnia wyższy poziom abstrakcji do wykonywania operacji CRUD w odniesieniu do zasobu za pomocą interfejsów API RESTful. Obiekt **$Resource** AngularJS ma metody akcji, które zapewniają zachowania wysokiego poziomu bez konieczności współdziałania z obiektem **$http** . Rozważ użycie obiektu **$Resource** w scenariuszach, które wymagają modelu CRUD (informacje dotyczące pierwszego planu, zobacz [dokumentację $Resource](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Następnym krokiem jest utworzenie szablonu AngularJS, który definiuje widok dla quizu. Aby to zrobić, Otwórz plik **index. cshtml** w **widokach | Folder macierzysty** i Zastąp zawartość następującym kodem.

    (Fragment kodu- *AspNetWebApiSpa-Ex2-GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Szablon AngularJS to deklaracyjne Specyfikacja, która używa informacji z modelu i kontrolera do przekształcania statycznej znaczników na Widok dynamiczny widoczny dla użytkownika w przeglądarce. Poniżej przedstawiono przykłady elementów AngularJS i atrybutów elementów, których można użyć w szablonie:
    > 
    > - Dyrektywa **ng-App** informuje ANGULARJS element dom, który reprezentuje element główny aplikacji.
    > - Dyrektywa o **kontrolerze ng** dołącza kontroler do modelu dom w punkcie, w którym jest zadeklarowana dyrektywa.
    > - Notacja nawiasów klamrowych **{{}}** oznacza powiązania z właściwościami zakresu zdefiniowanymi w kontrolerze.
    > - Dyrektywa **ng-kliknięcia** służy do wywoływania funkcji zdefiniowanych w zakresie w odpowiedzi na kliknięcia przez użytkownika.
10. Otwórz plik **site. css** wewnątrz folderu **Content** i Dodaj następujące wyróżnione style na końcu pliku, aby zapewnić wygląd i działanie widoku quizu.

    (Fragment kodu- *AspNetWebApiSpa-Ex2-GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Zadanie 2 — Uruchamianie rozwiązania

W tym zadaniu zostanie wykonane rozwiązanie przy użyciu nowego interfejsu użytkownika skompilowanego za pomocą AngularJS, aby odpowiedzieć na niektóre pytania quizu.

1. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie.
2. Zarejestruj nowe konto użytkownika. W tym celu postępuj zgodnie z instrukcjami rejestracji opisanymi w ćwiczeniu 1, zadanie 3.

    > [!NOTE]
    > Jeśli używasz rozwiązania z poprzedniego ćwiczenia, możesz zalogować się przy użyciu utworzonego wcześniej konta użytkownika.
3. Powinna zostać wyświetlona strona **główna** przedstawiająca pierwsze pytanie quizu. Odpowiedz na pytanie, klikając jedną z opcji. Spowoduje to wyzwolenie zdefiniowanej wcześniej funkcji **sendAnswer** , która powoduje wysłanie wybranej opcji do internetowego interfejsu API **kwizy** .

    ![Odpowiadanie na pytanie](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "Odpowiadanie na pytanie")

    *Odpowiadanie na pytanie*
4. Po kliknięciu jednego z przycisków powinna zostać wyświetlona odpowiedź. Kliknij przycisk **dalej pytanie** , aby wyświetlić poniższe pytanie. Spowoduje to wyzwolenie funkcji **nextQuestion** zdefiniowanej w kontrolerze.

    ![Żądanie następnego pytania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "Żądanie następnego pytania")

    *Żądanie następnego pytania*
5. Powinno zostać wyświetlone następne pytanie. Kontynuuj odpowiadanie na pytania dowolną liczbę razy. Po ukończeniu wszystkich pytań należy zwrócić się do pierwszego pytania.

    ![Inne pytanie](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "Inne pytanie")

    *Następne pytanie*
6. Wróć do programu Visual Studio i naciśnij klawisze **Shift + F5** , aby zatrzymać debugowanie.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Zadanie 3 — Tworzenie animacji przerzucania przy użyciu CSS3

W tym zadaniu będziesz używać właściwości CSS3 do wykonywania rozbudowanych animacji przez dodanie efektu przerzucania po odebraniu pytania i pobraniu następnego pytania.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **Content (zawartość** ) projektu **GeekQuiz** i wybierz polecenie **Dodaj | Istniejący element.** ...

    ![Dodawanie istniejącego elementu do folderu zawartości](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Dodawanie istniejącego elementu do folderu zawartości")

    *Dodawanie istniejącego elementu do folderu zawartości*
2. W oknie dialogowym **Dodaj istniejący element** przejdź do folderu **Source/Assets** i wybierz pozycję **Przerzuć. css**. Kliknij pozycję **Add** (Dodaj).

    ![Dodawanie pliku przerzucania CSS z zasobów](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Dodawanie pliku przerzucania CSS z zasobów")

    *Dodawanie pliku przerzucania CSS z zasobów*
3. Otwórz właśnie dodany plik **. css** i sprawdź jego zawartość.
4. Zlokalizuj komentarz **przerzucania transformacji** . Style poniżej tego komentarza wykorzystują **perspektywę** CSS i przekształcenia **obrócone** w celu wygenerowania efektu&quot; odwrócenia karty &quot;.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Znajdź **przycisk Ukryj tył okienka podczas przerzucania** komentarza. Styl poniżej tego komentarza powoduje ukrycie boków twarzy, gdy znajdują się one poza przeglądarką, ustawiając właściwość CSS z **widocznośćmi** niewidocznymi do *ukrycia*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Otwórz plik **BundleConfig.cs** w **aplikacji\_Start** folder i Dodaj odwołanie do pliku **. css** w pakiecie **&quot;~/Content/CSS&quot;**

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie i zalogować się przy użyciu swoich poświadczeń.
8. Odpowiedz na pytanie, klikając jedną z opcji. Zwróć uwagę na efekt przerzucenia podczas przechodzenia między widokami.

    ![Odpowiadanie na pytanie za pomocą efektu przerzucania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "Odpowiadanie na pytanie za pomocą efektu przerzucania")

    *Odpowiadanie na pytanie za pomocą efektu przerzucania*
9. Kliknij przycisk **dalej** , aby pobrać poniższe pytanie. Efekt przewrócenia powinien zostać wyświetlony ponownie.

    ![Pobieranie następującego pytania z efektem przerzucania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "Pobieranie następującego pytania z efektem przerzucania")

    *Pobieranie następującego pytania z efektem przerzucania*

---

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Dzięki zakończeniu tego praktycznego laboratorium wiesz, jak:

- Tworzenie kontrolera interfejsu API sieci Web ASP.NET za pomocą szkieletu ASP.NET
- Implementowanie akcji Get interfejsu API sieci Web w celu pobrania następnego pytania quizu
- Implementowanie akcji post interfejsu API sieci Web w celu przechowywania odpowiedzi quizu
- Zainstaluj AngularJS z konsoli Menedżera pakietów programu Visual Studio
- Implementowanie szablonów i kontrolerów AngularJS
- Używanie przejść CSS3 do wykonywania efektów animacji
