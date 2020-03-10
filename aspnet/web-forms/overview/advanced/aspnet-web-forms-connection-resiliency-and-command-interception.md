---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: Odporność połączenia formularzy sieci Web ASP.NET i przechwycenie poleceń | Microsoft Docs
author: Erikre
description: W tym samouczku opisano sposób modyfikowania przykładowej aplikacji do obsługi odporności połączeń i przechwycenia poleceń.
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 95f0b5635c12d5ef88622e5766c1278c6570dd4d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598428"
---
# <a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Odporność połączeń wzorca ASP.NET Web Forms i przejmowanie poleceń

Autor [Erik Reitan](https://github.com/Erikre)

W tym samouczku zmodyfikujesz przykładową aplikację Wingtip zabawki w celu zapewnienia obsługi odporności połączeń i przechwycenia poleceń. Po włączeniu odporności połączenia Przykładowa aplikacja Wingtip zabawki automatycznie ponowi próbę wykonania wywołań danych w przypadku wystąpienia błędów przejściowych typowych dla środowiska chmury. Ponadto przez implementację przechwycenia poleceń Przykładowa aplikacja Wingtip zabawki przechwytuje wszystkie zapytania SQL wysyłane do bazy danych w celu ich zarejestrowania lub zmiany.

> [!NOTE] 
> 
> Ten samouczek dotyczący formularzy sieci Web został oparty na następującym samouczku MVC Dykstra:  
> [Odporność połączeń i przechwycenie poleceń z Entity Framework w aplikacji ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="what-youll-learn"></a>Zawartość:

- Jak zapewnić odporność połączenia.
- Jak zaimplementować przechwycenie polecenia.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że na komputerze jest zainstalowane następujące oprogramowanie:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [Microsoft Visual Studio Express 2013 dla sieci Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework jest instalowana automatycznie.
- Przykładowy projekt Wingtip zabawki, dzięki czemu można zaimplementować funkcje wymienione w tym samouczku w projekcie Wingtip zabawki. Poniższy link zawiera szczegółowe informacje dotyczące pobierania:

    - [Wprowadzenie z ASP.NET 4.5.1 Web Forms — zabawki Wingtip](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Przed ukończeniem tego samouczka Rozważ przejrzenie powiązanej serii samouczków [wprowadzenie przy użyciu formularzy sieci Web ASP.NET 4,5 i Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Seria samouczków pomoże Ci zaznajomić się z projektem i kodem **WingtipToys** .

## <a name="connection-resiliency"></a>Elastyczność połączenia

W przypadku wdrażania aplikacji na platformie Microsoft Azure należy wziąć pod uwagę wdrożenie bazy danych do **systemu windows** **Azure SQL Database**, usługi bazy danych w chmurze. Błędy połączeń przejściowych są zwykle częstsze w przypadku łączenia się z usługą bazy danych w chmurze, niż w przypadku bezpośredniego połączenia serwera sieci Web i serwera bazy danych w tym samym centrum danych. Nawet jeśli serwer sieci Web w chmurze i usługa bazy danych w chmurze są hostowane w tym samym centrum danych, istnieje więcej połączeń sieciowych między nimi, które mogą mieć problemy, takie jak moduły równoważenia obciążenia.

Ponadto usługa w chmurze jest zwykle udostępniana innym użytkownikom, co oznacza, że może to mieć wpływ na jego czas odpowiedzi. A dostęp do bazy danych może podlegać ograniczaniu wydajności. Ograniczenie przepustowości oznacza, że usługa bazy danych zgłasza wyjątki, gdy próbujesz uzyskać do niej dostęp częściej niż jest to dozwolone w *Umowa dotycząca poziomu usług* (SLA).

Wiele lub większość problemów z połączeniem, które występują podczas uzyskiwania dostępu do usługi w chmurze, jest przejściowa, czyli rozstrzygają się w krótkim czasie. Dlatego podczas próby wykonania operacji bazy danych i uzyskania typu błędu, który zwykle jest przejściowy, można spróbować ponownie wykonać operację po krótkim czasie oczekiwania, a operacja może zakończyć się pomyślnie. W przypadku obsługi błędów przejściowych można zapewnić użytkownikom wiele lepszych możliwości, co powoduje, że większość z nich jest niewidoczna dla klienta. Funkcja odporności połączeń w Entity Framework 6 automatyzuje proces ponawiania nieudanych zapytań SQL.

Funkcja odporności połączenia musi być odpowiednio skonfigurowana dla określonej usługi bazy danych:

1. Należy wiedzieć, które wyjątki mogą być przejściowe. Na przykład chcesz ponowić próbę błędów spowodowanych przez tymczasową utratę łączności sieciowej, a nie błędy spowodowane przez błędy programu.
2. Musi upłynąć odpowiednią ilość czasu między ponownymi próbami nieudanej operacji. Możesz poczekać dłużej między ponownymi próbami procesu wsadowego niż w przypadku strony sieci Web w trybie online, w której użytkownik czeka na odpowiedź.
3. Należy ponowić próbę odpowiednią liczbę razy, zanim zostanie ona nadana. Można ponowić więcej razy w procesie wsadowym, który będzie w aplikacji online.

Te ustawienia można skonfigurować ręcznie dla dowolnego środowiska bazy danych obsługiwanego przez dostawcę Entity Framework.

Wszystkie czynności, które należy wykonać w celu włączenia odporności połączenia, to utworzenie klasy w zestawie, która pochodzi od klasy `DbConfiguration`, a w tej klasie ustawia strategię wykonywania SQL Database, która w Entity Framework jest innym terminem dla zasad ponawiania próby.

### <a name="implementing-connection-resiliency"></a>Implementowanie odporności połączeń

1. Pobierz i Otwórz przykładową aplikację [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) Web Forms w programie Visual Studio.
2. W folderze *logiki* aplikacji **WingtipToys** Dodaj plik klasy o nazwie *WingtipToysConfiguration.cs*.
3. Zastąp istniejący kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework automatycznie uruchamia kod znaleziony w klasie, która pochodzi od `DbConfiguration`. Klasy `DbConfiguration` można użyć do wykonywania zadań konfiguracyjnych w kodzie, który można w przeciwnym razie wykonać w pliku *Web. config* . Aby uzyskać więcej informacji, zobacz temat [Konfiguracja oparta na kodzie EntityFramework](https://msdn.microsoft.com/data/jj680699).

1. W folderze *logiki* otwórz plik *AddProducts.cs* .
2. Dodaj instrukcję `using` dla `System.Data.Entity.Infrastructure`, jak pokazano na żółto:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Dodaj blok `catch` do metody `AddProduct`, aby `RetryLimitExceededException` był rejestrowany jako wyróżniony na żółto:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Dodając wyjątek `RetryLimitExceededException`, można zapewnić lepsze rejestrowanie lub wyświetlić komunikat o błędzie dla użytkownika, na którym mogą oni zdecydować się na ponowne wypróbowanie procesu. Przechwycenie wyjątku `RetryLimitExceededException` powoduje, że jedynymi błędami, które prawdopodobnie byłyby przejściowe, zostaną już wypróbowane i kilkakrotnie zakończyły się niepowodzeniem. Faktyczny zwrócony wyjątek zostanie opakowany w wyjątek `RetryLimitExceededException`. Ponadto dodano również ogólny blok catch. Aby uzyskać więcej informacji o wyjątku `RetryLimitExceededException`, zobacz [Entity Framework odporność połączenia/logika ponowień](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Przechwycenie polecenia

Po włączeniu zasad ponawiania, jak przeprowadzić test, aby sprawdzić, czy działa zgodnie z oczekiwaniami? Nie jest tak łatwo, aby wymusić wystąpienie błędu przejściowego, szczególnie w przypadku uruchamiania lokalnego, a w szczególności trudno jest zintegrować rzeczywiste błędy przejściowe w zautomatyzowany test jednostkowy. Aby przetestować funkcję odporności połączeń, trzeba przechwycić zapytania wysyłane Entity Framework do SQL Server i zamienić SQL Server odpowiedź na typ wyjątku, który jest zazwyczaj przejściowy.

Można również użyć przechwycenia zapytania, aby zaimplementować najlepsze rozwiązanie dla aplikacji w chmurze: rejestrować opóźnienia oraz sukcesy i niepowodzenia wszystkich wywołań usług zewnętrznych, takich jak usługi bazy danych.

W tej części samouczka użyjesz [*funkcji przechwycenia*](https://msdn.microsoft.com/data/dn469464) Entity Framework zarówno w przypadku rejestrowania, jak i symulowania błędów przejściowych.

### <a name="create-a-logging-interface-and-class"></a>Tworzenie interfejsu rejestrowania i klasy

Najlepszym rozwiązaniem w przypadku rejestrowania jest użycie [`interface`](https://msdn.microsoft.com/library/ms173156.aspx) , a nie twardych wywołań do `System.Diagnostics.Trace` lub klasy rejestrowania. Dzięki temu można łatwiej zmienić mechanizm rejestrowania później, jeśli kiedykolwiek trzeba to zrobić. W tej sekcji utworzysz interfejs rejestrowania i klasę, aby zaimplementować ją.

Na podstawie powyższej procedury pobrano i otwarto przykładową aplikację **WingtipToys** w programie Visual Studio.

1. Utwórz folder w projekcie **WingtipToys** i nadaj mu nazwę *Rejestrowanie*.
2. W folderze *Rejestrowanie* Utwórz plik klasy o nazwie *ILogger.cs* i Zastąp domyślny kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   Interfejs udostępnia trzy poziomy śledzenia, aby wskazać względną ważność dzienników i został zaprojektowany z myślą o dostarczaniu informacji o opóźnieniu dla wywołań usług zewnętrznych, takich jak zapytania bazy danych. Metody rejestrowania mają przeciążenia umożliwiające przekazanie wyjątku. Jest tak dlatego, że informacje o wyjątkach, w tym ślady stosu i wyjątki wewnętrzne, są niezawodnie rejestrowane przez klasę, która implementuje interfejs, zamiast polegać na tym, że są wykonywane w ramach każdego wywołania metody rejestrowania w całej aplikacji.  
  
   Metody `TraceApi` umożliwiają śledzenie opóźnienia każdego wywołania usług zewnętrznych, takich jak SQL Database.
3. W folderze *Rejestrowanie* Utwórz plik klasy o nazwie *Logger.cs* i Zastąp domyślny kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

Implementacja używa `System.Diagnostics` do wykonania śledzenia. Jest to wbudowana funkcja programu .NET, która ułatwia generowanie i używanie informacji śledzenia. Istnieje wiele &quot;detektorów&quot; można użyć z śledzeniem `System.Diagnostics`, aby zapisywać dzienniki w plikach, na przykład lub zapisywać je w usłudze BLOB Storage w systemie Windows Azure. Zobacz niektóre opcje i linki do innych zasobów, aby uzyskać więcej informacji na [temat rozwiązywania problemów z witrynami sieci Web systemu Windows Azure w programie Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). W ramach tego samouczka nastąpi Przeglądanie dzienników w oknie **danych wyjściowych** programu Visual Studio.

W aplikacji produkcyjnej warto rozważyć użycie struktur śledzenia innych niż `System.Diagnostics`, a interfejs `ILogger` pozwala stosunkowo łatwo przełączyć się do innego mechanizmu śledzenia, jeśli zdecydujesz się to zrobić.

### <a name="create-interceptor-classes"></a>Utwórz klasy interceptorów

Następnie utworzysz klasy, do których będzie wywoływana Entity Framework za każdym razem, gdy wyśle zapytanie do bazy danych, jeden w celu symulowania błędów przejściowych i jednego do rejestrowania. Te klasy przechwyceń muszą pochodzić od klasy `DbCommandInterceptor`. W nich są zapisywane zastąpienia metod, które są automatycznie wywoływane, gdy zapytanie ma zostać wykonane. W tych metodach można badać lub rejestrować zapytania wysyłane do bazy danych, a następnie można zmienić zapytanie przed jego wysłaniem do bazy danych lub zwrócić coś do Entity Framework siebie bez przekazywania zapytania do bazy danych.

1. Aby utworzyć klasę interceptora, która będzie rejestrować wszystkie zapytania SQL przed wysłaniem ich do bazy danych, Utwórz plik klasy o nazwie *InterceptorLogging.cs* w folderze *Logic* i Zastąp domyślny kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   W przypadku pomyślnych zapytań lub poleceń ten kod zapisuje dziennik informacyjny z informacjami o opóźnieniu. W przypadku wyjątków tworzy dziennik błędów.
2. Aby utworzyć klasę interceptora, która będzie generować fikcyjne błędy przejściowe po wprowadzeniu &quot;throw&quot; w polu tekstowym **Nazwa** na stronie o nazwie *AdminPage. aspx*, Utwórz plik klasy o nazwie *InterceptorTransientErrors.cs* w folderze *logiki* i Zastąp kod domyślny następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Ten kod zastępuje tylko metodę `ReaderExecuting`, która jest wywoływana dla zapytań, które mogą zwracać wiele wierszy danych. Jeśli chcesz sprawdzić odporność połączenia dla innych typów zapytań, możesz również zastąpić metody `NonQueryExecuting` i `ScalarExecuting`, ponieważ przechwycenie rejestrowania działa.  
  
   Później zalogujesz się jako "Administrator" i wybierzesz link **administracyjny** na górnym pasku nawigacyjnym. Następnie na stronie *AdminPage. aspx* zostanie dodany produkt o nazwie Throw&quot;&quot;. Kod tworzy fikcyjny SQL Database wyjątek dla błędu o numerze 20, typ znany zwykle jako przejściowy. Inne numery błędów aktualnie rozpoznawane jako przejściowe to 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 i 40613, ale mogą ulec zmianie w nowych wersjach SQL Database. Nazwa produktu zostanie zmieniona na "TransientErrorExample", który można wykonać w kodzie pliku *InterceptorTransientErrors.cs* .  
  
   Kod zwraca wyjątek do Entity Framework zamiast uruchamiania zapytania i przekazywania wyników. Wyjątek przejściowy jest zwracany *cztery* razy, a następnie kod wraca do normalnej procedury przekazania zapytania do bazy danych.

    Ponieważ wszystko jest rejestrowane, będzie można zobaczyć, że Entity Framework próbuje wykonać zapytanie cztery razy przed zakończeniem, a jedyną różnicą w aplikacji jest to, że przerenderowanie strony z wynikami zapytania trwa dłużej.  
  
   Liczba ponowień prób Entity Framework będzie można skonfigurować; kod jest określany cztery razy, ponieważ jest to wartość domyślna dla zasad wykonywania SQL Database. W przypadku zmiany zasad wykonywania należy również zmienić tutaj kod określający, ile razy są generowane błędy przejściowe. Można również zmienić kod w celu wygenerowania większej liczby wyjątków, aby Entity Framework zgłosi wyjątek `RetryLimitExceededException`.
3. W *Global. asax*Dodaj następujące instrukcje using:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Następnie Dodaj wyróżnione wiersze do metody `Application_Start`:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Te wiersze kodu powodują, że kod interceptora ma być uruchamiany, gdy Entity Framework wysyła zapytania do bazy danych. Należy zauważyć, że ponieważ zostały utworzone oddzielne klasy interceptorów dla przejściowej symulacji błędów i rejestrowania, można je niezależnie włączać i wyłączać.   
  
 Można dodać Interceptory przy użyciu metody `DbInterception.Add` w dowolnym miejscu w kodzie. nie musi on znajdować się w metodzie `Application_Start`. Inna opcja, jeśli nie dodano przechwyceń w metodzie `Application_Start`, byłoby zaktualizować lub dodać klasę o nazwie *WingtipToysConfiguration.cs* i umieścić powyższy kod na końcu konstruktora klasy `WingtipToysConfiguration`.

Wszędzie tam, gdzie umieścisz ten kod, uważaj, aby nie wykonywać `DbInterception.Add` dla tego samego interceptora więcej niż raz, lub uzyskasz dodatkowe wystąpienia przechwyceń. Na przykład po dwukrotnym dodaniu przechwycenia rejestrowania dla każdego zapytania SQL zobaczysz dwa dzienniki.

Interceptory są wykonywane w kolejności rejestracji (kolejność, w jakiej wywoływana jest metoda `DbInterception.Add`). Kolejność może mieć znaczenie w zależności od tego, co robisz w wychwyceniu. Na przykład Interceptor może zmienić polecenie SQL, które jest pobierane we właściwości `CommandText`. Jeśli zmieni polecenie SQL, następnym przechwyceniem będzie uzyskanie zmienionego polecenia SQL, a nie oryginalnego polecenia SQL.

Kod symulacji błędu przejściowego został zapisany w sposób, który umożliwia spowodowanie błędów przejściowych przez wprowadzenie innej wartości w interfejsie użytkownika. Alternatywnie można napisać kod interceptora, aby zawsze generował sekwencję przejściowych wyjątków bez sprawdzania określonej wartości parametru. Następnie można dodać Interceptor tylko wtedy, gdy chcesz wygenerować błędy przejściowe. Jeśli to zrobisz, nie dodawaj interceptora do momentu zakończenia inicjowania bazy danych. Innymi słowy, należy wykonać co najmniej jedną operację bazy danych, taką jak zapytanie na jednym z zestawów jednostek przed rozpoczęciem generowania błędów przejściowych. Entity Framework wykonuje kilka zapytań podczas inicjowania bazy danych i nie są wykonywane w transakcji, więc błędy podczas inicjowania mogą spowodować, że kontekst przestanie być niespójny.

## <a name="test-logging-and-connection-resiliency"></a>Rejestrowanie testów i odporność połączeń

1. W programie Visual Studio naciśnij klawisz **F5** , aby uruchomić aplikację w trybie debugowania, a następnie zaloguj się jako "Administrator" przy użyciu "Pa $ $Word" jako hasło.
2. Na pasku nawigacyjnym u góry wybierz pozycję **administrator** .
3. Wprowadź nowy produkt o nazwie "throw" z odpowiednim opisem, ceną i plikiem obrazu.
4. Naciśnij przycisk **Dodaj produkt** .  
   Zobaczysz, że przeglądarka wydaje się odwiesić przez kilka sekund, podczas gdy Entity Framework próbuje wykonać zapytanie kilka razy. Pierwsze ponowienie próby odbywa się bardzo szybko, a następnie poczekaj, aż do momentu kolejnej dodatkowej próby. Ten proces czekania dłużej przed ponowną próbą nazywa się *wykładniczą wycofywania* .
5. Poczekaj, aż Strona nie będzie już próbować załadować.
6. Zatrzymaj projekt i Sprawdź okno **danych wyjściowych** programu Visual Studio, aby wyświetlić dane wyjściowe śledzenia. Okno **dane wyjściowe** można znaleźć, wybierając pozycję **Debuguj** -&gt; **Windows** -&gt; **dane wyjściowe**. Może być konieczne przewinięcie kilku innych dzienników zapisanych przez rejestrator.  
  
   Zwróć uwagę, że można zobaczyć rzeczywiste zapytania SQL wysyłane do bazy danych. Zobaczysz początkowe zapytania i polecenia, które Entity Framework rozpocząć, sprawdzając wersję bazy danych i tabelę historii migracji.   
    ![Okno Dane wyjściowe](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Należy zauważyć, że nie można powtórzyć tego testu, jeśli aplikacja nie zostanie zatrzymana i nie zostanie uruchomiona ponownie. Jeśli chcesz mieć możliwość wielokrotnego testowania odporności połączenia w jednym przebiegu aplikacji, możesz napisać kod, aby zresetować Licznik błędów w `InterceptorTransientErrors`.
7. Aby zapoznać się z różnicą strategii wykonywania (zasady ponawiania), należy wprowadzić komentarz w wierszu `SetExecutionStrategy` w pliku *WingtipToysConfiguration.cs* w folderze *logiki* , ponownie uruchomić stronę **administratora** w trybie debugowania i dodać produkt o nazwie &quot;throw&quot; ponownie.  
  
   Tym razem debuger przestaje korzystać z pierwszego wygenerowanego wyjątku bezpośrednio, gdy próbuje wykonać zapytanie po raz pierwszy.  
    ![Debugowanie — Wyświetl szczegóły](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Usuń komentarz z wiersza `SetExecutionStrategy` w pliku *WingtipToysConfiguration.cs* .

## <a name="summary"></a>Podsumowanie

W tym samouczku pokazano, jak zmodyfikować przykładową aplikację formularzy sieci Web w celu zapewnienia obsługi odporności połączenia i przechwycenia poleceń.

## <a name="next-steps"></a>Następne kroki

Po przejrzeniu odporności połączenia i przechwycenia poleceń w formularzach sieci Web ASP.NET zapoznaj się z tematem metody asynchroniczne w temacie ASP.NET Web Forms [w ASP.NET 4,5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). W tym temacie omówiono podstawy tworzenia asynchronicznej aplikacji formularzy sieci Web ASP.NET za pomocą programu Visual Studio.
