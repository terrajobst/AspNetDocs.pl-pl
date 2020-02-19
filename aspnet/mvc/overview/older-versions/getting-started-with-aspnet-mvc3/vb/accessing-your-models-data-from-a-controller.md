---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Uzyskiwanie dostępu do danych modelu z kontrolera (VB) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 37f45d8f12e3ab5c485718bcf2c59934ad272118
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458029"
---
# <a name="accessing-your-models-data-from-a-controller-vb"></a>Uzyskiwanie dostępu do danych modelu za pomocą kontrolera (VB)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępny do tego tematu. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przejdź do [ C# wersji](../cs/accessing-your-models-data-from-a-controller.md) tego samouczka.

W tej sekcji utworzysz nową klasę `MoviesController` i piszesz kod, który pobiera dane filmu i wyświetla go w przeglądarce przy użyciu szablonu widoku. Pamiętaj, aby skompilować aplikację przed kontynuowaniem.

Kliknij prawym przyciskiem myszy folder *controllers* i Utwórz nowy kontroler `MoviesController`. Wybierz następujące opcje:

- Nazwa kontrolera: **MoviesController**. (Jest to wartość domyślna).
- Szablon: **kontroler z akcjami odczytu/zapisu i widokami korzystającymi z Entity Framework**.
- Model klasy: **Movie (MvcMovie. models)** .
- Klasa kontekstu danych: **MovieDBContext (MvcMovie. models)** .
- Widoki: **Razor (cshtml)** . (Wartość domyślna).

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Kliknij pozycję **Add** (Dodaj). Visual Web Developer tworzy następujące pliki i foldery:

- Plik *MoviesController. vb* w folderze *controllers* projektu.
- Folder *filmów* w folderze *widoki* projektu.
- *Utwórz. vbhtml, Usuń. vbhtml, details. vbhtml, Edit. vbhtml*i *index. vbhtml* w nowym folderze *Views\Movies* .

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

Mechanizm szkieletu ASP.NET MVC 3 automatycznie utworzył metody i widoki akcji CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie). Masz teraz w pełni funkcjonalną aplikację sieci Web, która umożliwia tworzenie, wyświetlanie list, edytowanie i usuwanie wpisów filmów.

Uruchom aplikację i przejdź do kontrolera `Movies`, dołączając */Movies* do adresu URL na pasku adresu przeglądarki. Ponieważ aplikacja jest zależna od domyślnego routingu (zdefiniowanego w pliku *Global. asax* ), żądanie przeglądarki `http://localhost:xxxxx/Movies` jest kierowane do domyślnej metody akcji `Index` kontrolera `Movies`. Innymi słowy, `http://localhost:xxxxx/Movies` żądania przeglądarki są skutecznie takie same, jak `http://localhost:xxxxx/Movies/Index`żądania przeglądarki. Wynik jest pustą listą filmów, ponieważ nie został jeszcze dodany.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Tworzenie filmu

Wybierz łącze **Utwórz nowy** . Wprowadź szczegóły dotyczące filmu, a następnie kliknij przycisk **Utwórz** .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Kliknięcie przycisku **Utwórz** powoduje opublikowanie formularza na serwerze, gdzie informacje o filmie są zapisywane w bazie danych. Następnie nastąpi przekierowanie do adresu URL */Movies* , w którym można zobaczyć nowo utworzony film na liście.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Utwórz kilka dodatkowych wpisów filmu. Wypróbuj linki **Edytuj**, **szczegóły**i **Usuń** , które są wszystkie funkcjonalne.

## <a name="examining-the-generated-code"></a>Badanie wygenerowanego kodu

Otwórz plik *Controllers\MoviesController.vb* i przejrzyj wygenerowaną metodę `Index`. Poniżej przedstawiono część kontrolera filmu z metodą `Index`.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

