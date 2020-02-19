---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: Uzyskiwanie dostępu do danych modelu za pomocą kontroleraC#() | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2e9c7f24d426635760b613f570b85455ce71b3dc
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458171"
---
# <a name="accessing-your-models-data-from-a-controller-c"></a>Uzyskiwanie dostępu do danych modelu za pomocą kontrolera (C#)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.
> 
> 
> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt programu Visual Web Developer z C# kodem źródłowym jest dostępny do załączenia do tego tematu. [Pobierz wersję C# programu](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz Visual Basic, przejdź do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.

W tej sekcji utworzysz nową klasę `MoviesController` i piszesz kod, który pobiera dane filmu i wyświetla go w przeglądarce przy użyciu szablonu widoku. Pamiętaj, aby skompilować aplikację przed kontynuowaniem.

Kliknij prawym przyciskiem myszy folder *controllers* i Utwórz nowy kontroler `MoviesController`. Wybierz następujące opcje:

- Nazwa kontrolera: **MoviesController**. (Jest to ustawienie domyślne. )
- Szablon: **kontroler z akcjami odczytu/zapisu i widokami korzystającymi z Entity Framework**.
- Model klasy: **Movie (MvcMovie. models)** .
- Klasa kontekstu danych: **MovieDBContext (MvcMovie. models)** .
- Widoki: **Razor (cshtml)** . (Wartość domyślna).

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

Kliknij pozycję **Add** (Dodaj). Visual Web Developer tworzy następujące pliki i foldery:

- Plik *MoviesController.cs* w folderze *controllers* programu Project.
- Folder *filmów* w folderze *widoki* projektu.
- *Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*i *index. cshtml* w nowym folderze *Views\Movies* .

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

Mechanizm szkieletu ASP.NET MVC 3 automatycznie utworzył metody i widoki akcji CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie). Masz teraz w pełni funkcjonalną aplikację sieci Web, która umożliwia tworzenie, wyświetlanie list, edytowanie i usuwanie wpisów filmów.

Uruchom aplikację i przejdź do kontrolera `Movies`, dołączając */Movies* do adresu URL na pasku adresu przeglądarki. Ponieważ aplikacja jest zależna od domyślnego routingu (zdefiniowanego w pliku *Global. asax* ), żądanie przeglądarki `http://localhost:xxxxx/Movies` jest kierowane do domyślnej metody akcji `Index` kontrolera `Movies`. Innymi słowy, `http://localhost:xxxxx/Movies` żądania przeglądarki są skutecznie takie same, jak `http://localhost:xxxxx/Movies/Index`żądania przeglądarki. Wynik jest pustą listą filmów, ponieważ nie został jeszcze dodany.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Tworzenie filmu

Wybierz łącze **Utwórz nowy** . Wprowadź szczegóły dotyczące filmu, a następnie kliknij przycisk **Utwórz** .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Kliknięcie przycisku **Utwórz** powoduje opublikowanie formularza na serwerze, gdzie informacje o filmie są zapisywane w bazie danych. Następnie nastąpi przekierowanie do adresu URL */Movies* , w którym można zobaczyć nowo utworzony film na liście.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Utwórz kilka dodatkowych wpisów filmu. Wypróbuj linki **Edytuj**, **szczegóły**i **Usuń** , które są wszystkie funkcjonalne.

## <a name="examining-the-generated-code"></a>Badanie wygenerowanego kodu

