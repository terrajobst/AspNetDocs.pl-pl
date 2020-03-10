---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: używanie odporności połączeń i przechwycenia poleceń z EF w aplikacji ASP.NET MVC'
author: tdykstra
description: W tym samouczku dowiesz się, jak korzystać z odporności połączeń i przechwycenia poleceń. Są to dwie ważne funkcje Entity Framework 6.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 276266f8ae9df38529d44742ebe6ac0dc8e79727
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583455"
---
# <a name="tutorial-use-connection-resiliency-and-command-interception-with-entity-framework-in-an-aspnet-mvc-app"></a>Samouczek: korzystanie z odporności połączeń i przechwycenia poleceń za pomocą Entity Framework w aplikacji ASP.NET MVC

Do tej pory aplikacja została uruchomiona lokalnie w IIS Express na komputerze deweloperskim. Aby udostępnić rzeczywistą aplikację innym osobom korzystającym z Internetu, należy wdrożyć ją w dostawcy hostingu w sieci Web i wdrożyć bazę danych na serwerze bazy danych.

W tym samouczku dowiesz się, jak korzystać z odporności połączeń i przechwycenia poleceń. Są to dwie ważne funkcje Entity Framework 6, które są szczególnie przydatne podczas wdrażania w środowisku chmury: odporność połączeń (automatyczne ponawianie prób w przypadku błędów przejściowych) i przechwycenie polecenia (przechwyć wszystkie zapytania SQL wysłane do bazy danych w celu ich zarejestrowania lub zmiany).

Ten samouczek połączenia i przechwycenie polecenia są opcjonalne. W przypadku pominięcia tego samouczka konieczne będzie wprowadzenie kilku drobnych zmian w kolejnych samouczkach.

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Włącz odporność połączenia
> * Włącz przechwycenie polecenia
> * Przetestuj nową konfigurację

## <a name="prerequisites"></a>Wymagania wstępne

