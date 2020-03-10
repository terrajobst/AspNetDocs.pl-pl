---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 2 | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework. Przykładowa aplikacja to...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: bd6a2e29e6f0df04e39be29160e2e08cc99c4706
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566011"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 2

Autor [Dykstra](https://github.com/tdykstra)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework 4,0 i programu Visual Studio 2010. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="the-entitydatasource-control"></a>Formant EntityDataSource

W poprzednim samouczku została utworzona witryna sieci Web, baza danych i model danych. W tym samouczku poznasz formant `EntityDataSource`, który ASP.NET zapewnia, aby ułatwić współpracę z Entity Framework modelem danych. Utworzysz kontrolkę `GridView` do wyświetlania i edytowania danych uczniów, kontrolki `DetailsView` do dodawania nowych uczniów oraz `DropDownList` kontroli wybierania działu (którego będziesz używać później do wyświetlania skojarzonych kursów).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Zwróć uwagę, że w tej aplikacji nie będziesz dodawać walidacji danych wejściowych do stron, które aktualizują bazę danych, a niektóre z nich nie będą tak niezawodne, jak byłyby wymagane w aplikacji produkcyjnej. Dzięki temu samouczek jest ukierunkowany na Entity Framework i utrzymuje zbyt długi czas. Aby uzyskać szczegółowe informacje na temat sposobu dodawania tych funkcji do aplikacji, zobacz [Weryfikowanie danych wejściowych użytkownika na stronach sieci Web ASP.NET](https://msdn.microsoft.com/library/7kh55542.aspx) i [obsługa błędów w ASP.NET stronach i aplikacjach](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Dodawanie i Konfigurowanie kontrolki EntityDataSource

Zaczniesz od skonfigurowania formantu `EntityDataSource`, aby odczytywać jednostki `Person` z zestawu jednostek `People`.

Upewnij się, że masz otwarty program Visual Studio i że pracujesz nad projektem utworzonym w części 1. Jeśli projekt nie został skompilowany od czasu utworzenia modelu danych lub od momentu ostatniej zmiany w nim, Skompiluj projekt teraz. Zmiany w modelu danych nie są udostępniane projektantowi do czasu skompilowania projektu.

Utwórz nową stronę sieci Web przy użyciu **formularza sieci Web przy użyciu szablonu strony głównej** , a następnie nadaj mu nazwę *Students. aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Określ wartość *site. Master* jako stronę wzorcową. Na wszystkich stronach utworzonych dla tych samouczków zostanie użyta ta strona wzorcowa.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

W widoku **źródła** dodaj nagłówek `h2` do kontrolki `Content` o nazwie `Content2`, jak pokazano w następującym przykładzie:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Na karcie **dane** w **przyborniku**przeciągnij kontrolkę `EntityDataSource` na stronę, upuść ją pod nagłówkiem i zmień identyfikator, aby `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Przełącz się do widoku **projektu** , kliknij tag inteligentny kontroli źródła danych, a następnie kliknij przycisk **Konfiguruj źródło danych** , aby uruchomić kreatora **konfiguracji źródła danych** .

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

W kroku **Konfiguruj** pracę kreatora, wybierz opcję **SchoolEntities** jako wartość **nazwanego połączenia**i wybierz pozycję **SchoolEntities** jako wartość **DefaultContainerName** . Następnie kliknij przycisk **Next** (Dalej).

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Uwaga: Jeśli w tym momencie zostanie wyświetlone następujące okno dialogowe, należy skompilować projekt przed kontynuowaniem.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

W kroku **Konfigurowanie wyboru danych** wybierz **osoby** jako wartość dla **obiektu entitySetName**. W obszarze **Wybierz**upewnij się, że zaznaczone jest pole wyboru **zaznacz** wszystko. Następnie wybierz opcje, aby włączyć aktualizowanie i usuwanie. Gdy skończysz, kliknij przycisk **Zakończ**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Konfigurowanie reguł bazy danych w celu umożliwienia usuwania

Zostanie utworzona strona umożliwiająca użytkownikom usuwanie uczniów z tabeli `Person`, która ma trzy relacje z innymi tabelami (`Course`, `StudentGrade`i `OfficeAssignment`). Domyślnie baza danych uniemożliwi usunięcie wiersza w `Person`, jeśli istnieją powiązane wiersze w jednej z innych tabel. W pierwszej kolejności można ręcznie usunąć powiązane wiersze lub skonfigurować bazę danych, aby usunąć ją automatycznie po usunięciu `Person` wiersza. W przypadku rekordów uczniów w tym samouczku należy skonfigurować bazę danych do automatycznego usuwania powiązanych danych. Ponieważ uczniowie mogą mieć powiązane wiersze tylko w tabeli `StudentGrade`, należy skonfigurować tylko jedną z trzech relacji.

Jeśli używasz pliku *uczelni. mdf* pobranego z projektu, który zawiera ten samouczek, możesz pominąć tę sekcję, ponieważ te zmiany konfiguracji zostały już wykonane. Jeśli baza danych została utworzona przez uruchomienie skryptu, skonfiguruj bazę danych, wykonując następujące procedury.

W **Eksplorator serwera**Otwórz diagram bazy danych utworzony w części 1. Kliknij prawym przyciskiem myszy relację między `Person` i `StudentGrade` (wiersz między tabelami), a następnie wybierz polecenie **Właściwości**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

W oknie **Właściwości** rozwiń węzeł **Wstaw i Specyfikacja aktualizacji** i ustaw właściwość **DeleteRule** na **kaskadowo**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Zapisz i Zamknij diagram. Jeśli zostanie wyświetlony monit o zaktualizowanie bazy danych, kliknij przycisk **tak**.

Aby upewnić się, że model zachowuje jednostki znajdujące się w pamięci w synchronizacji z działaniem bazy danych, należy ustawić odpowiednie reguły w modelu danych. Otwórz *SchoolModel. edmx*, kliknij prawym przyciskiem myszy linię skojarzenia między `Person` i `StudentGrade`, a następnie wybierz polecenie **Właściwości**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

W oknie **Właściwości** Ustaw **elementu end1 onDelete** na **kaskadowo**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Zapisz i zamknij plik *SchoolModel. edmx* , a następnie Skompiluj ponownie projekt.

Ogólnie rzecz biorąc, gdy baza danych ulegnie zmianie, istnieje kilka opcji synchronizacji modelu:

- W przypadku niektórych rodzajów zmian (takich jak dodawanie lub odświeżanie tabel, widoków lub procedur składowanych) kliknij prawym przyciskiem myszy w Projektancie i wybierz polecenie **Aktualizuj model z bazy danych** , aby Projektant automatycznie wprowadza zmiany.
- Wygeneruj ponownie model danych.
- Wprowadź ręczne aktualizacje takie jak ten.

W takim przypadku można ponownie wygenerować model lub odświeżyć tabele, których dotyczy zmiana relacji, ale należy wprowadzić zmianę nazwy pola (z `FirstName` na `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Odczytywanie i aktualizowanie jednostek przy użyciu kontrolki GridView

W tej sekcji użyjesz kontrolki `GridView` do wyświetlania, aktualizowania lub usuwania uczniów.

Otwórz lub przejdź do *uczniów. aspx* i przejdź do widoku **projektu** . Na karcie **dane** w **przyborniku**przeciągnij kontrolkę `GridView` z prawej strony kontrolki `EntityDataSource`, nadaj jej nazwę `StudentsGridView`, kliknij tag inteligentny, a następnie wybierz pozycję **StudentsEntityDataSource** jako źródło danych.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Kliknij przycisk **Odśwież schemat** (kliknij przycisk **tak** , jeśli zostanie wyświetlony monit o potwierdzenie), a następnie kliknij pozycję **Włącz stronicowanie**, **Włącz sortowanie**, **Włącz edytowanie**i **Włącz usuwanie**.

Kliknij przycisk **Edytuj kolumny**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

W polu **wybrane pola** Usuń **PersonID**, **LastName**i **HireDate**. Zazwyczaj nie jest wyświetlany klucz rekordu dla użytkowników, data zatrudnienia nie ma znaczenia dla uczniów, a obie części nazwy są umieszczane w jednym polu, więc wystarczy jedno z nazw pól.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Wybierz pole **FirstMidName** , a następnie kliknij przycisk **Konwertuj to pole na TemplateField**.

Wykonaj te same czynności dla **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Kliknij przycisk **OK** , a następnie przejdź do widoku **źródła** . Pozostałe zmiany będą łatwiejsze do wykonania bezpośrednio w znacznikach. `GridView` znaczników kontrolki wygląda teraz podobnie jak w poniższym przykładzie.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

Pierwsza kolumna po polu polecenia to pole szablonu, które wyświetla imię i nazwisko. Zmień znaczniki pola tego szablonu, aby wyglądać jak w poniższym przykładzie:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

W trybie wyświetlania dwie `Label` kontrolki wyświetlają imię i nazwisko. W trybie edycji są podane dwa pola tekstowe, aby można było zmienić imię i nazwisko. Podobnie jak w przypadku kontrolek `Label` w trybie wyświetlania, wyrażenia `Bind` i `Eval` są dokładnie takie same jak w przypadku kontrolek źródła danych ASP.NET, które łączą się bezpośrednio z bazami danych. Jedyną różnicą jest to, że należy określić właściwości jednostki zamiast kolumn bazy danych.

Ostatnia kolumna to pole szablonu, które wyświetla datę rejestracji. Zmień znaczniki dla tego pola, aby wyglądać jak w poniższym przykładzie:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

W trybie wyświetlania i edycji ciąg formatu "{0, d}" powoduje wyświetlenie daty w formacie "Data krótka". (Komputer może być skonfigurowany do wyświetlania tego formatu inaczej niż obrazy ekranu wyświetlane w tym samouczku).

Zwróć uwagę, że w każdym z tych pól szablonu Projektant użył wyrażenia `Bind` domyślnie, ale został zmieniony na wyrażenie `Eval` w elementach `ItemTemplate`. Wyrażenie `Bind` sprawia, że dane są dostępne we właściwościach kontrolki `GridView` na wypadek, gdyby trzeba było uzyskać dostęp do danych w kodzie. Na tej stronie nie musisz uzyskiwać dostępu do tych danych w kodzie, więc możesz używać `Eval`, co jest bardziej wydajne. Aby uzyskać więcej informacji, zobacz [pobieranie danych z kontrolek danych](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Poprawianie znaczników kontrolki EntityDataSource w celu zwiększenia wydajności

W znaczniku dla kontrolki `EntityDataSource` usuń atrybuty `ConnectionString` i `DefaultContainerName` i zastąp je atrybutem `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Ta zmiana powinna być podejmowana za każdym razem, gdy tworzysz kontrolkę `EntityDataSource`, chyba że trzeba użyć połączenia innego niż to, które jest stałe w klasie kontekstu obiektu. Użycie atrybutu `ContextTypeName` zapewnia następujące korzyści:

- Lepsza wydajność. Gdy formant `EntityDataSource` inicjuje model danych przy użyciu atrybutów `ConnectionString` i `DefaultContainerName`, wykonuje dodatkowe prace w celu załadowania metadanych dla każdego żądania. Nie jest to konieczne, jeśli określisz atrybut `ContextTypeName`.
- Ładowanie z opóźnieniem jest domyślnie włączone w wygenerowanych klasach kontekstu obiektów (takich jak `SchoolEntities` w tym samouczku) w Entity Framework 4,0. Oznacza to, że właściwości nawigacji są ładowane z danymi powiązanymi automatycznie, gdy ich potrzebujesz. Ładowanie z opóźnieniem zostało wyjaśnione bardziej szczegółowo w dalszej części tego samouczka.
- Wszystkie dostosowania, które zostały zastosowane do klasy kontekstu obiektu (w tym przypadku Klasa `SchoolEntities`) będzie dostępna dla formantów, które używają kontrolki `EntityDataSource`. Dostosowywanie klasy kontekstu obiektów jest zaawansowanym tematem, który nie jest objęty tą serią samouczków. Aby uzyskać więcej informacji, zobacz [rozszerzanie wygenerowanych typów Entity Framework](https://msdn.microsoft.com/library/dd456844.aspx).

Znacznik będzie teraz podobny do poniższego przykładu (kolejność właściwości może się różnić):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

Atrybut `EnableFlattening` odwołuje się do funkcji, która była wymagana we wcześniejszych wersjach Entity Framework ponieważ kolumny klucza obcego nie zostały ujawnione jako właściwości jednostki. Bieżąca wersja umożliwia korzystanie z *skojarzeń kluczy obcych*, co oznacza, że właściwości klucza obcego są ujawniane dla wszystkich skojarzeń z wyjątkiem wielu-do-wielu. Jeśli jednostki mają właściwości klucza obcego i nie mają [typów złożonych](https://msdn.microsoft.com/library/bb738472.aspx), można pozostawić ten atrybut ustawiony na `False`. Nie usuwaj atrybutu z znaczników, ponieważ wartość domyślna to `True`. Aby uzyskać więcej informacji, zobacz [spłaszczanie obiektów (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Uruchom stronę i zobaczysz listę studentów i pracowników (filtr dotyczy tylko studentów w następnym samouczku). Imię i nazwisko są wyświetlane razem.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Aby posortować ekran, kliknij nazwę kolumny.

Kliknij przycisk **Edytuj** w dowolnym wierszu. Wyświetlane są pola tekstowe, w których można zmienić imię i nazwisko.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

Przycisk **Usuń** również działa. Kliknij przycisk Usuń dla wiersza, który ma datę rejestracji i znika wiersz. (Wiersze bez daty rejestracji przedstawiają instruktorów i może zostać wyświetlony błąd integralności referencyjnej. W następnym samouczku przefiltruje tę listę w celu uwzględnienia tylko studentów.)

## <a name="displaying-data-from-a-navigation-property"></a>Wyświetlanie danych z właściwości nawigacji

Teraz Załóżmy, że chcesz wiedzieć, ile kursów każdy student został zarejestrowany w usłudze. Entity Framework udostępnia te informacje we właściwości nawigacji `StudentGrades` jednostki `Person`. Ponieważ projekt bazy danych nie zezwala na zarejestrowanie studenta w kursie bez przypisanej klasy, można założyć, że posiadanie wiersza w wierszu tabeli `StudentGrade`, który jest skojarzony z kursem, jest takie samo jak w przypadku zarejestrowania w kursie. (Właściwość nawigacji `Courses` jest tylko dla instruktorów).

Gdy używasz atrybutu `ContextTypeName` formantu `EntityDataSource`, Entity Framework automatycznie pobiera informacje dla właściwości nawigacji podczas uzyskiwania dostępu do tej właściwości. Jest to nazywane *ładowaniem z opóźnieniem*. Jednak może to być niewydajne, ponieważ powoduje osobne wywołanie do bazy danych za każdym razem, gdy są niezbędne dodatkowe informacje. Jeśli potrzebujesz danych z właściwości nawigacji dla każdej jednostki zwracanej przez formant `EntityDataSource`, bardziej wydajne jest pobranie powiązanych danych wraz z samą jednostką w jednym wywołaniu bazy danych. Jest to tzw. *ładowanie eager*i określanie eager ładowania dla właściwości nawigacji przez ustawienie właściwości `Include` kontrolki `EntityDataSource`.

W *uczniach. aspx*chcesz wyświetlić liczbę kursów dla każdego ucznia, więc najlepszym wyborem jest ładowanie eager. Jeśli zostały wyświetlone wszystkie uczniów, ale podano liczbę kursów tylko dla kilku z nich (co wymagałoby zapisania kodu jako uzupełnienie znacznika), ładowanie z opóźnieniem może być lepszym wyborem.

Otwórz lub przejdź do opcji *uczniowie. aspx*, przejdź do widoku **projektu** , wybierz pozycję `StudentsEntityDataSource`, a w oknie **Właściwości** ustaw właściwość **include** na **StudentGrades**. (Jeśli chcesz uzyskać wiele właściwości nawigacji, możesz określić ich nazwy oddzielone przecinkami — na przykład **StudentGrades, kursy**.).

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Przejdź do widoku **źródła** . W kontrolce `StudentsGridView` po ostatnim elemencie `asp:TemplateField` Dodaj następujące pole nowego szablonu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

W wyrażeniu `Eval` można odwołać się do `StudentGrades`właściwości nawigacji. Ponieważ ta właściwość zawiera kolekcję, ma właściwość `Count`, która służy do wyświetlania liczby kursów, w których student jest zarejestrowany. W późniejszym samouczku zobaczysz, jak wyświetlać dane z właściwości nawigacji, które zawierają pojedyncze jednostki zamiast kolekcji. (Należy zauważyć, że nie można użyć elementów `BoundField` do wyświetlania danych z właściwości nawigacji).

Uruchom stronę i zobaczysz, ile kursów jest zarejestrowanych dla każdego ucznia.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Wstawianie jednostek przy użyciu kontrolki DetailsView

Następnym krokiem jest utworzenie strony z kontrolką `DetailsView`, która pozwoli dodawać nowych uczniów. Zamknij przeglądarkę, a następnie utwórz nową stronę sieci Web przy użyciu strony głównej programu *site. Master* . Nadaj stronie nazwę *StudentsAdd. aspx*, a następnie przejdź do widoku **źródła** .

Dodaj następujące znaczniki, aby zamienić istniejące znaczniki dla kontrolki `Content` o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Ten znacznik tworzy formant `EntityDataSource`, który jest podobny do tego, który został utworzony w *uczniów. aspx*, z tą różnicą, że umożliwia wstawianie. Podobnie jak w przypadku kontrolki `GridView`, pola powiązane kontrolki `DetailsView` są kodowane dokładnie tak, jak w przypadku formantu danych, który łączy się bezpośrednio z bazą danych, z tą różnicą, że odwołują się do właściwości jednostki. W takim przypadku kontrolka `DetailsView` jest używana tylko do wstawiania wierszy, więc ustawiono domyślny tryb `Insert`.

Uruchom stronę i Dodaj nowego ucznia.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Nic się nie stanie po wstawieniu nowego ucznia, ale jeśli teraz uruchomisz *uczniów. aspx*, zobaczysz nowe informacje ucznia.

## <a name="displaying-data-in-a-drop-down-list"></a>Wyświetlanie danych na liście rozwijanej

W poniższych krokach zostanie podjęta próba powiązania formantu `DropDownList` z zestawem jednostek przy użyciu kontrolki `EntityDataSource`. W tej części samouczka nie będzie można wykonać wielu czynności z tą listą. W kolejnych częściach należy użyć listy, aby umożliwić użytkownikom wybranie działu w celu wyświetlenia kursów skojarzonych z działem.

Utwórz nową stronę sieci Web o nazwie *kursy. aspx*. W widoku **źródła** Dodaj nagłówek do kontrolki `Content` o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

W widoku **projektu** dodaj formant `EntityDataSource` do strony tak jak wcześniej, z wyjątkiem tego, że ta nazwa `DepartmentsEntityDataSource`a. Wybierz pozycję **działy** jako wartość **entitySetName** i wybierz tylko właściwości **DepartmentID** i **name** .

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Na karcie **standardowe** **przybornika**przeciągnij kontrolkę `DropDownList` na stronę, nadaj jej nazwę `DepartmentsDropDownList`, kliknij tag inteligentny i wybierz pozycję **Wybierz źródło danych** , aby uruchomić **Kreatora konfiguracji źródła**danych.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

W kroku **Wybierz źródło danych** wybierz pozycję **DepartmentsEntityDataSource** jako źródło danych, kliknij przycisk **Odśwież schemat**, a następnie wybierz opcję **Nazwa** jako pole danych do wyświetlenia i **DepartmentID** jako pole danych wartości. Kliknij przycisk **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Metoda używana do powiązania formantu przy użyciu Entity Framework jest taka sama jak w przypadku innych kontrolek źródła danych ASP.NET, z wyjątkiem określania jednostek i właściwości jednostek.

Przełącz się do widoku **źródła** i Dodaj "Wybierz dział:" bezpośrednio przed formantem `DropDownList`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

W tym momencie Zmień znaczniki dla kontrolki `EntityDataSource`, zastępując atrybuty `ConnectionString` i `DefaultContainerName` atrybutem `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Często najlepiej czekać, aż po utworzeniu kontrolki powiązanej z danymi, która jest połączona z kontrolką źródła danych, przed zmianą `EntityDataSource` znaczników kontrolki, ponieważ po wprowadzeniu zmiany projektant nie będzie udostępniać opcji **Odśwież schemat** w kontrolce powiązanej z danymi.

Uruchom stronę i wybierz dział z listy rozwijanej.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

To kończy wprowadzenie do korzystania z formantu `EntityDataSource`. Praca z tą kontrolką zwykle nie różni się od pracy z innymi ASP.NETmi formantami źródła danych, z tą różnicą, że obiekty i właściwości są odwołujące się do tabel i kolumn. Jedyny wyjątek polega na tym, że chcesz uzyskać dostęp do właściwości nawigacji. W następnym samouczku zobaczysz, że składnia używana z kontrolką `EntityDataSource` może również różnić się od innych kontrolek źródła danych podczas filtrowania, grupowania i porządkowania danych.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-3.md)