Otwórz plik *Controllers\MoviesController.cs* i przejrzyj wygenerowaną metodę `Index`. Poniżej przedstawiono część kontrolera filmu z metodą `Index`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Poniższy wiersz z klasy `MoviesController` tworzy wystąpienie kontekstu bazy danych filmu, jak opisano wcześniej. Możesz użyć kontekstu bazy danych filmu, aby wysyłać zapytania, edytować i usuwać filmy.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Żądanie do kontrolera `Movies` zwraca wszystkie wpisy w tabeli `Movies` bazy danych filmów, a następnie przekazuje wyniki do widoku `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modele silnie wpisane i @model słowo kluczowe

Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do szablonu widoku przy użyciu obiektu `ViewBag`. `ViewBag` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.

ASP.NET MVC oferuje również możliwość przekazywania danych o jednoznacznie określonym typie lub obiektów do szablonu widoku. Takie silnie wpisane podejście umożliwia lepsze Sprawdzanie kodu w czasie kompilacji i bogatsze IntelliSense w edytorze Visual Web Developer. Korzystamy z tej metody z szablonem widoku klasy `MoviesController` i *index. cshtml* .

Zwróć uwagę, jak kod tworzy obiekt [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) , gdy wywołuje metodę pomocnika `View` w metodzie `Index` akcji. Następnie kod przekazuje tę `Movies` listę z kontrolera do widoku:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Dołączając instrukcję `@model` w górnej części pliku szablonu widoku, można określić typ obiektu, którego oczekuje widok. Po utworzeniu kontrolera filmu program Visual Web Developer automatycznie dołączał następujące `@model` instrukcji w górnej części pliku *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Ta `@model` dyrektywa pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony. Na przykład w szablonie *index. cshtml* kod przechodzi przez film przez wykonanie instrukcji `foreach` na obiekcie `Model` o jednoznacznie określonym typie:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy obiekt `item` w pętli jest wpisywany jako `Movie`. W związku z innymi korzyściami oznacza to, że w edytorze kodu są dostępne sprawdzanie w czasie kompilacji kodu i pełna obsługa technologii IntelliSense:

[![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntelliSense")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Praca z SQL Server Compact

Entity Framework Code First wykryła, że podane parametry połączenia z bazą danych wskazywały na `Movies` bazę danych, która jeszcze nie istnieje, więc Code First automatycznie utworzyć bazę danych. Możesz sprawdzić, czy został on utworzony, przeglądając folder *danych\_aplikacji* . Jeśli nie widzisz pliku *Films. sdf* , kliknij przycisk **Pokaż wszystkie pliki** na pasku narzędzi **Eksplorator rozwiązań** , kliknij przycisk **odśwież** , a następnie rozwiń folder *dane\_aplikacji* .

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Kliknij dwukrotnie ikonę *filmy. sdf* , aby otworzyć **Eksplorator serwera**. Następnie rozwiń folder **tabele** , aby wyświetlić tabele, które zostały utworzone w bazie danych programu.

> [!NOTE]
> Jeśli wystąpi błąd w przypadku dwukrotnego kliknięcia *filmów. sdf*, upewnij się, że zainstalowano [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + Tools). (W przypadku linków do oprogramowania zapoznaj się z listą wymagań wstępnych w części 1 tej serii samouczków). Jeśli zainstalujesz wersję teraz, musisz zamknąć i ponownie otworzyć program Visual Web Developer.

[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

Istnieją dwie tabele, jeden dla `Movie` jednostki, a następnie tabela `EdmMetadata`. `EdmMetadata` tabela jest używana przez Entity Framework do określenia, kiedy model i baza danych nie są zsynchronizowane.

Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz polecenie **Pokaż dane tabeli** , aby wyświetlić utworzone dane.

[![Filmy](accessing-your-models-data-from-a-controller/_static/image16.png "Filmy")](accessing-your-models-data-from-a-controller/_static/image15.png)

Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz polecenie **Edytuj schemat tabeli**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Zwróć uwagę, jak schemat tabeli `Movies` mapuje do utworzonej wcześniej klasy `Movie`. Entity Framework Code First automatycznie utworzył ten schemat na podstawie klasy `Movie`.

Po zakończeniu zamknij połączenie. (Jeśli połączenie nie zostanie zamknięte, podczas następnego uruchomienia projektu może wystąpić błąd).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Teraz masz bazę danych i prostą stronę z listą, aby wyświetlić z niej zawartość. W następnym samouczku sprawdzimy resztę kodu szkieletowego i dodamy metodę `SearchIndex` i widok `SearchIndex`, który umożliwi wyszukiwanie filmów w tej bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-model.md)
> [dalej](examining-the-edit-methods-and-edit-view.md)
