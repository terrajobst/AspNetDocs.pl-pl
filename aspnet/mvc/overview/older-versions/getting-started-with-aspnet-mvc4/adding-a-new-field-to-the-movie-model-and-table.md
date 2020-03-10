---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Dodawanie nowego pola do modelu i tabeli filmów | Microsoft Docs
author: Rick-Anderson
description: 'Uwaga: zaktualizowana wersja tego samouczka jest dostępna w tym miejscu, w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615088"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a>Dodawanie nowego pola do modelu Movie i tabeli

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.

W tej sekcji użyjesz migracje Code First platformy Entity Framework do migrowania niektórych zmian do klas modelu, aby zmiana została zastosowana do bazy danych.

Domyślnie w przypadku automatycznego tworzenia bazy danych za pomocą Code First Entity Framework, tak jak wcześniej w tym samouczku, Code First dodaje tabelę do bazy danych, aby ułatwić śledzenie, czy schemat bazy danych jest zsynchronizowany z klasami modelu, z których została wygenerowana. Jeśli nie są zsynchronizowane, Entity Framework zgłosi błąd. Ułatwia to Śledzenie problemów w czasie opracowywania, które można znaleźć w innym miejscu (poprzez zaciemnienie błędów) w czasie wykonywania.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Konfigurowanie Migracje Code First do zmiany modelu

Jeśli używasz programu Visual Studio 2012, dwukrotnie kliknij plik *film wideo. mdf* w Eksplorator rozwiązań, aby otworzyć narzędzie Database. Visual Studio Express dla sieci Web pokaże Eksplorator bazy danych, program Visual Studio 2012 będzie wyświetlał Eksplorator serwera. Jeśli używasz programu Visual Studio 2010, użyj Eksplorator obiektów SQL Server.

W narzędziu bazy danych (Eksplorator bazy danych, Eksplorator serwera lub Eksplorator obiektów SQL Server) kliknij prawym przyciskiem myszy pozycję `MovieDBContext` i wybierz pozycję Usuń, aby **usunąć** bazę danych filmów.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Przejdź wstecz do Eksplorator rozwiązań. Kliknij prawym przyciskiem myszy plik *wideo. mdf* i wybierz polecenie **Usuń** , aby usunąć bazę danych filmów.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Skompiluj aplikację, aby upewnić się, że nie występują żadne błędy.

W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **Konsola menedżera pakietów**.

