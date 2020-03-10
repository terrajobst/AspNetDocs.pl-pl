---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Dodawanie nowego pola do modelu filmu i tabeli (C#) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 40b02a2f608f07091ce6b5339688a1e6290e2e37
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540804"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Dodawanie nowego pola do modelu Movie i tabeli (C#)

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

W tej sekcji wprowadzisz pewne zmiany w klasach modelu i dowiesz się, jak można zaktualizować schemat bazy danych w taki sposób, aby pasował do zmiany modelu.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości oceny do modelu filmu

Zacznij od dodania nowej właściwości `Rating` do istniejącej klasy `Movie`. Otwórz plik *Movie.cs* i dodaj Właściwość `Rating` w następujący sposób:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Kompletna Klasa `Movie` teraz wygląda następująco:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Ponownie skompiluj aplikację przy użyciu polecenia **debuguj** &gt;**Kompiluj film** menu.

Teraz, po zaktualizowaniu klasy `Model` należy również zaktualizować szablony widoków *\Views\Movies\Index.cshtml* i *\Views\Movies\Create.cshtml* , aby obsługiwały nową właściwość `Rating`.

Otwórz plik *\Views\Movies\Index.cshtml* i Dodaj nagłówek kolumny `<th>Rating</th>` tuż po kolumnie **Price** . Następnie Dodaj `<td>` kolumnę blisko końca szablonu, aby renderować `@item.Rating` wartość. Poniżej znajduje się opis zaktualizowanego szablonu widoku *index. cshtml* :

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Następnie otwórz plik *\Views\Movies\Create.cshtml* i Dodaj następujący znacznik w górnej części formularza. Spowoduje to renderowanie pola tekstowego, aby można było określić klasyfikację po utworzeniu nowego filmu.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Zarządzanie modelami i różnicami w schemacie bazy danych

Kod aplikacji został już zaktualizowany, aby obsługiwał nową właściwość `Rating`.

Teraz uruchom aplikację i przejdź do adresu URL */Movies* . Gdy to zrobisz, zobaczysz następujący błąd:

![](adding-a-new-field/_static/image1.png)

Ten błąd jest wyświetlany, ponieważ zaktualizowana Klasa modelu `Movie` w aplikacji jest inna niż schemat tabeli `Movie` istniejącej bazy danych. (Brak kolumny `Rating` w tabeli bazy danych).

Domyślnie w przypadku automatycznego tworzenia bazy danych za pomocą Code First Entity Framework, tak jak wcześniej w tym samouczku, Code First dodaje tabelę do bazy danych, aby ułatwić śledzenie, czy schemat bazy danych jest zsynchronizowany z klasami modelu, z których została wygenerowana. Jeśli nie są zsynchronizowane, Entity Framework zgłosi błąd. Ułatwia to Śledzenie problemów w czasie opracowywania, które można znaleźć w innym miejscu (poprzez zaciemnienie błędów) w czasie wykonywania. Funkcja sprawdzania synchronizacji powoduje, że komunikat o błędzie zostanie wyświetlony.

Istnieją dwa podejścia do rozwiązania błędu:

1. Entity Framework automatycznie porzucić i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu. Takie podejście jest bardzo wygodne podczas aktywnego programowania na testowej bazie danych, ponieważ umożliwia szybkie rozwijanie modelu i schematu bazy danych. Minusem, mimo że utracisz istniejące dane w bazie danych, więc *nie* chcesz używać tego podejścia w produkcyjnej bazie danych.
2. Jawnie zmodyfikuj schemat istniejącej bazy danych, tak aby pasował do klas modelu. Zaletą tego podejścia jest utrzymywanie danych. Tę zmianę można wprowadzić ręcznie lub przez utworzenie skryptu zmiany bazy danych.

W tym samouczku użyjemy pierwszego podejścia — Entity Framework Code First automatycznie ponownie utworzyć bazę danych w dowolnym momencie zmiany modelu.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Automatyczne ponowne tworzenie bazy danych przy użyciu zmian modelu

Zaktualizujmy aplikację, tak aby Code First automatycznie porzucać i odtworzył bazę danych w dowolnym momencie zmiany modelu aplikacji.

> [!NOTE] 
> 
> **Ostrzeżenie** Należy włączyć tę metodę automatycznego porzucania i ponownego tworzenia bazy danych tylko w przypadku korzystania z bazy danych programistycznych lub testowych, a *nie* w produkcyjnej bazie danych, która zawiera rzeczywiste dane. Korzystanie z niego na serwerze produkcyjnym może prowadzić do utraty danych.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , wybierz polecenie **Dodaj**, a następnie wybierz pozycję **Klasa**.

![](adding-a-new-field/_static/image2.png)

Nazwij klasę "MovieInitializer". Zaktualizuj klasę `MovieInitializer`, aby zawierała następujący kod:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Klasa `MovieInitializer` określa, że baza danych używana przez model powinna zostać porzucona i automatycznie ponownie utworzona, jeśli ulegną zmianie klasy modelu. Kod zawiera metodę `Seed`, aby określić niektóre domyślne dane, które mają być automatycznie dodawane do bazy danych w dowolnym momencie, gdy zostanie ona utworzona (lub utworzona). Zapewnia to przydatny sposób wypełniania bazy danych niektórymi przykładowymi danymi, bez konieczności ręcznego wypełniania przy każdym wprowadzeniu zmiany modelu.

Teraz, po zdefiniowaniu klasy `MovieInitializer`, należy ją połączyć w taki sposób, aby po każdym uruchomieniu aplikacji sprawdzić, czy klasy modelu różnią się od schematu w bazie danych. Jeśli tak, można uruchomić inicjatora, aby ponownie utworzyć bazę danych w celu dopasowania jej do modelu, a następnie wypełnić bazę danych przykładowymi danymi.

Otwórz plik *Global. asax* , który znajduje się w katalogu głównym projektu `MvcMovies`:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

Plik *Global. asax* zawiera klasę, która definiuje całą aplikację dla projektu i zawiera `Application_Start` program obsługi zdarzeń, który jest uruchamiany podczas pierwszego uruchomienia aplikacji.

Dodajmy dwie instrukcje using na początku pliku. Pierwszy odwołuje się do przestrzeni nazw Entity Framework, a drugi odwołuje się do przestrzeni nazw, w której znajduje się Klasa `MovieInitializer`:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Następnie znajdź metodę `Application_Start` i Dodaj wywołanie do `Database.SetInitializer` na początku metody, jak pokazano poniżej:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

Właśnie dodana Instrukcja `Database.SetInitializer` wskazuje, że baza danych używana przez wystąpienie `MovieDBContext` powinna zostać automatycznie usunięta i ponownie utworzona, jeśli schemat i baza danych nie są zgodne. I w miarę wypełniania, spowoduje to również wypełnienie bazy danych danymi przykładowymi, które są określone w klasie `MovieInitializer`.

Zamknij plik *Global. asax* .

Uruchom aplikację jeszcze raz i przejdź do adresu URL */Movies* . Po uruchomieniu aplikacji wykryje, że struktura modelu nie jest już zgodna ze schematem bazy danych. Automatycznie ponownie tworzy bazę danych w celu dopasowania jej do nowej struktury modelu i wypełniania bazy danych za pomocą przykładowych filmów:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Kliknij link **Utwórz nowy** , aby dodać nowy film. Należy pamiętać, że można dodać klasyfikację.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Kliknij przycisk **Utwórz**. Nowy film, łącznie z klasyfikacją, znajduje się teraz na liście filmów:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

W tej sekcji pokazano, jak można modyfikować obiekty modelu i zachować synchronizację bazy danych ze zmianami. Poznasz również sposób wypełniania nowo utworzonej bazy danych z przykładowymi danymi, dzięki czemu można wypróbować scenariusze. Następnie Przyjrzyjmy się sposobom dodawania bogatszej logiki walidacji do klas modelu i włączania niektórych reguł firmowych.

> [!div class="step-by-step"]
> [Poprzednie](examining-the-edit-methods-and-edit-view.md)
> [dalej](adding-validation-to-the-model.md)
