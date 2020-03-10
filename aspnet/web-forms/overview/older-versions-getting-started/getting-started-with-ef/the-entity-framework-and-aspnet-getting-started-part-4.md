---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 4 | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework. Przykładowa aplikacja to...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: eb75a76038466bf30738387ed4739687de1df944
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638167"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 4

Autor [Dykstra](https://github.com/tdykstra)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework 4,0 i programu Visual Studio 2010. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="working-with-related-data"></a>Praca z danymi powiązanymi

W poprzednim samouczku użyto kontrolki `EntityDataSource` do filtrowania, sortowania i grupowania danych. W tym samouczku zostaną wyświetlone i zaktualizowane powiązane dane.

Utworzysz stronę instruktorów, która wyświetla listę instruktorów. Po wybraniu instruktora zostanie wyświetlona lista szkoleń szkoleniowych według tego instruktora. Po wybraniu kursu zobaczysz szczegóły dotyczące kursu i listę studentów zarejestrowanych w kursie. Można edytować nazwisko instruktora, datę zatrudnienia i przypisanie pakietu Office. Przypisanie pakietu Office to oddzielny zestaw jednostek, do którego uzyskujesz dostęp za pomocą właściwości nawigacji.

Dane główne można połączyć z danymi szczegółowymi w znacznikach lub w kodzie. W tej części samouczka będziesz używać obu tych metod.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Wyświetlanie i aktualizowanie powiązanych jednostek w kontrolce GridView

Utwórz nową stronę sieci Web o nazwie *instruktors. aspx* , która korzysta ze strony wzorcowej *site. Master* , i Dodaj następujące znaczniki do kontrolki `Content` o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Ten znacznik tworzy formant `EntityDataSource`, który wybiera instruktorów i włącza aktualizacje. Element `div` konfiguruje adiustację do renderowania po lewej stronie, aby można było dodać kolumnę po prawej stronie później.

Między znacznikiem `EntityDataSource` i zamykanym tagiem `</div>` Dodaj następujące znaczniki, które tworzą kontrolkę `GridView` i kontrolkę `Label`, która będzie używana dla komunikatów o błędach:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Ta kontrolka `GridView` umożliwia zaznaczanie wierszy, wyróżnianie zaznaczonego wiersza przy użyciu jasnego szarego koloru tła i określa programy obsługi (które utworzysz później) dla zdarzeń `SelectedIndexChanged` i `Updating`. Określa również `PersonID` właściwości `DataKeyNames`, dzięki czemu wartość klucza wybranego wiersza może być przenoszona do innego formantu, który zostanie dodany później.

Ostatnia kolumna zawiera przypisanie pakietu Office instruktora, które jest przechowywane we właściwości nawigacji jednostki `Person`, ponieważ pochodzi ze skojarzonej jednostki. Należy zauważyć, że element `EditItemTemplate` określa `Eval` zamiast `Bind`, ponieważ kontrolka `GridView` nie może zostać powiązana bezpośrednio z właściwościami nawigacji, aby je zaktualizować. Aktualizujesz przypisanie pakietu Office w kodzie. W tym celu należy odwołać się do kontrolki `TextBox` i uzyskać i zapisać ją w zdarzeniu `Init` kontrolki `TextBox`.

Kontrolka `GridView` jest formantem `Label` używanym do komunikatów o błędach. Właściwość `Visible` kontrolki jest `false`, a stan widoku jest wyłączony, dzięki czemu etykieta będzie wyświetlana tylko wtedy, gdy kod staje się widoczny w odpowiedzi na błąd.

Otwórz plik *Instructors.aspx.cs* i Dodaj następującą instrukcję `using`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Dodaj pole klasy prywatnej bezpośrednio po deklaracji nazwy częściowej klasy, aby pomieścić odwołanie do pola tekstowego przypisania pakietu Office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Dodaj element zastępczy dla programu obsługi zdarzeń `SelectedIndexChanged`, do którego kod zostanie dodany później. Dodaj również procedurę obsługi dla zdarzenia `Init` kontrolki `TextBox` przypisania pakietu Office, aby można było zapisać odwołanie do kontrolki `TextBox`. Użyjesz tego odwołania, aby pobrać wartość wprowadzoną przez użytkownika w celu zaktualizowania jednostki skojarzonej z właściwością nawigacji.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Będziesz używać zdarzenia `Updating` kontrolki `GridView`, aby zaktualizować Właściwość `Location` skojarzonej jednostki `OfficeAssignment`. Dodaj następującą procedurę obsługi dla zdarzenia `Updating`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Ten kod jest uruchamiany, gdy użytkownik kliknie przycisk **Aktualizuj** w wierszu `GridView`. Kod używa LINQ to Entities do pobrania jednostki `OfficeAssignment` skojarzonej z bieżącą jednostką `Person` przy użyciu `PersonID` wybranego wiersza z argumentu zdarzenia.

Następnie kod pobiera jedną z następujących akcji w zależności od wartości w kontrolce `InstructorOfficeTextBox`:

- Jeśli pole tekstowe ma wartość i nie ma `OfficeAssignment` jednostki do zaktualizowania, zostanie ona utworzona.
- Jeśli pole tekstowe ma wartość i istnieje jednostka `OfficeAssignment`, aktualizuje wartość właściwości `Location`.
- Jeśli pole tekstowe jest puste i istnieje jednostka `OfficeAssignment`, spowoduje to usunięcie jednostki.

Następnie zapisuje zmiany w bazie danych. Jeśli wystąpi wyjątek, zostanie wyświetlony komunikat o błędzie.

Uruchom stronę.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Kliknij przycisk **Edytuj** i wszystkie pola przejdź do pól tekstowych.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Zmień dowolne z tych wartości, w tym **przypisanie pakietu Office**. Kliknij przycisk **Aktualizuj** , aby zobaczyć zmiany odzwierciedlone na liście.

## <a name="displaying-related-entities-in-a-separate-control"></a>Wyświetlanie powiązanych jednostek w oddzielnym formancie

Każdy instruktor może nauczyć jeden lub więcej kursów, dlatego dodasz kontrolkę `EntityDataSource` i kontrolkę `GridView`, aby wyświetlić listę kursów skojarzonych z tym instruktorem w `GridView` kontroli. Aby utworzyć nagłówek i formant `EntityDataSource` dla jednostek kursów, Dodaj następujące znaczniki między formantem `Label` komunikatu o błędzie i zamykającym tagiem `</div>`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

Parametr `Where` zawiera wartość `PersonID` instruktora, którego wiersz został wybrany w kontrolce `InstructorsGridView`. Właściwość `Where` zawiera polecenie subselect, które pobiera wszystkie skojarzone jednostki `Person` z `People` właściwości nawigacji jednostki `Course` i wybiera jednostkę `Course` tylko wtedy, gdy jedna z skojarzonych jednostek `Person` zawiera wybraną wartość `PersonID`.

Aby utworzyć kontrolkę `GridView`. Dodaj następujące znaczniki bezpośrednio po kontrolce `CoursesEntityDataSource` (przed tagiem zamykającym `</div>`):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Ponieważ żadne kursy nie będą wyświetlane w przypadku wybrania żadnego instruktora, zostanie uwzględniony element `EmptyDataTemplate`.

Uruchom stronę.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Wybierz instruktora, do którego przypisano co najmniej jeden kurs, a kurs lub kursy pojawiają się na liście. (Uwaga: Chociaż schemat bazy danych umożliwia używanie wielu kursów, w danych testowych dostarczonych z bazą danych żaden instruktor nie ma więcej niż jednego kursu. Kursy do bazy danych można dodać samodzielnie przy użyciu okna **Eksplorator serwera** lub strony *CoursesAdd. aspx* , która zostanie dodana w kolejnym samouczku.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

Kontrolka `CoursesGridView` zawiera tylko kilka pól kursów. Aby wyświetlić wszystkie szczegóły dotyczące kursu, użyjesz kontrolki `DetailsView` dla kursu, który wybierze użytkownik. W przypadku *instruktorów. aspx*Dodaj następujące znaczniki po tagu zamykającym `</div>` (Upewnij się, że ten znacznik jest umieszczony **po** tagu zamykającego DIV, a nie przed nim):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Ten znacznik tworzy formant `EntityDataSource`, który jest powiązany z zestawem `Courses` Entity. Właściwość `Where` wybiera kurs przy użyciu wartości `CourseID` wybranego wiersza w kontrolce `GridView` kursów. Znacznik określa procedurę obsługi dla zdarzenia `Selected`, który będzie używany później do wyświetlania ocen uczniów, który jest niższym poziomem w hierarchii.

W *Instructors.aspx.cs*Utwórz następujące elementy zastępcze dla metody `CourseDetailsEntityDataSource_Selected`. (W dalszej części tego samouczka zostanie wypełniona ta procedura), która będzie potrzebna, aby strona została skompilowana i uruchomiona.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Uruchom stronę.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Początkowo nie ma szczegółowych informacji o kursie, ponieważ nie wybrano kursu. Wybierz instruktora, który ma przypisany kurs, a następnie wybierz kurs, aby wyświetlić szczegóły.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Wyświetlanie powiązanych danych przy użyciu zdarzenia EntityDataSource "selected"

Na koniec chcesz wyświetlić wszystkie zarejestrowane uczniowie i ich klasy dla wybranego kursu. W tym celu należy użyć zdarzenia `Selected` formantu `EntityDataSource` powiązanego z `DetailsView`em kursu.

W oknie *instruktors. aspx*Dodaj następujące znaczniki po kontrolce `DetailsView`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Ten znacznik tworzy formant `ListView`, który wyświetla listę studentów i ich klasy dla wybranego kursu. Nie określono źródła danych, ponieważ będziesz wiązać się z kontrolką w kodzie. Element `EmptyDataTemplate` udostępnia komunikat, który będzie wyświetlany, gdy nie jest wybrany żaden kurs — w tym przypadku nie ma studentów do wyświetlenia. Element `LayoutTemplate` tworzy tabelę HTML do wyświetlania listy, a `ItemTemplate` określa kolumny do wyświetlenia. Identyfikator ucznia i jakość ucznia pochodzą z jednostki `StudentGrade`, a nazwa ucznia pochodzi z jednostki `Person`, która Entity Framework udostępniana w `Person` właściwości nawigacji jednostki `StudentGrade`.

W *Instructors.aspx.cs*Zastąp metodę `CourseDetailsEntityDataSource_Selected` użyto metod zastępczych, używając następującego kodu:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Argument zdarzenia dla tego zdarzenia zawiera wybrane dane w postaci kolekcji, które będą miały zero elementów, jeśli nic nie jest zaznaczone lub jeden element w przypadku wybrania jednostki `Course`. Jeśli wybrano jednostkę `Course`, kod używa metody `First` do konwersji kolekcji na pojedynczy obiekt. Następnie pobiera `StudentGrade` jednostek z właściwości nawigacji, konwertuje je do kolekcji i wiąże formant `GradesListView` z kolekcją.

Jest to wystarczające do wyświetlania ocen, ale chcesz upewnić się, że komunikat w pustym szablonie danych jest wyświetlany podczas pierwszego wyświetlania strony i gdy nie wybrano kursu. W tym celu należy utworzyć następującą metodę, która będzie wywoływana z dwóch miejsc:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Wywołaj tę nową metodę z metody `Page_Load`, aby wyświetlić pusty szablon danych podczas pierwszego wyświetlania strony. I Wywołaj ją z metody `InstructorsGridView_SelectedIndexChanged`, ponieważ to zdarzenie jest zgłaszane w przypadku wybrania instruktora, co oznacza, że nowe kursy są ładowane do kontrolki kursy `GridView` i żadna nie została jeszcze wybrana. Oto dwa wywołania:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Uruchom stronę.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Wybierz instruktora, który ma przypisany kurs, a następnie wybierz kurs.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Widzisz teraz kilka sposobów pracy z powiązanymi danymi. W poniższym samouczku dowiesz się, jak dodać relacje między istniejącymi jednostkami, jak usunąć relacje i jak dodać nową jednostkę, która ma relację z istniejącą jednostką.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-5.md)
