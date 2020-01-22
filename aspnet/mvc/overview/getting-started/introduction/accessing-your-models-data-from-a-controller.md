---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Uzyskiwanie dostępu do danych modelu z kontrolera | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: e01953dcfb2abf2db53a8aa869aa75b40485daca
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519092"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Uzyskiwanie dostępu do danych modelu za pomocą kontrolera

Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

W tej sekcji utworzysz nową klasę `MoviesController` i piszesz kod, który pobiera dane filmu i wyświetla go w przeglądarce przy użyciu szablonu widoku.

**Skompiluj aplikację** przed przejściem do następnego kroku. Jeśli aplikacja nie zostanie utworzona, wystąpi błąd podczas dodawania kontrolera.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder *controllers* , a następnie kliknij polecenie **Dodaj**, a następnie pozycję **kontroler**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

W oknie dialogowym **Dodawanie szkieletu** kliknij pozycję **kontroler MVC 5 z widokami, używając Entity Framework**, a następnie kliknij przycisk **Dodaj**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Wybierz pozycję **film (MvcMovie. models)** dla klasy model.
- Wybierz pozycję **MovieDBContext (MvcMovie. models)** dla klasy kontekstu danych.
- Dla nazwy kontrolera wprowadź **MoviesController**.

  Na poniższej ilustracji przedstawiono okno dialogowe ukończone.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Kliknij przycisk **Dodaj**. (Jeśli wystąpi błąd, prawdopodobnie nie skompilowano aplikacji przed rozpoczęciem dodawania kontrolera). Program Visual Studio tworzy następujące pliki i foldery:

- Plik *MoviesController.cs* w folderze *controllers* .
- Folder *Views\Movies* .
- *Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*i *index. cshtml* w nowym folderze *Views\Movies* .

Program Visual Studio automatycznie utworzył metody i widoki akcji [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenie, odczytywanie, aktualizowanie i usuwanie) dla użytkownika (automatyczne tworzenie metod akcji CRUD i widoków jest znane jako rusztowania). Masz teraz w pełni funkcjonalną aplikację sieci Web, która umożliwia tworzenie, wyświetlanie list, edytowanie i usuwanie wpisów filmów.

Uruchom aplikację i kliknij link do **filmu MVC** (lub przejdź do kontrolera `Movies`, dołączając */Movies* do adresu URL na pasku adresu przeglądarki). Ponieważ aplikacja jest zależna od domyślnego routingu (zdefiniowanego w *aplikacji\_pliku Start\RouteConfig.cs* ), żądanie przeglądarki `http://localhost:xxxxx/Movies` jest kierowane do domyślnej metody akcji `Index` kontrolera `Movies`. Innymi słowy, `http://localhost:xxxxx/Movies` żądania przeglądarki są skutecznie takie same, jak `http://localhost:xxxxx/Movies/Index`żądania przeglądarki. Wynik jest pustą listą filmów, ponieważ nie został jeszcze dodany.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Tworzenie filmu

Wybierz łącze **Utwórz nowy** . Wprowadź szczegóły dotyczące filmu, a następnie kliknij przycisk **Utwórz** .

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> W polu cena może nie być możliwe wprowadzanie przecinków dziesiętnych ani przecinków. Aby zapewnić obsługę walidacji jQuery dla ustawień regionalnych innych niż angielskie, które używają przecinka (&quot;,&quot;) dla punktu dziesiętnego i formatów daty innych niż angielski, należy dołączyć plik *globalizacjs. js* i określone *kultury/globalizacja pliku kultury. js* (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) i JavaScript, aby użyć `Globalize.parseFloat`. Pokażę, jak to zrobić w następnym samouczku. Na razie po prostu wprowadź wartości całkowite, takie jak 10.

Kliknięcie przycisku **Utwórz** powoduje opublikowanie formularza na serwerze, gdzie informacje o filmie są zapisywane w bazie danych. Następnie nastąpi przekierowanie do adresu URL */Movies* , w którym można zobaczyć nowo utworzony film na liście.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Utwórz kilka dodatkowych wpisów filmu. Wypróbuj linki **Edytuj**, **szczegóły**i **Usuń** , które są wszystkie funkcjonalne.

## <a name="examining-the-generated-code"></a>Badanie wygenerowanego kodu

Otwórz plik *Controllers\MoviesController.cs* i przejrzyj wygenerowaną metodę `Index`. Poniżej przedstawiono część kontrolera filmu z metodą `Index`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Żądanie do kontrolera `Movies` zwraca wszystkie wpisy w tabeli `Movies`, a następnie przekazuje wyniki do widoku `Index`. Poniższy wiersz z klasy `MoviesController` tworzy wystąpienie kontekstu bazy danych filmu, jak opisano wcześniej. Możesz użyć kontekstu bazy danych filmu, aby wysyłać zapytania, edytować i usuwać filmy.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modele silnie wpisane i @model słowo kluczowe

Wcześniej w tym samouczku pokazano, jak kontroler może przekazać dane lub obiekty do szablonu widoku przy użyciu obiektu `ViewBag`. `ViewBag` jest obiektem dynamicznym, który zapewnia wygodny, późny sposób przekazywania informacji do widoku.

