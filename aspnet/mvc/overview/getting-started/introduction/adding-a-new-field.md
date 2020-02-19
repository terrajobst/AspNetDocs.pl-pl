---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Dodawanie nowego pola | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5974e53e4610dccc7812df261dc97a9b0327de85
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456689"
---
# <a name="adding-a-new-field"></a>Dodanie nowego pola

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

W tej sekcji użyjesz migracje Code First platformy Entity Framework do migrowania niektórych zmian do klas modelu, aby zmiana została zastosowana do bazy danych.

Domyślnie w przypadku automatycznego tworzenia bazy danych za pomocą Code First Entity Framework, tak jak wcześniej w tym samouczku, Code First dodaje tabelę do bazy danych, aby ułatwić śledzenie, czy schemat bazy danych jest zsynchronizowany z klasami modelu, z których została wygenerowana. Jeśli nie są zsynchronizowane, Entity Framework zgłosi błąd. Ułatwia to Śledzenie problemów w czasie opracowywania, które można znaleźć w innym miejscu (poprzez zaciemnienie błędów) w czasie wykonywania.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Konfigurowanie Migracje Code First do zmiany modelu

Przejdź do Eksplorator rozwiązań. Kliknij prawym przyciskiem myszy plik *wideo. mdf* i wybierz polecenie **Usuń** , aby usunąć bazę danych filmów. Jeśli nie widzisz pliku *wideo. mdf* , kliknij ikonę **Pokaż wszystkie pliki** poniżej w czerwonym konspekcie.

![](adding-a-new-field/_static/image1.png)

Skompiluj aplikację, aby upewnić się, że nie występują żadne błędy.

W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **Konsola menedżera pakietów**.

![Dodaj pakiet Man](adding-a-new-field/_static/image2.png)

W oknie **konsola Menedżera pakietów** w wierszu `PM>` wprowadź

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

Polecenie **enable-migrations** (pokazane powyżej) tworzy plik *Configuration.cs* w nowym folderze *migracji* .

![](adding-a-new-field/_static/image4.png)

Program Visual Studio otwiera plik *Configuration.cs* . Zastąp metodę `Seed` w pliku *Configuration.cs* następującym kodem:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Umieść kursor na czerwono linii falistej w obszarze `Movie` i kliknij pozycję `Show Potential Fixes`, a następnie kliknij pozycję **using** **MvcMovie. models;**

![](adding-a-new-field/_static/image5.png)