![Dodaj pakiet Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

W oknie **konsola Menedżera pakietów** w wierszu `PM>` wprowadź wartość "Enable-migrations-ContextTypeName MvcMovie. models. MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

Polecenie **enable-migrations** (pokazane powyżej) tworzy plik *Configuration.cs* w nowym folderze *migracji* .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Program Visual Studio otwiera plik *Configuration.cs* . Zastąp metodę `Seed` w pliku *Configuration.cs* następującym kodem:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Kliknij prawym przyciskiem myszy czerwoną linię falistej w obszarze `Movie` i wybierz pozycję **Rozwiąż** **przy użyciu** **MvcMovie. models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Spowoduje to dodanie następującej instrukcji using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Migracje Code First wywołuje metodę `Seed` po każdej migracji (czyli wywołującej **aktualizację bazy danych** w konsoli Menedżera pakietów), a ta metoda aktualizuje wiersze, które zostały już wstawione lub wstawia je, jeśli jeszcze nie istnieją.

**Naciśnij kombinację klawiszy Ctrl-Shift-B, aby skompilować projekt.** (Poniższe kroki zakończą się niepowodzeniem, jeśli w tym momencie nie zostanie utworzona).

Następnym krokiem jest utworzenie klasy `DbMigration` dla migracji początkowej. Ta migracja umożliwia utworzenie nowej bazy danych, dlatego plik *film. mdf* został usunięty w poprzednim kroku.

W oknie **konsola Menedżera pakietów** wprowadź polecenie "Add-Migration Initial" w celu utworzenia początkowej migracji. Nazwa "początkowa" jest dowolna i jest używana do natworzenia nazwy utworzonego pliku migracji.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migracje Code First tworzy inny plik klasy w folderze *migracji* (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych. Nazwa pliku migracji jest wstępnie naprawiona sygnaturą czasową, aby pomóc w określeniu kolejności. Przeanalizuj plik *{DateStamp}\_Initial.cs* , zawierający instrukcje tworzenia tabeli filmów dla bazy danych filmu. Po zaktualizowaniu bazy danych w poniższych instrukcjach ten plik *{DateStamp}\_Initial.cs* zostanie uruchomiony i zostanie utworzony schemat bazy danych. Następnie zostanie uruchomiona Metoda **inicjatora** , aby wypełnić bazę danych z danymi testowymi.

W **konsoli Menedżera pakietów**wprowadź polecenie "Update-Database", aby utworzyć bazę danych i uruchomić metodę **inicjatora** .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Jeśli zostanie wyświetlony komunikat o błędzie informujący o tym, że tabela już istnieje i nie można jej utworzyć, prawdopodobnie aplikacja została uruchomiona po usunięciu bazy danych programu i przed wykonaniem `update-database`. W takim przypadku Usuń ponownie plik *film. mdf* i spróbuj ponownie wykonać `update-database` polecenie. Jeśli nadal wystąpi błąd, Usuń folder migracji i zawartość, a następnie zacznij od instrukcji znajdujących się w górnej części tej strony (spowoduje to usunięcie pliku *film. mdf* , a następnie kontynuuj włączenie migracji).

Uruchom aplikację i przejdź do adresu URL */Movies* . Dane inicjatora są wyświetlane.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości oceny do modelu filmu

Zacznij od dodania nowej właściwości `Rating` do istniejącej klasy `Movie`. Otwórz plik *Models\Movie.cs* i dodaj Właściwość `Rating` w następujący sposób:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Kompletna Klasa `Movie` teraz wygląda następująco:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Kompilowanie aplikacji za pomocą polecenia **kompiluj** &gt;**Kompiluj film** menu lub naciskając klawisze CTRL-SHIFT-B.

Teraz, po zaktualizowaniu klasy `Model` należy również zaktualizować szablony widoków *\Views\Movies\Index.cshtml* i *\Views\Movies\Create.cshtml* w celu wyświetlenia nowej właściwości `Rating` w widoku przeglądarki.

Otwórz plik<em>\Views\Movies\Index.cshtml</em> i Dodaj nagłówek kolumny `<th>Rating</th>` tuż po kolumnie <strong>Price</strong> . Następnie Dodaj `<td>` kolumnę blisko końca szablonu, aby renderować `@item.Rating` wartość. Poniżej znajduje się opis zaktualizowanego szablonu widoku <em>index. cshtml</em> :

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Następnie otwórz plik *\Views\Movies\Create.cshtml* i Dodaj następujący znacznik w górnej części formularza. Spowoduje to renderowanie pola tekstowego, aby można było określić klasyfikację po utworzeniu nowego filmu.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Kod aplikacji został już zaktualizowany, aby obsługiwał nową właściwość `Rating`.

Teraz uruchom aplikację i przejdź do adresu URL */Movies* . Gdy to zrobisz, zobaczysz jeden z następujących błędów:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Ten błąd jest wyświetlany, ponieważ zaktualizowana Klasa modelu `Movie` w aplikacji jest inna niż schemat tabeli `Movie` istniejącej bazy danych. (Brak kolumny `Rating` w tabeli bazy danych).

Istnieje kilka metod rozpoznawania błędu:

1. Entity Framework automatycznie porzucić i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu. To podejście jest bardzo wygodne podczas aktywnego programowania na testowej bazie danych. pozwala ona szybko rozwijać model i schemat bazy danych. Minusem, mimo że utracisz istniejące dane w bazie danych, więc *nie* chcesz używać tego podejścia w produkcyjnej bazie danych. Użycie inicjatora do automatycznego umieszczania bazy danych z danymi testowymi jest często wydajnym sposobem na tworzenie aplikacji. Aby uzyskać więcej informacji o Entity Framework inicjatorach baz danych, zobacz [ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)Dykstra.
2. Jawnie zmodyfikuj schemat istniejącej bazy danych, tak aby pasował do klas modelu. Zaletą tego podejścia jest utrzymywanie danych. Tę zmianę można wprowadzić ręcznie lub przez utworzenie skryptu zmiany bazy danych.
3. Użyj Migracje Code First, aby zaktualizować schemat bazy danych.

W tym samouczku będziemy używać Migracje Code First.

Zaktualizuj metodę inicjatora, aby zapewnić wartość nowej kolumny. Otwórz plik Migrations\Configuration.cs i Dodaj pole Rating do każdego obiektu filmu.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Skompiluj rozwiązanie, a następnie otwórz okno **konsoli Menedżera pakietów** i wprowadź następujące polecenie:

`add-migration AddRatingMig`

Polecenie `add-migration` informuje platformę migracji, aby przeanalizować bieżący model filmu z bieżącym schematem bazy danych filmu i utworzyć wymagany kod w celu przeprowadzenia migracji bazy danych do nowego modelu. AddRatingMig jest dowolny i jest używany do nazwy pliku migracji. Warto użyć zrozumiałej nazwy dla kroku migracji.

Po zakończeniu tego polecenia program Visual Studio otwiera plik klasy, który definiuje nową klasę pochodną `DbMigration`, a w metodzie `Up` można zobaczyć kod, który tworzy nową kolumnę.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Skompiluj rozwiązanie, a następnie wprowadź polecenie "Update-Database" w oknie **konsola Menedżera pakietów** .

Na poniższej ilustracji przedstawiono dane wyjściowe w oknie **konsola Menedżera pakietów** (Sygnatura daty oczekująca na AddRatingMig będzie inna).

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Uruchom aplikację jeszcze raz i przejdź do adresu URL/Movies. Możesz zobaczyć nowe pole oceny.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Kliknij link **Utwórz nowy** , aby dodać nowy film. Należy pamiętać, że można dodać klasyfikację.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Kliknij przycisk **Utwórz**. Nowy film, łącznie z klasyfikacją, znajduje się teraz na liście filmów:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Należy również dodać pole `Rating` do szablonów widoku Edytuj, szczegóły i SearchIndex.

W oknie **konsoli Menedżera pakietów** można ponownie wprowadzić polecenie "Update-Database" i nie zostaną wprowadzone żadne zmiany, ponieważ schemat jest zgodny z modelem.

W tej sekcji pokazano, jak można modyfikować obiekty modelu i zachować synchronizację bazy danych ze zmianami. Poznasz również sposób wypełniania nowo utworzonej bazy danych z przykładowymi danymi, dzięki czemu można wypróbować scenariusze. Następnie Przyjrzyjmy się sposobom dodawania bogatszej logiki walidacji do klas modelu i włączania niektórych reguł firmowych.

> [!div class="step-by-step"]
> [Poprzednie](examining-the-edit-methods-and-edit-view.md)
> [dalej](adding-validation-to-the-model.md)
