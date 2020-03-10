---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Co nowego w Entity Framework 4,0 | Microsoft Docs
author: tdykstra
description: Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 29d2bd61e8361b130e7b9415dad4fe1d5b847945
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642675"
---
# <a name="whats-new-in-the-entity-framework-40"></a>Co nowego w programie Entity Framework 4.0

Autor [Dykstra](https://github.com/tdykstra)

> Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez [wprowadzenie z](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serią samouczków Entity Framework. Jeśli wcześniej samouczki nie zostały wykonane, jako punkt wyjścia dla tego samouczka możesz [pobrać utworzoną aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) utworzoną przez kompletną serię samouczków. Jeśli masz pytania dotyczące samouczków, możesz je ogłosić na [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).

W poprzednim samouczku zawarto kilka metod maksymalizowania wydajności aplikacji sieci Web, która używa Entity Framework. W tym samouczku przedstawiono niektóre najważniejsze nowe funkcje w wersji 4 Entity Framework i linki do zasobów, które zapewniają pełniejsze wprowadzenie do wszystkich nowych funkcji. Do funkcji wyróżnionych w tym samouczku należą:

- Skojarzenia kluczy obcych.
- Wykonywanie poleceń SQL zdefiniowanych przez użytkownika.
- Tworzenie modelu w pierwszej kolejności.
- Obsługa POCO.

Ponadto w samouczku zostanie krótko *zaprojektowana funkcja tworzenia kodu*, która jest dostępna w następnej wersji Entity Framework.

Aby rozpocząć pracę z samouczkiem, uruchom program Visual Studio i Otwórz aplikację sieci Web Contoso University, z którą pracowałeś w poprzednim samouczku.

## <a name="foreign-key-associations"></a>Skojarzenia klucza obcego

Wersja 3,5 Entity Framework uwzględnionych właściwości nawigacji, ale nie zawiera właściwości klucza obcego w modelu danych. Na przykład kolumny `CourseID` i `StudentID` tabeli `StudentGrade` zostałyby pominięte w jednostce `StudentGrade`.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Przyczyną tego podejścia było to, że mówiąc, że klucz obcy jest fizycznym szczegółem implementacji i nie należy do modelu danych koncepcyjnych. Jednak w praktyce często łatwiej jest współpracować z jednostkami w kodzie, gdy masz bezpośredni dostęp do kluczy obcych.

Aby zapoznać się z przykładem sposobu, w jaki klucze obce w modelu danych mogą uprościć kod, należy wziąć pod uwagę, jak miało być kod strony *DepartmentsAdd. aspx* bez nich. W jednostce `Department` Właściwość `Administrator` jest kluczem obcym, który odpowiada `PersonID` w jednostce `Person`. Aby ustanowić skojarzenie między nowym działem i jego administratorem, należy ustawić wartość właściwości `Administrator` w obsłudze zdarzeń `ItemInserting` formantu powiązanego z danymi:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Bez kluczy obcych w modelu danych, obsłużyć zdarzenie `Inserting` formantu źródła danych zamiast zdarzenia `ItemInserting` formantu powiązanego z danymi, aby uzyskać odwołanie do samej jednostki przed dodaniem jednostki do zestawu jednostek. W przypadku tego odwołania należy ustanowić skojarzenie przy użyciu kodu, takiego jak w następujących przykładach:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Jak widać w wpisie w blogu Entity Framework zespołu [na skojarzeniach kluczy obcych](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), istnieją inne przypadki, w których różnica w złożoności kodu jest znacznie większa. Aby zaspokoić potrzeby użytkowników, którzy wolą na żywo z szczegółowymi informacjami dotyczącymi implementacji w modelu danych koncepcyjnych dla uproszczenia kodu, Entity Framework teraz zapewnia opcję dołączenia kluczy obcych do modelu danych.

W Entity Framework terminologii, jeśli dołączysz klucze obce w modelu danych, z którego korzystasz *skojarzenia kluczy obcych*, a jeśli wykluczasz klucze obce, używane są *niezależne skojarzenia*.

## <a name="executing-user-defined-sql-commands"></a>Wykonywanie poleceń SQL zdefiniowanych przez użytkownika

We wcześniejszych wersjach Entity Framework nie było prostego sposobu tworzenia własnych poleceń SQL na bieżąco i uruchamiania ich. Entity Framework dynamicznie wygenerował polecenia SQL lub trzeba utworzyć procedurę składowaną i zaimportować ją jako funkcję. Wersja 4 dodaje `ExecuteStoreQuery` i `ExecuteStoreCommand` metody `ObjectContext`j klasy, która ułatwia przekazywanie wszelkich zapytań bezpośrednio do bazy danych.

Załóżmy, że administratorzy uczelni firmy Contoso chcą mieć możliwość wykonywania zbiorczych zmian w bazie danych bez konieczności przechodzenia przez proces tworzenia procedury składowanej i importowania jej do modelu danych. Pierwsze żądanie dotyczy strony, która pozwala im zmienić liczbę kredytów dla wszystkich kursów w bazie danych. Na stronie sieci Web chcą mieć możliwość wprowadzenia liczby, która ma być używana do pomnożenia wartości każdej kolumny `Credits` `Course` wierszy.

Utwórz nową stronę, która używa strony głównej *witryny. Master* i nadaj jej nazwę *UpdateCredits. aspx*. Następnie Dodaj następujący znacznik do kontrolki `Content` o nazwie `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Ten znacznik tworzy formant `TextBox`, w którym użytkownik może wprowadzić wartość mnożnika, `Button` kontrolkę do kliknięcia, aby wykonać polecenie, i kontrolkę `Label` wskazującą liczbę wierszy, których to dotyczy.

Otwórz *UpdateCredits.aspx.cs*i Dodaj następującą instrukcję `using` i procedurę obsługi dla zdarzenia `Click` przycisku:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Ten kod wykonuje polecenie SQL `Update` przy użyciu wartości w polu tekstowym i używa etykiety do wyświetlania liczby wierszy, których to dotyczy. Przed uruchomieniem strony należy uruchomić stronę *kursy. aspx* , aby uzyskać "przed" zdjęcie niektórych danych.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Uruchom *UpdateCredits. aspx*, wprowadź wartość "10" jako mnożnik, a następnie kliknij przycisk **Execute (wykonaj**).

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Ponownie uruchom stronę *kursy. aspx* , aby zobaczyć zmienione dane.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Jeśli chcesz ustawić liczbę kredytów z powrotem do swoich oryginalnych wartości, w *UpdateCredits.aspx.cs* Zmień `Credits * {0}`, aby `Credits / {0}` i ponownie uruchomić stronę, wprowadzając 10 jako dzielnik).

Aby uzyskać więcej informacji na temat wykonywania zapytań zdefiniowanych w kodzie, zobacz [How to: Direct Execute Commands to the Data Source](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Model — programowanie w pierwszej kolejności

W tych przewodnikach najpierw utworzono bazę danych, a następnie Wygenerowano model danych na podstawie struktury bazy danych. W Entity Framework 4 zamiast tego można zacząć od modelu danych i generować bazę danych na podstawie struktury modelu danych. W przypadku tworzenia aplikacji, dla której baza danych jeszcze nie istnieje, podejście w pierwszej kolejności pozwala tworzyć jednostki i relacje, które mają sens koncepcyjny dla aplikacji, a nie martwić się o szczegóły implementacji fizycznej . (Jest to jednak prawdziwe tylko w przypadku początkowych etapów programowania). Ostatecznie baza danych zostanie utworzona i będzie zawierać w niej dane produkcyjne i ponowne utworzenie jej z modelu nie będzie już praktyczne; w tym momencie nastąpi powrót do pierwszej metody bazy danych.

W tej części samouczka utworzysz prosty model danych i wygenerujesz z niego bazę danych.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *dal* i wybierz pozycję **Dodaj nowy element**. W oknie dialogowym **Dodaj nowy element** w obszarze **zainstalowane szablony** wybierz pozycję **dane** , a następnie wybierz szablon **Entity Data Model ADO.NET** . Nazwij nowy plik *AlumniAssociationModel. edmx* , a następnie kliknij przycisk **Dodaj**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Spowoduje to uruchomienie Kreatora Entity Data Model. W kroku **Wybierz zawartość modelu** wybierz pozycję **pusty model** , a następnie kliknij przycisk **Zakończ**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

Zostanie otwarty **projektant Entity Data Model** z pustą powierzchnią projektową. Przeciągnij element **jednostki** z **przybornika** na powierzchnię projektu.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Zmień nazwę jednostki z `Entity1` na `Alumnus`, Zmień nazwę właściwości `Id` na `AlumnusId`i Dodaj nową właściwość skalarną o nazwie `Name`. Aby dodać nowe właściwości, możesz nacisnąć klawisz Enter po zmianie nazwy kolumny `Id` lub kliknięciu prawym przyciskiem myszy jednostki i wybrać polecenie **Dodaj właściwość skalarną**. Domyślny typ nowych właściwości to `String`, co jest bardziej odpowiednie dla tej prostej demonstracji, ale oczywiście można zmienić elementy, takie jak typ danych w oknie **Właściwości** .

Utwórz inną jednostkę w taki sam sposób i nadaj jej nazwę `Donation`. Zmień właściwość `Id` na `DonationId` i Dodaj właściwość skalarną o nazwie `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Aby dodać skojarzenie między tymi dwiema jednostkami, kliknij prawym przyciskiem myszy jednostkę `Alumnus`, wybierz polecenie **Dodaj**, a następnie wybierz pozycję **skojarzenie**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Wartości domyślne w oknie dialogowym **Dodaj skojarzenie** są odpowiednie dla użytkownika (jeden do wielu, obejmują właściwości nawigacji, Uwzględnij klucze obce), więc wystarczy kliknąć przycisk **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Projektant dodaje linię skojarzenia i właściwość klucza obcego.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Teraz możesz przystąpić do tworzenia bazy danych. Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Generuj bazę danych na podstawie modelu**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Spowoduje to uruchomienie Kreatora generowania bazy danych. (Jeśli widzisz ostrzeżenia, które wskazują, że jednostki nie są zamapowane, możesz zignorować te elementy przez czas).

W kroku **Wybierz połączenie danych** kliknij pozycję **nowe połączenie**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

W oknie dialogowym **Właściwości połączenia** wybierz wystąpienie SQL Server Express lokalnego i nazwij `AlumniAssociation`bazy danych.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Kliknij przycisk **tak** po wyświetleniu monitu, jeśli chcesz utworzyć bazę danych. Po ponownym wyświetleniu kroku **Wybierz połączenie danych** kliknij przycisk **dalej**.

W kroku **Podsumowanie i ustawienia** kliknij przycisk **Zakończ**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Tworzony jest plik *SQL* z poleceniami języka definicji danych (DDL), ale polecenia nie zostały jeszcze uruchomione.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Użyj narzędzia, takiego jak **SQL Server Management Studio** , aby uruchomić skrypt i utworzyć tabele, jak można to zrobić podczas tworzenia bazy danych `School` dla [pierwszego samouczka w serii wprowadzenie samouczków](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Jeśli baza danych nie została pobrana).

Teraz można używać modelu danych `AlumniAssociation` na stronach sieci Web w taki sam sposób jak w przypadku modelu `School`. Aby wypróbować tę wartość, Dodaj dane do tabel i Utwórz stronę sieci Web, która wyświetla dane.

Za pomocą **Eksplorator serwera**Dodaj następujące wiersze do tabel `Alumnus` i `Donation`.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Utwórz nową stronę sieci Web o nazwie *studentów. aspx* , która korzysta ze strony wzorcowej *site. Master* . Dodaj następujący znacznik do kontrolki `Content` o nazwie `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Ten znacznik tworzy zagnieżdżone kontrolki `GridView`, zewnętrzny, aby wyświetlić nazwy studentów i wewnętrzną wartość, aby wyświetlić daty i kwoty darowizn.

Otwórz *Alumni.aspx.cs*. Dodaj instrukcję `using` dla warstwy dostępu do danych i programu obsługi zdarzenia `RowDataBound` zewnętrznego formantu `GridView`:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Ten kod tworzy powiązanie wewnętrznej kontrolki `GridView` przy użyciu właściwości nawigacji `Donations` jednostki `Alumnus` w bieżącym wierszu.

Uruchom stronę.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Uwaga: Ta strona jest uwzględniona w projekcie do pobrania, ale w celu jej działania należy utworzyć bazę danych w lokalnym wystąpieniu SQL Server Express. baza danych nie jest dołączana jako plik *MDF* do folderu *dane\_aplikacji* ).

Aby uzyskać więcej informacji na temat korzystania z funkcji pierwszej modelu Entity Framework, zobacz [model-First w Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Obsługa obiektów POCO

W przypadku korzystania z metodologii projektowania opartej na domenie, należy zaprojektować klasy danych, które reprezentują dane i zachowanie związane z domeną biznesową. Klasy te powinny być niezależne od żadnej konkretnej technologii używanej do przechowywania (utrwalania) danych; Innymi słowy, powinny to być *trwałość ignorujących*. Ignorujących trwałości może także ułatwić test jednostkowy, ponieważ projekt testu jednostkowego może korzystać z dowolnej technologii trwałości do testowania. Wcześniejsze wersje Entity Framework oferują ograniczoną obsługę ignorujących trwałości, ponieważ klasy jednostek musiały dziedziczyć z klasy `EntityObject` i w związku z tym obejmowały dużą liczbę Entity Framework funkcji.

Entity Framework 4 wprowadza możliwość używania klas jednostek, które nie dziedziczą z klasy `EntityObject` i dlatego są trwałością ignorujących. W kontekście Entity Framework takie klasy są zwykle nazywane *zwykłymi obiektami CLR* (POCO lub POCOs). Klasy POCO można pisać ręcznie lub można je generować automatycznie na podstawie istniejącego modelu danych przy użyciu szablonów zestawu narzędzi do przekształcania szablonów tekstowych (T4) udostępnianych przez Entity Framework.

Aby uzyskać więcej informacji na temat używania POCOs w Entity Framework, zobacz następujące zasoby:

- [Praca z jednostkami poco](https://msdn.microsoft.com/library/dd456853.aspx). Jest to dokument MSDN zawierający omówienie POCOs z linkami do innych dokumentów, które zawierają bardziej szczegółowe informacje.
- [Przewodnik: poco szablonu dla Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) To jest wpis w blogu od zespołu deweloperów Entity Framework z linkami do innych wpisów w blogu dotyczących usługi POCOs.

## <a name="code-first-development"></a>Programowanie po pierwszym kodzie

Obsługa POCO w Entity Framework 4 nadal wymaga utworzenia modelu danych i połączenia klas jednostek z modelem danych. Kolejna wersja Entity Framework będzie zawierać funkcję o nazwie *programowanie po pierwszym kodzie*. Ta funkcja umożliwia używanie Entity Framework z własnymi klasami POCO bez konieczności używania projektanta modelu danych ani pliku XML modelu danych. (W związku z tym ta opcja została również wywołana jako *tylko kod*; tylko *kod* i *kod —* odwołują się tylko do tej samej funkcji Entity Framework.)

Aby uzyskać więcej informacji o korzystaniu z metody tworzenia kodu po raz pierwszy, zobacz następujące zasoby:

- [Programowanie po raz pierwszy przy użyciu Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). To jest wpis w blogu, który Scott Guthrie wprowadza kod w pierwszej kolejności.
- [Blog zespołu deweloperów Entity Framework — Wpisy otagowane CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Blog zespołu deweloperów Entity Framework — Wpisy otagowane Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Samouczek dotyczący sklepu z muzyką MVC — część 4: modele i dostęp do danych](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Wprowadzenie z MVC 3-część 4: Entity Framework kod — pierwsze programowanie](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Oprócz tego nowy samouczek kodu MVC, który kompiluje aplikację podobną do aplikacji firmy Contoso University, jest przewidywany do opublikowania na wiosną 2011 o [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Więcej informacji

W tym celu zawarto informacje na temat Nowości w Entity Framework i kontynuowania pracy z serią samouczków Entity Framework. Aby uzyskać więcej informacji na temat nowych funkcji w Entity Framework 4, które nie zostały omówione w tym miejscu, zobacz następujące zasoby:

- [Co nowego w programie ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) Temat MSDN dotyczący nowych funkcji w wersji 4 Entity Framework.
- [Opublikowanie wersji Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) Wpis w blogu zespołu deweloperów Entity Framework dotyczący nowych funkcji w wersji 4.

> [!div class="step-by-step"]
> [Wstecz](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
