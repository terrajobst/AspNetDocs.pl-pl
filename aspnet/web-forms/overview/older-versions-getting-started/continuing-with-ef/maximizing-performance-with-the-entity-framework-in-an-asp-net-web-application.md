---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maksymalizacja wydajności za pomocą Entity Framework 4,0 w aplikacji sieci Web ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5630200a1ad1d30f6d89b38e15179f15b699fa9f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545963"
---
# <a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maksymalizowanie wydajności z Entity Framework 4,0 w aplikacji sieci Web ASP.NET 4

Autor [Dykstra](https://github.com/tdykstra)

> Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków [Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) . Jeśli wcześniej samouczki nie zostały wykonane, jako punkt wyjścia dla tego samouczka możesz [pobrać utworzoną aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) utworzoną przez kompletną serię samouczków. Jeśli masz pytania dotyczące samouczków, możesz je ogłosić na [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).

W poprzednim samouczku przedstawiono sposób obsługi konfliktów współbieżności. W tym samouczku przedstawiono opcje poprawy wydajności aplikacji sieci Web ASP.NET, która używa Entity Framework. Poznasz kilka metod maksymalizowania wydajności lub diagnozowania problemów z wydajnością.

Informacje przedstawione w poniższych sekcjach mogą być przydatne w wielu różnych scenariuszach:

- Efektywnie Załaduj powiązane dane.
- Zarządzanie stanem widoku.

Informacje przedstawione w poniższych sekcjach mogą być przydatne, jeśli masz pojedyncze zapytania, które przedstawiają problemy z wydajnością:

- Użyj opcji scalania `NoTracking`.
- Wstępnie Kompiluj zapytania LINQ.
- Zbadaj polecenia zapytania wysyłane do bazy danych.

Informacje przedstawione w poniższej sekcji są potencjalnie przydatne w przypadku aplikacji, które mają bardzo duże modele danych:

- Wstępnie Generuj widoki.

> [!NOTE]
> Wydajność aplikacji sieci Web ma wpływ wiele czynników, takich jak rozmiar danych żądań i odpowiedzi, szybkość zapytań bazy danych, liczba żądań, które serwer może umieścić w kolejce i jak szybko może je obsłużyć, a nawet ich wydajność używane biblioteki skryptów klienta. Jeśli wydajność ma krytyczne znaczenie dla aplikacji lub jeśli test lub środowisko pokazuje, że wydajność aplikacji nie jest zadowalająca, należy postępować zgodnie z normalnym protokołem dostrajania wydajności. Mierz, aby określić, gdzie występują wąskie gardła wydajności, a następnie zaadresować obszary, które będą miały największy wpływ na ogólną wydajność aplikacji.
> 
> Ten temat koncentruje się głównie na sposobach, w których można poprawić wydajność w szczególności Entity Framework w ASP.NET. Sugestie w tym miejscu są przydatne w przypadku określenia, że dostęp do danych jest jednym z wąskich gardeł wydajności aplikacji. Bez względu na to, że metody wyjaśnione w tym miejscu nie powinny być uważane za &quot;najlepszych rozwiązań&quot; ogólnie — wiele z nich jest odpowiednie tylko w wyjątkowych sytuacjach lub w celu rozwiązania bardzo konkretnych rodzajów wąskich gardeł wydajności.

Aby rozpocząć pracę z samouczkiem, uruchom program Visual Studio i Otwórz aplikację sieci Web Contoso University, z którą pracowałeś w poprzednim samouczku.

## <a name="efficiently-loading-related-data"></a>Wydajne ładowanie powiązanych danych

Istnieje kilka Entity Framework sposobów załadowania powiązanych danych do właściwości nawigacji jednostki:

- *Ładowanie z opóźnieniem*. Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane. Jednak przy pierwszej próbie uzyskania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie. Powoduje to, że wiele zapytań jest wysyłanych do bazy danych — jeden dla samej jednostki i po każdym każdej chwili, gdy powiązane dane dla jednostki muszą zostać pobrane. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Ładowanie eager*. Po odczytaniu jednostki są pobierane powiązane dane. Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne. Należy określić eager ładowania przy użyciu metody `Include`, ponieważ zaobserwowano już w tych samouczkach.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Jawne ładowanie*. Jest to podobne do ładowania z opóźnieniem, z tą różnicą, że jawnie pobierasz powiązane dane w kodzie; nie odbywa się to automatycznie podczas uzyskiwania dostępu do właściwości nawigacji. Powiązane dane można ładować ręcznie przy użyciu metody `Load` właściwości nawigacji dla kolekcji lub użyć metody `Load` właściwości Reference dla właściwości, które przechowują pojedynczy obiekt. (Na przykład wywoływanie metody `PersonReference.Load` w celu załadowania właściwości nawigacji `Person` jednostki `Department`).

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Ponieważ nie pobierają one natychmiast wartości właściwości, ładowanie z opóźnieniem i jawne ładowanie są również nazywane *opóźnionym ładowaniem*.

Ładowanie z opóźnieniem jest domyślnym zachowaniem dla kontekstu obiektu, który został wygenerowany przez projektanta. Jeśli otworzysz plik *SchoolModel.Designer.cs* , który definiuje klasę kontekstu obiektów, znajdziesz trzy metody konstruktora, a każda z nich zawiera następującą instrukcję:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Ogólnie rzecz biorąc, Jeśli wiesz, że potrzebne są powiązane dane dla każdej pobranej jednostki, ładowanie eager oferuje najlepszą wydajność, ponieważ pojedyncze zapytanie wysyłane do bazy danych jest zwykle bardziej wydajne niż osobne zapytania dla każdej pobranej jednostki. Z drugiej strony, jeśli chcesz uzyskać dostęp do właściwości nawigacji jednostki tylko rzadko lub tylko dla małego zestawu jednostek, ładowanie z opóźnieniem lub jawne ładowanie może być bardziej wydajne, ponieważ ładowanie eager spowodowałoby pobranie większej ilości danych niż jest to potrzebne.

W aplikacji sieci Web ładowanie z opóźnieniem może mieć stosunkowo małą wartość, ponieważ akcje użytkownika, które wpływają na potrzebę pokrewnych danych, są wykonywane w przeglądarce, która nie ma połączenia z kontekstem obiektu, który renderuje stronę. Z drugiej strony, gdy tworzysz powiązania kontrolki, zazwyczaj wiesz, jakie dane są potrzebne, i dlatego najlepiej jest wybrać eager ładowania lub odroczonego ładowania na podstawie tego, co jest odpowiednie w każdym scenariuszu.

Ponadto formant powiązany z danymi może używać obiektu jednostki po usunięciu kontekstu obiektu. W takim przypadku próba załadowania właściwości nawigacji nie powiedzie się. Wyświetlany komunikat o błędzie jest wyczyszczony: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

Formant `EntityDataSource` domyślnie wyłącza ładowanie z opóźnieniem. W przypadku kontrolki `ObjectDataSource` używanej w bieżącym samouczku (lub w przypadku uzyskania dostępu do kontekstu obiektów z kodu strony) istnieje kilka sposobów, aby ładowanie z opóźnieniem było domyślnie wyłączone. Można ją wyłączyć podczas tworzenia wystąpienia kontekstu obiektu. Na przykład można dodać następujący wiersz do metody konstruktora klasy `SchoolRepository`:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

W przypadku aplikacji firmy Contoso University, kontekst obiektu automatycznie wyłącza ładowanie z opóźnieniem, dzięki czemu ta właściwość nie będzie musiała być ustawiana za każdym razem, gdy zostanie utworzone wystąpienie kontekstu.

Otwórz model danych *SchoolModel. edmx* , kliknij powierzchnię projektową, a następnie w okienku właściwości ustaw właściwość **włączony ładowanie z opóźnieniem** na `False`. Zapisz i Zamknij model danych.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Zarządzanie stanem widoku

W celu zapewnienia funkcjonalności aktualizacji Strona sieci Web ASP.NET musi przechowywać oryginalne wartości właściwości jednostki, gdy strona jest renderowana. Podczas przetwarzania ogłaszania zwrotnego, formant może ponownie utworzyć oryginalny stan jednostki i wywołać metodę `Attach` jednostki przed zastosowaniem zmian i wywołaniem metody `SaveChanges`. Domyślnie formanty danych formularzy sieci Web ASP.NET używają stanu widoku do przechowywania oryginalnych wartości. Jednak stan widoku może mieć wpływ na wydajność, ponieważ jest przechowywany w ukrytych polach, które mogą znacznie zwiększyć rozmiar strony wysyłanej do i z przeglądarki.

Techniki zarządzania stanem widoku lub alternatywami, takimi jak stan sesji, nie są unikatowe dla Entity Framework, więc ten samouczek nie znajduje się w tym temacie szczegółowo. Aby uzyskać więcej informacji, zobacz linki na końcu samouczka.

Jednak wersja 4 programu ASP.NET zapewnia nowy sposób pracy ze stanem widoku, który powinien mieć świadomość każdy ASP.NET dewelopera aplikacji formularzy sieci Web: Właściwość `ViewStateMode`. Ta nowa właściwość może być ustawiona na poziomie strony lub kontrolki i umożliwia wyłączenie opcji wyświetlania stanu domyślnie dla strony i włączenie jej tylko dla formantów, które ich potrzebują.

W przypadku aplikacji, w których wydajność jest krytyczna, dobrym sposobem jest zawsze wyłączenie stanu widoku na poziomie strony i włączenie go tylko dla formantów, które go wymagają. Nie można znacząco obniżyć rozmiaru stanu widoku na stronach uniwersytetów firmy Contoso przy użyciu tej metody, ale aby zobaczyć, jak to działa, należy to zrobić na stronie *Instruktorzy. aspx* . Ta strona zawiera wiele kontrolek, w tym kontrolkę `Label`, która ma wyłączony stan widoku. Żadna z formantów na tej stronie nie musi mieć włączonego stanu widoku. (Właściwość `DataKeyNames` kontrolki `GridView` określa stan, który musi być utrzymywany między ogłaszaniem zwrotnym, ale te wartości są przechowywane w stanie kontroli, który nie ma wpływ na Właściwość `ViewStateMode`).

Dyrektywa `Page` i znaczniki kontroli `Label` obecnie przypominają następujący przykład:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Wprowadź następujące zmiany:

- Dodaj `ViewStateMode="Disabled"` do dyrektywy `Page`.
- Usuń `ViewStateMode="Disabled"` z kontrolki `Label`.

Znaczniki są teraz podobne do następujących:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Stan widoku jest teraz wyłączony dla wszystkich kontrolek. Jeśli później dodasz kontrolkę, która musi korzystać z stanu widoku, wystarczy uwzględnić atrybut `ViewStateMode="Enabled"` dla tej kontrolki.

## <a name="using-the-notracking-merge-option"></a>Korzystanie z opcji scalania NoTracking

Gdy kontekst obiektu pobiera wiersze bazy danych i tworzy obiekty jednostek, które reprezentują je, domyślnie śledzi te obiekty jednostki za pomocą menedżera stanu obiektów. Te dane śledzenia pełnią rolę pamięci podręcznej i są używane podczas aktualizowania jednostki. Ponieważ aplikacja sieci Web zwykle ma wystąpienia kontekstu obiektów krótkoterminowych, zapytania często zwracają dane, które nie muszą być śledzone, ponieważ kontekst obiektu, który odczytuje je, zostanie usunięty przed ponownym użyciem lub zaktualizowaniem.

W Entity Framework można określić, czy kontekst obiektu śledzi obiekty jednostek przez ustawienie *opcji scalania*. Można ustawić opcję scalania dla poszczególnych zapytań lub zestawów jednostek. Jeśli ustawisz go dla zestawu jednostek, oznacza to, że jest ustawiana opcja scalania domyślnego dla wszystkich zapytań, które są tworzone dla danego zestawu jednostek.

W przypadku aplikacji firmy Contoso University śledzenie nie jest konieczne dla żadnego z zestawów jednostek, do których uzyskuje się dostęp z repozytorium, więc możesz ustawić opcję scalania, aby `NoTracking` te zestawy jednostek podczas tworzenia wystąpienia kontekstu obiektu w klasie repozytorium. (Należy zauważyć, że w tym samouczku ustawienie opcji scalania nie będzie miało zauważalnego wpływu na wydajność aplikacji. Opcja `NoTracking` może spowodować zauważalne zwiększenie wydajności tylko w niektórych scenariuszach z dużą ilością danych.

W folderze DAL Otwórz plik *SchoolRepository.cs* i Dodaj metodę konstruktora, która ustawia opcję scalania dla zestawów jednostek, do których oduzyskuje się dostęp do repozytorium:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Wstępne Kompilowanie zapytań LINQ

Gdy Entity Framework wykonuje zapytanie Entity SQL w trakcie okresu istnienia danego wystąpienia `ObjectContext`, trwa jakiś czas na skompilowanie zapytania. Wynik kompilacji jest buforowany, co oznacza, że kolejne wykonania zapytania są znacznie szybsze. Zapytania LINQ obserwują podobny wzorzec, z tą różnicą, że niektóre prace wymagane do skompilowania zapytania są wykonywane za każdym razem, gdy zapytanie jest wykonywane. Innymi słowy, w przypadku zapytań LINQ domyślnie nie wszystkie wyniki kompilacji są buforowane.

Jeśli masz zapytanie LINQ, które ma być uruchamiane wielokrotnie w okresie istnienia kontekstu obiektu, możesz napisać kod, który powoduje, że wszystkie wyniki kompilacji mają być buforowane przy pierwszym uruchomieniu zapytania LINQ.

Jako ilustracja, należy to zrobić dla dwóch metod `Get` w klasie `SchoolRepository`, z których jeden nie przyjmuje żadnych parametrów (Metoda `GetInstructorNames`) i że wymaga parametru (Metoda `GetDepartmentsByAdministrator`). Te metody stają się teraz niepotrzebne do skompilowania, ponieważ nie są to zapytania LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Jednak dzięki temu można wypróbować skompilowane zapytania, tak jakby były zapisywane jako następujące zapytania LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Możesz zmienić kod w tych metodach, tak jak pokazano powyżej, i uruchomić aplikację, aby sprawdzić, czy działa przed kontynuowaniem. Ale poniższe instrukcje przeprowadzą bezpośrednio do tworzenia wstępnie skompilowanych wersji.

Utwórz plik klasy w folderze *dal* , nadaj mu nazwę *SchoolEntities.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Ten kod tworzy klasę częściową, która rozszerza automatycznie wygenerowaną klasę kontekstu obiektu. Klasa częściowa obejmuje dwie skompilowane zapytania LINQ przy użyciu metody `Compile` klasy `CompiledQuery`. Tworzy również metody, których można użyć do wywołania zapytań. Zapisz i Zamknij ten plik.

Następnie w *SchoolRepository.cs*zmień istniejące `GetInstructorNames` i `GetDepartmentsByAdministrator` metod w klasie repozytorium, aby wywoływać skompilowane zapytania:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Uruchom stronę *działS. aspx* , aby upewnić się, że działa tak jak wcześniej. Metoda `GetInstructorNames` jest wywoływana w celu wypełnienia listy rozwijanej administratora, a metoda `GetDepartmentsByAdministrator` jest wywoływana po kliknięciu przycisku **Aktualizuj** w celu sprawdzenia, czy żaden instruktor nie jest administratorem więcej niż jednego działu.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Wstępnie skompilowano zapytania w aplikacji uniwersytetów firmy Contoso, aby zobaczyć, jak to zrobić, nie ponieważ mogłoby to mierzyć zwiększenie wydajności. Wstępne Kompilowanie zapytań LINQ dodaje poziom złożoności kodu, dlatego należy się upewnić, że jest to możliwe tylko dla zapytań, które reprezentują wąskie gardła wydajności w aplikacji.

## <a name="examining-queries-sent-to-the-database"></a>Badanie zapytań wysyłanych do bazy danych

Podczas badania problemów z wydajnością czasami warto poznać dokładne polecenia SQL, które Entity Framework wysyła do bazy danych. Jeśli pracujesz z obiektem `IQueryable`, jednym ze sposobów jest użycie metody `ToTraceString`.

W *SchoolRepository.cs*Zmień kod w metodzie `GetDepartmentsByName`, aby pasował do poniższego przykładu:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

Zmienna `departments` musi być rzutowana na typ `ObjectQuery` tylko ponieważ metoda `Where` na końcu poprzedniego wiersza tworzy obiekt `IQueryable`; bez metody `Where` rzutowanie nie będzie konieczne.

Ustaw punkt przerwania w wierszu `return`, a następnie Uruchom stronę *działS. aspx* w debugerze. Po osiągnięciu punktu przerwania, należy przeanalizować zmienną `commandText` w oknie **zmiennych lokalnych** i użyć wizualizatora tekstu (Lupa w kolumnie **wartość** ), aby wyświetlić jej wartość w oknie **wizualizator tekstu** . Można zobaczyć całe polecenie SQL, które wynika z tego kodu:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Alternatywnie funkcja IntelliTrace w Visual Studio Ultimate zapewnia sposób wyświetlania poleceń SQL generowanych przez Entity Framework, które nie wymagają zmiany kodu lub nawet ustawienia punktu przerwania.

> [!NOTE]
> Poniższe procedury można wykonać tylko wtedy, gdy masz Visual Studio Ultimate.

Przywróć oryginalny kod w metodzie `GetDepartmentsByName`, a następnie Uruchom stronę *działS. aspx* w debugerze.

W programie Visual Studio wybierz menu **Debuguj** , a następnie **IntelliTrace**, a następnie **IntelliTrace zdarzenia**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

W oknie **IntelliTrace** kliknij pozycję **Przerwij wszystko**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

W oknie **IntelliTrace** zostanie wyświetlona lista ostatnich zdarzeń:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Kliknij linię **ADO.NET** . Zostanie rozwinięte, aby wyświetlić tekst polecenia:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Cały ciąg tekstowy polecenia można skopiować do schowka z okna **zmiennych lokalnych** .

Załóżmy, że pracujesz z bazą danych z większą liczbą tabel, relacji i kolumn niż prosta baza danych `School`. Może się okazać, że zapytanie, które zbiera wszystkie potrzebne informacje w jednej instrukcji `Select` zawierającej wiele klauzul `Join`, jest zbyt skomplikowane, aby działała wydajnie. W takim przypadku można przełączyć się z ładowania eager na jawne ładowanie, aby uprościć zapytanie.

Na przykład spróbuj zmienić kod w metodzie `GetDepartmentsByName` w *SchoolRepository.cs*. Obecnie w tej metodzie masz zapytanie obiektu, które ma `Include` metody dla właściwości nawigacji `Person` i `Courses`. Zastąp instrukcję `return` kodem, który wykonuje jawne ładowanie, jak pokazano w następującym przykładzie:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Uruchom stronę *działS. aspx* w debugerze i ponownie Sprawdź okno **IntelliTrace** , tak jak wcześniej. Teraz, gdy wcześniej było jedno zapytanie, zobaczysz długą sekwencję.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Kliknij pierwszy wiersz **ADO.NET** , aby zobaczyć, co się stało z zapytaniem złożonym wcześniej.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

Zapytanie z działów stało się prostą `Select`ą kwerendą bez klauzuli `Join`, ale następuje oddzielne zapytania, które pobierają powiązane kursy i administratorów, przy użyciu zestawu dwóch zapytań dla każdego działu zwróconego przez oryginalne zapytanie.

> [!NOTE]
> W przypadku pozostawienia włączonego ładowania z opóźnieniem wzorzec widoczny w tym miejscu, z tym samym zapytaniem wielokrotnie, może wynikać z załadowania z opóźnieniem. Wzorzec, który zazwyczaj chcesz uniknąć, jest ładowaniem powiązanym z opóźnieniem dla każdego wiersza tabeli podstawowej. O ile nie sprawdzono, że pojedyncze zapytanie sprzężenia jest zbyt złożone, aby było efektywne, zazwyczaj można poprawić wydajność w takich przypadkach, zmieniając zapytanie podstawowe w celu użycia ładowania eager.

## <a name="pre-generating-views"></a>Wstępnie generowane widoki

Gdy obiekt `ObjectContext` jest najpierw tworzony w nowej domenie aplikacji, Entity Framework generuje zestaw klas, które są używane w celu uzyskania dostępu do bazy danych. Klasy te są nazywane *widokami*, a jeśli masz bardzo duży model danych, generowanie tych widoków może opóźnić odpowiedź witryny sieci Web na pierwsze żądanie dla strony po zainicjowaniu nowej domeny aplikacji. Możesz zmniejszyć to opóźnienie pierwszego żądania, tworząc widoki w czasie kompilacji, a nie w czasie wykonywania.

> [!NOTE]
> Jeśli aplikacja nie ma bardzo dużego modelu danych lub jeśli ma duży model danych, ale nie ma żadnych problemów z wydajnością, które dotyczą tylko pierwszego żądania strony po odtworzeniu usług IIS, można pominąć tę sekcję. Tworzenie widoku nie odbywa się za każdym razem, gdy utworzysz wystąpienie `ObjectContext` obiektu, ponieważ widoki są buforowane w domenie aplikacji. W związku z tym, o ile często nie odtwarzasz aplikacji w usługach IIS, bardzo kilka żądań stron będzie korzystać z wstępnie wygenerowanych widoków.

Można wstępnie generować widoki przy użyciu narzędzia wiersza polecenia *EdmGen. exe* lub szablonu *zestawu narzędzi do przekształcania szablonu tekstu* (T4). W tym samouczku zostanie użyty szablon T4.

W folderze *dal* Dodaj plik za pomocą szablonu **szablonu tekstu** (znajduje się w węźle **Ogólne** na liście **zainstalowane szablony** ) i nadaj mu nazwę *SchoolModel.views.tt*. Zastąp istniejący kod w pliku następującym kodem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Ten kod generuje widoki dla pliku *. edmx* znajdującego się w tym samym folderze co szablon, który ma taką samą nazwę jak plik szablonu. Na przykład jeśli plik szablonu ma nazwę *SchoolModel.views.tt*, zostanie wyszukany plik modelu danych o nazwie *SchoolModel. edmx*.

Zapisz plik, a następnie kliknij prawym przyciskiem myszy plik w **Eksplorator rozwiązań** i wybierz polecenie **Uruchom narzędzie niestandardowe**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Program Visual Studio generuje plik kodu, który tworzy widoki o nazwie *SchoolModel.views.cs* na podstawie szablonu. (Być może zauważono, że plik kodu jest generowany nawet przed wybraniem opcji **Uruchom narzędzie niestandardowe**, gdy tylko zapiszesz plik szablonu).

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Teraz można uruchomić aplikację i sprawdzić, czy działa ona tak jak wcześniej.

Aby uzyskać więcej informacji dotyczących wstępnie wygenerowanych widoków, zobacz następujące zasoby:

- [Instrukcje: wstępne Generowanie widoków w celu zwiększenia wydajności zapytań](https://msdn.microsoft.com/library/bb896240.aspx) w witrynie MSDN w sieci Web. Wyjaśnia, jak za pomocą narzędzia wiersza polecenia `EdmGen.exe` wstępnie generować widoki.
- [Izolowanie wydajności ze wstępnie skompilowanymi/wygenerowanymi widokami w Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) w blogu zespołu ds. klientów systemu Windows Server AppFabric.

To uzupełnia wprowadzenie do poprawy wydajności w aplikacji sieci Web ASP.NET, która używa Entity Framework. Więcej informacji można znaleźć w następujących zasobach:

- [Zagadnienia dotyczące wydajności (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) w witrynie MSDN w sieci Web.
- [Wpisy dotyczące wydajności w blogu zespołu Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [Opcje scalania EF i skompilowane zapytania](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Wpis w blogu, w którym objaśniono nieoczekiwane zachowania skompilowanych zapytań i opcji scalania, takich jak `NoTracking`. Jeśli planujesz używać skompilowanych zapytań lub manipulujesz ustawieniami opcji scalania w aplikacji, przeczytaj ją jako pierwsze.
- [Wpisy dotyczące Entity Framework w blogu zespołu ds. danych i modelowania klientów](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Zawiera wpisy dotyczące skompilowanych zapytań i programu Visual Studio 2010 Profiler do odnajdywania problemów z wydajnością.
- [Entity Framework wątek forum z zaleceniami dotyczącymi poprawy wydajności wysoce złożonych zapytań](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Zalecenia dotyczące zarządzania stanem ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Używanie Entity Framework i elementu ObjectDataSource: niestandardowe stronicowanie](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Wpis w blogu, który kompiluje się w aplikacji ContosoUniversity utworzonej w tych samouczkach, aby wyjaśnić, jak zaimplementować stronicowanie na stronie *działS. aspx* .

W następnym samouczku przedstawiono kilka ważnych ulepszeń Entity Framework, które są nowe w wersji 4.

> [!div class="step-by-step"]
> [Poprzednie](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [dalej](what-s-new-in-the-entity-framework-4.md)