* [Sortowanie, filtrowanie i stronicowanie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-connection-resiliency"></a>Włącz odporność połączenia

Po wdrożeniu aplikacji na platformie Microsoft Azure zostanie wdrożona baza danych do systemu Windows Azure SQL Database, usługi bazy danych w chmurze. Błędy połączeń przejściowych są zwykle częstsze w przypadku łączenia się z usługą bazy danych w chmurze, niż w przypadku bezpośredniego połączenia serwera sieci Web i serwera bazy danych w tym samym centrum danych. Nawet jeśli serwer sieci Web w chmurze i usługa bazy danych w chmurze są hostowane w tym samym centrum danych, istnieje więcej połączeń sieciowych między nimi, które mogą mieć problemy, takie jak moduły równoważenia obciążenia.

Ponadto usługa w chmurze jest zwykle udostępniana innym użytkownikom, co oznacza, że może to mieć wpływ na jego czas odpowiedzi. A dostęp do bazy danych może podlegać ograniczaniu wydajności. Ograniczenie przepustowości oznacza, że usługa bazy danych zgłasza wyjątki, gdy próbujesz uzyskać do niej dostęp częściej niż jest to dozwolone w Umowa dotycząca poziomu usług (SLA).

Wiele lub większość problemów z połączeniem w przypadku uzyskiwania dostępu do usługi w chmurze jest przejściowa, czyli w krótkim czasie. Dlatego podczas próby wykonania operacji bazy danych i uzyskania typu błędu, który zwykle jest przejściowy, można spróbować ponownie wykonać operację po krótkim czasie oczekiwania, a operacja może zakończyć się pomyślnie. W przypadku obsługi błędów przejściowych można zapewnić użytkownikom wiele lepszych możliwości, co powoduje, że większość z nich jest niewidoczna dla klienta. Funkcja odporności połączeń w Entity Framework 6 automatyzuje proces ponawiania nieudanych zapytań SQL.

Funkcja odporności połączenia musi być odpowiednio skonfigurowana dla określonej usługi bazy danych:

- Należy wiedzieć, które wyjątki mogą być przejściowe. Na przykład chcesz ponowić próbę błędów spowodowanych przez tymczasową utratę łączności sieciowej, a nie błędy spowodowane przez błędy programu.
- Musi upłynąć odpowiednią ilość czasu między ponownymi próbami nieudanej operacji. Możesz poczekać dłużej między ponownymi próbami procesu wsadowego niż w przypadku strony sieci Web w trybie online, w której użytkownik czeka na odpowiedź.
- Należy ponowić próbę odpowiednią liczbę razy, zanim zostanie ona nadana. Można ponowić więcej razy w procesie wsadowym, który będzie w aplikacji online.

Te ustawienia można skonfigurować ręcznie dla dowolnego środowiska bazy danych obsługiwanego przez dostawcę Entity Framework, ale wartości domyślne, które zwykle dobrze działają dla aplikacji online, która używa Azure SQL Database systemu Windows, zostały już skonfigurowane dla Ciebie i są to ustawienia, które należy zaimplementować dla aplikacji firmy Contoso University.

Wszystkie czynności, które należy wykonać w celu włączenia odporności połączenia, to utworzenie klasy w zestawie, która pochodzi od klasy [dbconfiguration](https://msdn.microsoft.com/data/jj680699.aspx) , a w tej klasie ustawia *strategię wykonywania*SQL Database, która w EF jest innym terminem dla *zasad ponawiania prób*.

1. W folderze DAL Dodaj plik klasy o nazwie *SchoolConfiguration.cs*.
2. Zastąp kod szablonu poniższym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework automatycznie uruchamia kod znaleziony w klasie, która pochodzi od `DbConfiguration`. Klasy `DbConfiguration` można użyć do wykonywania zadań konfiguracyjnych w kodzie, który można w przeciwnym razie wykonać w pliku *Web. config* . Aby uzyskać więcej informacji, zobacz temat [Konfiguracja oparta na kodzie EntityFramework](https://msdn.microsoft.com/data/jj680699).
3. W *StudentController.cs*dodaj instrukcję `using` dla `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Zmień wszystkie bloki `catch`, które przechwytuje `DataException` wyjątki, aby przechwycić `RetryLimitExceededException` wyjątków. Na przykład:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Używasz `DataException`, aby spróbować zidentyfikować błędy, które mogą być przejściowe w celu uzyskania przyjaznego komunikatu "Spróbuj ponownie". Jednak po włączeniu zasad ponawiania jedynymi błędami, które prawdopodobnie będą przejściowe, zostaną już wypróbowane i nie powiodło się kilka razy, a rzeczywisty zwrócony wyjątek zostanie opakowany w wyjątek `RetryLimitExceededException`.

Aby uzyskać więcej informacji, zobacz [Entity Framework elastyczność połączenia/logika ponowienia próby](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Włącz przechwycenie polecenia

Po włączeniu zasad ponawiania, jak przeprowadzić test, aby sprawdzić, czy działa zgodnie z oczekiwaniami? Nie jest tak łatwo, aby wymusić wystąpienie błędu przejściowego, szczególnie w przypadku uruchamiania lokalnego, a w szczególności trudno jest zintegrować rzeczywiste błędy przejściowe w zautomatyzowany test jednostkowy. Aby przetestować funkcję odporności połączeń, trzeba przechwycić zapytania wysyłane Entity Framework do SQL Server i zamienić SQL Server odpowiedź na typ wyjątku, który jest zazwyczaj przejściowy.

Można również użyć przechwycenia zapytania, aby zaimplementować najlepsze rozwiązanie dla aplikacji w chmurze: [rejestrować opóźnienia oraz sukcesy i niepowodzenia wszystkich wywołań usług zewnętrznych](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) , takich jak usługi bazy danych. EF6 udostępnia [dedykowany interfejs API rejestrowania](https://msdn.microsoft.com/data/dn469464) , który może ułatwić rejestrowanie, ale w tej części samouczka dowiesz się, jak używać [funkcji przechwycenia](https://msdn.microsoft.com/data/dn469464) Entity Framework, zarówno w przypadku rejestrowania, jak i symulowania błędów przejściowych.

### <a name="create-a-logging-interface-and-class"></a>Tworzenie interfejsu rejestrowania i klasy

[Najlepszym rozwiązaniem w przypadku rejestrowania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) jest korzystanie z interfejsu, a nie kodowania twardego do klasy System. Diagnostics. Trace lub Klasa rejestrowania. Dzięki temu można łatwiej zmienić mechanizm rejestrowania później, jeśli kiedykolwiek trzeba to zrobić. W tej sekcji utworzysz interfejs rejestrowania i klasę, aby zaimplementować ją./p >

1. Utwórz folder w projekcie i nadaj mu nazwę *Rejestrowanie*.
2. W folderze *Rejestrowanie* Utwórz plik klasy o nazwie *ILogger.cs*i Zastąp kod szablonu następującym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Interfejs udostępnia trzy poziomy śledzenia, aby wskazać względną ważność dzienników i został zaprojektowany z myślą o dostarczaniu informacji o opóźnieniu dla wywołań usług zewnętrznych, takich jak zapytania bazy danych. Metody rejestrowania mają przeciążenia umożliwiające przekazanie wyjątku. Jest tak dlatego, że informacje o wyjątkach, w tym ślady stosu i wyjątki wewnętrzne, są niezawodnie rejestrowane przez klasę, która implementuje interfejs, zamiast polegać na tym, że są wykonywane w ramach każdego wywołania metody rejestrowania w całej aplikacji.

    Metody TraceApi umożliwiają śledzenie opóźnień każdego wywołania usługi zewnętrznej, takiej jak SQL Database.
3. W folderze *Rejestrowanie* Utwórz plik klasy o nazwie *Logger.cs*i Zastąp kod szablonu następującym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Implementacja używa programu System. Diagnostics do wykonania śledzenia. Jest to wbudowana funkcja programu .NET, która ułatwia generowanie i używanie informacji śledzenia. Istnieje wiele "odbiorników", których można używać z funkcją śledzenia system. Diagnostics, do zapisywania dzienników do plików, na przykład, lub do zapisywania ich w usłudze BLOB Storage na platformie Azure. Zobacz niektóre opcje i linki do innych zasobów, aby uzyskać więcej informacji na [temat rozwiązywania problemów z witrynami sieci Web systemu Azure w programie Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). W tym samouczku zaobserwujesz tylko dzienniki w oknie **danych wyjściowych** programu Visual Studio.

    W aplikacji produkcyjnej warto rozważyć użycie pakietów śledzenia innych niż system. Diagnostics, a interfejs ILogger umożliwia stosunkowo proste przełączenie się do innego mechanizmu śledzenia, jeśli zdecydujesz się to zrobić.

### <a name="create-interceptor-classes"></a>Utwórz klasy interceptorów

Następnie utworzysz klasy, do których będzie wywoływana Entity Framework za każdym razem, gdy wyśle zapytanie do bazy danych, jeden w celu symulowania błędów przejściowych i jednego do rejestrowania. Te klasy przechwyceń muszą pochodzić od klasy `DbCommandInterceptor`. W nich zastępują zastąpienia metod, które są automatycznie wywoływane, gdy zapytanie ma zostać wykonane. W tych metodach można badać lub rejestrować zapytania wysyłane do bazy danych, a następnie można zmienić zapytanie przed jego wysłaniem do bazy danych lub zwrócić coś do Entity Framework siebie bez przekazywania zapytania do bazy danych.

1. Aby utworzyć klasę interceptora, która będzie rejestrować wszystkie zapytania SQL, które są wysyłane do bazy danych, Utwórz plik klasy o nazwie *SchoolInterceptorLogging.cs* w folderze *dal* i Zastąp kod szablonu następującym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    W przypadku pomyślnych zapytań lub poleceń ten kod zapisuje dziennik informacyjny z informacjami o opóźnieniu. W przypadku wyjątków tworzy dziennik błędów.
2. Aby utworzyć klasę interceptora, która będzie generować fikcyjne błędy przejściowe po wprowadzeniu "throw" w polu **wyszukiwania** , Utwórz plik klasy o nazwie *SchoolInterceptorTransientErrors.cs* w folderze *dal* i Zastąp kod szablonu następującym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Ten kod zastępuje tylko metodę `ReaderExecuting`, która jest wywoływana dla zapytań, które mogą zwracać wiele wierszy danych. Jeśli chcesz sprawdzić odporność połączenia dla innych typów zapytań, możesz również zastąpić metody `NonQueryExecuting` i `ScalarExecuting`, ponieważ przechwycenie rejestrowania działa.

    Po uruchomieniu strony uczniów i wprowadzeniu "throw" jako ciągu wyszukiwania ten kod tworzy fikcyjny wyjątek SQL Database dla błędu o numerze 20, typ znany jako zwykle przejściowy. Inne numery błędów aktualnie rozpoznawane jako przejściowe to 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 i 40613, ale mogą ulec zmianie w nowych wersjach SQL Database.

    Kod zwraca wyjątek do Entity Framework zamiast uruchamiania zapytania i przekazywania wyników zapytania. Wyjątek przejściowy jest zwracany cztery razy, a następnie kod wraca do normalnej procedury przekazania zapytania do bazy danych.

    Ponieważ wszystko jest rejestrowane, będzie można zobaczyć, że Entity Framework próbuje wykonać zapytanie cztery razy przed zakończeniem, a jedyną różnicą w aplikacji jest to, że przerenderowanie strony z wynikami zapytania trwa dłużej.

    Liczba ponowień prób Entity Framework będzie można skonfigurować; kod jest określany cztery razy, ponieważ jest to wartość domyślna dla zasad wykonywania SQL Database. W przypadku zmiany zasad wykonywania należy również zmienić tutaj kod określający, ile razy są generowane błędy przejściowe. Można również zmienić kod w celu wygenerowania większej liczby wyjątków, aby Entity Framework zgłosi wyjątek `RetryLimitExceededException`.

    Wartość wprowadzona w polu wyszukiwania będzie w `command.Parameters[0]` i `command.Parameters[1]` (jedna zostanie użyta jako pierwsza nazwa i jeden dla nazwiska). Gdy zostanie znaleziona wartość "% throw%", "throw" jest zamieniany na te parametry przez "a", aby niektóre uczniowie znalazły i zwracały.

    Jest to wygodny sposób na przetestowanie odporności połączenia na podstawie zmiany danych wejściowych w interfejsie użytkownika aplikacji. Możesz również napisać kod generujący błędy przejściowe dla wszystkich zapytań lub aktualizacji, jak wyjaśniono w dalszej części komentarzy dotyczących metody *przechwycenie. Add* .
3. W *Global. asax*Dodaj następujące instrukcje `using`:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Dodaj wyróżnione wiersze do metody `Application_Start`:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Te wiersze kodu powodują, że kod interceptora ma być uruchamiany, gdy Entity Framework wysyła zapytania do bazy danych. Należy zauważyć, że ponieważ zostały utworzone oddzielne klasy interceptorów dla przejściowej symulacji błędów i rejestrowania, można je niezależnie włączać i wyłączać.

    Można dodać Interceptory przy użyciu metody `DbInterception.Add` w dowolnym miejscu w kodzie. nie musi on znajdować się w metodzie `Application_Start`. Innym rozwiązaniem jest umieszczenie tego kodu w klasie dbconfiguration utworzonej wcześniej w celu skonfigurowania zasad wykonywania.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Wszędzie tam, gdzie umieścisz ten kod, uważaj, aby nie wykonywać `DbInterception.Add` dla tego samego interceptora więcej niż raz, lub uzyskasz dodatkowe wystąpienia przechwyceń. Na przykład po dwukrotnym dodaniu przechwycenia rejestrowania dla każdego zapytania SQL zobaczysz dwa dzienniki.

    Interceptory są wykonywane w kolejności rejestracji (kolejność, w jakiej wywoływana jest metoda `DbInterception.Add`). Kolejność może mieć znaczenie w zależności od tego, co robisz w wychwyceniu. Na przykład Interceptor może zmienić polecenie SQL, które jest pobierane we właściwości `CommandText`. Jeśli zmieni polecenie SQL, następnym przechwyceniem będzie uzyskanie zmienionego polecenia SQL, a nie oryginalnego polecenia SQL.

    Kod symulacji błędu przejściowego został zapisany w sposób, który umożliwia spowodowanie błędów przejściowych przez wprowadzenie innej wartości w interfejsie użytkownika. Alternatywnie można napisać kod interceptora, aby zawsze generował sekwencję przejściowych wyjątków bez sprawdzania określonej wartości parametru. Następnie można dodać Interceptor tylko wtedy, gdy chcesz wygenerować błędy przejściowe. Jeśli to zrobisz, nie dodawaj interceptora do momentu zakończenia inicjowania bazy danych. Innymi słowy, należy wykonać co najmniej jedną operację bazy danych, taką jak zapytanie na jednym z zestawów jednostek przed rozpoczęciem generowania błędów przejściowych. Entity Framework wykonuje kilka zapytań podczas inicjowania bazy danych i nie są wykonywane w transakcji, więc błędy podczas inicjowania mogą spowodować, że kontekst przestanie być niespójny.

## <a name="test-the-new-configuration"></a>Przetestuj nową konfigurację

1. Naciśnij klawisz **F5** , aby uruchomić aplikację w trybie debugowania, a następnie kliknij kartę **uczniowie** .
2. Sprawdź okno **danych wyjściowych** programu Visual Studio, aby wyświetlić dane wyjściowe śledzenia. Może być konieczne przewinięcie w górę ostatnich błędów języka JavaScript w celu uzyskania dzienników pisanych przez rejestrator.

    Zwróć uwagę, że można zobaczyć rzeczywiste zapytania SQL wysyłane do bazy danych. Zobaczysz kilka początkowych zapytań i poleceń, które Entity Framework mają rozpocząć pracę, sprawdzając wersję bazy danych i tabelę historii migracji (dowiesz się więcej na temat migracji w następnym samouczku). Zobaczysz zapytanie dotyczące stronicowania, aby dowiedzieć się, ile uczniów istnieje, i wreszcie zobaczysz zapytanie, które pobiera dane ucznia.

    ![Rejestrowanie dla normalnego zapytania](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Na stronie **uczniów** wprowadź wartość "throw" jako ciąg wyszukiwania, a następnie kliknij przycisk **Wyszukaj**.

    ![Zgłoś ciąg wyszukiwania](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Zobaczysz, że przeglądarka wydaje się odwiesić przez kilka sekund, podczas gdy Entity Framework próbuje wykonać zapytanie kilka razy. Pierwsza ponowna próba przebiega bardzo szybko, a następnie przed każdą dodatkową ponowną próbą. Ten proces czekania dłużej przed ponowną próbą nazywa się *wykładniczą wycofywania*.

    Gdy zostanie wyświetlona strona, na której znajdują się imiona i nazwiska, Sprawdź okno dane wyjściowe i zobaczysz, że to samo zapytanie było podejmowane pięć razy, pierwsze cztery razy zwracające wyjątki przejściowe. Dla każdego błędu przejściowego zostanie wyświetlony dziennik zapisany podczas generowania błędu przejściowego w klasie `SchoolInterceptorTransientErrors` ("zwracanie błędu przejściowego dla polecenia...") i zobaczysz dziennik zapisany, gdy `SchoolInterceptorLogging` pobiera wyjątek.

    ![Dane wyjściowe rejestrowania pokazujące ponowne próby](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Ponieważ wprowadzono ciąg wyszukiwania, zapytanie zwracające dane ucznia jest sparametryzowane:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Nie rejestrujesz wartości parametrów, ale możesz to zrobić. Jeśli chcesz zobaczyć wartości parametrów, możesz napisać kod rejestrowania, aby uzyskać wartości parametrów z właściwości `Parameters` obiektu `DbCommand`, który otrzymujesz w metodach interceptora.

    Należy zauważyć, że nie można powtórzyć tego testu, jeśli aplikacja nie zostanie zatrzymana i nie zostanie uruchomiona ponownie. Jeśli chcesz mieć możliwość wielokrotnego testowania odporności połączenia w jednym przebiegu aplikacji, możesz napisać kod, aby zresetować Licznik błędów w `SchoolInterceptorTransientErrors`.
4. Aby zobaczyć różnicę w zakresie strategii wykonywania (zasady ponawiania), Dodaj komentarz do wiersza `SetExecutionStrategy` w *SchoolConfiguration.cs*, uruchom ponownie stronę uczniów w trybie debugowania i wyszukaj ciąg "throw" ponownie.

    Tym razem debuger przestaje korzystać z pierwszego wygenerowanego wyjątku bezpośrednio, gdy próbuje wykonać zapytanie po raz pierwszy.

    ![Fikcyjny wyjątek](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Usuń znaczniki komentarza z wiersza *SetExecutionStrategy* w *SchoolConfiguration.cs*.

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów Entity Framework można znaleźć w temacie [ASP.NET Data Access — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Włączona odporność połączenia
> * Włączono przechwycenie polecenia
> * Przetestowano nową konfigurację

Przejdź do następnego artykułu, aby dowiedzieć się więcej na temat migracji Code First i wdrażania platformy Azure.
> [!div class="nextstepaction"]
> [Code First migracje i wdrażanie na platformie Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