Spowoduje to dodanie następującej instrukcji using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Migracje Code First wywołuje metodę `Seed` po każdej migracji (czyli wywołującej **aktualizację bazy danych** w konsoli Menedżera pakietów), a ta metoda aktualizuje wiersze, które zostały już wstawione lub wstawia je, jeśli jeszcze nie istnieją.
> 
> Metoda [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) w poniższym kodzie wykonuje operację "upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Ponieważ metoda [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) jest uruchamiana przy każdej migracji, nie można tylko wstawić danych, ponieważ wiersze, które próbujesz dodać, będą już znajdować się po pierwszej migracji, która tworzy bazę danych. Operacja "[upsert](http://en.wikipedia.org/wiki/Upsert)" zapobiega błędom, które mogą wystąpić w przypadku próby wstawienia wiersza, który już istnieje, ale zastępuje wszelkie zmiany danych, które mogły zostać wprowadzone podczas testowania aplikacji. Dane testowe w niektórych tabelach mogą nie być potrzebne: w niektórych przypadkach, gdy zmieniasz dane podczas testowania, jeśli chcesz, aby zmiany były nadal wykonywane po aktualizacji bazy danych. W takim przypadku należy wykonać operację warunkowego wstawiania: Wstaw wiersz tylko wtedy, gdy jeszcze nie istnieje.   
> 
> Pierwszy parametr przesłany do metody [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) określa właściwość, która ma zostać użyta do sprawdzenia, czy wiersz już istnieje. Dla danych filmu testowego, które zapewniasz, właściwość `Title` może zostać użyta do tego celu, ponieważ każdy tytuł na liście jest unikatowy:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Ten kod zakłada, że tytuły są unikatowe. Jeśli ręcznie dodasz zduplikowany tytuł, podczas następnego przeprowadzenia migracji wystąpi Poniższy wyjątek.   
> 
> *Sekwencja zawiera więcej niż jeden element*  
> 
> Aby uzyskać więcej informacji na temat metody [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) , zobacz temat zapoznaj [się z metodą dr 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).

**Naciśnij kombinację klawiszy Ctrl-Shift-B, aby skompilować projekt.** (Poniższe kroki zakończą się niepowodzeniem, jeśli w tym momencie nie zostanie utworzona).

Następnym krokiem jest utworzenie klasy `DbMigration` dla migracji początkowej. Ta migracja tworzy nową bazę danych, dlatego plik *film. mdf* został usunięty w poprzednim kroku.

W oknie **konsola Menedżera pakietów** wprowadź polecenie `add-migration Initial`, aby utworzyć początkową migrację. Nazwa "początkowa" jest dowolna i jest używana do natworzenia nazwy utworzonego pliku migracji.

![](adding-a-new-field/_static/image6.png)

Migracje Code First tworzy inny plik klasy w folderze *migracji* (o nazwie *{DateStamp}\_Initial.cs* ), a ta klasa zawiera kod, który tworzy schemat bazy danych. Nazwa pliku migracji jest wstępnie naprawiona sygnaturą czasową, aby pomóc w określeniu kolejności. Przeanalizuj plik *{DateStamp}\_Initial.cs* , zawierający instrukcje tworzenia tabeli `Movies` dla bazy danych filmu. Po zaktualizowaniu bazy danych w poniższych instrukcjach ten plik *{DateStamp}\_Initial.cs* zostanie uruchomiony i zostanie utworzony schemat bazy danych. Następnie zostanie uruchomiona Metoda **inicjatora** , aby wypełnić bazę danych z danymi testowymi.

W **konsoli Menedżera pakietów**wprowadź polecenie `update-database`, aby utworzyć bazę danych i uruchomić metodę `Seed`.

![](adding-a-new-field/_static/image7.png)

Jeśli zostanie wyświetlony komunikat o błędzie informujący o tym, że tabela już istnieje i nie można jej utworzyć, prawdopodobnie aplikacja została uruchomiona po usunięciu bazy danych programu i przed wykonaniem `update-database`. W takim przypadku Usuń ponownie plik *film. mdf* i spróbuj ponownie wykonać `update-database` polecenie. Jeśli nadal wystąpi błąd, Usuń folder migracji i zawartość, a następnie zacznij od instrukcji znajdujących się w górnej części tej strony (spowoduje to usunięcie pliku *film. mdf* , a następnie kontynuuj włączenie migracji). Jeśli nadal pojawia się błąd, Otwórz Eksplorator obiektów SQL Server i Usuń bazę danych z listy.

Uruchom aplikację i przejdź do adresu URL */Movies* . Dane inicjatora są wyświetlane.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości oceny do modelu filmu

Zacznij od dodania nowej właściwości `Rating` do istniejącej klasy `Movie`. Otwórz plik *Models\Movie.cs* i dodaj Właściwość `Rating` w następujący sposób:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Kompletna Klasa `Movie` teraz wygląda następująco:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Kompiluj aplikację (Ctrl + Shift + B).

Ponieważ dodano nowe pole do klasy `Movie`, należy również zaktualizować *białe listy* powiązań, aby ta nowa właściwość została uwzględniona. Zaktualizuj `bind` atrybutu dla `Create` i `Edit` metod akcji, aby uwzględnić Właściwość `Rating`:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Należy również zaktualizować szablony widoków, aby wyświetlić, utworzyć i edytować nową właściwość `Rating` w widoku przeglądarki.

Otwórz plik *\Views\Movies\Index.cshtml* i Dodaj nagłówek kolumny `<th>Rating</th>` tuż po kolumnie **Price** . Następnie Dodaj `<td>` kolumnę blisko końca szablonu, aby renderować `@item.Rating` wartość. Poniżej znajduje się opis zaktualizowanego szablonu widoku *index. cshtml* :

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Następnie otwórz plik *\Views\Movies\Create.cshtml* i dodaj pole `Rating` z następującym wyróżnionym znacznikiem. Spowoduje to renderowanie pola tekstowego, aby można było określić klasyfikację po utworzeniu nowego filmu.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Kod aplikacji został już zaktualizowany, aby obsługiwał nową właściwość `Rating`.

Uruchom aplikację i przejdź do adresu URL */Movies* . Gdy to zrobisz, zobaczysz jeden z następujących błędów:

![](adding-a-new-field/_static/image9.png)  
  
Model z kopią zapasową kontekstu "MovieDBContext" został zmieniony od czasu utworzenia bazy danych. Rozważ użycie Migracje Code First do zaktualizowania bazy danych (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Ten błąd jest wyświetlany, ponieważ zaktualizowana Klasa modelu `Movie` w aplikacji jest inna niż schemat tabeli `Movie` istniejącej bazy danych. (Brak kolumny `Rating` w tabeli bazy danych).

Istnieje kilka metod rozpoznawania błędu:

1. Entity Framework automatycznie porzucić i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu. To podejście jest bardzo wygodne w cyklu programowania, gdy przeprowadzasz aktywne Programowanie na testowej bazie danych. pozwala ona szybko rozwijać model i schemat bazy danych. Minusem, mimo że utracisz istniejące dane w bazie danych, więc *nie* chcesz używać tego podejścia w produkcyjnej bazie danych. Użycie inicjatora do automatycznego umieszczania bazy danych z danymi testowymi jest często wydajnym sposobem na tworzenie aplikacji. Aby uzyskać więcej informacji na temat Entity Framework inicjatorów bazy danych, zobacz [samouczek ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Jawnie zmodyfikuj schemat istniejącej bazy danych, tak aby pasował do klas modelu. Zaletą tego podejścia jest utrzymywanie danych. Tę zmianę można wprowadzić ręcznie lub przez utworzenie skryptu zmiany bazy danych.
3. Użyj Migracje Code First, aby zaktualizować schemat bazy danych.

W tym samouczku będziemy używać Migracje Code First.

Zaktualizuj metodę inicjatora, aby zapewnić wartość nowej kolumny. Otwórz plik Migrations\Configuration.cs i Dodaj pole Rating do każdego obiektu filmu.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Skompiluj rozwiązanie, a następnie otwórz okno **konsoli Menedżera pakietów** i wprowadź następujące polecenie:

`add-migration Rating`

Polecenie `add-migration` informuje platformę migracji, aby przeanalizować bieżący model filmu z bieżącym schematem bazy danych filmu i utworzyć wymagany kod w celu przeprowadzenia migracji bazy danych do nowego modelu. *Klasyfikacja* nazw jest dowolna i jest używana do nazwy pliku migracji. Warto użyć zrozumiałej nazwy dla kroku migracji.

Po zakończeniu tego polecenia program Visual Studio otwiera plik klasy, który definiuje nową klasę pochodną `DbMigration`, a w metodzie `Up` można zobaczyć kod, który tworzy nową kolumnę.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Skompiluj rozwiązanie, a następnie wprowadź `update-database` polecenie w oknie **konsola Menedżera pakietów** .

Na poniższej ilustracji przedstawiono dane wyjściowe w oknie **konsola Menedżera pakietów** ( *Klasyfikacja* sygnatury czasowej w toku będzie inna).

![](adding-a-new-field/_static/image11.png)

Uruchom aplikację jeszcze raz i przejdź do adresu URL/Movies. Możesz zobaczyć nowe pole oceny.

![](adding-a-new-field/_static/image12.png)

Kliknij link **Utwórz nowy** , aby dodać nowy film. Należy pamiętać, że można dodać klasyfikację.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Kliknij przycisk **Utwórz**. Nowy film, łącznie z klasyfikacją, znajduje się teraz na liście filmów:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Teraz, gdy projekt korzysta z migracji, nie trzeba porzucić bazy danych podczas dodawania nowego pola lub w inny sposób aktualizacji schematu. W następnej sekcji wprowadzimy więcej zmian schematu i użyjesz migracji w celu zaktualizowania bazy danych.

Należy również dodać pole `Rating` do szablonów Edytuj, szczegóły i Usuń.

W oknie **konsoli Menedżera pakietów** można ponownie wprowadzić polecenie "Update-Database" i nie będzie można uruchomić kodu migracji, ponieważ schemat jest zgodny z modelem. Jednak uruchomienie polecenie "Update-Database" spowoduje ponowne uruchomienie `Seed`ej metody, a w przypadku zmiany dowolnego z danych inicjatora zmiany zostaną utracone, ponieważ metoda `Seed` upserts dane. Więcej informacji na temat metody `Seed` można znaleźć w popularnym [samouczku ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)Dykstra.

W tej sekcji pokazano, jak można modyfikować obiekty modelu i zachować synchronizację bazy danych ze zmianami. Poznasz również sposób wypełniania nowo utworzonej bazy danych z przykładowymi danymi, dzięki czemu można wypróbować scenariusze. To właśnie krótkie wprowadzenie do Code First, zobacz [tworzenie Entity Framework modelu danych dla aplikacji ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) w celu uzyskania pełniejszego samouczka dotyczącego tematu. Następnie Przyjrzyjmy się sposobom dodawania bogatszej logiki walidacji do klas modelu i włączania niektórych reguł firmowych.

> [!div class="step-by-step"]
> [Poprzednie](adding-search.md)
> [dalej](adding-validation.md)