MVC oferuje również możliwość przekazywania obiektów z *silną* typem do szablonu widoku. Takie silnie wpisane podejście umożliwia lepsze Sprawdzanie kodu w czasie kompilacji i bogatsze [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) w edytorze programu Visual Studio. Mechanizm tworzenia szkieletu w programie Visual Studio użył tego podejścia (czyli przekazywania *silnego* typu modelu) z klasą `MoviesController` i wyświetlania szablonów podczas tworzenia metod i widoków.

W pliku *Controllers\MoviesController.cs* Przejrzyj wygenerowaną metodę `Details`. Poniżej przedstawiono metodę `Details`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Parametr `id` jest zwykle przenoszona jako dane trasy, na przykład `http://localhost:1234/movies/details/1` ustawi kontroler na kontroler filmu, akcję `details` i `id` na 1. Można również przekazać identyfikator z ciągiem zapytania w następujący sposób:

`http://localhost:1234/movies/details?id=1`

Jeśli `Movie` zostanie znaleziona, wystąpienie modelu `Movie` zostanie przesłane do widoku `Details`:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Zapoznaj się z zawartością pliku *Views\Movies\Details.cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Dołączając instrukcję `@model` w górnej części pliku szablonu widoku, można określić typ obiektu, którego oczekuje widok. Po utworzeniu kontrolera filmu program Visual Studio automatycznie dołączał następującą instrukcję `@model` w górnej części pliku *details. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Ta `@model` dyrektywa pozwala uzyskać dostęp do filmu, który kontroler przeszedł do widoku przy użyciu obiektu `Model`, który jest silnie określony. Na przykład w szablonie *details. cshtml* kod przekazuje każde pole filmu do `DisplayNameFor` i pomocników HTML [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) z silnie wpisanąm obiektem `Model`. Metody `Create` i `Edit` i szablony widoków również przekazują obiekt modelu filmu.

Sprawdź szablon widoku *index. cshtml* i metodę `Index` w pliku *MoviesController.cs* . Zwróć uwagę, jak kod tworzy obiekt [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) , gdy wywołuje metodę pomocnika `View` w metodzie `Index` akcji. Następnie kod przekazuje tę `Movies` listę z metody akcji `Index` do widoku:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Po utworzeniu kontrolera filmu program Visual Studio automatycznie dołączał następującą instrukcję `@model` w górnej części pliku *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Ta `@model` dyrektywa pozwala uzyskać dostęp do listy filmów przekazaną przez kontroler do widoku przy użyciu obiektu `Model`, który jest silnie określony. Na przykład w szablonie *index. cshtml* kod przechodzi przez film przez wykonanie instrukcji `foreach` na obiekcie `Model` o jednoznacznie określonym typie:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Ponieważ obiekt `Model` jest silnie określony (jako obiekt `IEnumerable<Movie>`), każdy obiekt `item` w pętli jest wpisywany jako `Movie`. W związku z innymi korzyściami oznacza to, że w edytorze kodu są dostępne sprawdzanie w czasie kompilacji kodu i pełna obsługa technologii IntelliSense:

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Praca z SQL Server LocalDB

Entity Framework Code First wykryła, że podane parametry połączenia z bazą danych wskazywały na `Movies` bazę danych, która jeszcze nie istnieje, więc Code First automatycznie utworzyć bazę danych. Możesz sprawdzić, czy został on utworzony, przeglądając folder *danych\_aplikacji* . Jeśli plik *wideo. mdf* nie jest widoczny, kliknij przycisk **Pokaż wszystkie pliki** na pasku narzędzi **Eksplorator rozwiązań** , kliknij przycisk **odśwież** , a następnie rozwiń folder *dane\_aplikacji* .

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Kliknij dwukrotnie ikonę *filmy. mdf* , aby otworzyć **Eksploratora serwera**, a następnie rozwiń folder **tabele** , aby wyświetlić tabelę filmy. Zanotuj ikonę klucza obok pozycji ID. Domyślnie EF ustawi właściwość o nazwie ID klucza podstawowego. Aby uzyskać więcej informacji na temat EF i MVC, zobacz Doskonały samouczek Dykstra na [MVC i EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz polecenie **Pokaż dane tabeli** , aby wyświetlić utworzone dane.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Kliknij prawym przyciskiem myszy tabelę `Movies` i wybierz pozycję **Otwórz definicję tabeli** , aby wyświetlić strukturę tabeli utworzoną przez Entity Framework Code First.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Zwróć uwagę, jak schemat tabeli `Movies` mapuje do utworzonej wcześniej klasy `Movie`. Entity Framework Code First automatycznie utworzył ten schemat na podstawie klasy `Movie`.

Po zakończeniu zamknij połączenie, klikając prawym przyciskiem myszy pozycję *MovieDBContext* i wybierając pozycję **Zamknij połączenie**. (Jeśli połączenie nie zostanie zamknięte, podczas następnego uruchomienia projektu może wystąpić błąd).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Masz teraz bazę danych i strony do wyświetlania, edytowania, aktualizowania i usuwania danych. W następnym samouczku sprawdzimy resztę kodu szkieletowego i dodamy metodę `SearchIndex` i widok `SearchIndex`, który umożliwi wyszukiwanie filmów w tej bazie danych. Aby uzyskać więcej informacji na temat używania Entity Framework z MVC, zobacz [tworzenie Entity Framework modelu danych dla aplikacji ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Poprzedni](creating-a-connection-string.md)
> [Następny](examining-the-edit-methods-and-edit-view.md)