Poniższy wiersz z klasy `MoviesController` tworzy wystąpienie kontekstu bazy danych filmu, jak opisano wcześniej. Możesz użyć kontekstu bazy danych filmu, aby wysyłać zapytania, edytować i usuwać filmy.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Żądanie do kontrolera `Movies` zwraca wszystkie wpisy w tabeli `Movies` bazy danych filmów, a następnie przekazuje wyniki do widoku `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modele silnie wpisane i @model słowo kluczowe

Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do szablonu widoku przy użyciu obiektu `ViewBag`. `ViewBag` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.

ASP.NET MVC oferuje również możliwość przekazywania danych o jednoznacznie określonym typie lub obiektów do szablonu widoku. Takie silnie wpisane podejście umożliwia lepsze Sprawdzanie kodu w czasie kompilacji i bogatsze IntelliSense w edytorze Visual Web Developer. Używamy tej metody z szablonem widoku klasy `MoviesController` i *index. vbhtml* .

Zwróć uwagę, jak kod tworzy obiekt [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) , gdy wywołuje metodę pomocnika `View` w metodzie `Index` akcji. Następnie kod przekazuje tę `Movies` listę z kontrolera do widoku:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

Dołączając instrukcję `@ModelType` w górnej części pliku szablonu widoku, można określić typ obiektu, którego oczekuje widok. Po utworzeniu kontrolera filmu program Visual Web Developer automatycznie dołączał następujące `@model` instrukcji w górnej części pliku *index. vbhtml* :

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

Ta `@ModelType` dyrektywa pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony. Na przykład w szablonie *index. vbhtml* kod przechodzi przez film przez wykonanie instrukcji `foreach` na obiekcie `Model` o jednoznacznie określonym typie:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy obiekt `item` w pętli jest wpisywany jako `Movie`. W związku z innymi korzyściami oznacza to, że w edytorze kodu są dostępne sprawdzanie w czasie kompilacji kodu i pełna obsługa technologii IntelliSense:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Praca z SQL Server Compact

Entity Framework Code First wykryła, że podane parametry połączenia z bazą danych wskazywały na `Movies` bazę danych, która jeszcze nie istnieje, więc Code First automatycznie utworzyć bazę danych. Możesz sprawdzić, czy został on utworzony, przeglądając folder *danych\_aplikacji* . Jeśli nie widzisz pliku *Films. sdf* , kliknij przycisk **Pokaż wszystkie pliki** na pasku narzędzi **Eksplorator rozwiązań** , kliknij przycisk **odśwież** , a następnie rozwiń folder *dane\_aplikacji* .

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Kliknij dwukrotnie ikonę *filmy. sdf* , aby otworzyć **Eksplorator serwera**. Następnie rozwiń folder **tabele** , aby wyświetlić tabele, które zostały utworzone w bazie danych programu.

> [!NOTE]
> Jeśli wystąpi błąd w przypadku dwukrotnego kliknięcia *filmów. sdf*, upewnij się, że zainstalowano **Narzędzia programu Visual Studio 2010 SP1 dla SQL Server Compact 4,0**. (W przypadku linków do oprogramowania zapoznaj się z listą wymagań wstępnych w części 1 tej serii samouczków). Jeśli zainstalujesz wersję teraz, musisz zamknąć i ponownie otworzyć program Visual Web Developer.

[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Istnieją dwie tabele, jeden dla `Movie` jednostki, a następnie tabela `EdmMetadata`. `EdmMetadata` tabela jest używana przez Entity Framework do określenia, kiedy model i baza danych nie są zsynchronizowane.

Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz polecenie **Pokaż dane tabeli** , aby wyświetlić utworzone dane.

[![filmów](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz polecenie **Edytuj schemat tabeli**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Zwróć uwagę, jak schemat tabeli `Movies` mapuje do utworzonej wcześniej klasy `Movie`. Entity Framework Code First automatycznie utworzył ten schemat na podstawie klasy `Movie`.

Po zakończeniu zamknij połączenie. (Jeśli połączenie nie zostanie zamknięte, podczas następnego uruchomienia projektu może wystąpić błąd).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Teraz masz bazę danych i prostą stronę z listą, aby wyświetlić z niej zawartość. W następnym samouczku sprawdzimy resztę kodu szkieletowego i dodamy metodę `SearchIndex` i widok `SearchIndex`, który umożliwi wyszukiwanie filmów w tej bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-model.md)
> [dalej](examining-the-edit-methods-and-edit-view.md)
