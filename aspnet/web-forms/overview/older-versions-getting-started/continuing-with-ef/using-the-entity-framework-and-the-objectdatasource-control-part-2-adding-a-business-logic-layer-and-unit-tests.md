---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Używanie Entity Framework 4,0 i formantu ObjectDataSource, część 2: dodawanie warstwy logiki biznesowej i testów jednostkowych | Microsoft Docs'
author: tdykstra
description: Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 24344cc33d7c26d7c408db26c0530ef2c708a7d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546768"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Używanie Entity Framework 4,0 i formantu ObjectDataSource, część 2: dodawanie warstwy logiki biznesowej i testów jednostkowych

Autor [Dykstra](https://github.com/tdykstra)

> Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków [Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) . Jeśli wcześniej samouczki nie zostały wykonane, jako punkt wyjścia dla tego samouczka możesz [pobrać utworzoną aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) utworzoną przez kompletną serię samouczków. Jeśli masz pytania dotyczące samouczków, możesz je ogłosić na [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).

W poprzednim samouczku utworzono wielowarstwową aplikację sieci Web przy użyciu Entity Framework i kontrolki `ObjectDataSource`. W tym samouczku pokazano, jak dodać logikę biznesową przy zachowaniu warstwy logiki biznesowej (LOGIKI biznesowej) i warstwy dostępu do danych (DAL) oddzielonej, a ponadto przedstawiono sposób tworzenia zautomatyzowanych testów jednostkowych dla LOGIKI biznesowej.

W tym samouczku wykonasz następujące zadania:

- Utwórz interfejs repozytorium, który deklaruje wymagane metody dostępu do danych.
- Zaimplementuj interfejs repozytorium w klasie repozytorium.
- Utwórz klasę logiki biznesowej, która wywołuje klasę repozytorium w celu wykonywania funkcji dostępu do danych.
- Połącz formant `ObjectDataSource` z klasą logiki biznesowej, a nie z klasą repozytorium.
- Utwórz projekt jednostki testowej i klasy repozytorium, która używa kolekcji w pamięci dla magazynu danych.
- Utwórz test jednostkowy dla logiki biznesowej, która ma zostać dodana do klasy logiki biznesowej, a następnie uruchom test i zobacz, jak to się nie powiedzie.
- Zaimplementuj logikę biznesową w klasie logiki biznesowej, a następnie ponownie uruchom test jednostkowy i sprawdź, czy został on przekazany.

Będziesz korzystać z stron *działS. aspx* i *DepartmentsAdd. aspx* utworzonych w poprzednim samouczku.

## <a name="creating-a-repository-interface"></a>Tworzenie interfejsu repozytorium

Rozpocznie się Tworzenie interfejsu repozytorium.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

W folderze *dal* Utwórz nowy plik klasy, nadaj mu nazwę *ISchoolRepository.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Interfejs definiuje jedną metodę dla każdej metody CRUD (tworzenie, Odczyt, aktualizacja, usuwanie), która została utworzona w klasie repozytorium.

W klasie `SchoolRepository` w *SchoolRepository.cs*wskaż, że ta klasa implementuje interfejs `ISchoolRepository`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Tworzenie klasy logiki biznesowej

Następnie utworzysz klasę logiki biznesowej. Należy to zrobić, aby można było dodać logikę biznesową, która będzie wykonywana przez formant `ObjectDataSource`, chociaż nie będzie jeszcze to zrobione. Na razie nowa klasa logiki biznesowej wykona te same operacje CRUD, które wykonuje repozytorium.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Utwórz nowy folder i nadaj mu nazwę *logiki biznesowej*. (W świecie rzeczywistym Warstwa logiki biznesowej zwykle jest implementowana jako Biblioteka klas — oddzielny projekt — ale aby ten samouczek był prosty, klasy LOGIKI biznesowej będą przechowywane w folderze projektu).

W folderze *logiki biznesowej* Utwórz nowy plik klasy, nadaj mu nazwę *SchoolBL.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Ten kod tworzy te same metody CRUD, które zostały wytoczone wcześniej w klasie repozytorium, ale zamiast uzyskiwać bezpośredni dostęp do metod Entity Framework, wywołuje metody klasy repozytorium.

Zmienna klasy, która przechowuje odwołanie do klasy repozytorium, jest definiowana jako typ interfejsu, a kod, który tworzy wystąpienie klasy repozytorium, jest zawarty w dwóch konstruktorach. Konstruktor bez parametrów będzie używany przez formant `ObjectDataSource`. Tworzy wystąpienie klasy `SchoolRepository` utworzonej wcześniej. Inny Konstruktor umożliwia każdy kod, który tworzy wystąpienie klasy logiki biznesowej do przekazania do dowolnego obiektu, który implementuje interfejs repozytorium.

Metody CRUD wywołujące klasę repozytorium i dwa konstruktory umożliwiają używanie klasy logiki biznesowej z dowolnym wybranym magazynem danych zaplecza. Klasa logiki biznesowej nie musi wiedzieć, jak Klasa, która wywołuje, utrzymuje dane. (Jest to często nazywane *ignorujących trwałości*). Ułatwia to testowanie jednostkowe, ponieważ można połączyć klasę logiki biznesowej z implementacją repozytorium, która używa dowolnego elementu, jak kolekcje `List` w pamięci do przechowywania danych.

> [!NOTE]
> Technicznie obiekty jednostek nie są nadal trwałości — ignorujących, ponieważ są tworzone z klas, które dziedziczą z klasy `EntityObject` Entity Framework. W celu uzyskania pełnej trwałości ignorujących można użyć *zwykłych starych obiektów CLR*lub *POCOs*zamiast obiektów dziedziczących z klasy `EntityObject`. Korzystanie z POCOs wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji, zobacz [testowanie i Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx) w witrynie MSDN.

Teraz można połączyć kontrolki `ObjectDataSource` z klasą logiki biznesowej zamiast do repozytorium i sprawdzić, czy wszystko działa tak jak wcześniej.

W *działach. aspx* i *DepartmentsAdd. aspx*Zmień każde wystąpienie `TypeName="ContosoUniversity.DAL.SchoolRepository"` na `TypeName="ContosoUniversity.BLL.SchoolBL`". (Wszystkie wystąpienia są w ogóle dostępne).

Uruchom strony *działS. aspx* i *DepartmentsAdd. aspx* , aby sprawdzić, czy nadal działają tak jak wcześniej.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Tworzenie projektu testu jednostkowego i repozytorium

Dodaj nowy projekt do rozwiązania przy użyciu szablonu **projektu testowego** i nadaj mu nazwę `ContosoUniversity.Tests`.

W projekcie testowym Dodaj odwołanie do `System.Data.Entity` i Dodaj odwołanie projektu do projektu `ContosoUniversity`.

Teraz można utworzyć klasę repozytorium, która będzie używana z testami jednostkowymi. Magazyn danych dla tego repozytorium będzie znajdować się w klasie.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

W projekcie testowym Utwórz nowy plik klasy, nadaj mu nazwę *MockSchoolRepository.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Ta klasa repozytorium ma takie same metody CRUD, jak ta, która uzyskuje bezpośredni dostęp do Entity Framework, ale współpracują z `List`mi kolekcjami w pamięci, a nie z bazą danych. Ułatwia to klasie testowej skonfigurowanie i zweryfikowanie testów jednostkowych dla klasy logiki biznesowej.

## <a name="creating-unit-tests"></a>Tworzenie testów jednostkowych

Szablon projektu **testowego** utworzył klasę testów jednostkowych klasy zastępczej, a następne zadanie polega na zmodyfikowaniu tej klasy, dodając do niej metody testów jednostkowych dla logiki biznesowej, która ma zostać dodana do klasy logiki biznesowej.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

W firmie Contoso University każdy indywidualny instruktor może być tylko administratorem jednego działu i należy dodać logikę biznesową, aby wymusić tę regułę. Zacznij od dodania testów i uruchomienia testów, aby zobaczyć ich niepowodzenie. Następnie dodasz kod i ponownie uruchomimy testy, które będą widoczne jako zakończone powodzeniem.

Otwórz plik *UnitTest1.cs* i dodaj instrukcje `using` dla warstw logiki biznesowej i dostępu do danych, które zostały utworzone w projekcie ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Zastąp metodę `TestMethod1` następującymi metodami:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

Metoda `CreateSchoolBL` tworzy wystąpienie klasy repozytorium, które zostało utworzone dla projektu testów jednostkowych, który następnie przekazuje do nowego wystąpienia klasy logiki biznesowej. Następnie metoda używa klasy logiki biznesowej do wstawiania trzech działów, których można użyć w metodach testowych.

Metody testowe sprawdzają, czy Klasa logiki biznesowej zgłasza wyjątek, jeśli ktoś spróbuje wstawić nowy dział z tym samym administratorem jako istniejący dział lub jeśli ktoś spróbuje zaktualizować administratora działu, ustawiając go na identyfikator osoby kto jest już administratorem innego działu.

Nie utworzono jeszcze klasy Exception, więc ten kod nie zostanie skompilowany. Aby uzyskać kompilację, kliknij prawym przyciskiem myszy `DuplicateAdministratorException` i wybierz polecenie **Generuj**, a następnie **Class**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Spowoduje to utworzenie klasy w projekcie testowym, którą można usunąć po utworzeniu klasy wyjątku w projekcie głównym. i wdrożono logikę biznesową.

Uruchom projekt testowy. Zgodnie z oczekiwaniami testy zakończą się niepowodzeniem.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Dodawanie logiki biznesowej w celu przeprowadzenia testu

Następnie należy wdrożyć logikę biznesową, która nie będzie mogła być ustawiona jako administrator działu, który jest już administratorem innego działu. Należy zgłosić wyjątek z warstwy logiki biznesowej, a następnie przechwycić ją w warstwie prezentacji, jeśli użytkownik dokona edycji działu i klika opcję **Aktualizuj** po wybraniu osoby, która jest już administratorem. (Można również usunąć instruktorów z listy rozwijanej, którzy są już administratorami przed renderowaniem strony, ale celem tego problemu jest praca z warstwą logiki biznesowej).

Zacznij od utworzenia klasy wyjątku, która zostanie wytworzona, gdy użytkownik spróbuje utworzyć instruktora jako administratora więcej niż jednego działu. W głównym projekcie Utwórz nowy plik klasy w folderze *logiki biznesowej* , nadaj mu nazwę *DuplicateAdministratorException.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Teraz Usuń tymczasowy plik *DuplicateAdministratorException.cs* , który został utworzony w projekcie testowym wcześniej, aby możliwe było skompilowanie.

W głównym projekcie Otwórz plik *SchoolBL.cs* i Dodaj następującą metodę, która zawiera logikę walidacji. (Kod odnosi się do metody, którą utworzysz później).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Ta metoda zostanie wywołana podczas wstawiania lub aktualizowania jednostek `Department` w celu sprawdzenia, czy inny dział ma już tego samego administratora.

Kod wywołuje metodę w celu przeszukania bazy danych dla jednostki `Department`, która ma taką samą wartość właściwości `Administrator`, co jednostka, która jest wstawiana lub aktualizowana. Jeśli zostanie znaleziony, kod zgłosi wyjątek. Sprawdzanie poprawności nie jest wymagane, jeśli wstawiona lub zaktualizowana jednostka nie ma `Administrator` wartość i żaden wyjątek nie jest zgłaszany, jeśli metoda jest wywoływana podczas aktualizacji, a znaleziona jednostka `Department` jest zgodna z aktualizacją jednostki `Department`.

Wywołaj nową metodę z `Insert` i `Update` metod:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

W *ISchoolRepository.cs*, Dodaj następującą deklarację dla nowej metody dostępu do danych:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

W *SchoolRepository.cs*, Dodaj następującą instrukcję `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

W *SchoolRepository.cs*Dodaj następującą nową metodę dostępu do danych:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Ten kod pobiera `Department` jednostek, które mają określonego administratora. Należy znaleźć tylko jeden dział (jeśli istnieje). Jednak, ponieważ żadne ograniczenie nie jest wbudowane w bazie danych, zwracany typ jest kolekcją w przypadku znalezienia wielu działów.

Domyślnie, gdy kontekst obiektu Pobiera jednostki z bazy danych, śledzi je w Menedżerze stanu obiektów. Parametr `MergeOption.NoTracking` określa, że to śledzenie nie zostanie wykonane dla tego zapytania. Jest to konieczne, ponieważ zapytanie może zwrócić dokładną jednostkę, którą próbujesz zaktualizować, a następnie nie może dołączyć tej jednostki. Na przykład Jeśli edytujesz dział historii na stronie *działS. aspx* i nie zmienisz administratora, to zapytanie zwróci dział historii. Jeśli `NoTracking` nie jest ustawiona, kontekst obiektu ma już jednostkę działu historii w jej Menedżerze stanu obiektów. Po dołączeniu jednostki działu historii, która została utworzona ponownie z stanu widoku, kontekst obiektu zgłosi wyjątek, który jest wyświetlany `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Jako alternatywę dla określania `MergeOption.NoTracking`, można utworzyć nowy kontekst obiektu tylko dla tego zapytania. Ponieważ nowy kontekst obiektu będzie miał własny Menedżer stanu obiektów, nie będzie konfliktu podczas wywoływania metody `Attach`. Nowy kontekst obiektu udostępnia metadane i połączenie z bazą danych przy użyciu oryginalnego kontekstu obiektów, dzięki czemu spadek wydajności tego alternatywnego podejścia będzie minimalny. Tutaj przedstawiono podejście, w którym wprowadzono opcję `NoTracking`, która będzie przydatna w innych kontekstach. Opcja `NoTracking` została omówiona dalej w kolejnym samouczku w tej serii).

