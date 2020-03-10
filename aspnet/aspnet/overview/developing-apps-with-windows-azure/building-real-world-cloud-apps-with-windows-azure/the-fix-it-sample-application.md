---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Dodatek: przykładowa aplikacja do rozwiązywania problemów (Tworzenie aplikacji w chmurze w rzeczywistości na platformie Azure) | Microsoft Docs'
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 896196bdb6a6b0d12a6c798ead510e37dd38a9fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583448"
---
# <a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Dodatek: przykładowa aplikacja do rozwiązywania problemów (Tworzenie rzeczywistych aplikacji w chmurze przy użyciu platformy Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz projekt poprawki IT](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Ten dodatek do tworzenia aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure zawiera następujące sekcje, które zawierają dodatkowe informacje na temat rozwiązywania problemów przykładowej aplikacji, którą można pobrać:

- [Znane problemy](#knownissues)
- [Najlepsze rozwiązania](#bestpractices)
- [Jak uruchomić aplikację z programu Visual Studio na komputerze lokalnym](#run-in-vs)
- [Jak wdrożyć aplikację podstawową w Azure App Service Web Apps przy użyciu skryptów programu Windows PowerShell](#deploybase)
- [Rozwiązywanie problemów ze skryptami programu Windows PowerShell](#troubleshooting)
- [Jak wdrożyć aplikację przy użyciu przetwarzania w kolejce w celu Azure App Service Web Apps i usługi w chmurze platformy Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Znane problemy

Aplikacja do rozwiązywania problemów została pierwotnie opracowana w celu zilustrowania jak najprawdopodobniej niektórych wzorców przedstawionych w tej książce elektronicznej. Jednak ponieważ książka elektroniczna dotyczy tworzenia aplikacji w rzeczywistości, firma Microsoft pomogła rozwiązać ten problem w procesie przeglądu i testowania podobnym do tego, co robimy w przypadku wydanego oprogramowania. Znaleźliśmy wiele problemów i tak jak w przypadku wszystkich aplikacji w świecie rzeczywistym, niektóre z nich zostały naprawione i niektóre z nich zostały odroczone do nowszej wersji.

Poniższa lista zawiera problemy, które powinny być rozwiązane w aplikacji produkcyjnej, ale z jednej przyczyny lub innym, że nie postanowili się, aby nie poradzić sobie z początkową wersją przykładowej aplikacji.

### <a name="security"></a>Zabezpieczenia

- Upewnij się, że nie możesz przypisać zadania do nieistniejącego właściciela.
- Upewnij się, że możesz tylko wyświetlać i modyfikować zadania utworzone lub przypisane do Ciebie.
- Używaj protokołu HTTPS dla stron logowania i plików cookie uwierzytelniania.
- Określ limit czasu dla plików cookie uwierzytelniania.

### <a name="input-validation"></a>Walidacja danych wejściowych

Ogólnie rzecz biorąc, aplikacja produkcyjna będzie bardziej sprawdzać poprawność danych wejściowych niż poprawka IT. Na przykład rozmiar obrazu/rozmiar pliku obrazu dozwolony dla przekazywania powinien być ograniczony.

### <a name="administrator-functionality"></a>Funkcje administratora

Administrator powinien mieć możliwość zmiany własności istniejących zadań. Na przykład twórca zadania może opuścić firmę, pozostawiając bez nich uprawnienia do obsługi zadania, o ile nie jest włączony dostęp administracyjny.

### <a name="queue-message-processing"></a>Przetwarzanie komunikatów w kolejce

Przetwarzanie komunikatów w kolejce w aplikacji, która została zaprojektowana tak, aby było proste, aby zilustrować wzorzec pracy skoncentrowanej na kolejkach o minimalnej ilości kodu. Ten prosty kod nie będzie odpowiedni dla rzeczywistej aplikacji produkcyjnej.

- Kod nie gwarantuje, że każdy komunikat w kolejce zostanie przetworzony co najwyżej raz. Po otrzymaniu komunikatu z kolejki jest przedział czasu, w którym komunikat jest niewidoczny dla innych odbiorników kolejki. Jeśli limit czasu upłynie przed usunięciem komunikatu, komunikat zostanie ponownie widoczny. W związku z tym, jeśli wystąpienie roli procesu roboczego poświęca długi czas na przetwarzanie komunikatu, teoretycznie możliwe jest, aby ten sam komunikat został przetworzony dwa razy, co spowoduje zduplikowane zadanie w bazie danych. Aby uzyskać więcej informacji o tym problemie, zobacz [Korzystanie z kolejek usługi Azure Storage](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- Logika sondowania kolejki może być bardziej opłacalna dzięki wsadowemu pobieraniu komunikatów. Przy każdym wywołaniu metody [CloudQueue. GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx)występuje koszt transakcji. Zamiast tego można wywołać metodę [CloudQueue. GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (w liczbie mnogiej), która pobiera wiele komunikatów w jednej transakcji. Koszty transakcji dla kolejek usługi Azure Storage są bardzo małe, więc wpływ na koszty nie jest istotny w większości scenariuszy.
- Ścisła pętla w kodzie przetwarzania komunikatów w kolejce powoduje, że koligacja procesora CPU nie korzysta efektywnie z wielordzeniowych maszyn wirtualnych. Lepszym rozwiązaniem jest użycie równoległości zadań do równoległego uruchamiania kilku zadań asynchronicznych.
- Komunikat kolejki — przetwarzanie ma tylko podstawowe obsługę wyjątków. Na przykład kod nie obsługuje [skażonych komunikatów](https://msdn.microsoft.com/library/ms789028.aspx). (Gdy przetwarzanie komunikatów powoduje wyjątek, należy zarejestrować błąd i usunąć komunikat albo rola proces roboczy podejmie próbę przetworzenia go ponownie, a pętla będzie kontynuowała czas nieokreślony).

### <a name="sql-queries-are-unbounded"></a>Zapytania SQL są niepowiązane

Bieżąca poprawka kodu IT nie ma żadnego limitu liczby wierszy, które mogą zostać zwrócone przez zapytania dla stron indeksu. W przypadku wprowadzenia w bazie danych dużej ilości zadań, rozmiar odebranych list może spowodować problemy z wydajnością. Rozwiązaniem jest zaimplementowanie stronicowania. Aby zapoznać się z przykładem, zobacz [sortowanie, filtrowanie i stronicowanie przy użyciu Entity Framework w aplikacji ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Zalecane modele widoku

Aplikacja do rozwiązywania IT używa klasy jednostki FixItTask do przekazywania informacji między kontrolerem a widokiem. Najlepszym rozwiązaniem jest użycie modeli widoku. Model domeny (np. Klasa jednostki FixItTask) jest zaprojektowana na podstawie tego, co jest potrzebne w przypadku trwałości danych, podczas gdy model widoku może być przeznaczony do prezentacji danych. Aby uzyskać więcej informacji, zobacz [12 najważniejszych rozwiązań ASP.NET MVC](https://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Zalecany bezpieczny obiekt BLOB obrazu

Poprawka It zapisuje przekazane obrazy jako publiczne, co oznacza, że każda osoba, która znajdzie adres URL, może uzyskać do nich dostęp. Obrazy mogą być zabezpieczone zamiast publicznie.

### <a name="no-powershell-automation-scripts-for-queues"></a>Brak skryptów automatyzacji programu PowerShell dla kolejek

Przykładowe skrypty automatyzacji programu PowerShell zostały zapisane tylko dla podstawowej wersji poprawki, która działa w całości w Azure App Service Web Apps. Nie podano skryptów do konfigurowania i wdrażania aplikacji sieci Web oraz środowiska usługi w chmurze wymaganego do przetwarzania kolejki.

### <a name="special-handling-for-html-codes-in-user-input"></a>Specjalna obsługa kodów HTML w danych wejściowych użytkownika

ASP.NET automatycznie zapobiega wielu sposobom, w których Złośliwi użytkownicy mogą próbować ataki skryptów między lokacjami, wprowadzając skrypt w polach tekstowych wprowadzanie danych przez użytkownika. I pomocnik `DisplayFor` MVC używany do wyświetlania tytułów zadań i notatek automatycznie kodu HTML — koduje wartości wysyłane do przeglądarki. Ale w aplikacji produkcyjnej możesz potrzebować dodatkowych miar. Aby uzyskać więcej informacji, zobacz temat [Weryfikowanie żądań w ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Najlepsze praktyki

Poniżej przedstawiono niektóre problemy, które zostały poprawione po wykryciu w przeglądzie kodu i przetestowaniu oryginalnej wersji aplikacji poprawki IT. Niektóre z nich wynikają z faktu, że oryginalny program codeer nie wie o konkretnym najlepszym wykorzystaniu, po prostu, ponieważ kod został zapisany szybko i nie był przeznaczony do wydania oprogramowania. Omawiamy tutaj problemy na wypadek wystąpienia tego przeglądu i testowania, które mogą okazać się przydatne dla innych użytkowników, którzy również opracowują aplikacje sieci Web.

### <a name="dispose-the-database-repository"></a>Usuwanie repozytorium bazy danych

Klasa `FixItTaskRepository` musi usunąć wystąpienie `DbContext` Entity Framework. Wprowadziliśmy to przez implementację `IDisposable` w klasie `FixItTaskRepository`:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Należy pamiętać, że AutoFac automatycznie usunie wystąpienie `FixItTaskRepository`, więc nie trzeba go jawnie usunąć.

Inna opcja polega na usunięciu zmiennej składowej `DbContext` z `FixItTaskRepository`, a zamiast tego utworzyć lokalną zmienną `DbContext` w ramach każdej metody repozytorium, wewnątrz instrukcji `using`. Na przykład:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Zarejestruj pojedyncze jako takie przy użyciu DI

Ponieważ wymagana jest tylko jedno wystąpienie klasy `PhotoService` i klasy `Logger`, te klasy powinny być [rejestrowane jako pojedyncze wystąpienia iniekcji zależności](https://code.google.com/p/autofac/wiki/InstanceScope) w *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Zabezpieczenia: nie pokazuj szczegółów błędu użytkownikom

Oryginalna aplikacja do usuwania poprawek nie miała ogólnej strony błędu i wystarczy zezwolić wszystkim wyjątkom na dostęp do interfejsu użytkownika, więc niektóre wyjątki, takie jak błędy połączenia z bazą danych, mogą spowodować wyświetlenie pełnego śladu stosu w przeglądarce. Szczegółowe informacje o błędzie mogą czasami ułatwić ataki złośliwym użytkownikom. Rozwiązanie to rejestruje szczegóły wyjątku i wyświetla stronę błędu użytkownikowi, który nie zawiera szczegółów błędu. Ta aplikacja rozwiązania IT została już zarejestrowana i aby można było wyświetlić stronę błędu, dodaliśmy `<customErrors mode=On>` w pliku Web. config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Domyślnie to powoduje, że *Views\Shared\Error.cshtml* są wyświetlane w przypadku błędów. Można dostosować *Error. cshtml* lub utworzyć własny widok strony błędu i dodać atrybut `defaultRedirect`. Można również określić różne strony błędów dla określonych błędów.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Zabezpieczenia: Zezwalaj tylko na edytowanie zadania przez jego twórcę

Na stronie indeks pulpitu nawigacyjnego są wyświetlane tylko zadania utworzone przez zalogowanego użytkownika, ale złośliwy użytkownik może utworzyć adres URL z IDENTYFIKATORem do zadania innego użytkownika. Dodaliśmy kod w *DashboardController.cs* , aby zwrócić 404 w tym przypadku:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Wyjątki unpołknięci

Oryginalna poprawka aplikacji IT zwróciła wartość null po zarejestrowaniu wyjątku, który powstał w wyniku zapytania SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Spowoduje to przeszukanie użytkownika tak, jakby kwerenda zakończyła się pomyślnie, ale nie zwróciła żadnych wierszy. Rozwiązanie to ponowne zgłoszenie wyjątku Po przechwyceniu i zarejestrowaniu:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Przechwyć wszystkie wyjątki w rolach procesów roboczych

Wszystkie Nieobsłużone wyjątki w roli procesu roboczego spowodują, że maszyna wirtualna zostanie odtworzona, więc chcesz otoczyć wszystkie elementy w bloku try-catch i obsłużyć wszystkie wyjątki.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Określ długość dla właściwości ciągu w klasach jednostek

Aby można było wyświetlić prosty kod, oryginalna wersja aplikacji Fix it nie określiła długości dla pól jednostki FixItTask, a w efekcie zostały zdefiniowane jako varchar (max) w bazie danych. W efekcie interfejs użytkownika akceptuje niemal dowolną ilość danych wejściowych. Określanie długości ustawia limity, które mają zastosowanie zarówno do danych wejściowych użytkownika na stronie sieci Web, jak i w rozmiarze kolumny w bazie danych:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Oznacz prywatne elementy członkowskie jako tylko do odczytu, jeśli nie będą oczekiwane zmiany

Na przykład w klasie `DashboardController` jest tworzone wystąpienie `FixItTaskRepository` i nie jest oczekiwane jego zmiana, dlatego definiujemy ją jako tylko do [odczytu](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Użyj listy. Any () zamiast listy. Count () &gt; 0

Jeśli wiesz, że co najmniej jeden element na liście spełnia określone kryteria, użyj [dowolnej](https://msdn.microsoft.com/library/bb534972.aspx) metody, ponieważ jest ona zwracana zaraz po znalezieniu kryteriów, a metoda `Count` zawsze musi wykonać iterację każdego elementu. Plik *. cshtml indeksu* pulpitu nawigacyjnego pierwotnie miał ten kod:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Ten element został zmieniony na:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generowanie adresów URL w widokach MVC przy użyciu pomocników MVC

W przypadku przycisku **Utwórz poprawkę** do tej strony głównej aplikacja Poprawka It stała zakotwiczenie elementu:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

W przypadku linków do wyświetlania/akcji, takich jak ta, lepiej jest używać pomocnika HTML [. Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) , na przykład:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Użyj zadania. opóźnienie zamiast wątku. uśpienia w roli procesu roboczego

Szablon New-Project umieszcza `Thread.Sleep` w przykładowym kodzie dla roli proces roboczy, ale powoduje, że wątek przestanie być w stanie uśpienia w celu rozdzielenia dodatkowych niepotrzebnych wątków w puli wątków. Można uniknąć tego, aby zamiast tego użyć polecenia [Task. Delay](https://msdn.microsoft.com/library/hh139096.aspx) .

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Unikaj asynchronicznego unieważnienia

Jeśli Metoda asynchroniczna nie musi zwracać wartości, zwróć typ `Task`, a nie `void`.

Ten przykład pochodzi z klasy `FixItQueueManager`:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Należy używać `async void` tylko dla programów obsługi zdarzeń najwyższego poziomu. Jeśli określisz metodę jako `async void`, obiekt wywołujący nie może **czekać** na metodę ani wychwycić żadnych wyjątków zwracanych przez metodę. Aby uzyskać więcej informacji, zobacz [najlepsze rozwiązania w programowaniu asynchronicznym](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Użyj tokenu anulowania, aby przerwać pętlę roli procesu roboczego

Typowo, Metoda **Run** w roli proces roboczy zawiera pętlę nieskończoną. Gdy rola procesu roboczego jest zatrzymywana, wywoływana jest metoda [RoleEntryPoint. OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) . Tej metody należy użyć, aby anulować pracę wykonywaną wewnątrz metody **Run** i bezpiecznie zakończyć pracę. W przeciwnym razie proces może zostać zakończony w trakcie operacji.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Rezygnacja z automatycznej procedury wykrywania MIME

W niektórych przypadkach program Internet Explorer zgłasza typ MIME inny niż typ określony przez serwer sieci Web. Na przykład jeśli program Internet Explorer odnajdzie zawartość HTML w pliku dostarczonym z zawartością nagłówka odpowiedzi HTTP-Type: text/zwykły, program Internet Explorer określa, że zawartość powinna być renderowana jako HTML. Niestety, to "wykrywanie MIME" może również prowadzić do problemów z zabezpieczeniami serwerów obsługujących niezaufaną zawartość. Aby rozwiązać ten problem, w programie Internet Explorer 8 wprowadzono wiele zmian w kodzie wyznaczania typów MIME i umożliwiają deweloperom aplikacji [rezygnację z wykrywania MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Następujący kod został dodany do pliku *Web. config* .

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Włącz zgrupowanie i minifikacja

Gdy program Visual Studio tworzy nowy projekt sieci Web, zgrupowanie i minifikacja plików JavaScript nie jest domyślnie włączona. Dodaliśmy wiersz kodu w BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Ustawianie limitu czasu wygaśnięcia dla plików cookie uwierzytelniania

Domyślnie pliki cookie uwierzytelniania wygasają w dwóch tygodniach. Krótszy czas jest bezpieczniejszy. To ustawienie można zmienić w *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Jak uruchomić aplikację z programu Visual Studio na komputerze lokalnym

Istnieją dwa sposoby uruchamiania aplikacji Fix it:

- Uruchom aplikację podstawową, która zapisuje nowe zadania bezpośrednio w bazie danych SQL.
- Uruchom aplikację przy użyciu kolejki oraz usługi wewnętrznej bazy danych, aby utworzyć zadania. Wzorzec kolejki jest opisany w rozdziale [roboczym zorientowanym na kolejki](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Uruchom aplikację podstawową

1. Zainstaluj program [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Zainstaluj [zestaw Azure SDK dla programu .NET dla programu Visual Studio](https://azure.microsoft.com/downloads/).
3. Pobierz plik zip z [galerii kodu MSDN](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. W Eksploratorze plików kliknij prawym przyciskiem myszy plik zip, a następnie kliknij pozycję Właściwości, a następnie w okno Właściwości kliknij pozycję Odblokuj.
5. Rozpakuj plik.
6. Kliknij dwukrotnie plik. sln, aby uruchomić program Visual Studio.
7. W menu **Narzędzia** kliknij kolejno pozycje **Menedżer pakietów NuGet**, **konsola Menedżera pakietów**.
8. W konsoli Menedżera pakietów (PMC) kliknij przycisk Przywróć.
9. Zamknij program Visual Studio.
10. Uruchom [emulator usługi Azure Storage](/azure/storage/common/storage-use-emulator).
11. Uruchom ponownie program Visual Studio, otwierając plik rozwiązania zamknięty w poprzednim kroku.
12. Upewnij się, że projekt FixIt jest ustawiony jako projekt startowy, a następnie naciśnij klawisze CTRL + F5, aby uruchomić projekt.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Uruchamianie aplikacji przy użyciu przetwarzania kolejki

1. Postępuj zgodnie z instrukcjami dotyczącymi [uruchamiania aplikacji podstawowej](#runbase), a następnie zamknij przeglądarkę i Zamknij program Visual Studio.
2. Uruchom program Visual Studio z uprawnieniami administratora. (Będziesz korzystać z emulatora obliczeń platformy Azure, który wymaga uprawnień administratora).
3. W pliku *Web. config* aplikacji w projekcie *MyFixIt* (projekt sieci Web) Zmień wartość `appSettings/UseQueues` na "true":

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Jeśli [emulator usługi Azure Storage](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) nie jest nadal uruchomiony, uruchom go ponownie.
5. Uruchom projekt sieci Web FixIt i projekt MyFixItCloudService jednocześnie.

    Korzystanie z programu Visual Studio:

   1. Naciśnij klawisz **F5** , aby uruchomić projekt fixit.
   2. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt MyFixItCloudService, a następnie kliknij pozycję **Debuguj** > **Uruchom nowe wystąpienie**.

    Korzystanie z programu Visual Studio 2013 Express for Web:

   3. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązanie FixIt i wybierz pozycję **Właściwości**.
   4. Wybierz **wiele projektów startowych**.
   5. Na liście rozwijanej **Akcja** w obszarze MyFixIt i MyFixItCloudService wybierz pozycję **Rozpocznij**.
   6. Kliknij przycisk **OK**.
   7. Naciśnij klawisz **F5**, aby uruchomić oba projekty.

      Po uruchomieniu projektu MyFixItCloudService program Visual Studio uruchamia emulator obliczeń platformy Azure. W zależności od konfiguracji zapory może być konieczne zezwolenie emulatora przez zaporę.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Jak wdrożyć aplikację podstawową w Azure App Service Web Apps przy użyciu skryptów programu Windows PowerShell

Aby zilustrować zautomatyzowany wzorzec [wszystkiego](automate-everything.md) , poprawka aplikacji IT jest dostarczana ze skryptami, które konfigurują środowisko na platformie Azure i wdrażają projekt w nowym środowisku. Poniższe instrukcje wyjaśniają, jak używać skryptów.

Jeśli chcesz uruchomić program na platformie Azure bez korzystania z kolejek, a zmiany mają być uruchamiane lokalnie z kolejkami, przed wykonaniem poniższych instrukcji upewnij się, że wartość UseQueues element appSetting została ustawiona na false.

W tych instrukcjach przyjęto założenie, że zostało już pobrane i uruchomione rozwiązanie poprawki IT i że masz konto platformy Azure lub masz subskrypcję platformy Azure, którą chcesz zarządzać.

1. Zainstaluj konsolę **Azure PowerShell** . Aby uzyskać instrukcje, zobacz [How to Install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Ta dostosowana konsola jest skonfigurowana do pracy z subskrypcją platformy Azure. Moduł platformy Azure jest instalowany w katalogu *Program Files* i automatycznie importowany przy każdym użyciu konsoli Azure PowerShell.

    Jeśli wolisz korzystać z innego programu hosta, takiego jak Windows PowerShell ISE, należy użyć polecenia cmdlet [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) do zaimportowania modułu platformy Azure lub użyć polecenia w module platformy Azure, aby wyzwolić automatyczne Importowanie modułu.
2. Uruchom Azure PowerShell przy użyciu opcji **Uruchom jako administrator** .
3. Uruchom polecenie cmdlet [Set-executionpolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) , aby ustawić zasady wykonywania Azure PowerShell na `RemoteSigned`. Wprowadź wartość **Y** (tak), aby zakończyć zmianę zasad.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    To ustawienie umożliwia uruchamianie lokalnych skryptów, które nie są podpisane cyfrowo. (Można również ustawić zasady wykonywania na `Unrestricted`, co eliminuje konieczność późniejszego odblokowania, ale nie jest to zalecane ze względów bezpieczeństwa).
4. Uruchom polecenie cmdlet `Add-AzureAccount`, aby skonfigurować program PowerShell z poświadczeniami dla Twojego konta.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Te poświadczenia wygasną po upływie upływu czasu i konieczne będzie ponowne uruchomienie `Add-AzureAccount`go polecenia cmdlet. Podczas pisania tej książki elektronicznej limit czasu przed wygaśnięciem poświadczeń wynosi 12 godzin.
5. Jeśli masz wiele subskrypcji, użyj polecenia cmdlet Select-AzureSubscription, aby określić subskrypcję, w której chcesz utworzyć środowisko testowe.
6. Zaimportuj certyfikat zarządzania dla tej samej subskrypcji platformy Azure za pomocą poleceń cmdlet `Get-AzurePublishSettingsFile` i `Import-AzurePublishSettingsFile`. Pierwsze z tych poleceń cmdlet pobiera plik certyfikatu, a drugi określa lokalizację tego pliku do zaimportowania. > [!IMPORTANT]
   > Zachowaj pobrany plik w bezpiecznym miejscu lub usuń go po zakończeniu pracy z nim, ponieważ zawiera certyfikat, którego można użyć do zarządzania usługami platformy Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Certyfikat jest używany do wywołania interfejsu API REST, który wykrywa adres IP maszyny deweloperskiej w celu ustawienia reguły zapory na serwerze SQL Database.
7. Uruchom polecenie cmdlet [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) (aliasy są `cd`, `chdir`i `sl`), aby przejść do katalogu zawierającego skrypty. (Znajdują się one w folderze *Automation* w folderze rozwiązanie IT). Ścieżka należy umieścić w cudzysłowie, jeśli którykolwiek z nazw katalogów zawiera spacje. Na przykład, aby przejść do katalogu `c:\Sample Apps\FixIt\Automation`, można wprowadzić następujące polecenie:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Aby można było uruchamiać te skrypty za pomocą programu Windows PowerShell, należy użyć polecenia cmdlet [odblokowywania pliku](https://go.microsoft.com/fwlink/p/?linkid=294021) . (Skrypty są blokowane, ponieważ zostały pobrane z Internetu).

    > [!WARNING]
    > Zabezpieczenia — przed uruchomieniem `Unblock-File` na dowolnym skrypcie lub pliku wykonywalnym Otwórz plik w Notatniku, sprawdź polecenia i sprawdź, czy nie zawierają one złośliwego kodu.

    Na przykład następujące polecenie uruchamia `Unblock-File` polecenie cmdlet dla wszystkich skryptów w bieżącym katalogu.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Aby utworzyć aplikację sieci Web dla bazy (bez przetwarzania kolejek), należy uruchomić skrypt tworzenia środowiska.

    Wymagany `Name` parametr określa nazwę bazy danych i jest również używany dla konta magazynu tworzonego przez skrypt. Nazwa musi być globalnie unikatowa w domenie azurewebsites.net. Jeśli określisz nazwę, która nie jest unikatowa, np. fixit lub test (lub nawet tak jak w przykładzie, fixitdemo), polecenie cmdlet `New-AzureWebsite` kończy się niepowodzeniem z powodu wewnętrznego błędu, który zgłasza konflikt. Skrypt konwertuje nazwę na małe litery, aby zapewnić zgodność z wymaganiami dotyczącymi nazw dla aplikacji sieci Web, kont magazynu i baz danych.

    Wymagany `SqlDatabasePassword` parametr określa hasło dla konta administratora, które zostanie utworzone dla SQL Database. Nie dodawaj specjalnych znaków XML w haśle (&amp; &lt; &gt;;). Jest to ograniczenie sposobu pisania skryptów, a nie ograniczenia platformy Azure.

    Jeśli na przykład chcesz utworzyć aplikację sieci Web o nazwie "fixitdemo" i użyć hasła administratora SQL Server "Passw0rd1", możesz wprowadzić następujące polecenie:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Nazwa musi być unikatowa w domenie azurewebsites.net, a hasło musi spełniać SQL Database wymagania dotyczące złożoności hasła. (Przykład Passw0rd1 spełnia wymagania).

    Należy pamiętać, że polecenie zaczyna się od ".\". Aby zapobiec złośliwemu wykonywaniu skryptów, program Windows PowerShell wymaga podania w pełni kwalifikowanej ścieżki do pliku skryptu podczas uruchamiania skryptu. Możesz użyć kropki, aby wskazać bieżący katalog (".\") lub podaj w pełni kwalifikowaną ścieżkę, taką jak:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Aby uzyskać więcej informacji na temat skryptu, użyj polecenia cmdlet `Get-Help`.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Możesz użyć parametrów `Detailed`, `Full`, `Parameters`i `Examples` polecenia cmdlet Get-Help, aby odfiltrować zwracaną pomoc.

    Jeśli skrypt zakończy się niepowodzeniem lub generuje błędy, takie jak "New-AzureWebsite: Call Set-AzureSubscription i Select-AzureSubscription First", prawdopodobnie nie ukończono konfiguracji Azure PowerShell.

    Po zakończeniu działania skryptu możesz użyć portal zarządzania platformy Azure, aby wyświetlić zasoby, które zostały utworzone, jak pokazano w rozdziale [Automatyzowanie wszystkiego](automate-everything.md) .
10. Aby wdrożyć projekt FixIt w nowym środowisku platformy Azure, Użyj skryptu *AzureWebsite. ps1* . Na przykład:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Po zakończeniu wdrażania zostanie otwarta przeglądarka z poprawkami działającymi na platformie Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Rozwiązywanie problemów ze skryptami programu Windows PowerShell

Najczęstsze błędy występujące podczas uruchamiania tych skryptów są związane z uprawnieniami. Upewnij się, że `Add-AzureAccount` i `Import-AzurePublishSettingsFile` zostały pomyślnie i zostały użyte dla tej samej subskrypcji platformy Azure. Nawet w przypadku pomyślnego `Add-AzureAccount` może być konieczne jego ponowne uruchomienie. Uprawnienia dodane przez `Add-AzureAccount` wygasną w ciągu 12 godzin.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Odwołanie do obiektu nie jest ustawione na wystąpienie obiektu.

Jeśli skrypt zwróci błędy, takie jak "odwołanie do obiektu nie jest ustawione na wystąpienie obiektu", co oznacza, że środowisko Windows PowerShell nie może znaleźć obiektu do przetworzenia (jest to wyjątek odwołania o wartości null), uruchom polecenie cmdlet `Add-AzureAccount` i spróbuj ponownie wykonać skrypt.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: Serwer napotkał błąd wewnętrzny.

`New-AzureWebsite` polecenie cmdlet zwraca błąd wewnętrzny, gdy nazwa nie jest unikatowa w domenie azurewebsites.net. Aby rozwiązać ten problem, użyj innej wartości dla nazwy, która znajduje się w parametrze name elementu *New-AzureWebsiteEnv. ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Ponowne uruchamianie skryptu

Jeśli konieczne jest ponowne uruchomienie skryptu *New-AzureWebsiteEnv. ps1* , ponieważ nie powiodło się, zanim wywydrukowano komunikat "skrypt jest kompletny", można usunąć zasoby utworzone przez skrypt przed jego zatrzymaniem. Jeśli na przykład skrypt utworzył już aplikację sieci Web ContosoFixItDemo, a skrypt zostanie uruchomiony ponownie z tą samą nazwą, skrypt zakończy się niepowodzeniem, ponieważ nazwa jest w użyciu.

Aby określić, które zasoby utworzono przed zatrzymaniem skryptu, użyj następujących poleceń cmdlet:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Aby uruchomić to polecenie cmdlet, potoku nazwę serwera bazy danych do `Get-AzureSqlDatabase`: `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Aby usunąć te zasoby, użyj następujących poleceń. Należy pamiętać, że w przypadku usunięcia serwera bazy danych program automatycznie usunie bazy danych skojarzone z serwerem.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Jak wdrożyć aplikację przy użyciu przetwarzania w kolejce w celu Azure App Service Web Apps i usługi w chmurze platformy Azure

Aby włączyć kolejki, wprowadź następujące zmiany w pliku MyFixIt\Web.config. W obszarze `appSettings`Zmień wartość `UseQueues` na "true":

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Następnie wdróż aplikację MVC w aplikacji sieci Web w Azure App Service, zgodnie z [wcześniejszym](#deploybase)opisem.

Następnie utwórz nową usługę w chmurze platformy Azure. Skrypty dołączone do aplikacji Fix it nie tworzą ani nie wdrażają usługi w chmurze, dlatego należy użyć Azure Portal. W portalu kliknij kolejno pozycje **nowy** -- **obliczenia** — **Usługa w chmurze** -- **szybkie tworzenie**, a następnie wprowadź adres URL i lokalizację centrum danych. Użyj tego samego centrum danych, w którym została wdrożona aplikacja internetowa.

![](the-fix-it-sample-application/_static/image1.png)

Aby można było wdrożyć usługę w chmurze, należy zaktualizować niektóre pliki konfiguracji.

W MyFixIt. WorkerRole\app.config w obszarze `connectionStrings`Zastąp wartość parametrów połączenia `appdb` z rzeczywistymi parametrami połączenia dla SQL Database. Parametry połączenia można uzyskać z portalu. W portalu kliknij pozycję **bazy danych SQL** - **AppDB** - **Wyświetl parametry połączenia SQL Database dla obiektów ADO .NET, ODBC, php i JDBC**. Skopiuj parametry połączenia ADO.NET i wklej wartość do pliku App. config. Zastąp ciąg "{Twoim\_hasłem\_tutaj}" hasłem Twojej bazy danych. (Przy założeniu, że do wdrożenia aplikacji MVC użyto skryptów, w parametrze skryptu `SqlDatabasePassword` określono hasło bazy danych).

Wynik powinien wyglądać następująco:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

W tym samym pliku MyFixIt. WorkerRole\app.config w obszarze `appSettings`Zamień dwie wartości zastępcze dla konta usługi Azure Storage.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Klucz dostępu można uzyskać z portalu. Zobacz [jak zarządzać kontami magazynu](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

W programie MyFixItCloudService\ServiceConfiguration.Cloud.cscfg Zastąp te same dwa symbole zastępcze dla konta usługi Azure Storage.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Teraz możesz przystąpić do wdrażania usługi w chmurze. W oknie Eksplorowanie rozwiązań kliknij prawym przyciskiem myszy projekt MyFixItCloudService i wybierz polecenie **Publikuj**. Aby uzyskać więcej informacji, zobacz sekcję "[wdrażanie aplikacji na platformie Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", która znajduje się w części 2 [tego samouczka](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Wstecz](more-patterns-and-guidance.md)
