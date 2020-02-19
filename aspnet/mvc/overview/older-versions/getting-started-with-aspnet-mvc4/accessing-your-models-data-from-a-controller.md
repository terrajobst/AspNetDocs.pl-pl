---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Uzyskiwanie dostępu do danych modelu z kontrolera | Microsoft Docs
author: Rick-Anderson
description: 'Uwaga: zaktualizowana wersja tego samouczka jest dostępna w tym miejscu, w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 7c4aa34567ac4fb31d1ed874cf65986c4e779e66
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456169"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Uzyskiwanie dostępu do danych modelu za pomocą kontrolera

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.

W tej sekcji utworzysz nową klasę `MoviesController` i piszesz kod, który pobiera dane filmu i wyświetla go w przeglądarce przy użyciu szablonu widoku.

**Skompiluj aplikację** przed przejściem do następnego kroku.

Kliknij prawym przyciskiem myszy folder *controllers* i Utwórz nowy kontroler `MoviesController`. Poniższe opcje nie będą wyświetlane do momentu skompilowania aplikacji. Wybierz następujące opcje:

- Nazwa kontrolera: **MoviesController**. (Jest to ustawienie domyślne. )
- Szablon: **kontroler MVC z akcjami odczytu/zapisu i widokami korzystającymi z Entity Framework**.
- Model klasy: **Movie (MvcMovie. models)** .
- Klasa kontekstu danych: **MovieDBContext (MvcMovie. models)** .
- Widoki: **Razor (cshtml)** . (Wartość domyślna).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Kliknij pozycję **Add** (Dodaj). Visual Studio Express tworzy następujące pliki i foldery:

- Plik *MoviesController.cs* w folderze *controllers* programu Project.
- Folder *filmów* w folderze *widoki* projektu.
- *Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*i *index. cshtml* w nowym folderze *Views\Movies* .

ASP.NET MVC 4 automatycznie utworzył metody i widoki akcji CRUD (Create, Read, Update i Delete) dla Ciebie (automatyczne tworzenie metod i widoków akcji CRUD jest znane jako rusztowania). Masz teraz w pełni funkcjonalną aplikację sieci Web, która umożliwia tworzenie, wyświetlanie list, edytowanie i usuwanie wpisów filmów.

Uruchom aplikację i przejdź do kontrolera `Movies`, dołączając */Movies* do adresu URL na pasku adresu przeglądarki. Ponieważ aplikacja jest zależna od domyślnego routingu (zdefiniowanego w pliku *Global. asax* ), żądanie przeglądarki `http://localhost:xxxxx/Movies` jest kierowane do domyślnej metody akcji `Index` kontrolera `Movies`. Innymi słowy, `http://localhost:xxxxx/Movies` żądania przeglądarki są skutecznie takie same, jak `http://localhost:xxxxx/Movies/Index`żądania przeglądarki. Wynik jest pustą listą filmów, ponieważ nie został jeszcze dodany.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Tworzenie filmu

Wybierz łącze **Utwórz nowy** . Wprowadź szczegóły dotyczące filmu, a następnie kliknij przycisk **Utwórz** .

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Kliknięcie przycisku **Utwórz** powoduje opublikowanie formularza na serwerze, gdzie informacje o filmie są zapisywane w bazie danych. Następnie nastąpi przekierowanie do adresu URL */Movies* , w którym można zobaczyć nowo utworzony film na liście.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Utwórz kilka dodatkowych wpisów filmu. Wypróbuj linki **Edytuj**, **szczegóły**i **Usuń** , które są wszystkie funkcjonalne.

## <a name="examining-the-generated-code"></a>Badanie wygenerowanego kodu

Otwórz plik *Controllers\MoviesController.cs* i przejrzyj wygenerowaną metodę `Index`. Poniżej przedstawiono część kontrolera filmu z metodą `Index`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Poniższy wiersz z klasy `MoviesController` tworzy wystąpienie kontekstu bazy danych filmu, jak opisano wcześniej. Możesz użyć kontekstu bazy danych filmu, aby wysyłać zapytania, edytować i usuwać filmy.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Żądanie do kontrolera `Movies` zwraca wszystkie wpisy w tabeli `Movies` bazy danych filmów, a następnie przekazuje wyniki do widoku `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modele silnie wpisane i @model słowo kluczowe

Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do szablonu widoku przy użyciu obiektu `ViewBag`. `ViewBag` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.

ASP.NET MVC oferuje również możliwość przekazywania danych o jednoznacznie określonym typie lub obiektów do szablonu widoku. Takie silnie wpisane podejście umożliwia lepsze Sprawdzanie kodu w czasie kompilacji i bogatsze IntelliSense w edytorze programu Visual Studio. Mechanizm tworzenia szkieletu w programie Visual Studio używa tego podejścia z klasą `MoviesController` i wyświetlania szablonów podczas tworzenia metod i widoków.

W pliku *Controllers\MoviesController.cs* Przejrzyj wygenerowaną metodę `Details`. Poniżej przedstawiono część kontrolera filmu z metodą `Details`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Jeśli `Movie` zostanie znaleziona, wystąpienie modelu `Movie` zostanie przesłane do widoku szczegółów. Zapoznaj się z zawartością pliku *Views\Movies\Details.cshtml* .

Dołączając instrukcję `@model` w górnej części pliku szablonu widoku, można określić typ obiektu, którego oczekuje widok. Po utworzeniu kontrolera filmu program Visual Studio automatycznie dołączał następującą instrukcję `@model` w górnej części pliku *details. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Ta `@model` dyrektywa pozwala uzyskać dostęp do filmu, który kontroler przeszedł do widoku przy użyciu obiektu `Model`, który jest silnie określony. Na przykład w szablonie *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` i pomocników HTML [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) z silnie wpisanąm obiektem `Model`. Metody Create i Edit i View templates również przekazują obiekt modelu filmu.