W projekcie testowym Dodaj nową metodę dostępu do danych do *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Ten kod używa LINQ do wykonania tych samych danych, których repozytorium projektu `ContosoUniversity` używa LINQ to Entities dla programu.

Uruchom ponownie projekt testowy. Ta godzina przebiegu testów.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Obsługa wyjątków ObjectDataSource

W projekcie `ContosoUniversity` Uruchom stronę działu *. aspx* , a następnie spróbuj zmienić administratora dla działu na kogoś, kto jest już administratorem dla innego działu. (Należy pamiętać, że można edytować tylko te działy, które zostały dodane w tym samouczku, ponieważ baza danych jest wstępnie załadowana z nieprawidłowymi danymi). Zostanie uzyskana następująca strona błędu serwera:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Nie chcesz, aby użytkownicy widzieli ten rodzaj strony błędów, więc musisz dodać kod obsługi błędu. Otwórz *działy. aspx* i określ procedurę obsługi dla zdarzenia `OnUpdated` `DepartmentsObjectDataSource`. `ObjectDataSource` tagu otwierającego jest teraz podobny do poniższego przykładu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

W *Departments.aspx.cs*, Dodaj następującą instrukcję `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Dodaj następującą procedurę obsługi dla zdarzenia `Updated`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Jeśli formant `ObjectDataSource` przechwytuje wyjątek podczas próby wykonania aktualizacji, przekazuje wyjątek w argumencie zdarzenia (`e`) do tej procedury obsługi. Kod w programie obsługi sprawdza, czy wyjątek jest duplikatem wyjątku administratora. Jeśli tak jest, kod tworzy kontrolkę modułu sprawdzania poprawności, która zawiera komunikat o błędzie dla kontrolki `ValidationSummary` do wyświetlenia.

Uruchom stronę i spróbuj ponownie wykonać kogoś przez administratora dwóch działów. Tym razem formant `ValidationSummary` wyświetla komunikat o błędzie.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Wprowadź podobne zmiany na stronie *DepartmentsAdd. aspx* . W *DepartmentsAdd. aspx*Określ procedurę obsługi dla zdarzenia `OnInserted` `DepartmentsObjectDataSource`. Znaczniki będące wynikiem są podobne do poniższego przykładu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

W *DepartmentsAdd.aspx.cs*, Dodaj tę samą instrukcję `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Dodaj następujący program obsługi zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Teraz można przetestować stronę *DepartmentsAdd.aspx.cs* , aby upewnić się, że prawidłowo obsługuje ona próby nadania jednej osobie administratora więcej niż jednego działu.

W tym celu wprowadzamy wprowadzenie do wdrożenia wzorca repozytorium w celu użycia formantu `ObjectDataSource` z Entity Framework. Aby uzyskać więcej informacji na temat wzorca i testowania repozytorium, zobacz temat testowanie dokumentacji MSDN [i Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx).

W poniższym samouczku dowiesz się, jak dodać funkcje sortowania i filtrowania do aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [dalej](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