Sprawdź szablon widoku *index. cshtml* i metodę `Index` w pliku *MoviesController.cs* . Zwróć uwagę, jak kod tworzy obiekt [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) , gdy wywołuje metodę pomocnika `View` w metodzie `Index` akcji. Następnie kod przekazuje tę `Movies` listę z kontrolera do widoku:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Po utworzeniu kontrolera filmu Visual Studio Express automatycznie dołączać następujące instrukcje `@model` w górnej części pliku *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Ta `@model` dyrektywa pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony. Na przykład w szablonie *index. cshtml* kod przechodzi przez film przez wykonanie instrukcji `foreach` na obiekcie `Model` o jednoznacznie określonym typie:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy obiekt `item` w pętli jest wpisywany jako `Movie`. W związku z innymi korzyściami oznacza to, że w edytorze kodu są dostępne sprawdzanie w czasie kompilacji kodu i pełna obsługa technologii IntelliSense:

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Praca z SQL Server LocalDB

Entity Framework Code First wykryła, że podane parametry połączenia z bazą danych wskazywały na `Movies` bazę danych, która jeszcze nie istnieje, więc Code First automatycznie utworzyć bazę danych. Możesz sprawdzić, czy został on utworzony, przeglądając folder *danych\_aplikacji* . Jeśli plik *wideo. mdf* nie jest widoczny, kliknij przycisk **Pokaż wszystkie pliki** na pasku narzędzi **Eksplorator rozwiązań** , kliknij przycisk **odśwież** , a następnie rozwiń folder *dane\_aplikacji* .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Kliknij dwukrotnie ikonę *filmy. mdf* , aby otworzyć **Eksploratora bazy danych**, a następnie rozwiń folder **tabele** , aby wyświetlić tabelę filmy.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Jeśli Eksplorator bazy danych nie jest wyświetlany, w menu **Narzędzia** wybierz pozycję **Połącz z bazą danych**, a następnie Anuluj okno dialogowe **Wybieranie źródła danych** . Spowoduje to wymuszenie otwarcia Eksploratora bazy danych.

> [!NOTE]
> Jeśli używasz programu pliku VWD lub Visual Studio 2010 i wystąpi błąd podobny do jednego z następujących:
> 
> - Baza danych "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. Nie można otworzyć środowiska MDF, ponieważ jest w wersji 706. Ten serwer obsługuje wersję 655 i wcześniejszą. Ścieżka obniżania poziomu nie jest obsługiwana.
> - &quot;wyjątek InvalidOperation został nieobsłużony przez kod użytkownika&quot; dostarczonego elementu SqlConnection nie określa katalogu początkowego.
> 
> Musisz zainstalować [SQL Server narzędzia danych](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) i [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Sprawdź parametry połączenia `MovieDBContext` określone na poprzedniej stronie.

Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz polecenie **Pokaż dane tabeli** , aby wyświetlić utworzone dane.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz pozycję **Otwórz definicję tabeli** , aby wyświetlić strukturę tabeli utworzoną przez Entity Framework Code First.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Zwróć uwagę, jak schemat tabeli `Movies` mapuje do utworzonej wcześniej klasy `Movie`. Entity Framework Code First automatycznie utworzył ten schemat na podstawie klasy `Movie`.

Po zakończeniu zamknij połączenie, klikając prawym przyciskiem myszy pozycję *MovieDBContext* i wybierając pozycję **Zamknij połączenie**. (Jeśli połączenie nie zostanie zamknięte, podczas następnego uruchomienia projektu może wystąpić błąd).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Teraz masz bazę danych i prostą stronę z listą, aby wyświetlić z niej zawartość. W następnym samouczku sprawdzimy resztę kodu szkieletowego i dodamy metodę `SearchIndex` i widok `SearchIndex`, który umożliwi wyszukiwanie filmów w tej bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-model.md)
> [dalej](examining-the-edit-methods-and-edit-view.md)
